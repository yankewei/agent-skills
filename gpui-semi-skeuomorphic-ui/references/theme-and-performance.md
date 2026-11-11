# Theme, compositing, and performance

Read this before tuning a theme, a scale factor, clipping, or paint
cost. A one-control change still follows it.

## Theme and compositing

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

## Performance

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
