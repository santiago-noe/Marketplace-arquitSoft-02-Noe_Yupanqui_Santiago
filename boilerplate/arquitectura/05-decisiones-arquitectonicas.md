# 5. Decisiones arquitectónicas (ADR)

> **Pregunta guía:** ¿Cómo responderemos a los drivers?

**ADR (Architecture Decision Record)** — Registro de Decisión Arquitectónica:
documenta cada decisión importante junto con su contexto, las alternativas
evaluadas y sus consecuencias.

## Resumen

| ID | Decisión arquitectónica | Driver relacionado | Justificación | Resultado |
|----|-------------------------|--------------------|---------------|-----------|
| ADR-001 | Monolito modular | DA01 – Escalabilidad; DA06 – Mantenibilidad | Organizar las funcionalidades en módulos independientes dentro de una misma aplicación desplegable. | Módulos de Catálogo, Carrito, Pedidos, Pagos y Usuarios. |
| ADR-002 | Clean Architecture | DA06 – Mantenibilidad | Separar las reglas del negocio de los detalles tecnológicos. | Dominio, Aplicación, Infraestructura y Presentación. |
| ADR-003 | Estrategia de caché | DA02 – Rendimiento | Reducir consultas repetitivas a la fuente de datos. | Caché para información de consulta frecuente. |
| ADR-004 | Integración de pagos mediante interfaces y adaptadores | DA04 – Integración con pagos | Desacoplar los casos de uso del proveedor de pagos. | Contrato de pagos y adaptador para la pasarela externa. |

---

## ADR-001 · Monolito modular

- **Estado:** Aceptada
- **Drivers:** DA01, DA06, DA05

**Contexto.** El equipo es pequeño, el dominio todavía está en evolución y se
necesita salir rápido al mercado. A la vez, se espera crecimiento y picos de
tráfico en campañas.

**Alternativas evaluadas.**

| Alternativa | Ventaja | Por qué se descarta (por ahora) |
|-------------|---------|---------------------------------|
| Monolito tradicional sin módulos | Simple | Todo se acopla; contradice DA06. |
| Microservicios | Escalado independiente | Complejidad operativa (red, despliegues, datos distribuidos) desproporcionada para el tamaño actual. |
| **Monolito modular** | Un solo despliegue, límites claros entre módulos | — |

**Decisión.** Un único backend desplegable (Node.js + Express) organizado en
módulos `usuarios`, `sellers`, `catalogo`, `carrito`, `pedidos` (y `pagos`).
Un módulo solo se comunica con otro a través de su *service*, nunca accediendo
a sus tablas. El frontend Angular es una aplicación separada que consume la API
REST (DA05).

**Consecuencias.**
- (+) Despliegue y depuración simples; transacciones locales.
- (+) Escala horizontalmente replicando el monolito detrás de un balanceador.
- (+) Los límites de módulo permiten extraer un microservicio más adelante.
- (−) Todo el backend escala junto; requiere disciplina para no romper límites.

---

## ADR-002 · Clean Architecture

- **Estado:** Aceptada
- **Drivers:** DA06 (principal), DA04, AC06

**Contexto.** Las reglas del negocio (stock, comisión 10 %, IGV 18 %, estados
del pedido) no deben depender de Angular, HttpClient, la base de datos ni del
proveedor de pagos, porque esas tecnologías cambian con más frecuencia que el
negocio.

**Alternativas evaluadas.** Capas tradicionales (la lógica termina dependiendo
de la capa de datos), MVC (no define dónde viven las reglas de negocio),
Hexagonal / Onion (equivalentes en esencia; se elige Clean Architecture por su
vocabulario de casos de uso, más cercano a las historias de usuario).

**Decisión.** Cada módulo se organiza en cuatro capas con la **regla de
dependencia**: las importaciones solo apuntan hacia el dominio.

| Carpeta | Capa | Puede importar |
|---------|------|----------------|
| `dominio/` | Entidades, reglas y contratos (puertos) | Nada externo |
| `aplicacion/` | Casos de uso | Solo `dominio/` |
| `infraestructura/` | Adaptadores que implementan contratos | `dominio/` + frameworks |
| `presentacion/` | Componentes Angular | `aplicacion/`, `dominio/` + Angular |

Los contratos viven en el dominio y una **raíz de composición**
(`app.config.ts`) es el único lugar que conoce las implementaciones concretas.

**Consecuencias.**
- (+) El dominio se prueba sin framework (`npm run pruebas`: 16 pruebas en ms).
- (+) Cambiar de tecnología = cambiar un adaptador y una línea de `app.config.ts`.
- (−) Más archivos e indirección (interfaces, tokens, factorías).
- (−) Requiere que el equipo respete la regla de dependencia (se verifica con `tsconfig.pruebas.json`).

---

## ADR-003 · Estrategia de caché

- **Estado:** Propuesta
- **Drivers:** DA02, DA01

**Contexto.** El catálogo es la operación más frecuente y cambia poco;
consultar la base de datos en cada visita no escala en campañas.

**Alternativas.** Sin caché (simple, no soporta picos); caché solo en el
navegador (no reduce carga del backend); **caché distribuida (Redis) en el
backend + caché HTTP corta en el frontend**.

**Decisión.** Cachear en Redis las consultas de catálogo y categorías con TTL
corto (p. ej. 60 s) e invalidar al cambiar precio o stock. **No** se cachean
el carrito, el pago ni el pedido. En Clean Architecture la caché es un
**adaptador** (decorador del `RepositorioProductos`), por lo que el dominio y
los casos de uso no se enteran.

**Consecuencias.**
- (+) Menor latencia y menos carga en PostgreSQL.
- (−) Posible dato desactualizado durante el TTL; el stock real se vuelve a
  validar al registrar la compra (`RegistrarCompraCasoUso`).

---

## ADR-004 · Integración de pagos mediante interfaces y adaptadores

- **Estado:** Aceptada
- **Drivers:** DA04, DA03, DA06

**Contexto.** El marketplace debe cobrar mediante una pasarela externa
(Niubiz, Culqi, Stripe…) cuyo API, vocabulario y credenciales pueden cambiar.

**Alternativas.** Llamar al SDK del proveedor desde el caso de uso (acopla el
negocio al proveedor); **definir un contrato propio y un adaptador por
proveedor**.

**Decisión.** El dominio define el contrato `ProcesadorPagos` con lenguaje del
negocio (`cobrar(monto, medioPago)` → `ResultadoCobro`). Cada proveedor tiene su
adaptador (`ProcesadorPagosSimulado`, `ProcesadorPagosNiubiz`) que **traduce**
el vocabulario del proveedor al nuestro. El stock solo se descuenta después de
un cobro aprobado. Lo mismo se aplica a notificaciones (`NotificadorCliente`)
y, a futuro, al servicio de envío.

**Consecuencias.**
- (+) Cambiar de proveedor no toca el dominio ni el caso de uso.
- (+) Se puede probar la compra completa con el adaptador simulado.
- (+) Los datos de tarjeta no se persisten: solo pasan al adaptador (DA03).
- (−) Hay que mantener un adaptador por proveedor.
