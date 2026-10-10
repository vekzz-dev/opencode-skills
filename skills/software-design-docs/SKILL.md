---
name: software-design-docs
description: "Trigger: software design docs, PRD, SDD, DBDD, TDD, ADR, diseño de software, arquitectura, modelo de datos, requisitos, MVP, greenfield. Guide software engineering from idea to implementation or evolve an existing system, adapting documentation to risk."
metadata:
  author: "vekzz-dev"
  version: "2.0.0"
  language: "es"
  license: "MIT"
---

# Software Design Documentation

Contrato de instrucciones para guiar la ingeniería y el diseño de software de proyectos nuevos o existentes. Ayuda a pasar del problema y los requisitos a un diseño validable y un plan de entrega, manteniendo la documentación proporcional al tamaño, complejidad, riesgo y forma de trabajo del proyecto. Los tipos de documento describen responsabilidades; **no implican que cada tipo deba convertirse en un archivo separado**.

## Activation Contract

Carga esta skill cuando el usuario:

- empieza un proyecto nuevo o una funcionalidad y necesita decidir el diseño;
- pide documentar arquitectura, datos, API, requisitos (PRD), decisiones (ADR) o plan de entrega;
- pide "diseño de software", "PRD", "SDD", "design doc", "documentar el sistema" o similar;
- quiere alinear diseño con un repositorio existente.

No activar para: escribir código sin decisión de diseño que documentar, ni reviews de PR.

## Hard Rules

1. **Empieza por la intención.** Determina primero si el usuario necesita descubrir requisitos, definir el producto, diseñar arquitectura, resolver una decisión, documentar datos o preparar implementación.
2. **Inspecciona antes de escribir.** Si existe repositorio: revisa estructura, documentación, configuración, modelos, migraciones, pruebas y código relacionado. No reemplaces documentación existente sin comprenderla.
3. **No inventes.** No inventes reglas de negocio, tablas, campos, endpoints, volúmenes, objetivos de rendimiento, roles, permisos ni controles. Separa explícitamente hechos verificados, decisiones aceptadas, propuestas, supuestos y preguntas abiertas. Lo desconocido e importante queda como "Pendiente de definir".
4. **No des por hecha una arquitectura.** No recomiendes microservicios, patrones, frameworks, tecnologías ni infraestructura sin contexto suficiente; explica trade-offs en decisiones de impacto.
5. **Documentación mínima suficiente.** No generes todos los artefactos por defecto; omite secciones irrelevantes. Combina antes de fragmentar: en proyectos pequeños, `project-design.md` integrado. Separa solo por consumidores distintos, complejidad propia, versionado, reutilización o motivos de seguridad/auditoría.
6. **Una sola fuente de verdad.** Un dato, contrato o decisión tiene una ubicación canónica; otros documentos enlazan o resumen sin copiar.
7. **Diseño iterativo.** No exijas diseño exhaustivo antes de programar: reduce los riesgos relevantes, implementa incrementos verificables, actualiza al aprender.
8. **Documenta decisiones, no cada detalle del código.** No dupliques código ni describas cada clase sin necesidad.
9. **No confundas documentar con validar.** No afirmes haber revisado código, ejecutado pruebas ni verificado requisitos si no lo hiciste.
10. **Idioma y formato.** Responde en el idioma del usuario, Markdown por defecto, respeta las convenciones del repositorio.

## Decision Gates

### Nivel de documentación

| Señales observadas | Nivel | Artefactos |
|---|---|---|
| Dominio acotado, pocas integraciones, pocas personas, fallo manejable | **Ligero** | `project-design.md` (requisitos+datos+API+arquitectura en secciones) + `README.md`; OpenAPI solo si terceros consumen la API |
| Varios módulos/flujos, varios colaboradores, integraciones relevantes | **Estándar** | `prd.md` + `system-design.md`; `database-design.md` y `openapi.yaml` solo si aportan detalle independiente; TDDs para cambios complejos, ADRs para decisiones relevantes |
| Múltiples equipos, dominio complejo, cumplimiento/seguridad/privacidad exigentes, disponibilidad estricta, migraciones delicadas | **Riguroso** | PRD, SDD, DBDD, contratos API, TDDs, ADRs, estrategia de pruebas, despliegue, observabilidad, trazabilidad |
| Tamaño pequeño pero riesgo alto en un área | Mixto | Rigor solo en las áreas críticas; resto ligero |
| Contexto insuficiente | Ligero inicial | Registra supuestos y amplía cuando una necesidad concreta lo justifique |

Factores (evalúa cualitativamente, el tamaño por sí solo no decide): complejidad e incertidumbre del dominio, impacto de fallos y recuperación, sensibilidad/integridad/volumen de datos, integraciones externas, seguridad/privacidad/cumplimiento/disponibilidad/rendimiento, número de colaboradores, probabilidad y coste de cambios. Ajusta el nivel al aprender; comunica solo una justificación breve si cambias los entregables de forma importante.

### Separar o combinar un artefacto

| Factor | Combina en un documento integrado | Separa |
|---|---|---|
| Lectores/consumidores | Mismos, pocos | Distintos o revisión independiente |
| Cambios y ownership | Van juntos, mismo owner | Ritmo u owner distintos |
| Necesita versionado, validación con herramienta o publicación | No | Sí |
| Complejidad/tamaño | Encaja en una sección | Merece navegarlo solo |
| Trazabilidad/auditoría/acceso | No requiere | Requiere |

Regla: **combina primero; separa solo cuando un factor de la columna "Separa" esté presente.** Nunca crees archivos vacíos ni separados solo por plantilla.

## Reglas especializadas (solo si el tema aplica)

Para datos (DBDD, ERD, migraciones, motor), usuarios/roles/permisos (requisitos funcionales de acceso vs estrategia técnica RBAC/JWT) y diseño técnico detallado (diagramas, TDD, OpenAPI, ADR, RNF medibles): carga [`references/specialized-rules.md`](references/specialized-rules.md) y aplica solo lo que aplique. Resumen de los topes: no inventes roles/permisos ni reglas de acceso; los requisitos funcionales de acceso van en el PRD, la estrategia técnica en el SDD; una restricción documentada no está implementada ni verificada.

## Execution Steps

1. **Determina el punto de partida.**
   - Proyecto nuevo → workflow greenfield: carga [`references/greenfield-workflow.md`](references/greenfield-workflow.md). Empieza por problema, usuarios, resultado esperado y restricciones; no generes arquitectura, tablas ni endpoints antes de entender la necesidad.
   - Proyecto existente → inspecciona el repositorio; trata código, configuración, pruebas y migraciones como evidencia del estado actual. Distingue estado actual vs diseño propuesto vs trabajo futuro.
   - Nueva funcionalidad → revisa contexto, convenciones y diseños existentes.
   - Ausencia de información no bloqueante → registra supuestos y preguntas abiertas. Pregunta al usuario solo cuando la ambigüedad bloquee materialmente una decisión.
2. **Clasifica necesidad y nivel.** Carga [`references/document-types.md`](references/document-types.md) para identificar responsabilidades (tipo ≠ archivo). Aplica la Decision Gate de la sección "Nivel de documentación" (criterios arriba).
3. **Reúne el contexto disponible.** Inspecciona archivos relevantes, marca lo importante como "Pendiente de definir" (no bloques el trabajo por detalles menores). Para decisiones de arquitectura, seguridad, persistencia, interoperabilidad o coste de cambio: explica alternativas y trade-offs apropiados al nivel.
4. **Elige la organización documental.** Estructura mínima que sirva a los usuarios del proyecto; los layouts de ejemplo por nivel están en [`references/templates.md`](references/templates.md). Si ya existe una convención en el repositorio, respétala salvo que haya una razón clara para cambiarla.
5. **Redacta o actualiza.** Usa [`references/templates.md`](references/templates.md) como guía, no formulario. Omite secciones irrelevantes. Al combinar, encabezados claros que preserven propósitos distintos sin duplicar contenido.
6. **Verifica coherencia.** Requisitos/reglas/criterios sin contradicciones; dominio-datos-interfaces-flujos coherentes; endpoints/DTOs/errores/códigos HTTP coinciden con sus fuentes canónicas; enlaces, diagramas y referencias válidos (no inventes destinos); separación hechos/decisiones/propuestas/supuestos/preguntas.
7. **Prepara entrega incremental.** Cuando aplique, hitos o vertical slices pequeños con criterios de aceptación y pruebas verificables. No inventes fechas ni estimaciones precisas. Señala bloqueos y decisiones de alto impacto pendientes.
8. **Entrega según Output Contract** (abajo).

## Output Contract

Al finalizar, reporta:

1. Archivos creados o modificados.
2. Por qué se eligió ese nivel de documentación (y qué responsabilidad quedó dónde si se combinaron artefactos).
3. Los supuestos más importantes.
4. Las decisiones pendientes y bloqueos.
5. El siguiente incremento recomendado.

## Criterio de calidad

La documentación está lista cuando ayuda a entender qué debe construirse o cómo funciona lo existente, identificar decisiones y restricciones, localizar contratos y verificar requisitos. El número de archivos y la longitud no son indicadores de calidad. **Prefiere la estructura más sencilla que mantenga el diseño comprensible, verificable y fácil de actualizar.**

Para el razonamiento de fondo (por qué esta skill existe, anti-burocracia, por qué cada regla) carga [`references/philosophy.md`](references/philosophy.md) cuando necesites justificar o adaptar decisiones donde la regla no cubre el caso concreto.

## References

- [`references/greenfield-workflow.md`](references/greenfield-workflow.md) — flujo iterativo para proyectos nuevos (descubrimiento → requisitos → dominio → diseño → entrega).
- [`references/document-types.md`](references/document-types.md) — responsabilidades de cada tipo de artefacto (PRD, SDD, TDD, DBDD, OpenAPI, ADR) y cuándo separar.
- [`references/templates.md`](references/templates.md) — plantillas orientativas por nivel (integrada ligera, PRD, SDD, DBDD…) y criterios de separación.
- [`references/specialized-rules.md`](references/specialized-rules.md) — reglas específicas de datos, usuarios/roles/permisos y diseño técnico.
- [`references/philosophy.md`](references/philosophy.md) — razonamiento de fondo, motivación y mapeo de la v1.2.1 (por qué de cada regla).
