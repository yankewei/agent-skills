---
name: gpui-semi-skeuomorphic-ui
description: >-
  Use when designing or implementing restrained semi-skeuomorphic UI for
  native GPUI desktop apps: glossy buttons, recessed segmented controls,
  pointer-following highlight cards, layered panels, light and dark themes,
  and accessibility and performance acceptance.
---

# GPUI native desktop semi-skeuomorphic UI

## Goal and sources

Keep the clear layout of a modern desktop interface. Add depth with
restrained highlights, gradients, contact shadows, and press feedback.
Do not simulate heavy materials.

The visual mechanisms come from:

- [localhost_4173 on X](https://x.com/localhost_4173/status/2120436166373908589)
- [egoist/tinytraffic](https://github.com/egoist/tinytraffic)

The reference video shows four effects:

- A glossy button with a thin highlight along the top
- A raised segment that slides smoothly inside a recessed track
- A card highlight along the top edge that follows the pointer
- Layering of text, data bars, panels, and the surrounding tray

The 3D exploded view is a demonstration only. It is not a default
production interaction.

References define visual mechanisms only. They do not prove a GPUI API,
a GPUI feature, or a measured size. This skill is general: it is not
tied to any one app, including Herdr, and it does not cover video
playback or decoding.

## Boundaries

- Build these effects in native GPUI. Do not substitute HTML, CSS, or a
  WebView.
- Do not invent Rust or GPUI APIs, and do not treat CSS as GPUI code.
  This skill has no API samples, because an unverified signature would
  be a fabrication.
- The numbers below are tunable starting points. They are not
  measurements from the video and they are not cross-platform constants.
- Do not present the reference video as a GPUI rendering test.
- Using this skill does not authorize further repository edits, pull
  requests, or cloning the reference repositories. Follow the target
  project's own contribution workflow.
- Decide anything the request, the repository, or the existing UI
  already implies. Ask only when the ambiguity is material.

## What to read

Read **Goal and sources**, **Boundaries**, and **§1** before changing
visuals. Read **§2** before picking color or motion. Read only the
**§3** components in scope. Read **§4–§8** before calling the work
done. The cautions in **§1**, **§6**, and **§8** still apply to a
one-control change.

## 1. Set the scope and check capabilities

From the request, the repository, and the existing interface, determine:

- Target components
- Platform
- Brand color
- Light and dark themes
- Accessibility settings
- Screenshot baseline

Then verify what this tree actually ships:

- GPUI version, local patches, and render backend
- Component library and theme
- Whether gradients, shadows, clipping, corner radius, pointer input,
  focus, and animation exist here, and which calls the project already
  uses

Record that inventory. A missing call is a reason to degrade, not a
reason to guess a signature.

Prefer extending an existing native component. Use a small custom paint
path only after that path has been verified in this project. If this
version lacks a capability, fall back to flat color layers, hairlines,
and state colors, and state the difference. Do not switch to a WebView
to fill the gap.

## 2. Semantic tokens

Put color, elevation, radius, spacing, and motion in shared tokens. At
minimum:

`canvas`, `surface`, `surface-raised`, `surface-recessed`,
`edge-highlight`, `edge-contact`, `text-primary`, `text-secondary`,
`accent`, `focus-ring`, `disabled`, `shadow`, `radius`, `spacing`,
`motion`

Do not scatter hardcoded colors inside components.

Suggested starting points, to be tuned:

| Concern | Starting point |
| --- | --- |
| Control height | 28–36 logical pixels |
| Card padding | 12–16 |
| Spacing | 8–12 |
| Corner radius | 8–12 |
| Top highlight | about 1 physical pixel, aligned to the scale factor |
| Resting shadow | offset down by 1, blur 2 |
| Raised shadow | may use offset 2, blur 6 |
| Light-theme shadow opacity | 0.08–0.12 |
| Dark-theme shadow | tune independently |
| Segment travel | 120–180 ms |
| Press feedback | within 80 ms |

Keep logical pixels and physical pixels distinct. The thin highlight is
in physical pixels and should snap to the device scale. Map shadow
offset, blur, and opacity through the native shadow API you verified,
and adjust them there.

Motion durations stay configurable. Honor reduced motion (§4).

Contrast: ordinary text at least 4.5:1, large text at least 3:1.
Control boundaries and focus stay distinguishable. Disabled text stays
readable.

## 3. Four components

### Glossy button

The brand color does not have to be blue.

- Gentle vertical gradient: lighter on top, darker on the bottom
- Thin highlight on the top edge
- Contact shadow along the bottom
- Hover: slightly brighter
- Pressed: darker, with a weaker highlight. An optional downward shift
  of about 1 logical pixel is paint-only and must not change layout
- Pointer release, pointer leave, and cancel all restore the resting
  look
- The focus ring is its own shape. The button's rounded clip does not
  cut it

### Recessed segmented control

A darker recessed track holds a brighter raised thumb.

- Inner gap between thumb and track: 2–3 logical pixels
- Hit targets stay fixed while the thumb moves
- A new choice sends the thumb to the latest target only. Do not stack
  animations
- Selected and focus-visible may show at the same time
- Reuse this platform's Tab, arrow-key, and activation semantics for a
  segmented control

### Metric card

Keep the title and the value distinct. The moving highlight is a top
edge decoration.

Coordinate rule, not a GPUI API: with the pointer in the card's local
coordinates, `u = clamp((x - left) / width, 0, 1)`. Map `u` along the
top edge after insetting by the corner radius and the padding. If the
width is zero, do not divide; use the static top highlight.

- Limit the spot's width and brightness
- Only the card under the pointer follows it. On leave, that card
  returns to a static top highlight
- Invalidate the previous card and the current card only when that
  change needs a redraw
- A non-clickable card does not resemble a button and does not add a
  tab stop

### Layered panel

Separate text, proportion bars, the foreground plate, and the base by
luminance steps, contact shadow, and hairlines. The surrounding tray in
the reference is this base, not a separate 3D shell. Do not use a heavy
outline.

- Proportion bars do not cover text
- Clip the content. Do not use that same clip on the outer shadow or
  the focus ring
- Scrolling content stays inside the rounded shape
- Do not sink the whole card, and do not rotate it in 3D, as the
  default interaction

## 4. States and accessibility

Interactive components cover `default`, `hover`, `pressed`,
`focus-visible`, and `disabled`. Add `selected` when the control has a
selection.

- Disabled does not activate and has no moving highlight. Focus
  behavior follows that control's own semantics
- Read-only, disabled, and selected stay distinct
- A button can be activated from the keyboard. The segmented control
  can be changed from the keyboard
- Focus is visible by itself. A shadow or a small color shift is not
  enough, and the focus indicator does not follow the pointer spot
- Reduced motion: the thumb arrives immediately, metric cards keep a
  static top highlight, and buttons change color with no shift
- With no pointer, skip the dynamic highlight. The control still works

## 5. Theme and compositing

A dark theme is not a simple inversion of the light theme.

- Set panel and base luminance for dark on their own
- Keep the top highlight weak, and use a light edge at low opacity
- Do not use a thick black shadow or a glow across the whole surface
- Keep the same light direction across controls, and the same meaning
  for each state, in both themes
- Check 1×, 2×, and fractional scale
- Use one coordinate space and one corner-radius treatment at every
  scale
- Keep the highlight inside the face, keep the shadow from turning
  muddy, and leave room outside the control so the focus ring is not
  clipped
- Tooltips and popovers paint above decorative highlights and shadows

## 6. Performance

- On pointer movement, update only the current lightweight parameters
  (such as that card's pointer coordinate) and coalesce them onto the
  frame schedule
- Do not allocate a texture or create a timer on every event
- Do not walk every card per event, and do not leave a full-window
  frame loop running
- Narrow invalidation to what this framework version can actually do.
  Until a partial GPU redraw has been verified, do not claim one
- Record real repaints and frame time so a needed redraw is separate
  from waste
- Reuse paint resources
- When nothing is hovered and animations have finished, stop requesting
  frames
- If a complex shadow or a custom-painted card is expensive, simplify
  it and limit where it appears before using it more widely

## 7. Hand-off

Tell the implementer:

- Authorized scope
- Repository and the GPUI version that was verified
- Components
- Tokens
- States
- Degradation, when a capability is missing
- Acceptance checks from §8

First build a native preview of one button, one segmented control, a
few metric cards, and one panel. Reuse that preview. Do not rewrite the
layout at large to introduce the treatment.

Keep these labels separate:

| Label | Means |
| --- | --- |
| Video observation | What the reference shows |
| Suggested parameter | A starting value from §2, not a measurement |
| Implemented | What this codebase does, via an API that was found |
| Unverified | Not yet checked on a device, scale, or theme |

The reference video is not a GPUI measurement.

## 8. Acceptance

Save real screenshots for light and dark, at 1× and 2×, covering
`default`, `hover`, `pressed`, `focus`, `disabled`, and `selected`
where that state exists.

Judge motion from a short recording or from using the UI. A screenshot
does not show that motion is smooth.

Check:

- Button light direction, press-and-restore, and the disabled state
- The thumb stays inside the track, animations do not pile up, focus
  and selected can appear together, and the keyboard path works
- The card spot stays on the top edge, clamped, on one card at a time;
  leave and reduced motion are static
- Panel scroll clipping, shadow, focus, and popovers
- Theme contrast, and hairlines at the scales you support
- Sustained hover, fast pointer movement, rapid switching, and scale or
  theme changes do not obviously stutter, and resource use does not
  keep growing
- The result is still native GPUI, with no WebView
- A fallback is described as a fallback, not as a full copy of the
  reference

Repair contrast, input, clipping, and performance before adding more
decoration.
