# Example: Critique an existing UI

## User request
Review this macOS app screenshot and tell me why it does not feel native.

## Good response shape

### What already works
- Clear left navigation
- Main content area is easy to identify
- Search is in a discoverable location

### What feels weak
- Too many bright colored tags
- Toolbar is overloaded with equal-weight controls
- Primary and secondary actions compete visually
- Card styling feels web-like rather than macOS-native

### Structural issues
- Main content lacks a clear focal point
- Detail controls should move into an inspector or contextual menu

### Typography issues
- Section titles are too similar to body text
- Metadata is too loud
- Helper text is too long

### Color/material issues
- Accent color is overused
- Warning colors are used decoratively
- Dark mode will likely look muddy with the current layered grays

### Component hierarchy issues
- Too many outlined buttons
- Table rows show too many always-visible actions
- Icons use mixed styles

### Concrete redesign advice
- Reduce color count to one accent plus semantic states
- Simplify the toolbar
- Move low-frequency actions into menus
- Strengthen title and section hierarchy
- Reduce card/border/shadow treatment

### Final verdict
The interface is functional, but it currently feels closer to a SaaS admin panel than a refined macOS application.
