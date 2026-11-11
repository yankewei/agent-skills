# Preview spec: button and segmented control

Use this shape when the task is a spec rather than an edit. Record
API evidence under `Verified API` and proposed work under `Planned
approach`. Fill `Implemented` only for behavior already present in the
target codebase, citing its source location; otherwise mark it as not
implemented. Leave unknown checks under `Unverified`. Do not add GPUI
or Rust signatures.

## Scope

One primary button and one two-option segmented control in an existing
native GPUI settings row. Light and dark. Do not restyle the rest of
the window.

## Capability

Record the GPUI version and the gradient, shadow, clip, radius, and
animation calls this repository already uses, with source locations.
If a call is missing, name the flat fallback in `Planned approach` and
do not describe it as the full effect. Finding an API does not establish
that the proposed behavior is implemented.

## Button

| Label | Detail |
| --- | --- |
| Specified effect | Glossy button with a thin highlight along the top |
| Suggested parameter | Height 32 logical pixels, radius 8, top highlight about 1 physical pixel, press feedback within 80 ms, no layout shift |
| Verified API | Record the relevant paint calls and their source locations, or mark unknown |
| Planned approach | Describe the proposed treatment or flat fallback |
| Implemented | Cite existing behavior in code, or mark not implemented |
| Unverified | Light and dark, 1× and 2×, focus ring outside the clip, disabled contrast |

States: `default`, `hover`, `pressed`, `focus-visible`, `disabled`.

## Segmented control

| Label | Detail |
| --- | --- |
| Specified effect | Raised segment sliding inside a recessed track |
| Suggested parameter | Thumb gap 2 logical pixels, travel 160 ms, hit targets fixed |
| Verified API | Record the relevant paint and animation calls and their source locations, or mark unknown |
| Planned approach | Describe the proposed treatment or flat fallback |
| Implemented | Cite existing behavior in code, or mark not implemented |
| Unverified | Thumb stays inside the track, no stacked animations, keyboard path, focus and selected together |

## Out of this preview

Metric cards, the layered panel, a 3D exploded view, and a WebView.
