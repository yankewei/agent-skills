# Paint details

## Characters to draw as sprites (not font glyphs)

Treat a single-codepoint cell as a sprite when it falls in:

| Range | Content |
|-------|---------|
| U+2500–U+257F | Box drawing (lines, arcs, dashes) |
| U+2580–U+259F | Block elements |
| U+2800–U+28FF | Braille |
| U+E0B0–U+E0BF (and nearby Powerline) | Nerd / Powerline separators |
| U+23FA | Record mark (Claude Code step marker) — few fonts have it |

Draw to **fill the cell** and snap edges to device pixels so adjacent cells
share a hard edge (anti-aliased seams look like gaps).

## Contrast

Ghostty-style `minimum-contrast` (WCAG ratio, often default **1** = off):

- Compare fg vs cell bg (or terminal default bg).
- If below threshold, mix toward near-white or near-black — whichever
  contrasts more — just enough to reach the ratio.
- **Do not** adjust intentional graphics: sprites and ranges
  `U+2500–259F`, `U+E0B0–E0D7`, `U+1FB00–1FBFF`, etc.

## Fonts

| Role | Example | Why |
|------|---------|-----|
| Terminal body | JetBrains Mono | Stable grid, coding ligatures optional |
| Icons / PUA | Symbols Nerd Font | Prompts, agent TUIs, file trees |
| Fallback | System mono last | Proportional fallback breaks the grid |

Register bundled faces at startup before any terminal shapes text.
If the primary face cannot advance a PUA codepoint, reshape with the
symbols font (or outline-fit the glyph into the cell like herdr).

## Metrics defaults that work

- Line height multiplier: **1.15–1.2**
- Regular weight: Medium (500) is fine for JetBrains; bold = Bold
- Padding: ~6–16 DIP horizontal, ~4–10 vertical; center leftover height
  above/below the grid

## Dirty rows

Key cache entries by row identity (`Arc` pointer equality or Ghostty row
id). Clear the whole cache on: font change, cell size change, theme
default-cell change, or VT “all dirty”. Agent spinners should rebuild
one row, not the full screen.

## Common failure modes

| Symptom | Likely cause |
|---------|----------------|
| Table borders have gaps | Box chars from font, not sprites |
| Powerline triangles misaligned / wrong glyph | Missing Nerd font or font fallback |
| Emoji overflows into next column | No fallback size shrink |
| Columns slightly jitter | Rounded cell width instead of snapping quads |
| Thin / gray text on macOS | No thicken / light weight on dark bg |
| Good in Ghostty, bad in app | Paint/fonts — not a different VT needed |
