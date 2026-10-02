# 4. Drivers arquitectónicos

> **Pregunta guía:** ¿Qué condiciona las decisiones de arquitectura?

Un **driver** es un requisito, atributo o restricción que influye
significativamente en la arquitectura. En la Guía 03 se agrega **DA06**.

## Drivers identificados

| ID | Driver arquitectónico | Origen | ¿Por qué influye? |
|----|-----------------------|--------|-------------------|
| DA01 | Escalabilidad: el sistema debe soportar el aumento de usuarios en campañas. | AC04, RNF04 | Define cómo se despliega y replica el sistema. |
| DA02 | Rendimiento: habrá alta concurrencia en el catálogo. | AC01, RNF01 | Obliga a introducir caché y optimizar el acceso a datos. |
| DA03 | Seguridad: se manejan datos personales y de pago. | AC02, RNF02 | Exige autenticación, autorización y no almacenar datos de tarjeta. |
| DA04 | Pago externo: hay que comunicarse con una pasarela. | R06, RF05 | El negocio no debe acoplarse a un proveedor concreto. |
| DA05 | API REST: frontend y backend se comunican por REST. | R03 | Separa interfaz y backend en dos aplicaciones. |
| **DA06** | **Mantenibilidad / evolución modular:** el sistema debe permitir modificar funcionalidades sin afectar innecesariamente otros módulos. | AC05 | Influye en la separación de responsabilidades, la modularidad y las dependencias internas. |

## Driver → problema → decisión

| Driver | Problema que plantea | Decisión que responde |
|--------|----------------------|-----------------------|
| DA01 – Escalabilidad | Aumentarán usuarios en campañas | Monolito modular con posibilidad de escalamiento horizontal |
| DA02 – Rendimiento | Habrá alta concurrencia | Incorporar caché y optimizar comunicación/procesamiento |
| DA03 – Seguridad | Hay datos sensibles | Autenticación y autorización (JWT) |
| DA04 – Pago externo | Hay que comunicarse con una pasarela | Integración mediante API y adaptadores |
| DA05 – API REST | Frontend/backend deben comunicarse mediante REST | Separar interfaz y backend mediante API REST |
| DA06 – Mantenibilidad | Cambios no deben afectar otros módulos | Modularidad + Clean Architecture |

## Priorización

| Prioridad | Drivers |
|-----------|---------|
| 1 (crítico) | DA06, DA04, DA03 |
| 2 (alto) | DA02, DA05 |
| 3 (medio) | DA01 |
