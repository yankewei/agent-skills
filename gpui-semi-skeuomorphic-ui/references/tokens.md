# Tokens

Put color, elevation, radius, spacing, and motion in shared tokens.
Do not scatter hardcoded colors inside components.

Minimum set:

`canvas`, `surface`, `surface-raised`, `surface-recessed`,
`edge-highlight`, `edge-contact`, `text-primary`, `text-secondary`,
`accent`, `focus-ring`, `disabled`, `shadow`, `radius`, `spacing`,
`motion`

These are tunable starting points, not measurements and not
cross-platform constants.

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
offset, blur, and opacity through the native shadow API already found
in the project, and adjust them there.

Motion durations stay configurable. Honor reduced motion in `SKILL.md`.

Contrast: ordinary text at least 4.5:1, large text at least 3:1.
Control boundaries and focus stay distinguishable. Disabled text stays
readable.
