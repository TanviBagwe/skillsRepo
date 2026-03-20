# Parsing Reference — MD → Diagram Layers

## Layer detection rules

### Full-width band (most common)
Trigger: a `##` heading OR a numbered section like `1. Ingress layer`
Output: one horizontal band, full width (x=40, width=600)
Title: the heading text (sentence case, ≤40 chars — truncate if needed)
Subtitle: first sentence of the section body, OR key field names joined with ` · `

**Example MD:**
```
## OTel span context propagation
Attaches gql.hash, gql.operation, http.method=POST, and request_id to every span.
```
**Output band:**
- Title: `OTel span context propagation`
- Subtitle: `gql.hash · gql.operation · http.method=POST · request_id`

---

### Fan-out row (parallel subsystems)
Trigger: a section that contains a bullet list of 2–4 parallel items, OR
a heading that says "fan-out", "parallel", "pipelines", "split into", "outputs"
Output: 3 side-by-side boxes (or 2 if only 2 items)
Each box: title = bullet heading, subtitle lines = bullet sub-items (max 4 lines)

**Example MD:**
```
## Output pipelines
- **Access log** — existing pipeline, add gql_hash field, body omitted
- **Prometheus** — label: gql_hash=<slug>, latency histograms
- **OTel outbound** — attach gql.hash attr, traceparent header
```
**Output:** 3 fan-out boxes: Access log / Prometheus / OTel outbound

**2-item fan-out layout:** x=80/380, width=220 (centered pair)
**3-item fan-out layout:** x=40/251/462, width=178 (standard)
**4-item fan-out layout:** x=40/198/356/514, width=126 (compact)

---

### Storage row (backends / datastores)
Trigger: section mentions "storage", "backend", "store", "database", "Jaeger",
"Prometheus", "Loki", "ELK", "S3", "Redis", or similar concrete systems
Output: compact row of 2–3 boxes (height=52, not 66)
Title: system name, Subtitle: product name (e.g. "Jaeger / Tempo")

---

### Sub-boxes inside a band
Trigger: a section has a numbered list of internal steps (e.g. "1. normalize · 2. sort · 3. hash")
Output: the band renders at height=76 with 3 inner boxes at y=band_y+42, height=28
Inner boxes use c-blue nested inside the parent band color

---

### Correlation / join layer
Trigger: keywords: "correlation", "join", "same key", "link signals", "single source of truth"
Output: full-width teal band
Subtitle: the join key expression, e.g. `trace_id ↔ gql_hash ↔ log line ↔ Prom label`

---

### Dashboard / output layer
Trigger: keywords: "dashboard", "Grafana", "alerting", "visualization", "query", "output"
Output: full-width blue band at the bottom
Subtitle: metrics shown, e.g. `per-query latency p50/p99 · error rate · top slow queries`

---

## Arrow routing between layers

Between every two adjacent layers, draw a vertical arrow:
```svg
<line x1="340" y1="{prev_bottom}" x2="340" y2="{next_top}" class="arr" marker-end="url(#arrow)"/>
```

Between a full-width band and a fan-out row, draw a spread:
```svg
<!-- center point at bottom of band -->
<line x1="340" y1="{band_bottom}" x2="340" y2="{spread_y}" class="arr"/>
<!-- then 3 angled lines from spread_y to each box top -->
<line x1="{spread_x_left}"  y1="{spread_y}" x2="{box_a_cx}" y2="{fanout_top}" class="arr" marker-end="url(#arrow)"/>
<line x1="340"              y1="{spread_y}" x2="{box_b_cx}" y2="{fanout_top}" class="arr" marker-end="url(#arrow)"/>
<line x1="{spread_x_right}" y1="{spread_y}" x2="{box_c_cx}" y2="{fanout_top}" class="arr" marker-end="url(#arrow)"/>
```

---

## Subtitle truncation rules

Max chars for subtitle text at 12px:
- Full-width band (width=600): 90 chars
- Fan-out box (width=178): 28 chars per line, max 4 lines
- Storage box (width=188): 30 chars

If subtitle exceeds limit, split into multiple `<tspan>` lines spaced 18px apart,
OR truncate with `…` and move detail to prose below the diagram.

---

## Section → layer priority order

When a section could match multiple types, use this priority:
1. Fan-out (if it has a bullet list of 2-4 parallel items)
2. Storage row (if it names concrete backend systems)
3. Sub-boxes inside band (if it has a numbered step list)
4. Full-width band (default)

---

## Example: full MD → layer mapping

```markdown
# POST /graphql observability

## Ingress
Extract trace-id, canonical hash, request fingerprint.

## Canonical hash computation
1. Normalize GQL string
2. Sort variable keys
3. SHA-256 → 8-char slug

## OTel span context
Attach gql.hash, gql.operation, http.method=POST.

## Output pipelines
- **Access log** — gql_hash field, body omitted
- **Prometheus** — gql_hash label, latency histograms
- **OTel outbound** — gql.hash attr, traceparent

## Request-scoped debug
Store gql_hash in MDC/baggage, sampling decision.

## Storage backends
- Jaeger / Tempo
- Prometheus / Thanos
- Loki / ELK

## Correlation
trace_id ↔ gql_hash ↔ log line ↔ Prom label

## Dashboard
Grafana: per-query latency p50/p99, error rate, top slow queries
```

Maps to:
1. Full-width band (c-blue) — Ingress
2. Full-width band with sub-boxes (c-teal) — Canonical hash
3. Full-width band (c-purple) — OTel span context
4. Fan-out 3-col (c-coral / c-amber / c-green) — Output pipelines
5. Full-width band (c-gray) — Request-scoped debug
6. Storage row (c-purple / c-amber / c-coral) — Storage backends
7. Full-width band (c-teal) — Correlation
8. Full-width band (c-blue) — Dashboard
