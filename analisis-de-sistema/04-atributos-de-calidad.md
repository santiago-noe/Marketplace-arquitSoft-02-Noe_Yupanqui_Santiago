# 04 - Atributos de Calidad

## Objetivo

Determinar las características de calidad que debe cumplir el sistema, además de las funcionalidades que debe proporcionar.

Los atributos de calidad permiten establecer cómo debe comportarse el sistema ante diferentes escenarios, especialmente cuando existe una alta cantidad de usuarios y operaciones simultáneas.

## Escenario de Calidad

Durante una campaña comercial, el marketplace podría recibir una gran cantidad de usuarios consultando productos, agregando productos al carrito y realizando compras de manera simultánea.

Ante este escenario, el sistema debe mantener un funcionamiento adecuado, garantizando un buen rendimiento, disponibilidad, escalabilidad, seguridad y facilidad de mantenimiento.

## Atributos de Calidad

| ID       | Atributo de Calidad | Escenario de Calidad                                                                                                                                   |
| -------- | ------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **AC01** | **Rendimiento**     | Las consultas de productos y las operaciones del carrito deben responder rápidamente incluso cuando exista una alta cantidad de usuarios concurrentes. |
| **AC02** | **Disponibilidad**  | El sistema debe permanecer disponible durante la campaña comercial y permitir que los usuarios realicen sus operaciones.                               |
| **AC03** | **Escalabilidad**   | El sistema debe poder soportar un incremento de usuarios y solicitudes sin afectar significativamente su funcionamiento.                               |
| **AC04** | **Seguridad**       | Los datos de los usuarios, cuentas y operaciones de compra deben estar protegidos frente a accesos no autorizados.                                     |
| **AC05** | **Mantenibilidad**  | El sistema debe estar organizado de manera que permita realizar cambios y correcciones sin afectar innecesariamente otras funcionalidades.             |

## Descripción de los Atributos

### AC01 - Rendimiento

El sistema debe proporcionar tiempos de respuesta adecuados durante las consultas de productos, gestión del carrito y demás operaciones, incluso cuando exista una alta concurrencia de usuarios.

### AC02 - Disponibilidad

El sistema debe mantenerse operativo durante las campañas comerciales, permitiendo a los usuarios acceder a la plataforma, consultar productos y realizar sus compras.

### AC03 - Escalabilidad

El sistema debe poder aumentar su capacidad de procesamiento cuando se incremente la cantidad de usuarios o solicitudes, evitando una degradación significativa del servicio.

### AC04 - Seguridad

El sistema debe proteger la información de los usuarios y las operaciones realizadas en la plataforma, evitando accesos no autorizados y protegiendo los datos asociados a las compras.

### AC05 - Mantenibilidad

El sistema debe presentar una estructura organizada y modular que facilite la corrección de errores, incorporación de nuevas funcionalidades y realización de cambios sin afectar innecesariamente otros componentes.

## Resumen

Los principales atributos de calidad considerados para el marketplace son:

- **Rendimiento:** respuesta rápida ante solicitudes concurrentes.
- **Disponibilidad:** funcionamiento continuo durante las campañas comerciales.
- **Escalabilidad:** capacidad para soportar el crecimiento de usuarios y solicitudes.
- **Seguridad:** protección de usuarios, cuentas y operaciones.
- **Mantenibilidad:** facilidad para modificar, corregir y evolucionar el sistema.
