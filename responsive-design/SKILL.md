---
name: responsive-design
description: "Ensure that web applications and websites are usable and visually appealing across a wide range of devices and screen sizes. Focus on techniques such as flexible layouts, media queries, and responsive images."
---

## Images

- Use `srcset` to serve images of different resolutions based on the device's screen size and pixel density.
- Use `<picture>` element to provide different image sources for different screen sizes or orientations.
- When using `<picture>`, ensure you match the pixel sizes with the actual breakpoints defined in CSS.
- When using `<picture>`, ensure that you add widths and heights to the `<img>` and `<source>` elements to prevent layout shifts.
