# 2. Requisitos

> **Pregunta guía:** ¿Qué debe hacer el sistema y qué limitaciones existen?

## 2.1 Historias de usuario principales

| ID | Como… | Quiero… | Para… |
|----|-------|---------|-------|
| HU01 | Cliente | buscar productos por mascota y categoría | encontrar rápido lo que necesito |
| HU02 | Cliente | agregar productos al carrito | comprar varios productos a la vez |
| HU03 | Cliente | pagar con tarjeta | confirmar mi compra en línea |
| HU04 | Cliente | ver el estado de mi pedido | saber cuándo llegará |
| HU05 | Seller | publicar productos con precio y stock | vender en el marketplace |
| HU06 | Administrador | aprobar sellers y definir categorías | mantener la calidad del catálogo |

## 2.2 Requisitos funcionales (RF)

| ID | Requisito | Actor | Estado en el boilerplate |
|----|-----------|-------|--------------------------|
| RF01 | Registrar usuarios (cliente, seller, administrador). | Todos | Pendiente |
| RF02 | Buscar y filtrar productos (por mascota y categoría). | Cliente | `ConsultarCatalogoCasoUso` |
| RF03 | Agregar productos al carrito validando stock. | Cliente | `AgregarAlCarritoCasoUso` |
| RF04 | Confirmar la compra y generar el pedido. | Cliente | `RegistrarCompraCasoUso` |
| RF05 | Procesar el pago mediante pasarela externa. | Sistema | Contrato `ProcesadorPagos` |
| RF06 | Notificar al cliente la confirmación del pedido. | Sistema | Contrato `NotificadorCliente` |
| RF07 | Hacer seguimiento del pedido (pagado → despachado → entregado). | Cliente | Entidad `Pedido` (estados) |
| RF08 | Publicar y administrar productos. | Seller | Pendiente |
| RF09 | Gestionar sellers, categorías y comisiones. | Administrador | Pendiente |

## 2.3 Requisitos no funcionales (RNF)

| ID | Requisito | Atributo |
|----|-----------|----------|
| RNF01 | El catálogo debe responder en menos de 2 s con carga normal. | Rendimiento |
| RNF02 | Los datos de pago no se almacenan en el sistema; viajan solo a la pasarela por HTTPS. | Seguridad |
| RNF03 | El sistema debe estar disponible 99 % del tiempo, en especial en campañas (Cyber, Navidad). | Disponibilidad |
| RNF04 | Debe soportar picos de usuarios en campañas sin rediseñar el sistema. | Escalabilidad |
| RNF05 | Cambiar de proveedor de pagos, envíos o base de datos no debe obligar a modificar las reglas del negocio. | Mantenibilidad |
| RNF06 | Las reglas de negocio deben poder probarse sin framework, navegador ni servidor. | Testabilidad |
| RNF07 | Registrar logs y métricas de pedidos y pagos. | Observabilidad |

## 2.4 Restricciones (R)

| ID | Restricción |
|----|-------------|
| R01 | Base de datos PostgreSQL. |
| R02 | Integración con el ERP de los sellers. |
| R03 | Comunicación frontend–backend mediante API REST (JSON sobre HTTPS). |
| R04 | Backend en Node.js; frontend en Angular 18 + TypeScript. |
| R05 | Versionamiento en Git / GitHub. |
| R06 | Integración con pasarela de pago (Niubiz / Culqi) y servicio de envío. |
| R07 | Reglas fiscales peruanas: IGV 18 %. |

> **Nota:** no todo requisito se convierte en driver arquitectónico. Solo los
> que condicionan la estructura del sistema pasan a la etapa 4.
