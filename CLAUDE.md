# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Shopify theme called Horizon, which is the flagship of a new generation of first-party Shopify themes. It incorporates the latest Liquid Storefronts features, including theme blocks, and follows web-native principles with progressive enhancement.

## Essential Commands

### Theme Development
```bash
# Development server with hot reload against your development store
shopify theme dev --store <store>

# Check theme for issues (run before every commit)
shopify theme check

# Sync code with connected store
shopify theme pull    # Pull before editing
shopify theme push    # Push after validation
```

**Important**: Run commands from repo root with Shopify CLI authenticated to correct `.shopify` profile.

### Git Workflow
```bash
# Stay up to date with Horizon changes (if using as base)
git remote add upstream https://github.com/Shopify/horizon.git
git fetch upstream
git pull upstream main
```

## Project Structure & Module Organization

Horizon is a Shopify Online Store 2.0 theme with the following organization:

### Core Theme Structure
- **`layout/`** - Global wrappers and includes (theme.liquid, etc.)
- **`templates/`** - Storefront routes defined by JSON files
- **`sections/`** - Page-level modules and group configurations
- **`blocks/`** - Reusable presets (theme blocks prefixed with `_`)
- **`snippets/`** - Granular fragments and reusable Liquid code
- **`assets/`** - Styles, JavaScript, and static media (keep compiled bundles beside sources)
- **`config/`** - Schema and defaults (settings_schema.json, settings_data.json)
- **`locales/`** - Translation files for internationalization

**Note**: Review `release-notes.md` when touching shared components to avoid regressing recent work.

### Theme Blocks Architecture
This theme heavily uses **theme blocks** - reusable components that can be:
- Nested under sections and other blocks
- Configured via theme editor settings
- Used as static blocks by developers or dynamic blocks by merchants
- Referenced in templates using `{% content_for 'block', type: 'block-name' %}`

### Key Architectural Patterns

#### Static vs Dynamic Blocks
- **Static blocks**: Predetermined placement by developers with `{% content_for 'block', type: 'text', id: 'unique-id' %}`
- **Dynamic blocks**: Added by merchants through theme editor with `{% content_for 'blocks' %}`

#### CSS Organization
- Scoped styles using `{% stylesheet %}` tags within blocks
- BEM naming convention (block__element--modifier)
- CSS variables for theming and customization
- Mobile-first approach with logical properties for RTL support
- Component-scoped variables with proper namespacing

#### Block Structure Pattern
```liquid
{% doc %}
  Block description and usage
{% enddoc %}

<div {{ block.shopify_attributes }} class="block-name">
  <!-- Block content -->
</div>

{% stylesheet %}
  /* Scoped CSS */
{% endstylesheet %}

{% schema %}
{
  "name": "Block Name",
  "settings": [],
  "presets": []
}
{% endschema %}
```

## Coding Standards & Development Workflow

### Code Style
- **Indentation**: Two-space indentation for Liquid, JSON, and SCSS
- **Liquid**: Prefer `{% liquid %}` blocks for control flow and `{% render %}` over `include`
- **Component naming**: Match names across directories (e.g., `sections/hero.liquid`, `snippets/hero-media.liquid`, `assets/hero.css`)
- **Handles**: Use lowercase with hyphens for schema IDs and data attributes
- **Translation keys**: Namespace by feature (`customer.login.title`)

### CSS Guidelines
- Never use IDs as selectors, avoid `!important`
- Maintain `0 1 0` specificity where possible
- Use BEM naming convention consistently (`.block__element--modifier`)
- Namespace CSS variables (e.g., `--hero-padding`, `--component-padding`)
- Prefer logical properties for RTL support
- Mobile-first media queries
- Scope variables to components when not global

### Block Development
- All blocks must include `{{ block.shopify_attributes }}` for editor functionality
- Use descriptive documentation with `{% doc %}` tags
- Follow standard scaffold: markup, optional `{% stylesheet %}`, schema
- Implement proper schema configuration for theme editor
- Follow consistent naming patterns (blocks prefixed with `_`)

### Testing & Quality Assurance
- **Theme Check**: Run before every commit - this is the baseline gate
- **Manual testing**: Preview key flows (home, product, cart, search) on desktop and mobile
- **Section settings**: Exercise newly added section settings
- **JavaScript**: Test with sample data from `config/settings_data.json`
- **Documentation**: Include manual QA steps and storefronts used in PRs

### Accessibility Requirements
- Support keyboard navigation
- Respect `prefers-reduced-motion`
- Maintain WCAG AA contrast ratios
- Use `:focus-visible` for focus indicators
- Test with screen readers

## File Naming Conventions
- **Blocks**: Prefixed with `_` (e.g., `_slide.liquid`)
- **CSS classes**: BEM convention (`.product-card__title--large`)
- **Variables**: Namespaced with component name (`--product-card-padding`)
- **IDs**: Use block.id for theme blocks, section.id for sections

## Commit & Pull Request Guidelines

### Commit Style
- Use short imperative subjects with optional bracketed scopes
- Example: `[Cart] Adjust empty state spacing`
- Keep summaries under 65 characters
- Note merchant impact in the body
- Reference linked issues

### Pull Request Requirements
- Describe the change clearly
- Include screenshots for UI tweaks
- List validation steps (Theme Check, preview store, translations updated)
- Request additional reviewer for edits in `layout/` or shared snippets

## Localization & Configuration

### Schema Settings
- Add new settings to `config/settings_schema.json` with clear `info` text
- Update every locale file when adding new schema
- Avoid hard-coded copy in Liquid—use translation keys with sensible defaults
- Keep translation keys namespaced by feature (`customer.login.title`)

### Metafields
- Configure in `.shopify/metafields.json` for different resource types (products, collections, etc.)
- Document new metafields with usage notes for merchant configuration

## Important Notes
- This theme is web-native and server-rendered with Liquid
- Business logic belongs on the server, not client-side
- Functionality defaults to "no" unless it meets performance requirements
- The main branch may include unreleased features
- Theme blocks are the primary architectural pattern for reusability
- Review `release-notes.md` when touching shared components to avoid regressing recent work