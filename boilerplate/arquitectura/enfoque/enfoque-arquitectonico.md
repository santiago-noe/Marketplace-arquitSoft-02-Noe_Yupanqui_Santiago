# 7. Enfoque arquitectónico: Clean Architecture

> **Pregunta guía:** ¿Qué estructura arquitectónica aplicaremos para organizar
> responsabilidades y dependencias internas?

## Ficha del enfoque

| Elemento | Descripción aplicada al Marketplace |
|----------|-------------------------------------|
| Patrón / enfoque arquitectónico | Clean Architecture (Arquitectura Limpia). |
| Objetivo | Separar responsabilidades y controlar las dependencias hacia el dominio. |
| ¿Qué problema resuelve? | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas, como bases de datos, API y servicios de pago. |
| Capas definidas | Presentación, Aplicación, Dominio e Infraestructura. |
| Beneficios | • Facilita el mantenimiento y las pruebas unitarias.<br>• Permite cambiar implementaciones técnicas sin modificar innecesariamente las reglas del negocio.<br>• Mejora la organización y separación de responsabilidades del código. |
| Driver / ADR | DA06 – Mantenibilidad · ADR-002 (y ADR-004 para pagos). |

## Regla de dependencia

> Las dependencias del código **siempre apuntan hacia el centro** (dominio).
> El dominio no sabe que existen Angular, HttpClient, la API ni la pasarela.

```mermaid
%%{init: {"theme": "base", "themeVariables": {"fontFamily": "Segoe UI, Arial", "fontSize": "14px", "lineColor": "#64748b"}}}%%
flowchart LR
    PRE["🖥️ <b>Presentación</b><br/><small>componentes Angular</small>"]
    APP["⚙️ <b>Aplicación</b><br/><small>casos de uso</small>"]
    DOM(["🧠 <b>DOMINIO</b><br/><small>entidades · reglas · contratos</small>"])
    INF["🔌 <b>Infraestructura</b><br/><small>adaptadores</small>"]
    CFG{{"🧩 <b>app.config.ts</b><br/><small>raíz de composición</small>"}}

    PRE --> APP
    PRE --> DOM
    APP --> DOM
    INF -. "implementa contratos" .-> DOM
    CFG --> INF
    CFG --> APP

    classDef pres fill:#dbeafe,stroke:#2563eb,stroke-width:2px,color:#1e3a8a
    classDef apl  fill:#dcfce7,stroke:#16a34a,stroke-width:2px,color:#14532d
    classDef dom  fill:#fef3c7,stroke:#d97706,stroke-width:3px,color:#78350f
    classDef inf  fill:#ede9fe,stroke:#7c3aed,stroke-width:2px,color:#4c1d95
    classDef cfg  fill:#f1f5f9,stroke:#334155,stroke-width:2px,color:#0f172a
    class PRE pres
    class APP apl
    class DOM dom
    class INF inf
    class CFG cfg
```

## Responsabilidades por capa (mapeadas al código)

| Carpeta | Capa | Contiene | Archivos en el proyecto | Puede importar |
|---------|------|----------|-------------------------|----------------|
| `src/app/dominio/modelos/` | Dominio – entidades | Entidades y reglas de negocio | `producto.modelo.ts` (stock, categoría), `carrito.modelo.ts` (inmutable, totales), `pedido.modelo.ts` (estados, cancelación), `precios.ts` (comisión 10 %, IGV 18 %) | Nada externo |
| `src/app/dominio/contratos/` | Dominio – puertos | Interfaces que el negocio necesita | `RepositorioProductos`, `RepositorioPedidos`, `ProcesadorPagos`, `NotificadorCliente` | Solo modelos del dominio |
| `src/app/aplicacion/` | Aplicación | Casos de uso: orquestan el flujo | `ConsultarCatalogoCasoUso`, `AgregarAlCarritoCasoUso`, `RegistrarCompraCasoUso` | Solo `dominio/` |
| `src/app/infraestructura/` | Infraestructura | Adaptadores que implementan contratos | `RepositorioProductosMemoria` / `Http`, `RepositorioPedidosMemoria`, `ProcesadorPagosSimulado` / `Niubiz`, `NotificadorConsola` / `WhatsApp`, `tokens.ts` | `dominio/` + Angular, HttpClient, RxJS |
| `src/app/presentacion/` | Presentación | Pantallas y estado de la UI | `CatalogoComponent`, `CarritoComponent`, `EstadoCarrito`, `AppComponent` | `aplicacion/`, `dominio/` + Angular |
| `src/app/app.config.ts` | Raíz de composición | Elige qué adaptador cumple cada contrato | Único archivo que nombra implementaciones concretas | Todas las capas |

## Diagrama del enfoque (aplicación Angular)

```mermaid
%%{init: {"theme": "base", "flowchart": {"curve": "basis", "nodeSpacing": 30, "rankSpacing": 55, "padding": 12}, "themeVariables": {"fontFamily": "Segoe UI, Arial", "fontSize": "13px", "lineColor": "#64748b", "clusterBkg": "#ffffff", "clusterBorder": "#94a3b8"}}}%%
flowchart LR
    USR(["👤 <b>Usuario</b><br/><small>Cliente · navegador</small>"])

    subgraph WEB["🌐 «aplicación» Marketplace Web — Angular 18 · TypeScript · src/app/"]
        direction LR

        subgraph PRES["🖥️ PRESENTACIÓN — src/app/presentacion/"]
            direction TB
            AC["«componente»<br/><b>AppComponent</b><br/><small>shell de la aplicación</small>"]
            CC["«componente»<br/><b>CatalogoComponent</b><br/><small>lista y filtra productos</small>"]
            KC["«componente»<br/><b>CarritoComponent</b><br/><small>resume y confirma compra</small>"]
            EC["«servicio de estado»<br/><b>EstadoCarrito</b><br/><small>signals · sin reglas</small>"]
        end

        subgraph NUCLEO["⭕ NÚCLEO — TypeScript puro, sin Angular / HttpClient / RxJS"]
            direction TB
            subgraph APL["⚙️ APLICACIÓN — casos de uso · src/app/aplicacion/"]
                direction LR
                UC1["<b>ConsultarCatalogoCasoUso</b><br/><small>ejecutar()</small>"]
                UC2["<b>AgregarAlCarritoCasoUso</b><br/><small>ejecutar()</small>"]
                UC3["<b>RegistrarCompraCasoUso</b><br/><small>ejecutar()</small>"]
            end
            subgraph DOM["🧠 DOMINIO — src/app/dominio/"]
                direction LR
                subgraph MOD["📦 Modelos · entidades y reglas"]
                    direction TB
                    E1["«entidad»<br/><b>Producto</b><br/><small>stock · categoría · precio</small>"]
                    E2["«entidad»<br/><b>Carrito</b><br/><small>inmutable · subtotal · total</small>"]
                    E3["«entidad»<br/><b>Pedido</b><br/><small>estados · cancelación</small>"]
                    E4["«reglas»<br/><b>precios.ts</b><br/><small>comisión 10 % · IGV 18 %</small>"]
                end
                subgraph CON["📜 Contratos · puertos"]
                    direction TB
                    I1["«interface»<br/><i>RepositorioProductos</i>"]
                    I2["«interface»<br/><i>RepositorioPedidos</i>"]
                    I3["«interface»<br/><i>ProcesadorPagos</i>"]
                    I4["«interface»<br/><i>NotificadorCliente</i>"]
                end
            end
        end

        subgraph INFRA["🔌 INFRAESTRUCTURA — src/app/infraestructura/"]
            direction TB
            A1["«adaptador»<br/><b>RepositorioProductosMemoria</b><br/><b>RepositorioProductosHttp</b>"]
            A2["«adaptador»<br/><b>RepositorioPedidosMemoria</b>"]
            A3["«adaptador»<br/><b>ProcesadorPagosSimulado</b><br/><b>ProcesadorPagosNiubiz</b>"]
            A4["«adaptador»<br/><b>NotificadorConsola</b><br/><b>NotificadorWhatsApp</b>"]
            TK["«Angular DI»<br/><b>tokens.ts</b><br/><small>InjectionToken por contrato</small>"]
        end

        CFG{{"🧩 «raíz de composición» <b>app.config.ts</b><br/><small>único archivo que elige qué adaptador cumple cada contrato<br/>useFactory + InjectionToken → inyecta en los casos de uso</small>"}}
    end

    API[("☁️ «sistema externo»<br/><b>Marketplace API REST</b><br/><small>Node.js · monolito modular<br/>/api/productos · /api/pedidos<br/>/api/authorization · /api/mensajes</small>")]

    %% ── Tiempo de ejecución (flechas continuas) ──
    USR == navega ==> AC
    CC -- invoca --> UC1
    CC -- invoca --> UC2
    KC -- invoca --> UC3
    CC --> EC
    KC --> EC
    A1 -- "HTTP · JSON" --> API
    A3 -- "HTTP · JSON" --> API
    A4 -- "HTTP · JSON" --> API

    %% ── Dependencias de código (import): siempre hacia el centro ──
    EC -.-> E2
    UC1 -.-> I1
    UC2 -.-> I1
    UC2 -.-> E2
    UC3 -.-> I1 & I2 & I3 & I4
    UC3 -.-> E3

    %% ── Inversión de dependencias ──
    A1 -. implementa .-> I1
    A2 -. implementa .-> I2
    A3 -. implementa .-> I3
    A4 -. implementa .-> I4
    TK -.-> CON

    %% ── Composición ──
    CFG -. registra .-> TK
    CFG -. registra .-> APL

    %% ── Estilos por capa ──
    classDef actor fill:#0f172a,stroke:#0f172a,color:#ffffff
    classDef pres  fill:#eff6ff,stroke:#3b82f6,stroke-width:1.5px,color:#1e3a8a
    classDef apl   fill:#f0fdf4,stroke:#22c55e,stroke-width:1.5px,color:#14532d
    classDef ent   fill:#fffbeb,stroke:#f59e0b,stroke-width:1.5px,color:#78350f
    classDef port  fill:#fff7ed,stroke:#ea580c,stroke-width:1.5px,stroke-dasharray:4 3,color:#7c2d12
    classDef inf   fill:#f5f3ff,stroke:#8b5cf6,stroke-width:1.5px,color:#4c1d95
    classDef cfg   fill:#f8fafc,stroke:#334155,stroke-width:2px,color:#0f172a
    classDef ext   fill:#f1f5f9,stroke:#475569,stroke-width:2px,color:#0f172a

    class USR actor
    class AC,CC,KC,EC pres
    class UC1,UC2,UC3 apl
    class E1,E2,E3,E4 ent
    class I1,I2,I3,I4 port
    class A1,A2,A3,A4,TK inf
    class CFG cfg
    class API ext

    style WEB    fill:#fafafa,stroke:#1f2937,stroke-width:2px,stroke-dasharray:6 4
    style PRES   fill:#dbeafe,stroke:#2563eb,stroke-width:2px
    style NUCLEO fill:#ecfdf5,stroke:#059669,stroke-width:2px
    style APL    fill:#dcfce7,stroke:#16a34a,stroke-width:2px
    style DOM    fill:#fef3c7,stroke:#d97706,stroke-width:3px
    style MOD    fill:#fefce8,stroke:#ca8a04,stroke-width:1px
    style CON    fill:#ffedd5,stroke:#ea580c,stroke-width:1px
    style INFRA  fill:#ede9fe,stroke:#7c3aed,stroke-width:2px
```

| Color | Capa | Rol |
|:-----:|------|-----|
| 🟦 | **Presentación** | Componentes Angular y estado de la UI |
| 🟩 | **Aplicación** | Casos de uso que orquestan el flujo |
| 🟨 | **Dominio** | Entidades, reglas y contratos (centro del sistema) |
| 🟪 | **Infraestructura** | Adaptadores intercambiables que implementan los contratos |
| ⬜ | **Raíz de composición / externo** | `app.config.ts` y la API REST del backend |

> 📊 Versión interactiva (Archify): [enfoque-arquitectonico.html](./enfoque-arquitectonico.html) · fuente [enfoque-arquitectonico.architecture.json](./enfoque-arquitectonico.architecture.json)

**Leyenda**

- Flecha continua: llamada en tiempo de ejecución.
- Flecha punteada: dependencia de código (`import`); siempre apunta hacia el centro.
- «implementa»: el adaptador cumple el contrato definido en el dominio (**inversión de dependencias**, la "D" de SOLID).
- Anillos: **Dominio ⊂ Aplicación ⊂ Adaptadores y frameworks**.

## Flujo de un caso de uso: Registrar una compra

```mermaid
sequenceDiagram
    actor Cliente
    participant UI as CarritoComponent<br/>(Presentación)
    participant UC as RegistrarCompraCasoUso<br/>(Aplicación)
    participant D as Carrito / Producto / Pedido<br/>(Dominio)
    participant PP as ProcesadorPagos<br/>(contrato)
    participant RP as RepositorioPedidos<br/>(contrato)
    participant NC as NotificadorCliente<br/>(contrato)

    Cliente->>UI: Confirmar compra
    UI->>UC: ejecutar(clienteId, carrito, medioPago)
    UC->>D: carrito.estaVacio() / producto.hayStockPara()
    UC->>D: carrito.calcularTotal()  (comisión + IGV)
    UC->>PP: cobrar(total, medioPago)
    PP-->>UC: ResultadoCobro
    alt pago rechazado
        UC-->>UI: Error (no se descuenta stock ni se crea pedido)
    else pago aprobado
        UC->>D: producto.descontar() · Pedido.crear()
        UC->>RP: guardar(pedido)
        UC->>NC: confirmarPedido()
        UC-->>UI: Pedido
    end
```

- El **caso de uso** decide el orden de los pasos.
- Las **entidades** deciden qué es válido.
- Los **contratos** ocultan con qué tecnología se hace cada cosa.

## Auditoría de dependencias del proyecto

Revisión de las líneas `import` del boilerplate:

| Capa | ¿Importa Angular / HttpClient / RxJS? | Resultado |
|------|---------------------------------------|-----------|
| `dominio/` | No. Solo se importa a sí mismo. | ✅ Cumple |
| `aplicacion/` | No. Solo importa `dominio/`. | ✅ Cumple |
| `infraestructura/` | Sí (`HttpClient`, `InjectionToken`, `firstValueFrom`). Importa contratos del dominio. | ✅ Correcto: es la capa externa |
| `presentacion/` | Sí (Angular). Importa casos de uso y modelos del dominio. | ✅ Cumple (apunta hacia adentro) |
| `app.config.ts` | Conoce todas las capas. | ✅ Es la raíz de composición |

**Verificación mecánica:** `tsconfig.pruebas.json` compila solo `dominio/`,
`aplicacion/` y los adaptadores en memoria. Si alguna de esas capas importara
Angular, la compilación fallaría. Resultado de `npm run pruebas`:
**16 pruebas OK en ~11 ms, sin levantar Angular.**

## Cómo cambiar una tecnología sin tocar el negocio

| Cambio | Qué se modifica | Qué **no** se modifica |
|--------|-----------------|------------------------|
| Catálogo en memoria → API REST | `app.config.ts`: usar `RepositorioProductosHttp` | Dominio, casos de uso, componentes |
| Pago simulado → Niubiz | `app.config.ts`: usar `ProcesadorPagosNiubiz` | Dominio, `RegistrarCompraCasoUso` |
| Consola → WhatsApp | `app.config.ts`: usar `NotificadorWhatsApp` | Dominio, casos de uso |
| Agregar caché (ADR-003) | Nuevo adaptador decorador de `RepositorioProductos` | Dominio, casos de uso |

## Aplicación al backend (misma idea, otro lado del monolito)

Cada módulo del monolito Node.js puede aplicar las mismas capas:

```
src/modules/pedidos/
├── dominio/           Pedido, reglas de estado, contratos (PedidoRepository, PasarelaPago)
├── aplicacion/        CrearPedido, ConfirmarCompra, CancelarPedido
├── adaptadores/       pedidos.controller.js, pedidos.routes.js, PedidoDTO
└── infraestructura/   PostgresPedidoRepository, NiubizPaymentGateway, ShippingAdapter
```
