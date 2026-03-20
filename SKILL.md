---
name: md-to-diagram
description: Converts any Markdown file or architecture description into a layered SVG architecture diagram rendered inline using the Visualizer tool. Use this skill whenever the user wants to visualize a system, pipeline, architecture, or flow from a .md file, a text description, or any structured document — even if they just say "draw this", "diagram this", "visualize my architecture", "turn this doc into a diagram", or "make a diagram from this file". Also triggers for observability designs, API pipeline docs, service maps, data flow descriptions, or any multi-layer system where a visual would help. Always use this skill proactively when an .md file is provided alongside a request to diagram or visualize.
---

# MD → Architecture Diagram Skill

Converts a Markdown file or freeform architecture description into a layered
SVG diagram rendered inline via the Visualizer tool. No external libraries.
No artifacts. Inline, interactive, dark-mode-safe SVG.

## Step 0 — Read the input

If the user provides a file path, read it with the `view` tool first.
If the content is already in the conversation, use it directly.

## Step 1 — Parse the MD into diagram layers

Read `references/parsing.md` for the full parsing ruleset.

Quick summary of what to extract:
- **Layers** — each top-level section (`##`) or numbered stage becomes one full-width horizontal band
- **Fan-out groups** — parallel subsystems (bullet lists, side-by-side boxes, "A / B / C") become a 3-column fan-out row
- **Storage row** — any section mentioning databases, backends, stores becomes a compact 3-box storage row
- **Correlation/join layer** — any section about "correlation", "join", "linking signals", "single key" becomes a teal band
- **Output/dashboard layer** — any section about dashboards, alerting, Grafana, output becomes a blue bottom band

## Step 2 — Assign colors

Read `references/colors.md` for the full color → meaning mapping.

Quick reference:

| Layer type                        | c-ramp   |
|-----------------------------------|----------|
| Ingress, API gateway, entry       | c-blue   |
| Hash, canonicalization, context   | c-teal   |
| Metrics, Prometheus, storage      | c-amber  |
| Access log, log pipeline          | c-coral  |
| Tracing, OTel, outbound spans     | c-green  |
| Debug, neutral, request scope     | c-gray   |
| Trace backend, span store         | c-purple |
| Auth, identity                    | c-pink   |
| Errors, alerts                    | c-red    |
| Dashboard, output, query layer    | c-blue   |

If the doc doesn't map cleanly, alternate: c-blue → c-teal → c-gray → c-purple for sequential layers.

## Step 3 — Compute the viewBox height

```
H = 40                          ← top padding
  + (num_full_bands × 66)       ← each full-width layer band
  + (num_fanout_rows × 156)     ← each fan-out section
  + (num_storage_rows × 78)     ← each storage row
  + (num_layers × 26)           ← arrow + gap between each layer
  + 50                          ← legend row + bottom padding
```

Round up to the nearest 20px.

## Step 4 — Render via Visualizer

Read `references/svg_primitives.md` for the exact SVG patterns to use.

Call `visualize:read_me` with `["diagram"]` before the first Visualizer call.
Then call `visualize:show_widget` with the assembled SVG.

**Hard rules for the SVG:**
- `viewBox="0 0 680 H"` — never change the 680
- Full-width bands: `x=40, width=600, rx=10, stroke-width=0.5`
- Fan-out 3-col: `x=40/251/462, width=178, rx=10`
- Storage 3-col: `x=40/246/452, width=188, rx=8`
- Always include `<defs>` arrow marker at the top
- Titles: `class="th"` + `dominant-baseline="central"` + `text-anchor="middle"`
- Subtitles: `class="ts"` + same alignment
- Arrows: `<line class="arr" marker-end="url(#arrow)"/>`
- Fan-out spread: horizontal line at mid-y, then 3 diagonal lines to box tops
- Never let an arrow path cross an unrelated box — use L-shaped detours
- Add a legend row at the bottom (12×12 colored rects + ts labels)
- All `c-{ramp}` color classes handle dark mode automatically — never hardcode hex

## Step 5 — Add prose context

After the diagram, write a short section (3–6 sentences) explaining:
- What the diagram shows
- The key correlation key or linking mechanism
- Any layer that needs special attention

Keep it concise — the diagram does the heavy lifting.

---

## Edge cases

**Single-section MD (no clear layers):** Treat top-level bullet groups as layers.
Wrap in a single structural overview diagram with 3–4 bands max.

**Very large MD (10+ sections):** Split into two diagrams:
1. Overview — 4–5 top-level layers only
2. Detail — zoom into the most complex subsection

**No clear fan-out:** Use only full-width bands. Skip fan-out entirely.

**Already has a diagram in the MD:** Still generate the SVG.
The inline visual is more useful than ASCII art or Mermaid source.

---

## Reference files

- `references/parsing.md` — full MD → layers parsing rules with examples
- `references/colors.md` — complete color assignment logic
- `references/svg_primitives.md` — copy-paste SVG patterns for every element type
