# Exemplar — the quality bar

Below is a **condensed real note** (*Streams*), trimmed to keep only what carries the quality. It is not a template to copy — it is the **bar to match**.

A real note expands every section. This keeps the shape and the tone.

---

## Frontmatter (English, every key filled)

```
type: tech-note
status: active
domain: java
version: "21+"
tags: [java, streams, collectors, java21]
created: "2026-09-12"
updated: "2026-09-12"
source: https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html
```

## TL;DR

Un Stream es un pipeline declarativo y perezoso sobre datos. Describes lo que quieres y él se encarga del recorrido.

## La idea

**Un Stream es una línea de ensamblaje, no un bucle.**

Tú montas las estaciones (`filter`, `map`, `sorted`) y al final das la orden de arrancar (la operación terminal). De esa imagen salen tres consecuencias que explican casi todo:

1. **Nada se mueve hasta la orden final.** Las estaciones están montadas, pero la cinta no anda hasta que llega `collect`, `toList` o `forEach`.
2. **Pieza por pieza, no estación por estación.** Cada elemento atraviesa todas las estaciones antes de que entre el siguiente. Por eso `findFirst` puede detener la línea entera.
3. **Las estaciones son puras y de un solo uso.** Reciben un elemento y devuelven otro, sin estado compartido. Por eso **reusar un stream está prohibido**.

## Referencia (abreviada)

| Operación | Qué hace | Desde |
|---|---|---|
| `map(f)` | Transforma cada elemento | 8 |
| `flatMap(f)` | Aplana: 1 → N | 8 |
| `gather(g)` | Intermedias custom (ventanas, scans) | 24 |

## Ejemplo trabajado

**El problema.** Top 3 de clientes por facturación de los últimos 30 días, ignorando los cancelados.

**Paso 1 — pensar en transformaciones, no en pasos:**

| Lo que quiero | Operación |
|---|---|
| Quedarme con algunos pedidos | `filter` |
| Agrupar por cliente | `groupingBy` |
| Sumar el monto de cada grupo | `summingDouble` |

**Ese cuadro es el trabajo real.** El código de abajo es solo la traducción.

**Paso 2 — el pipeline:**

```java
pedidos.stream()
    .filter(p -> !p.cancelado())
    .filter(p -> p.fecha().isAfter(hace30Dias))
    .collect(Collectors.groupingBy(
        Pedido::cliente,
        Collectors.summingDouble(Pedido::monto)))
    .entrySet().stream()
    .sorted(Map.Entry.<String, Double>comparingByValue().reversed())
    .limit(3)
    .map(Map.Entry::getKey)
    .toList();
```

**La variación (donde se aprende).** Cambiar la suma por un conteo es cambiar **una estación** — el collector — no reescribir la lógica:

```java
.collect(Collectors.groupingBy(Pedido::cliente, Collectors.counting()))
```

## Errores comunes (muestra de 8)

- **Reusar un stream.** Lanza `IllegalStateException`. La fuente (la lista) sí se reutiliza; el stream no.
- **`parallelStream` con estado compartido.** Da resultados incorrectos **en silencio**.

## Pruébate (muestra de 7)

> [!question]- La lista tiene 1000 elementos y el que matchea es el 3.º. ¿Cuántas veces se ejecuta `f`?
> Tres. Si respondiste 1000, seguiste pensando en un bucle.

## Referencias

- [java.util.stream — API Java 21](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/stream/package-summary.html)

## Notas relacionadas

- [[Lambdas]]
- [[Colecciones]]
- [[Java]]

---

## Why this is the bar

1. **"La idea" predicts the rules.** An assembly line *implies* laziness, element-by-element flow, and single-use. The model generates the rules instead of listing them.
2. **The worked example shows the thinking.** Step 1 is a table of transformations — the reasoning — before any code. Anyone can copy the code; the table is what teaches.
3. **The variation proves the model.** Changing one requirement changes one stage. That is how you know the reader understood.
4. **Pruébate questions catch wrong models.** "How many times does `f` run?" separates readers who understood laziness from those still thinking in loops.
5. **Versions are labeled everywhere.** Every operation carries the version it appeared in — never written from memory.
