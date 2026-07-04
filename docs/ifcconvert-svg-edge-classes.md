# IfcConvert SVG edge classes

This document describes SVG edge-classification options for IfcConvert linework output.

## Overview

When generating SVG drawings, projection edges can be classified into CSS classes:

- `contour`
- `crease`
- `sharp`
- `hidden`

This allows you to style linework differently in downstream SVG/CSS workflows.

## Command-line options

Use these options with IfcConvert:

- `--svg-crease-threshold-deg <float>`
  - Edges with face-angle below this value (degrees) are treated as `crease`.
- `--svg-sharp-threshold-deg <float>`
  - Edges with face-angle above this value (degrees) are treated as `sharp`.
- `--svg-emit-hidden-edges`
  - Emit `hidden` edges into the SVG (otherwise hidden edges are omitted).

## Example

```bash
IfcConvert model.ifc drawing.svg \
  --svg-crease-threshold-deg 12 \
  --svg-sharp-threshold-deg 45 \
  --svg-emit-hidden-edges
```

## CSS example

```css
.projection.contour path,
.contour path {
  stroke: #222;
  stroke-opacity: 0.9;
  fill: none;
}

.projection.crease path,
.crease path {
  stroke: #555;
  stroke-opacity: 0.7;
  fill: none;
}

.projection.sharp path,
.sharp path {
  stroke: #000;
  stroke-opacity: 1;
  fill: none;
}

.projection.hidden path,
.hidden path {
  display: none; /* or style as dashed, faint, etc. */
}
```

## Notes

- Threshold values are in degrees.
- If `crease` threshold is greater than `sharp` threshold, implementations may normalize/swap values.
- Non-manifold and fallback edge cases may be classified as `contour` by default.
