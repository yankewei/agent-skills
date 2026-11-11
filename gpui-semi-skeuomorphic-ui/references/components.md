# Components

Read only the sections in scope. Shared interaction rules are in
`SKILL.md`.

## Glossy button

The brand color does not have to be blue.

- Gentle vertical gradient: lighter on top, darker on the bottom
- Thin highlight on the top edge
- Contact shadow along the bottom
- Hover: slightly brighter
- Pressed: darker, with a weaker highlight. An optional downward shift
  of about 1 logical pixel is paint-only and must not change layout
- Pointer release, pointer leave, and cancel all restore the resting
  look

## Recessed segmented control

A darker recessed track holds a brighter raised thumb.

- Inner gap between thumb and track: 2–3 logical pixels
- Hit targets stay fixed while the thumb moves
- A new choice sends the thumb to the latest target only. Do not stack
  animations
- Selected and focus-visible may show at the same time

## Metric card

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

## Layered panel

Separate text, proportion bars, the foreground plate, and the base by
luminance steps, contact shadow, and hairlines. The surrounding tray is
this base, not a separate 3D shell. Do not use a heavy outline.

- Proportion bars do not cover text
- Clip the content. Do not use that same clip on the outer shadow or
  the focus ring
- Scrolling content stays inside the rounded shape
- Do not sink the whole card, and do not rotate it in 3D, as the
  default interaction
