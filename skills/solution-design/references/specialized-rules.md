# Reglas especializadas: datos, usuarios/permisos y diseño técnico

Reglas de dominio para cuando la documentación cubre persistencia, control de acceso o diseño técnico detallado. Cárgalas solo cuando el proyecto las necesita; no fuerces secciones que no aplican.

## Datos (DBDD o sección de datos)

- Distingue modelo conceptual, lógico y físico en la medida que lo requiera la complejidad.
- Documenta tablas/entidades, atributos/columnas, tipos, nulabilidad, valores predeterminados, claves y restricciones relevantes; cardinalidad e integridad referencial. ERD solo cuando aporte claridad.
- Incluye definiciones de campos importantes; puede ser una tabla breve dentro de `project-design.md`.
- Documenta fechas y zonas horarias, borrado lógico/físico, auditoría, datos sensibles, transacciones y migraciones cuando apliquen.
- No presumas que el diseño físico es portable entre motores. Declara motor y versión solo si se conocen.
- Con ORM (por ejemplo, JPA): diferencia el modelo de dominio de la persistencia y revisa las migraciones como evidencia del esquema desplegable.
- No crees un DBDD separado si una sección concisa satisface las necesidades del proyecto; sepáralo cuando complejidad, colaboración, revisión o cambio independiente lo justifiquen.

## Usuarios, roles y permisos

| Etapa | Regla |
|---|---|
| Descubrimiento | Identifica usuarios y actores; determina si sus capacidades o restricciones de acceso difieren. No presupongas que toda aplicación necesita roles. |
| PRD (o sección de `project-design.md`) | Documenta requisitos funcionales de acceso: qué usuarios pueden realizar qué acciones, sobre qué recursos y bajo qué restricciones. Matriz de roles y permisos solo cuando facilite la revisión o los criterios de aceptación. Si no hay diferencias relevantes, omite la matriz. |
| Faltan definiciones | Registra preguntas abiertas; no inventes roles, privilegios ni reglas de acceso. |
| SDD | Documenta la estrategia técnica (RBAC/ABAC, JWT, sesiones, anotaciones del framework, estructura de tablas). No mezcles necesidades del negocio con decisiones de implementación. |
| TDD | Detalla la implementación de autorización cuando la funcionalidad lo justifique. |
| Validación | Incluye permisos en criterios de aceptación y pruebas de autorización; no des por implementada ni verificada una restricción solo porque aparezca en un documento. |

## Diseño técnico (SDD/TDD)

- Describe componentes, límites, responsabilidades, dependencias, flujos e interfaces al nivel apropiado.
- Diagramas (por ejemplo, Mermaid) solo si mejoran la comprensión y reflejan el estado actual o la propuesta con claridad.
- ADR para decisiones importantes, incluso en un proyecto pequeño, si sus consecuencias son difíciles de revertir.
- TDD separado solo para funcionalidades o cambios con complejidad técnica significativa, no por cada tarea sencilla. TDD también puede significar *Test-Driven Development*; escribe el término completo si hay ambigüedad.
- OpenAPI como fuente estructurada de verdad solo para APIs con consumidores o requisitos de contrato explícitos; no lo generes por defecto para una API interna trivial.
- Define criterios medibles para requisitos no funcionales cuando sea posible; no inventes objetivos cuantitativos.
