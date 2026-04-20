# groovy-ops

Groovy scripting and template rendering for Ops4J.

This is an optional plugin module. Add it to a project that already depends on `ops4j-core` to enable dynamic Groovy expressions and Groovy template-based output inside pipelines. It is also used internally by `http-ops` and `visual-ops`.

---

## Overview

`groovy-ops` lets you embed Groovy logic directly in an Ops4J pipeline — either as a NodeOp expression applied field-by-field, or as a full Groovy template that collects a stream of records and renders them as a single document.

---

## Operations

### Pipeline Operations (Ops)

| Operation | Description |
| --- | --- |
| `groovy-template` (`GroovyTemplate`) | Accumulates the full record stream into a context variable, then renders a Groovy template (`.gt` file or inline string) against that context and emits the rendered output as a single record. Used heavily by `http-ops` for HTML dashboard generation. |

### Field-Level Operations (NodeOps)

| NodeOp | Description |
| --- | --- |
| `eval` (`Eval`) | Evaluates a Groovy expression against the current record. The expression has access to the record's fields as bindings and to standard `Math` functions. The result is written back into the target field. |

---

## Usage Examples

Compute a field value dynamically:

```bash
map /bmi="eval:weight / (height * height):"
```

Render a Groovy template over an accumulated stream and write output to a file:

```bash
cat data.json | groovy-template -t report.gt > report.html
```

Use a Groovy expression with math functions:

```bash
map /radians="eval:Math.toRadians(degrees):"
```

---

## Configuration

`groovy-ops` registers itself automatically. The default `groovy.conf` sets the execution mode:

```hocon
groovy {
  mode = LOCAL
}
```

---

## Used By

- [http-ops](../http-ops/) — uses `groovy-template` to render HTML and chart pages.
- [visual-ops](../visual-ops/) — uses `groovy-template` to render PlantUML diagram sources.

---

## Key Dependencies

| Dependency | Purpose |
| --- | --- |
| [ops4j-core](../ops4j-core/) | Ops4J runtime and base classes |
| Apache Groovy 4 | Groovy language runtime and template engine |
