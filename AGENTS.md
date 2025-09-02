# Repository Guidelines

## Project Structure & Architecture
Horizon is a Shopify Online Store 2.0 theme. Global wrappers and includes live in `layout/`, storefront routes in `templates/`, and page-level modules in `sections/`. Reusable presets live in `blocks/` (prefixed `_`), fragments in `snippets/`, and assets in `assets/`. Configuration in `config/` defines settings, defaults, and metafields; translated copy resides in `locales/`. Theme blocks drive composition; document static and merchant-managed dynamic blocks with `{% doc %}`.

## Development Workflow
- `shopify theme dev --store <store>` serves the theme with hot reload against your development store.
- `shopify theme pull` / `shopify theme push` synchronize code with the connected store—pull before editing, push after validation.
- `shopify theme check` runs the repository Theme Check profile; resolve warnings or note intentional ignores.
Run commands from the repo root with Shopify CLI logged into the right `.shopify` profile.

## Coding Standards
Use two-space indentation for Liquid, JSON, and SCSS. Prefer `{% liquid %}` for control flow and `{% render %}` over `include`. Every block must output `{{ block.shopify_attributes }}` and follow the standard scaffold (markup, optional `{% stylesheet %}`, schema). Adopt BEM-style classes, namespace CSS variables (e.g., `--hero-padding`), avoid IDs or `!important`. Match component names across directories, use lowercase hyphenated handles, and keep translation keys namespaced by feature (`customer.login.title`).

## Testing & Accessibility
Theme Check is the baseline gate; run it before every commit. Manually preview key flows (home, product, cart, search) on desktop and mobile, exercising new section settings and dynamic JS with data from `config/settings_data.json`. Honor accessibility requirements: support keyboard navigation, respect `prefers-reduced-motion`, maintain WCAG AA contrast, include `:focus-visible` styles. Document manual QA steps and storefronts used in your PR.

## Commit & PR Guidelines
History favors short imperative subjects with optional bracketed scopes, e.g., `[Cart] Adjust empty state spacing`. Keep summaries under 65 characters, note merchant impact in the body, and reference linked issues. Pull requests should describe the change, include screenshots for UI tweaks, list validation steps, and request another reviewer for edits in `layout/` or shared snippets.

## Localization & Configuration
Add schema settings to `config/settings_schema.json` with clear `info` text and update every locale file. Avoid hard-coded copy; rely on translation keys with sensible defaults. Document new metafields in `.shopify/metafields.json` with notes for merchant configuration.
