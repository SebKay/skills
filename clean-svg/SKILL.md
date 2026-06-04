---
name: clean-svg
description: Clean exported SVG files so they are lightweight, theme-ready, and easier to style. Use when Codex needs to simplify SVG markup by removing IDs, data-name attributes, XML/export metadata, and unnecessary wrapper groups, or add fill="currentColor" when appropriate, while preserving the SVG's visual output, dimensions, and viewBox.
---

# Clean SVG

Clean SVG files surgically so they keep the same visual output with less export noise and better CSS styling behavior.

## Workflow

1. Inspect nearby cleaned SVGs first.
   - Compare against similar files in the same directory.
   - Match local formatting, attribute order, indentation, and whether the XML header is kept or removed.

2. Clean only unnecessary export markup.
   - Remove XML declarations when nearby cleaned SVGs omit them.
   - Remove `id` attributes.
   - Remove `data-name` attributes.
   - Remove empty or purely wrapping `<g>` elements.
   - Move child paths directly under the root `<svg>` when the wrapper has no meaningful transform, style, mask, clip-path, opacity, or accessibility role.

3. Add color inheritance.
   - Add `fill="currentColor"` to the root `<svg>` when the SVG is a single-color filled graphic that should inherit text color.
   - Do not add root `fill` if the SVG uses intentional multicolor fills, gradients, masks, or stroke-only styling.
   - Preserve existing `stroke`, `fill-rule`, `clip-rule`, `aria-*`, `role`, `width`, `height`, and `viewBox` attributes when they affect rendering or accessibility.

4. Preserve rendering.
   - Do not edit path `d` data unless explicitly asked.
   - Do not change dimensions or `viewBox`.
   - Do not flatten groups that contain meaningful attributes such as `transform`, `clip-path`, `mask`, `filter`, `opacity`, or shared stroke/fill styles unless those styles are safely moved to children.

5. Verify the result.
   - Re-open the SVG after editing.
   - Check that no `id=` or `data-name=` remains unless required by a referenced gradient, mask, clip path, or filter.
   - Check that unnecessary wrapper groups are gone.
   - Check that the root `<svg>` includes `fill="currentColor"` when appropriate.
   - Review the diff to confirm only the requested cleanup changed.

## Simple Example

Before:

```svg
<?xml version="1.0" encoding="UTF-8"?>
<svg id="uuid-example" data-name="Layer 2" xmlns="http://www.w3.org/2000/svg" width="16" height="12" viewBox="0 0 16 12">
  <g id="uuid-layer" data-name="Layer 1">
    <path d="..."/>
  </g>
</svg>
```

After:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="16" height="12" viewBox="0 0 16 12" fill="currentColor">
  <path d="..."/>
</svg>
```

## Guardrails

- Keep changes minimal and SVG-specific.
- Do not run broad SVG optimization across unrelated files unless requested.
- Do not remove IDs that are referenced by `url(#...)`.
- Do not remove groups that affect rendering.
- If the SVG appears complex, inspect references before flattening.
