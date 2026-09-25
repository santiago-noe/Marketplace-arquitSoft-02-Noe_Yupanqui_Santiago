# Restricciones

## Objetivo

Identificar las restricciones que condicionan las decisiones de diseño, desarrollo y arquitectura del sistema.

Las restricciones establecen condiciones técnicas, tecnológicas y de integración que deben ser consideradas durante la construcción del marketplace.

## Restricciones identificadas

| ID       | Restricción                 | Descripción                                                                                                                                                          |
| -------- | --------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **RC01** | **Aplicación web**          | El sistema debe desarrollarse como una aplicación accesible mediante un navegador web.                                                                               |
| **RC02** | **Control de versiones**    | El código fuente debe gestionarse utilizando Git y mantenerse en un repositorio compartido.                                                                          |
| **RC03** | **API REST**                | La comunicación entre el frontend y los servicios del sistema debe realizarse mediante una API REST.                                                                 |
| **RC04** | **Pasarela de pago**        | El sistema debe integrarse con una pasarela de pago externa para procesar las operaciones de pago.                                                                   |
| **RC05** | **Servicio de envío**       | El sistema debe integrarse con un servicio externo de envío para gestionar la información relacionada con la entrega de pedidos.                                     |
| **RC06** | **Base de datos**           | El sistema debe utilizar un sistema gestor de base de datos para almacenar y gestionar la información de usuarios, productos, pedidos y demás datos del marketplace. |
| **RC07** | **Seguridad de acceso**     | El sistema debe implementar mecanismos de autenticación y autorización para controlar el acceso a las funcionalidades según el tipo de usuario.                      |
| **RC08** | **Integración con ERP**     | El sistema debe integrarse con un ERP externo para obtener información relacionada con productos y stock disponible.                                                 |
| **RC09** | **Servicio de facturación** | El sistema debe integrarse con un servicio externo de facturación para generar los comprobantes de pago correspondientes a las compras.                              |
| **RC10** | **Compatibilidad web**      | La aplicación debe ser compatible con los principales navegadores web utilizados por los usuarios.                                                                   |
| **RC11** | **Arquitectura modular**    | La solución debe organizarse de manera modular para facilitar el mantenimiento, evolución e integración de nuevos servicios.                                         |
| **RC12** | **Protección de datos**     | La información sensible de los usuarios y las operaciones realizadas en la plataforma debe transmitirse y almacenarse utilizando mecanismos adecuados de protección. |

## Descripción de las Restricciones

### RC01 - Aplicación web

El marketplace debe desarrollarse como una aplicación web, de manera que los usuarios puedan acceder al sistema mediante un navegador sin necesidad de instalar una aplicación de escritorio.

### RC02 - Control de versiones

El código fuente debe gestionarse mediante Git y mantenerse en un repositorio compartido, permitiendo controlar los cambios realizados por los integrantes del equipo.

### RC03 - API REST

La comunicación entre el frontend y los servicios del sistema debe realizarse mediante una API REST, permitiendo separar la interfaz de usuario de la lógica y los servicios del backend.

### RC04 - Pasarela de pago

El sistema debe utilizar una pasarela de pago externa para procesar las transacciones realizadas por los clientes.

### RC05 - Servicio de envío

El sistema debe integrarse con un servicio externo que permita gestionar la información relacionada con el envío y entrega de los pedidos.

### RC06 - Base de datos

La información necesaria para el funcionamiento del marketplace debe almacenarse en una base de datos, incluyendo usuarios, productos, stock, carritos, pedidos y demás información relacionada.

### RC07 - Seguridad de acceso

El sistema debe controlar el acceso a sus funcionalidades mediante mecanismos de autenticación y autorización, diferenciando los permisos de clientes, sellers y administradores.

### RC08 - Integración con ERP

El sistema debe considerar la integración con un ERP externo para consultar información relacionada con los productos y el stock disponible.

### RC09 - Servicio de facturación

El sistema debe integrarse con un servicio externo de facturación para generar los comprobantes correspondientes a las operaciones de compra.

### RC10 - Compatibilidad web

La aplicación debe funcionar correctamente en los principales navegadores web, garantizando una experiencia de acceso adecuada para los usuarios.

### RC11 - Arquitectura modular

La solución debe mantener una estructura modular que permita desarrollar, modificar y mantener diferentes componentes del sistema sin generar dependencias innecesarias entre ellos.

### RC12 - Protección de datos

Los datos de los usuarios y la información relacionada con las operaciones de compra deben contar con mecanismos adecuados de protección durante su transmisión y almacenamiento.

## Resumen

Las restricciones identificadas condicionan las decisiones de arquitectura y desarrollo del marketplace, especialmente en aspectos relacionados con:

- Aplicación web.
- Control de versiones.
- Comunicación mediante API REST.
- Integración con servicios externos.
- Gestión de datos.
- Seguridad y control de acceso.
- Compatibilidad con navegadores.
- Modularidad de la arquitectura.
- Protección de información.
