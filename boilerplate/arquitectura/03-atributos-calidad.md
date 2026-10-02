# 3. Atributos de calidad

> **Pregunta guía:** ¿Qué características importantes debe tener el sistema?

## Atributos priorizados

| ID | Atributo | Descripción en el marketplace | Prioridad |
|----|----------|-------------------------------|-----------|
| AC01 | Rendimiento | Respuesta rápida en consultas de catálogo y en la compra. | Alta |
| AC02 | Seguridad | Protección de datos personales y de pago; acceso según rol. | Alta |
| AC03 | Disponibilidad | Sistema disponible, sobre todo en campañas. | Alta |
| AC04 | Escalabilidad | Soportar más usuarios y sellers sin rediseño. | Media |
| AC05 | Mantenibilidad | Modificar o reemplazar una parte sin afectar a las demás. | Alta |
| AC06 | Testabilidad | Verificar reglas del negocio de forma aislada y rápida. | Media |
| AC07 | Observabilidad | Logs, métricas y trazas de pedidos y pagos. | Media |
| AC08 | Usabilidad | Fácil de usar para clientes y sellers. | Media |

## Escenarios de calidad

| Atributo | Estímulo | Respuesta esperada | Medida |
|----------|----------|--------------------|--------|
| Rendimiento | 500 usuarios consultan el catálogo a la vez | Se responde desde caché | < 2 s por consulta |
| Seguridad | Un cliente intenta acceder al pedido de otro | Acceso denegado | 0 accesos indebidos |
| Disponibilidad | Cae la pasarela de pago | El pedido no se registra, el stock no se descuenta y se informa al cliente | 0 pedidos inconsistentes |
| Escalabilidad | Campaña Cyber triplica el tráfico | Se agregan instancias del monolito | Sin cambios de código |
| Mantenibilidad | Se cambia Niubiz por Culqi | Se escribe un adaptador nuevo y se cambia `app.config.ts` | 0 líneas cambiadas en dominio y casos de uso |
| Testabilidad | Se modifica la regla de comisión | Las pruebas del dominio detectan el cambio | Ejecución en milisegundos, sin Angular |

> Los escenarios de **mantenibilidad** y **disponibilidad** ya se cumplen en el
> boilerplate: ver `src/app/app.config.ts` y la prueba
> *"si el pago es rechazado no se registra el pedido"*.
