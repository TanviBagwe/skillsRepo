# SVG Primitives Reference

Copy-paste patterns for every element type in an architecture diagram.
Replace {PLACEHOLDERS} with computed values.

---

## SVG shell

```svg
<svg width="100%" viewBox="0 0 680 {H}" xmlns="http://www.w3.org/2000/svg">
<defs>
  <marker id="arrow" viewBox="0 0 10 10" refX="8" refY="5"
          markerWidth="6" markerHeight="6" orient="auto-start-reverse">
    <path d="M2 1L8 5L2 9" fill="none" stroke="context-stroke"
          stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round"/>
  </marker>
</defs>
```

---

## Full-width band (single line title + subtitle)

Height: 56px. Use when band has a title and one subtitle line.

```svg
<g class="c-{ramp}">
  <rect x="40" y="{Y}" width="600" height="56" rx="10" stroke-width="0.5"/>
  <text class="th" x="340" y="{Y+22}" text-anchor="middle" dominant-baseline="central">{Title}</text>
  <text class="ts" x="340" y="{Y+40}" text-anchor="middle" dominant-baseline="central">{subtitle · fields · here}</text>
</g>
```

---

## Full-width band with sub-boxes (3 internal step boxes)

Band height: 76px. Sub-boxes start at Y+42, height=28.

```svg
<g class="c-{ramp}">
  <rect x="40" y="{Y}" width="600" height="76" rx="10" stroke-width="0.5"/>
  <text class="th" x="340" y="{Y+20}" text-anchor="middle" dominant-baseline="central">{Band title}</text>
</g>
<g class="c-blue">
  <rect x="56" y="{Y+42}" width="170" height="28" rx="5" stroke-width="0.5"/>
  <text class="ts" x="141" y="{Y+57}" text-anchor="middle" dominant-baseline="central">{Step 1 label}</text>
</g>
<g class="c-blue">
  <rect x="254" y="{Y+42}" width="172" height="28" rx="5" stroke-width="0.5"/>
  <text class="ts" x="340" y="{Y+57}" text-anchor="middle" dominant-baseline="central">{Step 2 label}</text>
</g>
<g class="c-blue">
  <rect x="454" y="{Y+42}" width="170" height="28" rx="5" stroke-width="0.5"/>
  <text class="ts" x="539" y="{Y+57}" text-anchor="middle" dominant-baseline="central">{Step 3 label}</text>
</g>
```

---

## Vertical arrow (between layers)

```svg
<line x1="340" y1="{prev_band_bottom}" x2="340" y2="{next_band_top}"
      class="arr" marker-end="url(#arrow)"/>
```

Gap between layers: 26px total (6px gap + 20px arrow line).
prev_band_bottom = Y + band_height
next_band_top = Y + band_height + 26

---

## Fan-out: arrow spread into 3 boxes

Place a spread point 14px below the source band bottom.
Then draw 3 angled lines from the spread point to each box top-center.

```svg
<!-- Spread: horizontal line from left-box-cx to right-box-cx -->
<line x1="129" y1="{spread_y}" x2="551" y2="{spread_y}" class="arr"/>
<!-- Left arrow -->
<line x1="129" y1="{spread_y}" x2="129" y2="{fanout_top}"
      class="arr" marker-end="url(#arrow)"/>
<!-- Center arrow -->
<line x1="340" y1="{spread_y}" x2="340" y2="{fanout_top}"
      class="arr" marker-end="url(#arrow)"/>
<!-- Right arrow -->
<line x1="551" y1="{spread_y}" x2="551" y2="{fanout_top}"
      class="arr" marker-end="url(#arrow)"/>
```

cx values for 3-col fan-out: left=129, center=340, right=551

---

## 3-column fan-out boxes

Each box: width=178, height=130 (adjust if more/fewer subtitle lines).
Subtitle lines spaced 20px apart starting at title_y + 26.

```svg
<!-- Box A -->
<g class="c-{ramp_a}">
  <rect x="40" y="{FY}" width="178" height="130" rx="10" stroke-width="0.5"/>
  <text class="th" x="129" y="{FY+22}" text-anchor="middle" dominant-baseline="central">{Box A title}</text>
  <text class="ts" x="129" y="{FY+44}" text-anchor="middle" dominant-baseline="central">{line 1}</text>
  <text class="ts" x="129" y="{FY+62}" text-anchor="middle" dominant-baseline="central">{line 2}</text>
  <text class="ts" x="129" y="{FY+80}" text-anchor="middle" dominant-baseline="central">{line 3}</text>
  <text class="ts" x="129" y="{FY+98}" text-anchor="middle" dominant-baseline="central">{line 4}</text>
</g>
<!-- Box B (center) -->
<g class="c-{ramp_b}">
  <rect x="251" y="{FY}" width="178" height="130" rx="10" stroke-width="0.5"/>
  <text class="th" x="340" y="{FY+22}" text-anchor="middle" dominant-baseline="central">{Box B title}</text>
  <text class="ts" x="340" y="{FY+44}" text-anchor="middle" dominant-baseline="central">{line 1}</text>
</g>
<!-- Box C (right) -->
<g class="c-{ramp_c}">
  <rect x="462" y="{FY}" width="178" height="130" rx="10" stroke-width="0.5"/>
  <text class="th" x="551" y="{FY+22}" text-anchor="middle" dominant-baseline="central">{Box C title}</text>
  <text class="ts" x="551" y="{FY+44}" text-anchor="middle" dominant-baseline="central">{line 1}</text>
</g>
```

---

## Storage row (3 compact boxes)

Height: 52px each.

```svg
<g class="c-{ramp_1}">
  <rect x="40" y="{SY}" width="188" height="52" rx="8" stroke-width="0.5"/>
  <text class="th" x="134" y="{SY+18}" text-anchor="middle" dominant-baseline="central">{Store 1 name}</text>
  <text class="ts" x="134" y="{SY+36}" text-anchor="middle" dominant-baseline="central">{product names}</text>
</g>
<g class="c-{ramp_2}">
  <rect x="246" y="{SY}" width="188" height="52" rx="8" stroke-width="0.5"/>
  <text class="th" x="340" y="{SY+18}" text-anchor="middle" dominant-baseline="central">{Store 2 name}</text>
  <text class="ts" x="340" y="{SY+36}" text-anchor="middle" dominant-baseline="central">{product names}</text>
</g>
<g class="c-{ramp_3}">
  <rect x="452" y="{SY}" width="188" height="52" rx="8" stroke-width="0.5"/>
  <text class="th" x="546" y="{SY+18}" text-anchor="middle" dominant-baseline="central">{Store 3 name}</text>
  <text class="ts" x="546" y="{SY+36}" text-anchor="middle" dominant-baseline="central">{product names}</text>
</g>
```

---

## Fan-out → storage arrows (3 down-arrows after fan-out boxes)

```svg
<line x1="129" y1="{fanout_bottom}" x2="134" y2="{storage_top}"
      class="arr" marker-end="url(#arrow)"/>
<line x1="340" y1="{fanout_bottom}" x2="340" y2="{storage_top}"
      class="arr" marker-end="url(#arrow)"/>
<line x1="551" y1="{fanout_bottom}" x2="546" y2="{storage_top}"
      class="arr" marker-end="url(#arrow)"/>
```

---

## Storage → next layer (3 arrows converging to single band)

Route all 3 through a single center point to avoid crossing other boxes:

```svg
<line x1="134" y1="{storage_bottom}" x2="134" y2="{mid_y}"
      class="arr"/>
<line x1="340" y1="{storage_bottom}" x2="340" y2="{mid_y}"
      class="arr"/>
<line x1="546" y1="{storage_bottom}" x2="546" y2="{mid_y}"
      class="arr"/>
<line x1="134" y1="{mid_y}" x2="340" y2="{mid_y}" class="arr"/>
<line x1="546" y1="{mid_y}" x2="340" y2="{mid_y}" class="arr"/>
<line x1="340" y1="{mid_y}" x2="340" y2="{next_top}"
      class="arr" marker-end="url(#arrow)"/>
```

mid_y = storage_bottom + 14

---

## Legend row

Place at y = H - 40. Space entries 110px apart from x=40.

```svg
<rect x="40"  y="{LY}" width="12" height="12" rx="2" fill="#E6F1FB" stroke="#185FA5" stroke-width="0.5"/>
<text class="ts" x="58"  y="{LY+9}">{label 1}</text>
<rect x="150" y="{LY}" width="12" height="12" rx="2" fill="#E1F5EE" stroke="#0F6E56" stroke-width="0.5"/>
<text class="ts" x="168" y="{LY+9}">{label 2}</text>
<rect x="260" y="{LY}" width="12" height="12" rx="2" fill="#FAEEDA" stroke="#854F0B" stroke-width="0.5"/>
<text class="ts" x="278" y="{LY+9}">{label 3}</text>
<rect x="370" y="{LY}" width="12" height="12" rx="2" fill="#FAECE7" stroke="#993C1D" stroke-width="0.5"/>
<text class="ts" x="388" y="{LY+9}">{label 4}</text>
<rect x="480" y="{LY}" width="12" height="12" rx="2" fill="#EAF3DE" stroke="#3B6D11" stroke-width="0.5"/>
<text class="ts" x="498" y="{LY+9}">{label 5}</text>
```

---

## Coordinate cheat sheet

| Layout element       | x      | cx  | width | notes                     |
|----------------------|--------|-----|-------|---------------------------|
| Full-width band      | 40     | 340 | 600   | rx=10                     |
| Fan-out left box     | 40     | 129 | 178   | rx=10                     |
| Fan-out center box   | 251    | 340 | 178   | rx=10                     |
| Fan-out right box    | 462    | 551 | 178   | rx=10                     |
| Storage left box     | 40     | 134 | 188   | rx=8                      |
| Storage center box   | 246    | 340 | 188   | rx=8                      |
| Storage right box    | 452    | 546 | 188   | rx=8                      |
| Sub-box left         | 56     | 141 | 170   | rx=5, inside parent band  |
| Sub-box center       | 254    | 340 | 172   | rx=5, inside parent band  |
| Sub-box right        | 454    | 539 | 170   | rx=5, inside parent band  |

## Standard heights

| Element                    | Height |
|----------------------------|--------|
| Full-width band (1 line)   | 44px   |
| Full-width band (2 lines)  | 56px   |
| Full-width band + sub-boxes| 76px   |
| Fan-out box (4 lines)      | 130px  |
| Fan-out box (3 lines)      | 108px  |
| Storage box                | 52px   |
| Arrow gap between layers   | 26px   |
| Legend row                 | 30px   |
