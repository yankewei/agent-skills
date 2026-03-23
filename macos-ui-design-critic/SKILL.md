---
name: macos-ui-design-critic
description: Use this skill when the user wants to design, refine, review, or critique the UI of a macOS app. Focus on native macOS feel, layout structure, typography, spacing, color, materials, component hierarchy, and visual polish. Use references progressively instead of loading all guidance at once.
---

# macOS UI Design Critic

## Purpose

Use this skill for macOS UI work only.

This skill helps with:
- designing a new macOS screen
- reviewing an existing macOS UI
- making a macOS interface feel more native
- improving typography, spacing, color, hierarchy, and overall beauty
- turning a rough requirement into a polished macOS screen specification

Do not use this skill for:
- backend architecture
- business logic
- API design
- pricing
- App Store policy
unless the user explicitly asks and it is directly relevant

## Progressive loading rule

Do not load every reference file by default.

Start from the user request, then read only the minimum files needed:

- For new screen design, read:
  - `references/design-principles.md`
  - `references/layout-rules.md`
  - `references/typography-rules.md`
  - `references/color-and-materials.md`
  - `references/component-rules.md`

- For UI critique, read:
  - `references/design-principles.md`
  - `references/critique-checklist.md`
  - plus any relevant detailed reference files as needed

- For typography-only questions, read:
  - `references/typography-rules.md`

- For color/style questions, read:
  - `references/color-and-materials.md`

- For toolbar/sidebar/form/list design, read:
  - `references/component-rules.md`

## Core behavior

Always optimize for:
- native macOS feel
- clarity
- strong hierarchy
- restrained elegance
- desktop efficiency
- high readability
- low visual noise

Do not design the interface like:
- a stretched mobile app
- a generic web admin dashboard
- an overly card-heavy SaaS page
- a highly decorative concept shot

## Response mode

When the user asks for a new macOS screen design, output:

1. Screen purpose
2. Layout structure
3. Visual hierarchy
4. Typography guidance
5. Color and materials guidance
6. Component decisions
7. Aesthetic improvements
8. Final design direction

When the user asks for critique of an existing design, output:

1. What already works
2. What feels non-native or visually weak
3. Structural issues
4. Typography issues
5. Color/material issues
6. Component hierarchy issues
7. Concrete redesign advice
8. Final verdict

## Decision priority

When multiple solutions are possible, prefer:
1. more native
2. clearer hierarchy
3. more restrained visual styling
4. better long-term usability
5. better scanability
6. better support for high-frequency desktop use

## Non-negotiable red flags

Always call these out if present:
- looks like a blown-up mobile UI
- no clear toolbar/sidebar/content structure
- too many colorful buttons or tags
- unclear primary action
- weak typography hierarchy
- inconsistent spacing rhythm
- excessive blur, borders, shadows, or cards
- poor dark mode viability
- mixed icon styles
- no visual center of gravity
