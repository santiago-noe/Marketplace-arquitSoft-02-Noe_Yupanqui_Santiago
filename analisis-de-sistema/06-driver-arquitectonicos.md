# 06 - Drivers Arquitectónicos

## Objetivo

Integrar los elementos identificados durante el análisis del sistema y determinar cuáles tienen una influencia significativa en las decisiones de arquitectura.

Los drivers arquitectónicos representan los requisitos, restricciones y atributos de calidad que condicionan las principales decisiones relacionadas con la estructura, comunicación, seguridad, escalabilidad y despliegue del sistema.

## Drivers Arquitectónicos Identificados

| ID       | Driver Arquitectónico                                                                       | Origen                                                  | ¿Por qué influye en la arquitectura?                                                                                                              |
| -------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **DA01** | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales. | **AC03 - Escalabilidad**                                | Puede influir en la estrategia de escalamiento, distribución de carga y despliegue de los componentes del sistema.                                |
| **DA02** | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia.      | **AC01 - Rendimiento**                                  | Puede influir en la comunicación entre componentes, procesamiento de solicitudes, uso de caché y almacenamiento de información.                   |
| **DA03** | El sistema debe proteger los datos de usuarios y operaciones de compra.                     | **AC04 - Seguridad**                                    | Puede influir en los mecanismos de autenticación, autorización, protección de datos y control de acceso.                                          |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante una API.               | **RC04 - Pasarela de pago**                             | Condiciona la forma de comunicación e integración con servicios externos y el manejo de las transacciones.                                        |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre frontend y backend.        | **RC03 - API REST**                                     | Limita las alternativas de comunicación entre las diferentes partes del sistema y establece una forma estándar de interacción.                    |
| **DA06** | El sistema debe integrarse con servicios externos para gestionar envíos y facturación.      | **RC05, RC09 - Servicios externos**                     | Influye en el diseño de las interfaces de integración, manejo de errores, disponibilidad y comunicación con sistemas externos.                    |
| **DA07** | El sistema debe mantener una estructura modular que facilite su evolución y mantenimiento.  | **AC05 - Mantenibilidad / RC11 - Arquitectura modular** | Influye en la separación de responsabilidades, organización de componentes y facilidad para incorporar cambios sin afectar otras funcionalidades. |
| **DA08** | El sistema debe obtener información de productos y stock desde un ERP externo.              | **RC08 - Integración con ERP**                          | Condiciona la estrategia de integración, comunicación y sincronización de información entre el marketplace y el ERP.                              |

## Descripción de los Drivers

### DA01 - Escalabilidad

El sistema debe soportar incrementos importantes de usuarios durante campañas comerciales. Este driver influye en las decisiones relacionadas con el escalamiento de los componentes, distribución de carga y estrategia de despliegue.

### DA02 - Rendimiento

El sistema debe mantener tiempos de respuesta adecuados cuando exista una alta cantidad de usuarios concurrentes. Esto puede requerir decisiones relacionadas con procesamiento, comunicación entre componentes, almacenamiento y mecanismos de optimización.

### DA03 - Seguridad

El sistema debe proteger los datos de los usuarios y las operaciones de compra. Este driver influye en la implementación de autenticación, autorización, control de acceso y protección de información.

### DA04 - Integración con pasarela de pago

El sistema debe comunicarse con una pasarela de pago externa mediante una API. Esto condiciona la arquitectura de integración y requiere mecanismos adecuados para gestionar solicitudes, respuestas, errores y transacciones.

### DA05 - API REST

La comunicación entre el frontend y backend debe realizarse mediante una API REST. Esta decisión condiciona la forma en que los componentes intercambian información y permite mantener una separación entre la interfaz y los servicios del sistema.

### DA06 - Integración con servicios externos

El sistema debe comunicarse con servicios externos relacionados con el envío y la facturación. La arquitectura debe considerar interfaces de integración, manejo de errores y disponibilidad de estos servicios.

### DA07 - Modularidad y mantenibilidad

El sistema debe contar con una estructura modular que facilite su evolución. Esto influye en la separación de responsabilidades y permite realizar cambios en determinados componentes reduciendo el impacto sobre el resto del sistema.

### DA08 - Integración con ERP

El sistema debe obtener información de productos y stock desde un ERP externo. Esto influye en la estrategia de integración y en los mecanismos utilizados para consultar y mantener actualizada la información.

## Resumen

Los principales drivers arquitectónicos identificados están relacionados con:

- **Escalabilidad** frente al incremento de usuarios.
- **Rendimiento** ante escenarios de alta concurrencia.
- **Seguridad** de los datos y operaciones.
- **Integración con servicios externos.**
- **Comunicación mediante API REST.**
- **Modularidad y mantenibilidad.**
- **Integración con sistemas empresariales como ERP.**

Estos drivers servirán como base para justificar las decisiones arquitectónicas que se adopten durante el diseño del sistema.
