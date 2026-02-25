# Design System - Copilot Instructions

This is an opinionated CSS design system built on a layered architecture with a theme-agnostic foundation and swappable theme tokens.

## Architecture Overview

### CSS Layering System

The design system uses a 4-layer architecture:

1. **reset.css** - Browser normalization + utility classes (.sr-only, .mono)
2. **base.css** - Theme-agnostic structural tokens + global element styles
3. **Theme files** - Theme-specific tokens (colors, fonts, shadows)
4. **Showcase styles** - Component styles in design-system-base.css

### Token Separation: Theme-Agnostic vs Theme-Specific

**Theme-Agnostic Tokens (base.css)** - These remain constant across all themes:
- Typography scale (1.25 geometric ratio: `--font-base-size`, `--font-scale`, computed sizes)
- Spacing (4pt grid: `--spacing-1` through `--spacing-16`)
- Border radius (`--radius-sm` through `--radius-full`)
- Line height (`--line-height-base`)
- Transitions (`--transition-fast`, `--transition-spring`)

**Theme-Specific Tokens (theme-*.css)** - These change when switching themes:
- **All color tokens** - primary, accents, semantic, text, background, border
- **Font families** - `--font-family-main`, `--font-family-heading`, `--font-family-mono`
- **Font weights** - varies by font family (e.g., Outfit uses 500/700/800, Manrope uses different weights)
- **Shadows** - may need adjustments for dark themes
- **Gradients** - `--gradient-primary`, etc.

### File Structure

```
src/
├── css/
│   ├── reset.css                    # Browser normalization + utility classes
│   ├── base.css                     # Structural tokens + global styles (83 lines)
│   ├── design-system-base.css       # Base imports (no theme) - pair with a theme file
│   ├── design-system.css            # Legacy: all-in-one with embedded Vibrant theme (455 lines)
│   ├── theme-vibrant.css            # Vibrant theme tokens (Plus Jakarta Sans, Outfit)
│   ├── theme-citrus.css             # Citrus theme tokens (Varela Round, Fredoka)
│   ├── theme-business.css           # Business theme tokens (Manrope, Fira Code)
│   ├── theme-electric-gelato.css    # Electric Gelato theme (Quicksand, Shrikhand)
│   └── theme-mariner-parchment.css  # Mariner's Parchment (Libre Baskerville, Cinzel)
├── index.html                       # Landing page
├── design-system-guide.html         # Documentation
└── theme-switcher.html              # Interactive theme demo
```

## Creating New Themes

To create a new theme:

1. Copy any existing `theme-*.css` file as a starting point
2. Import your custom fonts at the top (if needed)
3. Modify only the theme-specific tokens:
   - Colors (all `--color-*` variables)
   - Font families (`--font-family-main`, `--font-family-heading`, `--font-family-mono`)
   - Font weights (to match your font family's available weights)
   - Shadows (optional: adjust for dark themes)
   - Gradients (optional)
4. Save as `theme-yourname.css` in the `src/css/` directory
5. Use it by pairing with `design-system-base.css`:
   ```html
   <link rel="stylesheet" href="css/design-system-base.css">
   <link rel="stylesheet" href="css/theme-yourname.css">
   ```

**Important**: Do NOT modify structural tokens in theme files. Spacing, typography scale, border radius, and transitions belong in `base.css`.

## Usage Patterns

### For Production Projects

**Recommended approach** (modular):
```html
<link rel="stylesheet" href="css/design-system-base.css">
<link rel="stylesheet" href="css/theme-vibrant.css">
```

**Legacy approach** (all-in-one):
```html
<link rel="stylesheet" href="css/design-system.css">
```

### Dynamic Theme Switching

See `theme-switcher.html` for a working example. Key approach:
```javascript
document.getElementById('theme-css').href = 'css/theme-citrus.css';
```

## Key Design Decisions

### Typography
- **Scale**: 1.25 geometric ratio for consistent sizing
- **Text wrapping**: Headings use `text-wrap: balance`, list items use `text-wrap: pretty`
- **Font smoothing**: Enabled for crisp text on Mac/iOS

### Spacing
- All spacing uses a 4pt grid system
- Use spacing tokens, not arbitrary pixel values

### Accessibility
- Links keep underline on hover (WCAG)
- `text-size-adjust: 100%` allows user zoom
- `.sr-only` utility for screen reader content
- Reduced motion preferences supported

### Reset Principles
- Box-sizing: border-box (inherited)
- No default margins
- Images/media: `max-width: 100%` by default
- Form controls inherit fonts
- Utility classes included: `.sr-only` (screen reader only), `.mono` (monospace font)

## Conventions

### When modifying existing themes:
- Keep font imports at the top of theme files
- Maintain the token comment structure for clarity
- Test changes with the theme-switcher to ensure visual consistency

### When adding new token types:
- If it varies by theme (e.g., a new color), add it to ALL theme files
- If it's structural (e.g., a new spacing value), add it to `base.css`

### When creating components:
- Use CSS custom properties (tokens) rather than hardcoded values
- Component-specific styles should go in separate CSS files or inline styles
- Avoid adding global styles to `base.css` unless they apply to all HTML elements

## File Naming

- Theme files: `src/css/theme-{name}.css` (lowercase with hyphens)
- Showcase/demo HTML: `src/{descriptive-name}.html`
- Structural CSS: `src/css/{base|reset|utilities}.css`
