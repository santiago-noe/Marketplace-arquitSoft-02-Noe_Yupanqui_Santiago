# 03 - Requisitos Funcionales

## Objetivo

Identificar y documentar las funcionalidades que el sistema debe proporcionar a los diferentes actores, tomando como base las historias de usuario identificadas previamente.

## Requisitos Funcionales

| ID       | Requisito Funcional                                                                        |
| -------- | ------------------------------------------------------------------------------------------ |
| **RF01** | El sistema debe permitir buscar productos mediante criterios de búsqueda.                  |
| **RF02** | El sistema debe permitir consultar la información y disponibilidad de los productos.       |
| **RF03** | El sistema debe permitir registrar, actualizar y gestionar productos en la plataforma.     |
| **RF04** | El sistema debe permitir agregar, modificar y eliminar productos del carrito de compra.    |
| **RF05** | El sistema debe permitir generar un pedido a partir de los productos del carrito.          |
| **RF06** | El sistema debe permitir consultar los pedidos realizados y su estado.                     |
| **RF07** | El sistema debe permitir registrar, actualizar y desactivar sellers de la plataforma.      |
| **RF08** | El sistema debe permitir consultar el detalle de un pedido realizado.                      |
| **RF09** | El sistema debe permitir procesar el pago de un pedido mediante una pasarela de pago.      |
| **RF10** | El sistema debe permitir consultar la información relacionada con la entrega de un pedido. |
| **RF11** | El sistema debe permitir generar y consultar el comprobante de pago de una compra.         |
| **RF12** | El sistema debe permitir consultar la disponibilidad y stock de los productos.             |
| **RF13** | El sistema debe permitir gestionar los usuarios registrados en la plataforma.              |
| **RF14** | El sistema debe permitir supervisar y gestionar los productos registrados por los sellers. |

## Relación entre Historias de Usuario y Requisitos Funcionales

| Historia de Usuario                     | Requisitos Funcionales Relacionados |
| --------------------------------------- | ----------------------------------- |
| **HU01 - Buscar y consultar productos** | RF01, RF02                          |
| **HU02 - Gestionar productos**          | RF03, RF12                          |
| **HU03 - Gestionar carrito**            | RF04                                |
| **HU04 - Realizar pedido**              | RF05, RF08, RF09, RF11              |
| **HU05 - Gestionar sellers**            | RF07                                |
| **HU06 - Consultar pedidos**            | RF06, RF08                          |
| **HU07 - Realizar pago**                | RF09, RF11                          |
| **HU08 - Consultar entrega**            | RF10                                |
| **HU09 - Obtener comprobante**          | RF11                                |
| **HU10 - Consultar stock**              | RF12                                |
| **HU11 - Gestionar usuarios**           | RF13                                |
| **HU12 - Supervisar productos**         | RF14                                |

## Trazabilidad

La relación entre historias de usuario y requisitos funcionales permite establecer una trazabilidad entre las necesidades de los usuarios y las funcionalidades que deberá implementar el sistema.

Cada requisito funcional se deriva de una o más historias de usuario, permitiendo verificar que las funcionalidades definidas respondan a las necesidades identificadas durante el análisis del sistema.
