# Layout and Token Contract

Use this contract during implementation and review.

## Layout Baseline

- Use `grid grid-cols-12 gap-(--grid-gap)` for primary layout grids.
- Start from desktop structure, then adapt for tablet/mobile while preserving intent.
- Keep section wrappers explicit so spacing and background layers stay predictable.

## Background Rules

- Set explicit background styling for page root and each major section.
- Do not rely on browser default background.
- Preserve alternating or layered section backgrounds when shown in the mockup.

## Color Rules

- Prefer project Tailwind tokens/classes for text, surfaces, borders, and accents.
- Use direct color literals only when no token exists and fidelity requires it.
- Keep contrast and hierarchy consistent with the mockup.

## Spacing and Alignment

- Match margin/padding rhythm across peer components.
- Match vertical cadence between headings, copy, controls, and cards.
- Preserve alignment strategy (left edge alignment, centered blocks, baseline rhythm).

## Typography

- Preserve heading/body/caption hierarchy.
- Match apparent weight and emphasis patterns.
- Keep line length and paragraph rhythm close to mockup intent.
