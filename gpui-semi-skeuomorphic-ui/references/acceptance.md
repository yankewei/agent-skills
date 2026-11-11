# Acceptance

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
  four effects

Repair contrast, input, clipping, and performance before adding more
decoration.
