# Flujo de ingeniería para un proyecto nuevo (greenfield)

Este flujo convierte una idea en requisitos, un diseño validable y un plan de entrega. No es una cascada rígida: se adapta al riesgo y a la incertidumbre y puede volver a pasos anteriores cuando se aprende algo nuevo. **La documentación es proporcional al proyecto; no es obligatorio crear un archivo por cada tipo de artefacto.**

## 0. Selecciona un nivel inicial de documentación

Usa los factores de la skill: complejidad del dominio, impacto de fallos, seguridad y sensibilidad de los datos, integraciones, cantidad de colaboradores, incertidumbre y coste de cambios.

- **Ligero:** un proyecto acotado y de bajo riesgo. Normalmente un `project-design.md` y un `README.md` son suficientes.
- **Estándar:** varios módulos, flujos relevantes, integraciones o colaboración. Separa PRD y diseño de sistema; separa datos/API solo si ayuda a los consumidores o a la evolución.
- **Riguroso:** sistema grande, crítico o con requisitos fuertes de cumplimiento, seguridad, disponibilidad o trazabilidad. Separa artefactos para revisión y ownership independientes.

El nivel puede variar por área: un proyecto pequeño con datos sensibles puede necesitar seguridad rigurosa y el resto de la documentación ligera. Empieza con el menor nivel que controle los riesgos conocidos y amplíalo cuando exista una razón concreta.

## 1. Descubrimiento del problema

Define, con el nivel de detalle disponible:

- Problema u oportunidad que se quiere resolver.
- Usuarios o actores implicados y sus necesidades. Determina si hay tipos de usuario con capacidades diferentes, acceso anónimo/autenticado o reglas de propiedad de recursos.
- Resultado esperado y señales de éxito; no inventes métricas numéricas.
- Contexto, restricciones conocidas, dependencias y límites.
- Qué queda explícitamente fuera del alcance inicial.

**Salida:** una sección de contexto y objetivos en `project-design.md` o un brief/PRD independiente si el alcance o los colaboradores lo requieren. Si la idea sigue vaga, presenta una hipótesis inicial y las preguntas prioritarias.

## 2. Requisitos y alcance del MVP

Captura, según corresponda:

- Usuarios, casos de uso y flujos principales.
- Roles, permisos funcionales y restricciones de acceso, si hay diferencias relevantes: quién puede ver, crear, modificar, eliminar o administrar qué recursos. Incluye una matriz solo cuando ayude a validar requisitos.
- Requisitos funcionales y reglas de negocio.
- Criterios de aceptación observables.
- Requisitos de calidad y restricciones relevantes (seguridad, privacidad, rendimiento, disponibilidad, accesibilidad, mantenibilidad o cumplimiento).
- Alcance del MVP, exclusiones, dependencias, riesgos y preguntas abiertas.

No uses requisitos vagos como “rápido”, “seguro” o “escalable” sin explicar cómo se evaluarán. No conviertas una solución técnica preferida en una necesidad de producto. En un proyecto pequeño, estos elementos pueden ser secciones breves del documento integrado.

## 3. Modela el dominio y los procesos

Identifica conceptos, términos ambiguos, actores, estados, transiciones e invariantes de negocio. Usa glosario o diagramas de flujo solo cuando ayuden a aclarar la lógica.

No conviertas automáticamente cada sustantivo en una tabla o clase, ni supongas que el modelo de dominio coincide exactamente con el esquema de persistencia.

## 4. Diseña la arquitectura suficiente

Define las responsabilidades y límites necesarios para implementar el MVP. Considera, cuando aplique:

- Componentes o módulos, dependencias y estilo arquitectónico.
- Integraciones y límites de confianza.
- Cómo se implementarán técnicamente los requisitos funcionales de roles/permisos (por ejemplo, en la capa de API, servicios o políticas de acceso), identidad y protección de datos sensibles. No confundas las capacidades exigidas por el negocio con la tecnología elegida para hacerlas cumplir.
- Despliegue, configuración, manejo de errores y observabilidad.
- Mantenibilidad, complejidad operacional y riesgos de evolución.

Explica trade-offs relevantes. Evita adoptar microservicios, nube, frameworks o patrones sin contexto. Registra con un ADR las decisiones de alto impacto que merecen conservar su razonamiento, no todas las elecciones triviales.

## 5. Diseña los datos y contratos necesarios

Describe lo necesario para implementar y validar:

- **Datos:** entidades, relaciones, atributos clave, restricciones e índices relevantes. Añade ERD, diccionario detallado o un DBDD independiente si la complejidad o el trabajo de varias personas lo justifica.
- **API:** operaciones, parámetros, esquemas, autenticación y errores. Usa OpenAPI cuando el contrato tenga que compartirse, validarse o mantenerse como especificación.
- **Integraciones:** contratos, dependencias, fallos esperados y reintentos cuando correspondan.

En un proyecto pequeño, un modelo conciso de datos y los principales endpoints pueden vivir en `project-design.md`. No fuerces contratos o archivos especializados que todavía no sean útiles.

## 6. Define calidad, seguridad y validación

Vincula los criterios de aceptación con su método de verificación. Selecciona pruebas unitarias, de integración, contrato, extremo a extremo, rendimiento, seguridad o accesibilidad según los riesgos. Incorpora logging, métricas, trazas, backups y recuperación cuando sean necesidades reales.

No añadas listas genéricas de controles sin considerar el contexto; tampoco omitas controles importantes solo porque el proyecto sea pequeño.

## 7. Planifica un primer incremento

Propón el primer *vertical slice* o hito que permita validar una parte útil del sistema. Para cada elemento de trabajo, detalla según corresponda el resultado, alcance, dependencias, criterios de aceptación, pruebas y decisiones pendientes.

No inventes fechas ni estimaciones precisas. Evita dividir el trabajo en tareas artificialmente pequeñas.

## 8. Revisión antes de implementar

Resume:

- Qué requisitos y límites están acordados.
- Qué nivel de documentación elegiste y por qué.
- Qué arquitectura y modelo de datos propones.
- Qué contratos y controles de calidad son necesarios.
- Qué supuestos y decisiones de alto impacto continúan abiertos.
- Qué archivos creaste y dónde se encuentra la fuente canónica de cada asunto.
- Cuál es el primer incremento y cómo se verificará.

No afirmes que el proyecto está “completamente diseñado” si quedan decisiones importantes sin resolver. La revisión habilita un inicio informado, no congela el diseño para siempre. Ajusta las secciones o documentos afectados conforme cambie el entendimiento.

## Entregables orientativos por nivel

### Ligero

```text
docs/
└── project-design.md
README.md
```

`project-design.md` reúne, según corresponda: problema y alcance, MVP/requisitos, reglas de negocio, decisiones de arquitectura, modelo de datos, API/integraciones, estrategia de pruebas, riesgos y preguntas abiertas. `README.md` cubre preparación y ejecución del proyecto.

### Estándar

```text
docs/
├── prd.md
├── system-design.md
├── database-design.md   # si necesita detalle independiente
├── api/openapi.yaml     # si el contrato lo justifica
└── adr/                 # decisiones relevantes
```

Usa solo los archivos necesarios. Un TDD se añade para una funcionalidad o cambio con decisiones técnicas sustanciales.

### Riguroso

Separa PRD, SDD, DBDD, contratos, TDD, ADR, pruebas, despliegue y operación según las necesidades de trazabilidad, auditoría, colaboración y evolución independiente. Mantén una sola fuente de verdad y enlaces entre documentos.

El objetivo en todos los niveles es el mismo: reducir ambigüedad, controlar riesgos y facilitar la implementación. Lo que cambia es la profundidad y la organización, no la necesidad de razonar sobre requisitos y diseño.
