# 6. Estilo arquitectónico

> **Pregunta guía:** ¿Cómo estructuramos globalmente el sistema?

## Estilo seleccionado

**Cliente-servidor + Monolito modular en capas.**

| Concepto             | Qué significa en este proyecto                                                                                                         |
| -------------------- | -------------------------------------------------------------------------------------------------------------------------------------- |
| **Cliente-servidor** | El frontend (Angular 18) se ejecuta en el navegador y consume el backend mediante API REST (HTTPS + JSON).                             |
| **Monolito**         | El backend (Node.js 20 + Express) es **una sola aplicación, un solo proceso, un solo despliegue** y una sola base de datos PostgreSQL. |
| **Modular**          | Dentro del monolito, cada funcionalidad es un módulo con límites claros: `usuarios`, `sellers`, `catalogo`, `carrito`, `pedidos`.      |
| **En capas**         | Cada módulo se divide en presentación (routes/controller), lógica de negocio (service) y datos (repository).                           |

> Capas = organización **lógica**. Monolito = unidad de **despliegue**.
> Ambos conceptos coexisten.

## Justificación (trazabilidad con drivers)

| Driver                | Cómo lo atiende el estilo                                                                                                                |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| DA01 – Escalabilidad  | El monolito se replica horizontalmente detrás de un balanceador; los módulos permiten extraer un microservicio si un módulo lo necesita. |
| DA02 – Rendimiento    | Caché (Redis) frente al módulo de catálogo; llamadas en proceso, sin latencia de red entre módulos.                                      |
| DA03 – Seguridad      | Middleware transversal de autenticación (JWT), validación de entrada y HTTPS.                                                            |
| DA04 – Pago externo   | El módulo `pedidos` se integra con la pasarela mediante un adaptador HTTPS/REST.                                                         |
| DA05 – API REST       | Frontend y backend separados; contrato `/api/v1/*`.                                                                                      |
| DA06 – Mantenibilidad | Módulos independientes; cada módulo solo usa a otro a través de su _service_.                                                            |

## Alternativas descartadas

| Estilo            | Motivo                                                                                                  |
| ----------------- | ------------------------------------------------------------------------------------------------------- |
| Microservicios    | Alto costo operativo para un equipo pequeño y un dominio en evolución.                                  |
| SOA / ESB         | Pensado para integrar muchos sistemas corporativos; excesivo aquí.                                      |
| Event-driven puro | Útil para notificaciones/envíos asíncronos (se puede incorporar luego), pero no como estructura global. |
| Serverless        | Dificulta transacciones de compra y genera dependencia del proveedor cloud.                             |

## Diagrama de arquitectura (estructura global)

![Monolito modular en capas — Marketplace Backend](./img/monolito-modular-en-capas.png)

Versión en Mermaid (incluye además la caché Redis del ADR-003):

```mermaid
flowchart TB
    subgraph Actores
        CL["Cliente"]
        SE["Seller"]
        AD["Administrador"]
    end

    WEB["Cliente Web<br/>Angular 18 · TypeScript<br/>(navegador)"]
    CL --> WEB
    SE --> WEB
    AD --> WEB

    WEB -- "HTTPS · JSON<br/>/api/v1/*" --> MW

    subgraph MONO["«monolito» Marketplace Backend — Node.js 20 LTS · Express (un solo proceso, un solo despliegue)"]
        MW["Middlewares transversales<br/>cors · express.json · auth JWT · validación · manejo de errores · logger"]

        subgraph P["1. Capa de presentación — recibe HTTP, autentica, valida y responde JSON"]
            U1["usuarios.routes / controller"]
            S1["sellers.routes / controller"]
            C1["catalogo.routes / controller"]
            K1["carrito.routes / controller"]
            D1["pedidos.routes / controller"]
        end

        subgraph L["2. Capa de lógica de negocio — reglas y coordinación entre módulos"]
            U2["usuarios.service<br/>registro, login, roles"]
            S2["sellers.service<br/>alta de tiendas, validación"]
            C2["catalogo.service<br/>productos, categorías, stock"]
            K2["carrito.service<br/>ítems, totales"]
            D2["pedidos.service<br/>checkout, estados, pago/envío"]
        end

        subgraph DAT["3. Capa de datos — persistencia y consultas"]
            U3["usuarios.repository"]
            S3["sellers.repository"]
            C3["catalogo.repository"]
            K3["carrito.repository"]
            D3["pedidos.repository"]
            ORM["Acceso a datos compartido<br/>Sequelize ORM · modelos · pool de conexiones"]
        end

        MW --> U1 & S1 & C1 & K1 & D1
        U1 --> U2 --> U3
        S1 --> S2 --> S3
        C1 --> C2 --> C3
        K1 --> K2 --> K3
        D1 --> D2 --> D3
        U3 & S3 & C3 & K3 & D3 --> ORM

        S2 -.-> U2
        K2 -.-> C2
        D2 -.-> K2
        D2 -.-> C2
    end

    CACHE[("Redis<br/>caché de catálogo")]
    DB[("PostgreSQL<br/>marketplace_db")]
    PAY["«sistema externo»<br/>Pasarela de pagos<br/>Niubiz / Culqi"]
    SHIP["«sistema externo»<br/>Servicio de envíos<br/>API del courier"]

    C2 -. "ADR-003" .-> CACHE
    ORM -- "SQL · TCP 5432" --> DB
    D2 -- "HTTPS / REST" --> PAY
    D2 -- "HTTPS / REST" --> SHIP
```

**Leyenda**

- Flecha continua: llamada síncrona entre capas (de arriba hacia abajo).
- Flecha punteada entre services: uso entre módulos (solo a través de su _service_).
- Cilindros: almacenamiento. Cajas «sistema externo»: fuera del monolito.

## Estructura interna del cliente web (Angular)

El nodo **Cliente Web** del diagrama anterior se organiza internamente con
Clean Architecture: Presentación, Aplicación (casos de uso), Dominio e
Infraestructura (adaptadores), con `app.config.ts` como raíz de composición.
Los adaptadores HTTP consumen la **Marketplace API REST** del monolito.

![Estructura interna del cliente web Angular — Clean Architecture](./img/estructura-cliente-web-angular.png)

## Reglas de la arquitectura

1. Cada capa solo invoca a la capa inmediatamente inferior.
2. Un módulo no accede al _repository_ ni a las tablas de otro módulo.
3. La comunicación entre módulos se hace llamando a su _service_.
4. Todo se ejecuta en un único proceso Node.js con una única base de datos.
5. Los sistemas externos (pagos, envíos) se consumen siempre mediante adaptadores (ADR-004).

## Despliegue

```mermaid
flowchart LR
    B["Navegador"] -- HTTPS --> LB["Balanceador de carga"]
    LB --> I1["Instancia 1<br/>monolito (Docker)"]
    LB --> I2["Instancia 2<br/>monolito (Docker)"]
    LB -.-> IN["Instancia N<br/>(campañas)"]
    I1 & I2 & IN --> R[("Redis")]
    I1 & I2 & IN --> PG[("PostgreSQL")]
```

La aplicación es **una sola imagen** que se replica; escalar en campaña es
levantar más instancias, sin cambiar código (DA01).
