# Plantillas orientativas

Usa estas plantillas como esqueletos, no como formularios obligatorios. Omite secciones irrelevantes y no rellenes huecos con información inventada. Elige la plantilla integrada para proyectos ligeros y las plantillas especializadas cuando separar los artefactos aporte valor.

## Nivel ligero: Project Design Document integrado

Úsalo como punto de partida para proyectos pequeños y de bajo riesgo. Es una plantilla de una sola fuente de verdad; no hace falta completar todas las secciones antes de empezar.

```markdown
# Project Design — <nombre>

## 1. Resumen
- Problema que resuelve:
- Usuarios/actores principales:
- Resultado esperado:
- Estado del documento: Draft / In Review / Accepted

## 2. Alcance del MVP
### Incluido
### Excluido

## 3. Requisitos y reglas de negocio
- Requisitos funcionales:
- Reglas importantes:
- Criterios de aceptación:
- Restricciones y requisitos de calidad relevantes:

## 4. Usuarios, roles, permisos y restricciones de acceso (si aplica)
- Tipos de usuario/actores y necesidades:
- Roles de negocio o del sistema confirmados:
- Acciones permitidas y restricciones por rol:
- Matriz de roles y permisos, si aporta claridad:
- Acceso de usuarios anónimos/autenticados y reglas de propiedad de recursos, si aplica:
- Preguntas abiertas; no asumir roles ni permisos sin evidencia:

## 5. Diseño de la solución
- Arquitectura y componentes principales:
- Responsabilidades y flujo principal:
- Estrategia técnica de autenticación/autorización (o referencia al diseño de seguridad):
- Tecnologías ya decididas y razones, si se conocen:

## 6. Datos (si aplica)
- Entidades y relaciones principales:
- Atributos/restricciones importantes:
- Diagrama ERD si aporta claridad:
- Motor de base de datos y migraciones, si se conocen:

## 7. API e integraciones (si aplica)
- Interfaces/endpoints principales:
- Contratos de entrada y salida:
- Requisitos funcionales de acceso por operación; los detalles técnicos de implementación van en el diseño técnico:
- Dependencias externas y fallos relevantes:

## 8. Seguridad y operación (según riesgo)
- Autenticación, autorización técnica y datos sensibles:
- Configuración, despliegue, logging/backups si aplican:

## 9. Pruebas y validación
- Cómo se verifican los criterios de aceptación, incluidos los permisos relevantes:
- Pruebas prioritarias:

## 10. Plan inicial
- Primer incremento vertical:
- Dependencias y riesgos:

## 11. Supuestos, decisiones pendientes y referencias
- Confirmado:
- Propuesto:
- Pendiente de definir:
```

Adapta, elimina o renombra secciones. Si no hay base de datos, elimina la sección de datos; si no hay API, elimina esa sección. Si no existen roles ni restricciones de acceso diferenciadas, omite la matriz de permisos y anota brevemente que no aplica solo cuando esa aclaración sea útil. No documentes cada endpoint o columna si todavía no se necesita ese nivel de detalle. En el PRD y en el documento integrado, registra las capacidades y restricciones de acceso desde el punto de vista funcional; deja los mecanismos técnicos (por ejemplo, RBAC/ABAC, JWT, sesiones o configuración del framework) para el diseño del sistema o el TDD.

## Metadatos comunes para documentos separados

```yaml
# Puede ser una tabla si el proyecto no usa frontmatter.
title: "<nombre del documento>"
status: "Draft | In Review | Approved | Superseded"
version: "<versión o revisión>"
last_updated: "<fecha ISO 8601>"
owners: []
related_documents: []
```

## PRD

```markdown
# Product Requirements Document — <producto>

## 1. Resumen y problema
## 2. Objetivos y métricas de éxito
## 3. Usuarios, actores y necesidades
## 4. Roles, permisos funcionales y restricciones de acceso (si aplica)
## 5. Alcance e exclusiones
## 6. Flujos e historias de usuario
## 7. Requisitos funcionales
## 8. Reglas de negocio
## 9. Requisitos de calidad y restricciones
## 10. Criterios de aceptación
## 11. Dependencias y riesgos
## 12. Preguntas abiertas
## 13. Documentos relacionados
```

## System Design Document

```markdown
# System Design Document — <sistema>

## 1. Propósito, alcance y estado actual
## 2. Contexto y restricciones
## 3. Arquitectura de alto nivel
## 4. Componentes y responsabilidades
## 5. Flujos principales
## 6. Interfaces e integraciones
## 7. Modelo de datos (resumen y referencia al DBDD, si existe)
## 8. Seguridad
## 9. Atributos de calidad y objetivos conocidos
## 10. Despliegue y operación
## 11. Decisiones arquitectónicas y trade-offs
## 12. Riesgos y preguntas abiertas
## 13. Documentos relacionados
```

## Database Design Document

```markdown
# Database Design Document — <sistema>

## 1. Propósito, alcance y motor conocido
## 2. Convenciones y terminología
## 3. Modelo conceptual
## 4. Modelo lógico y ERD
## 5. Modelo físico
### Tabla: <nombre>
| Columna | Tipo | Nulo | Default | Clave/restricción | Descripción |
|---|---|---|---|---|---|
## 6. Relaciones y cardinalidades
## 7. Restricciones e integridad de datos
## 8. Índices y consultas relevantes
## 9. Diccionario de datos
## 10. Auditoría, privacidad y retención (si aplica)
## 11. Transacciones y concurrencia (si aplica)
## 12. Migraciones y compatibilidad de cambios
## 13. Supuestos, riesgos y preguntas abiertas
## 14. Documentos relacionados
```

## Technical Design Document (para un cambio relevante)

```markdown
# Technical Design Document — <funcionalidad/cambio>

## 1. Resumen
## 2. Contexto y problema técnico
## 3. Requisitos y restricciones relevantes
## 4. Diseño propuesto
## 5. Componentes afectados y flujo
## 6. Cambios de API/contratos
## 7. Cambios en persistencia y migraciones
## 8. Errores, seguridad y observabilidad
## 9. Alternativas y trade-offs
## 10. Estrategia de pruebas
## 11. Despliegue, compatibilidad y rollback
## 12. Riesgos y preguntas abiertas
## 13. Documentos relacionados
```

## Architecture Decision Record

```markdown
# ADR <número>: <decisión>

- Estado: Proposed | Accepted | Rejected | Superseded
- Fecha: <fecha>
- Decisores: <personas/equipo, si se conoce>

## Contexto
## Opciones consideradas
## Decisión
## Consecuencias positivas y negativas
## Riesgos
## Referencias
```

## Revisión antes de entregar

- [ ] La estructura y cantidad de archivos son proporcionales al contexto y al riesgo.
- [ ] El documento declara propósito, alcance y estado.
- [ ] Los nombres y decisiones coinciden con la fuente canónica del proyecto.
- [ ] Supuestos, propuestas y preguntas abiertas están identificados.
- [ ] Hechos sobre código existente fueron verificados en archivos relevantes.
- [ ] No se duplicó el contenido completo de otra fuente de verdad.
- [ ] Los diagramas distinguen estado actual de diseño propuesto.
- [ ] Cuando aplica, los roles, permisos funcionales y restricciones de acceso están documentados en los requisitos y reflejados en criterios de aceptación/pruebas; la estrategia técnica está en el diseño correspondiente.
- [ ] Los enlaces apuntan a documentos existentes o se marcan como pendientes.
- [ ] Se omitieron las secciones y artefactos que no aportan valor.
