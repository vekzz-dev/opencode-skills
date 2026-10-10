# Filosofía y razonamiento de fondo

Referencia de apoyo para `SKILL.md`. Contiene la motivación y el razonamiento detrás de las reglas. Úsala cuando necesites justificar una decisión, adaptar una regla a un caso que no cubre, o explicar el enfoque al usuario. No añade requisitos nuevos.

## La meta real: menos ambigüedad, no más documentos

La meta es reducir ambigüedad y facilitar decisiones seguras, no producir la mayor cantidad posible de documentación. La ingeniería puede ser rigurosa sin burocracia: en un proyecto pequeño, un documento bien organizado puede contener los elementos esenciales del PRD, SDD y DBDD. Un PDF de 40 páginas que nadie lee y que se desincroniza del código es documentación muerta; una página viva, verificable y actualizada vale más.

## Los tipos describen responsabilidades, no archivos

Un error común es tratar la lista de artefactos (PRD, SDD, DBDD, TDD, ADR) como una lista de archivos obligatorios. Es una lista de **responsabilidades de contenido**. Un proyecto pequeño cumple el PRD con una sección del documento integrado; un sistema grande separa el mismo contenido porque hay revisores, ownership y evolución distintos. Pregunta siempre: ¿qué pregunta responde este artefacto y quién la lee? Los detalles por tipo están en `document-types.md`.

## Rigor ≠ volumen

Incluso en nivel riguroso, no rellenes plantillas sin propósito. El rigor significa que las decisiones y controles relevantes estén definidos y verificables, no que cada tema tenga un archivo propio. Un sistema crítico bien documentado demuestra su rigor con decisiones trazables y controles verificables, no con longitud.

## Por qué "no inventes" ocupa el lugar central

Un invento documentado se convierte en verdad de referencia para quien no puede verificarlo: alguien implementa la tabla que nunca existió, o asume un objetivo de rendimiento que nadie fijó. Por eso la separación hechos / decisiones / propuestas / supuestos / preguntas no es formato decorativo: es el mecanismo que evita fabricar certeza. En descubrimiento, lo mismo aplica a usuarios, actores, roles y permisos: documenta lo confirmado, marca lo desconocido como pendiente, y si no hay diferencias de acceso relevantes, no fuerces una matriz.

## Por qué no "des por hecha" la arquitectura

Recomendar microservicios, un ORM, un framework o una infraestructura sin contexto es imponer presupuesto técnico a alguien que aún no definió el problema. Las decisiones de arquitectura tienen costes de reversión asimétricos: cuanto más cargada la apuesta y menos contexto, más caro el error. Explica trade-offs en proporción al impacto real de la decisión.

## La estructura sigue a los consumidores (no a la plantilla)

El criterio para separar artefactos no es la complejidad por sí misma, sino quién consume la información y cómo: revisores distintos, versionado, validación con herramientas, ownership independiente, auditoría. Cuando esos factores no existen, la separación solo añade navegación, duplicación y desincronización. Por eso el nivel se elige por factores de riesgo (complejidad del dominio, impacto de fallos, datos, integraciones, colaboradores) y no por tamaño.

## Documentar ≠ validar, iterar ≠ no diseñar

Dos confusiones simétricas:

- Creer que escribir el diseño lo valida: solo el código, las pruebas o la revisión verifican. Un documento no implementado ni probado es una hipótesis.
- Exigir diseño exhaustivo antes de programar: el diseño sirve para reducir riesgos relevantes, no para eliminarlos todos. Diseña lo suficiente, implementa incrementos verificables, actualiza al aprender.

## 12 principios originales (v1.2.1) — mapeo a reglas actuales

Los principios de la versión 1.2.1 se condensaron en las Hard Rules; este es el trazado para auditoría:

| v1.2.1 | Ahora |
|---|---|
| 1. Empieza por la intención | Hard Rule 1 |
| 2. Inspecciona antes de escribir | Hard Rule 2 |
| 3. No inventes hechos | Hard Rule 3 (+ filosofía arriba) |
| 4. Adapta el rigor al riesgo | Decision Gate "Nivel de documentación" |
| 5. Documentación mínima suficiente | Hard Rule 5 |
| 6. Combina antes de fragmentar | Hard Rule 5 + Decision Gate "Separar o combinar" |
| 7. Una sola fuente de verdad | Hard Rule 6 |
| 8. Diseña iterativamente | Hard Rule 7 (+ filosofía arriba) |
| 9. Documenta decisiones, no cada detalle | Hard Rule 8 |
| 10. No des por hecha la arquitectura | Hard Rule 4 |
| 11. Adapta idioma y convenciones | Hard Rule 10 |
| 12. Documentar ≠ validar | Hard Rule 9 |
