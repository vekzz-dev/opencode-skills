# Tipos de artefactos y límites de responsabilidad

Los nombres no son universales en todos los equipos. Usa estas definiciones operativas y respeta las convenciones establecidas en el proyecto. **Un tipo de artefacto no equivale necesariamente a un archivo.** Un proyecto pequeño puede contener varias de estas responsabilidades en secciones de `project-design.md`; sepáralas cuando se requieran revisión, ownership, versionado o evolución independientes.

## PRD — Product Requirements Document

**Pregunta:** ¿Qué problema se resolverá, para quién y qué debe hacer el producto?

Suele incluir objetivo, usuarios/actores y sus necesidades, alcance/exclusiones, casos de uso, requisitos funcionales, criterios de aceptación, reglas de negocio, requisitos de calidad desde la perspectiva del producto, métricas, restricciones y preguntas abiertas. **Cuando haya diferencias de acceso, incluye roles y permisos funcionales:** qué acciones puede realizar cada tipo de usuario, qué restricciones existen y, si aporta claridad, una matriz de roles y permisos. Considera también acceso anónimo/autenticado y reglas de propiedad de recursos cuando corresponda.

No inventes roles, privilegios ni requisitos de autorización que no estén respaldados por el contexto; registra lo desconocido como pregunta abierta. Si todos los usuarios tienen las mismas capacidades o el sistema no requiere control de acceso, no fuerces una matriz. El PRD define el comportamiento requerido, no el mecanismo técnico. No debe convertirse en una especificación detallada de tablas SQL, clases, frameworks o arquitectura interna. En un proyecto pequeño puede ser una sección de `project-design.md`.

## SDD — System Design Document

**Pregunta:** ¿Cómo se organiza técnicamente el sistema como conjunto?

Suele incluir contexto y alcance técnico, arquitectura de alto nivel, componentes, límites de módulos o servicios, flujos principales, integraciones, seguridad (incluida la estrategia técnica de autenticación y autorización cuando aplique), atributos de calidad, despliegue y referencias al diseño de datos/API.

Puede ser una sección de un documento integrado en proyectos sencillos. Sepáralo cuando su arquitectura necesite revisión o mantenimiento independiente.

## TDD — Technical Design Document

**Pregunta:** ¿Cómo implementaremos una funcionalidad o cambio técnico concreto?

Suele incluir contexto del cambio, requisitos/restricciones, solución propuesta, alternativas, componentes afectados, flujo, cambios de API y datos, errores, seguridad, pruebas, despliegue y riesgos.

No crees un TDD por cada tarea pequeña. Úsalo cuando las decisiones técnicas de una funcionalidad merezcan una propuesta revisable. TDD también significa *Test-Driven Development*; escribe el término completo cuando haya ambigüedad.

## DBDD — Database Design Document

**Pregunta:** ¿Cómo se modelan, relacionan, restringen y persisten los datos?

Suele incluir motor conocido, modelos conceptual/lógico/físico, ERD, tablas y columnas, claves, relaciones, restricciones, índices, diccionario de datos, integridad, migraciones y consideraciones de rendimiento, privacidad, retención o auditoría cuando apliquen.

En un proyecto pequeño puede ser una sección o una tabla concisa dentro de `project-design.md`. Sepáralo si el esquema es complejo, se revisa por separado, tiene cambios delicados o lo consumen distintos equipos.

## Contrato de API — por ejemplo, OpenAPI

**Pregunta:** ¿Cómo interactúan los consumidores con la API?

Define operaciones, rutas, parámetros, esquemas de solicitud y respuesta, autenticación, errores y códigos HTTP. Usa OpenAPI como fuente estructurada de verdad cuando el contrato necesite compartirse, validarse o versionarse. No siempre aporta valor como archivo aparte para una API pequeña de uso local y sin consumidores independientes.

## ADR — Architecture Decision Record

**Pregunta:** ¿Qué decisión técnica relevante se tomó, por qué y qué consecuencias tiene?

Registra contexto, opciones relevantes, decisión, consecuencias y estado. Un ADR puede ser breve y vale la pena cuando una decisión importante sea difícil de revertir, incluso en un proyecto pequeño. No registres como ADR todas las decisiones triviales.

## README y guía operativa

**Pregunta:** ¿Cómo se instala, configura, ejecuta y prueba este repositorio?

El README documenta preparación y uso práctico. No debe duplicar todo el diseño del sistema; enlaza al documento de diseño canónico cuando sea necesario.

## Cómo elegir qué separar

Mantén las responsabilidades en un documento integrado si son cortas, cambian juntas, tienen los mismos lectores y la separación solo añade navegación o duplicación. Separa un artefacto cuando uno o varios de estos factores lo justifiquen:

- Tiene consumidores o revisores distintos.
- Debe validarse, versionarse o publicarse con una herramienta especializada.
- Es suficientemente complejo como para navegarlo de forma independiente.
- Cambia a un ritmo distinto o tiene ownership independiente.
- Requiere trazabilidad, auditoría o controles de acceso específicos.
- Separarlo reduce la duplicación o los riesgos de inconsistencias.

Preguntas rápidas:

- “¿Qué debe hacer el producto?” → PRD, o sección de requisitos.
- “¿Cómo se organiza el sistema?” → SDD, o sección de arquitectura.
- “¿Cómo resolveremos este cambio complejo?” → TDD.
- “¿Qué tablas, relaciones, tipos y restricciones necesitamos?” → DBDD, o sección de datos.
- “¿Qué operaciones y esquemas expone la API?” → contrato de API/OpenAPI si es útil.
- “¿Por qué elegimos esta arquitectura o tecnología?” → ADR si la decisión importa.

Combinar es válido; duplicar la fuente de verdad no. Mantén nombres, contratos y decisiones canónicos en un solo lugar y enlaza desde otros artefactos.
