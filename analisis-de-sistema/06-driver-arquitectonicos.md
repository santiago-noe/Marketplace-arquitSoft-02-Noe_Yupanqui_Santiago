# 06 - Drivers Arquitectónicos

## Objetivo

Integrar los elementos identificados durante el análisis del sistema y determinar cuáles tienen una influencia significativa en las decisiones de arquitectura.

Los drivers arquitectónicos representan los requisitos, restricciones y atributos de calidad que condicionan las principales decisiones relacionadas con la estructura, comunicación, seguridad, escalabilidad y despliegue del sistema.

## Drivers Arquitectónicos Identificados

| ID       | Driver Arquitectónico                                                                           | Origen                      | ¿Por qué influye en la arquitectura?                                                     |
| -------- | ----------------------------------------------------------------------------------------------- | --------------------------- | ---------------------------------------------------------------------------------------- |
| **DA01** | El sistema debe soportar un incremento importante de usuarios durante campañas comerciales.     | **AC03 - Escalabilidad**    | Puede influir en la estrategia de escalamiento y despliegue.                             |
| **DA02** | El sistema debe mantener tiempos de respuesta adecuados durante una alta concurrencia.          | **AC01 - Rendimiento**      | Puede influir en la comunicación entre componentes, procesamiento y almacenamiento.      |
| **DA03** | El sistema debe proteger los datos de usuarios y operaciones de compra.                         | **AC04 - Seguridad**        | Puede influir en autenticación, autorización y protección de datos.                      |
| **DA04** | El sistema debe integrarse con una pasarela de pago externa mediante una API.                   | **RC04 - Pasarela de pago** | Condiciona la forma de comunicación e integración con servicios externos.                |
| **DA05** | El sistema debe utilizar una API REST para la comunicación entre frontend y backend.            | **RC03 - API REST**         | Limita las alternativas de comunicación entre las partes del sistema.                    |
| **DA06** | El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | **AC05 - Mantenibilidad**   | Influye en la separación de responsabilidades, modularidad y dependencias internas.      |

## Descripción de los Drivers

### DA01 - Escalabilidad

El sistema debe soportar incrementos importantes de usuarios durante campañas comerciales. Este driver influye en las decisiones relacionadas con la estrategia de escalamiento y despliegue de los componentes.

### DA02 - Rendimiento

El sistema debe mantener tiempos de respuesta adecuados cuando exista una alta cantidad de usuarios concurrentes. Esto puede requerir decisiones relacionadas con la comunicación entre componentes, el procesamiento y el almacenamiento.

### DA03 - Seguridad

El sistema debe proteger los datos de los usuarios y las operaciones de compra. Este driver influye en la implementación de autenticación, autorización y protección de datos.

### DA04 - Integración con pasarela de pago

El sistema debe comunicarse con una pasarela de pago externa mediante una API. Esto condiciona la forma de comunicación e integración con servicios externos.

### DA05 - API REST

La comunicación entre el frontend y backend debe realizarse mediante una API REST. Esta restricción limita las alternativas de comunicación entre las partes del sistema.

### DA06 - Mantenibilidad / evolución modular

El sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. Esto influye en la separación de responsabilidades, la modularidad y el manejo de las dependencias internas.

## Resumen

Los principales drivers arquitectónicos identificados están relacionados con:

- **Escalabilidad** frente al incremento de usuarios.
- **Rendimiento** ante escenarios de alta concurrencia.
- **Seguridad** de los datos y operaciones.
- **Integración con la pasarela de pago externa.**
- **Comunicación mediante API REST.**
- **Mantenibilidad y evolución modular.**

Estos drivers servirán como base para justificar las decisiones arquitectónicas que se adopten durante el diseño del sistema.
