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

## Installation

```bash
npx skills add agent-skills
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

## Skill Structure

Each skill contains:

- `SKILL.md` - Instructions for the agent
- `scripts/` - Helper scripts for automation (optional)
- `references/` - Supporting documentation (optional)

## Adding New Skills

To add a new skill:

1. Create a new directory with a descriptive name (e.g., `my-skill/`)
2. Add a `SKILL.md` file with skill metadata and instructions
3. Optionally add `scripts/` and `references/` directories
4. Update this README to document the new skill

## License

MIT
