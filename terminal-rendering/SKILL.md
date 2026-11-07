---
name: terminal-rendering
description: >-
  Design and implement embedded terminal rendering (cell grid, VT, fonts,
  sprites, paint cache). Use when building or improving a terminal pane,
  PTY viewer, agent TUI host, libghostty-vt / Ghostty-style paint, box
  drawing, Powerline glyphs, Nerd Font icons, or when terminal output
  looks wrong, soft, or misaligned compared to Ghostty / kero / herdr.
---

# Terminal rendering

A terminal screen is a **`cols × rows` cell grid**. Correctness comes from
VT; “好看” comes from metrics + paint. Same emulator ≠ same look.

## Four layers

| Layer | Job | If wrong |
|-------|-----|----------|
| **1. VT** | Bytes → cell grid (cursor, modes, scrollback, alt screen) | Garbled / wrong size / stuck cursor |
| **2. Metrics** | Cell width/height from font; resize PTY with cols/rows **and** cell px | Wobbly columns, blurry edges |
| **3. Paint** | Draw cells: bg runs, glyphs, sprites, contrast | Gaps, thin text, broken TUI borders |
| **4. Session** (optional) | Persist / remote: daemon owns VT, client mirrors frames | Scrollback dies with the window |

**One-liner:** VT for correctness; font + metrics + per-cell placement +
geometric box drawing for looks; dirty-row cache for speed.

## 1. VT (grid must be right)

- Prefer a real emulator (e.g. **libghostty-vt**), not a hand-rolled ANSI
  subset, for agent TUIs.
- Each cell: text (grapheme), fg/bg, bold/italic/faint, underline/strike,
  wide flag.
- Handle: wide chars / emoji width, alt screen, synchronized output
  (mode 2026), query replies written back before the next PTY read.
- UI reads a **snapshot** of the grid; do not re-parse the byte stream in
  the paint path.

## 2. Metrics

- Column width ≈ advance of `'M'` (optional `CellWidth` multiplier).
- Row height ≈ ascent + descent, then × ~**1.15–1.2**; snap to device
  pixels.
- Keep width as the font advance; snap **box/bg edges** to pixels so
  neighbors meet without seams (do not round cell width in a way that
  makes glyphs wobble).
- On resize, send `cols`, `rows`, `cell_width_px`, `cell_height_px` to
  the PTY (programs use pixel size for mouse / images).

## 3. Paint (where look is won or lost)

Priority order when improving an existing terminal:

1. **Ship a fixed mono + Nerd symbols** (e.g. JetBrains Mono + Symbols
   Nerd Font). System default mono / missing Powerline icons ruin agent
   TUIs.
2. **Draw box / block / braille / Powerline as geometry**, not font
   glyphs — fonts leave gaps between rows; full-screen borders must join.
3. **Place glyphs on the grid**: same-style runs with force-width shaping,
   or per-cell centering. Scale down fallback glyphs (emoji) that overflow
   a narrow cell.
4. **Route missing PUA / Nerd icons** to the symbols font (or outline-fit
   them into the cell).
5. Optional polish: `minimum-contrast` (skip sprites / intentional
   graphics), macOS font-thicken, multi-style underlines, padding color
   extend.

### Paint pipeline (per frame)

1. Fill pane with terminal bg.
2. Origin = padding + vertical spare/2; snap origin to device pixels.
3. Rebuild row ops only when the row `Arc`/id changed (or font/cell size
   changed / full dirty).
4. Paint all **background spans**, then **sprites**, then **text runs**,
   then cursor / IME / overlays.

### Row ops

- Merge consecutive same-bg cells into one quad.
- One sprite cell → geometric draw (see sprites approach in herdr /
  Ghostty).
- Else shape a same-style text run; force advance to `cell_w` for narrow
  cells so columns stay locked.

### What not to blame on VT

If borders gap, icons are tofu, or text looks thin/uneven while the grid
content is correct — fix **paint/fonts**, do not swap the emulator.

## 4. Session topology

**In-process (simple):** PTY → VT → Snapshot → UI paint.

**Daemon + client (kero-style):** daemon headless VT + PTY; `Mirror`
encodes VT frames of the current screen; client VT with `Conn` paints
cell-for-cell; scrollback survives window close / remote attach.

## Reference implementations

| Project | Stack | Notes |
|---------|-------|-------|
| **Ghostty** | Zig + libghostty-vt | Source of VT + sprite semantics |
| **herdr-app** | GPUI + libghostty-vt | Cell paint, sprites, contrast, bundled fonts |
| **kero** | MyGo + libghostty-vt | Mirror frames, Ghostty themes, GPU UI |
| **kumu** | GPUI-kit + libghostty-vt | Same paint ideas: sprites + JetBrains + Nerd |

## Checklist

When adding or fixing terminal rendering:

- [ ] VT owns the grid; UI only paints snapshots
- [ ] Cell metrics from mono face; resize includes pixel cell size
- [ ] Bundled (or guaranteed) mono + Nerd symbols
- [ ] Box / Powerline / braille drawn as shapes
- [ ] Dirty-row paint cache keyed by row identity
- [ ] Cursor, IME, and selection sit on cell coordinates
- [ ] Compare a known TUI (htop, Claude Code, statusline) against Ghostty

## Additional detail

- Sprite / geometry ranges and contrast rules: [paint.md](paint.md)
