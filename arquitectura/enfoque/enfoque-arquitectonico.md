# 7. Patrones o enfoque arquitectónico

> **Pregunta guía:** ¿Cómo organizamos las dependencias internas del sistema?

Las dependencias internas se organizan mediante **Clean Architecture**.

| Elemento                         | Descripción aplicada al Marketplace                                                                                                              |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Patrón / enfoque arquitectónico** | Clean Architecture (Arquitectura Limpia).                                                                                                     |
| **Objetivo**                     | Separar responsabilidades y controlar las dependencias hacia el dominio.                                                                         |
| **¿Qué problema resuelve?**      | Evita el acoplamiento entre la interfaz Angular, las reglas del negocio y las tecnologías externas, como bases de datos, API y servicios de pago. |
| **Capas definidas**              | Presentación, Aplicación, Dominio e Infraestructura.                                                                                             |
| **Beneficios**                   | • Facilita el mantenimiento y las pruebas unitarias.                                                                                             |

## Capas en el cliente web (`src/app/`)

| Capa               | Carpeta                    | Contenido                                                                                                  |
| ------------------ | -------------------------- | ---------------------------------------------------------------------------------------------------------- |
| **Presentación**   | `src/app/presentacion/`    | `CatalogoComponent`, `CarritoComponent`, `AppComponent` y el servicio de estado `EstadoCarrito` (signals). |
| **Aplicación**     | `src/app/aplicacion/`      | Casos de uso: `ConsultarCatalogoCasoUso`, `AgregarAlCarritoCasoUso`, `RegistrarCompraCasoUso`.             |
| **Dominio**        | `src/app/dominio/`         | Entidades (`Producto`, `Carrito`, `Pedido`), reglas (`precios.ts`) y contratos (puertos).                  |
| **Infraestructura** | `src/app/infraestructura/` | Adaptadores que implementan los contratos (memoria, HTTP, pagos, notificaciones) y `tokens.ts`.            |

`app.config.ts` es la **raíz de composición**: el único archivo que decide qué adaptador cumple cada contrato y lo inyecta en los casos de uso.

## Regla de dependencia

1. El dominio no importa nada de las capas externas.
2. Los casos de uso solo conocen entidades y contratos.
3. Los adaptadores implementan contratos; son intercambiables.
4. Cambiar de tecnología = cambiar `app.config.ts`, no el dominio.

## Diagrama: estructura interna del cliente web

![Estructura interna del cliente web Angular con Clean Architecture](../img/estructura-cliente-web-angular.png)
