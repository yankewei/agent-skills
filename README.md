A collection of skills for AI coding agents. Skills are packaged instructions and scripts that extend agent capabilities.

Skills follow the Agent Skills format.

## Available Skills

### macos-ui-design-critic

Use this skill when the user wants to design, refine, review, or critique the UI of a macOS app. Focus on native macOS feel, layout structure, typography, spacing, color, materials, component hierarchy, and visual polish.

**Use when:**

- Designing a new macOS screen
- Reviewing an existing macOS UI
- Making a macOS interface feel more native
- Improving typography, spacing, color, and hierarchy
- Turning rough requirements into polished macOS screen specifications

**Covers:**

- Native macOS design principles
- Layout structure and visual hierarchy
- Typography best practices
- Color, materials, and visual effects
- Component design (toolbars, sidebars, forms, lists)
- UI critique checklists

### terminal-rendering

Use this skill when building or improving embedded terminal rendering: VT cell grid, metrics, fonts, sprites, box drawing, and paint caches. Also use it when terminal output looks wrong, soft, or misaligned compared to Ghostty / kero / herdr.

**Use when:**

- Building a terminal pane, PTY viewer, or agent TUI host
- Terminal output looks garbled, soft, or misaligned versus Ghostty
- Fixing broken box drawing, Powerline glyphs, or Nerd Font icons
- Working with libghostty-vt or Ghostty-style paint

**Covers:**

- VT grid correctness (cursor, modes, scrollback, alt screen)
- Cell metrics and PTY resize including pixel cell size
- Paint pipeline: background spans, sprites, then glyph runs
- Drawing box / Powerline / braille as geometry instead of font glyphs
- Missing-glyph fallback, contrast handling, and dirty-row caches
- Session topology: in-process paint vs daemon + client mirroring

**Files:** `SKILL.md` for the main workflow, `paint.md` for sprite ranges and contrast rules.

### gpui-semi-skeuomorphic-ui

Use this skill when designing or implementing restrained semi-skeuomorphic UI for a native GPUI desktop app: glossy buttons, recessed segmented controls, pointer-following highlight cards, layered panels, light and dark themes, and accessibility and performance acceptance.

**Use when:**

- Designing or building native GPUI controls with restrained depth
- Adding a glossy button, a recessed segmented control, a pointer-following card highlight, or a layered panel
- Tuning light and dark themes without treating dark as an inverted light theme
- Checking accessibility, motion, clipping, and paint cost for that treatment

**Covers:**

- Scope, and a capability check against the GPUI version in the target project
- Shared color, elevation, radius, spacing, and motion tokens
- The four component treatments and their interaction states
- Theme, scale, and compositing cautions
- Performance limits and an acceptance checklist

**Files:** `SKILL.md` for the workflow, `references/` for tokens, components, theme, performance, and acceptance, `examples/preview-spec.md` for a spec with no code.

Guidance only. It does not add application UI, and it does not replace native GPUI with HTML, CSS, or a WebView.

## Installation

```bash
npx skills add https://github.com/yankewei/agent-skills
```

## Usage

Skills are automatically available once installed. The agent will use them when relevant tasks are detected.

**Examples:**

```
Review this macOS app UI for native design issues
```

```
Help me design a new preferences window for my macOS app
```

```
Make this interface feel more native to macOS
```

```
My terminal panel clips the bottom row and skips box-drawing glyphs — fix the rendering
```

```
Design a restrained semi-skeuomorphic button and segmented control for this native GPUI app
```

## Skill Structure

Each skill contains:

- `SKILL.md` - Instructions for the agent
- `scripts/` - Helper scripts for automation (optional)
- `references/` - Supporting documentation (optional)
- Additional `.md` files referenced from `SKILL.md` (optional, e.g. `terminal-rendering/paint.md`)

## Adding New Skills

To add a new skill:

1. Create a new directory with a descriptive name (e.g., `my-skill/`)
2. Add a `SKILL.md` file with skill metadata and instructions
3. Optionally add `scripts/` and `references/` directories
4. Update this README to document the new skill

## License

MIT
