---
name: gpui-semi-skeuomorphic-ui
description: >-
  Design or implement restrained semi-skeuomorphic UI for native GPUI
  desktop apps: glossy buttons, recessed segmented controls,
  pointer-following highlight cards, layered panels, light and dark
  themes, and accessibility and performance acceptance. Use when adding
  depth to native GPUI controls or checking motion, clipping, contrast,
  and paint cost. Read references only for the work in scope.
---

# GPUI native desktop semi-skeuomorphic UI

## Purpose

Keep a modern desktop layout. Add depth with restrained highlights,
gradients, contact shadows, and press feedback. Do not simulate heavy
materials, and do not use a 3D exploded view as a default interaction.

Four effects:

- Glossy button with a thin highlight along the top
- Raised segment that slides inside a recessed track
- Card highlight along the top edge that follows the pointer
- Layering of text, data bars, panels, and the surrounding tray

[egoist/tinytraffic](https://github.com/egoist/tinytraffic) illustrates
these mechanisms only. It is not a GPUI API, a measured size, or a
rendering test. This skill is not tied to one app, and it does not
cover video playback or decoding.

## Boundaries

- Build in native GPUI. Do not substitute HTML, CSS, or a WebView.
- Do not invent Rust or GPUI APIs. This skill has no API samples.
  Extend a call this repository already uses.
- Numbers in [references/tokens.md](references/tokens.md) are tunable
  starting points, not measurements and not cross-platform constants.
- A missing capability is a reason to degrade to flat layers, hairlines,
  and state colors, and to say so. It is not a reason to guess a
  signature or switch to a WebView.

## What to read

Read this file before changing visuals. Then only the files the task
needs:

- Color, radius, shadow, or motion → [references/tokens.md](references/tokens.md)
- A control in scope → the matching section of [references/components.md](references/components.md)
- Theme, scale, clipping, or paint cost → [references/theme-and-performance.md](references/theme-and-performance.md)
- Before calling the work done → [references/acceptance.md](references/acceptance.md)
- A spec with no code → [examples/preview-spec.md](examples/preview-spec.md)

A one-control change still follows Boundaries, the interaction rules
below, the capability check, and acceptance.

## Workflow

1. From the request and the existing UI, fix the target components,
   platform, brand color, themes, accessibility settings, and screenshot
   baseline.
2. Verify the GPUI version, local patches, render backend, component
   library, and theme. Record whether gradients, shadows, clipping,
   corner radius, pointer input, focus, and animation exist here, and
   which calls the project already uses.
3. Prefer extending an existing native component. Use a custom paint
   path only after this project has shown that path works.
4. Put color, elevation, radius, spacing, and motion in shared tokens.
   Do not hardcode colors inside components.
5. Build a native preview of the controls in scope. Reuse it. Do not
   rewrite the surrounding layout to introduce the treatment.
6. Run the acceptance checks. Repair contrast, input, clipping, and
   performance before adding more decoration.

## Interaction rules

Interactive controls cover `default`, `hover`, `pressed`,
`focus-visible`, and `disabled`. Add `selected` when the control has a
selection.

- Disabled does not activate and has no moving highlight. Read-only,
  disabled, and selected stay distinct.
- A button activates from the keyboard. A segmented control changes
  from the keyboard, using this platform's Tab, arrow-key, and
  activation semantics.
- Focus is visible by itself. A shadow or a small color shift is not
  enough. The focus indicator does not follow the pointer spot, and the
  control's rounded clip does not cut the focus ring.
- Reduced motion: the thumb arrives immediately, metric cards keep a
  static top highlight, and buttons change color with no shift.
- With no pointer, skip the dynamic highlight. The control still works.
- Any pressed offset is paint-only and must not change layout.
- Use the same light direction, and the same meaning for each state, in
  light and dark. A dark theme is not an inversion of the light theme.

## Labels

| Label | Means |
| --- | --- |
| Specified effect | One of the four effects above |
| Suggested parameter | A starting value from the token reference, not a measurement |
| Verified API | An API found in the target repository, with its source location |
| Planned approach | Proposed work or a fallback, not yet implemented |
| Implemented | Behavior present in the target codebase, with its source location |
| Unverified | Not yet checked on a device, scale, or theme |

An external demo is not a GPUI measurement.
