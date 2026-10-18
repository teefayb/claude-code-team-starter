<!--
  CLAUDE.md for a Shopify Online Store 2.0 theme. Copy it to the root of your
  theme repo, replace the <PLACEHOLDERS> and cut what you don't use.
  HTML comments like this one never reach Claude.
-->

# <THEME_NAME>

Shopify Online Store 2.0 theme for <STORE_DESCRIPTION>. Liquid, JSON templates,
plain CSS and JavaScript. No build step: files in this repo are what gets uploaded.

## Commands

- Local preview: `shopify theme dev --store <your-store>`
- Lint: `shopify theme check` (run before saying a task is done; fix every error)
- Upload for review: `shopify theme push --unpublished`
- Pull merchant edits: `shopify theme pull`

## Layout

- `sections/`: sections merchants add in the theme editor. Each has a `{% schema %}`.
- `blocks/`: theme blocks reused across sections.
- `snippets/`: partials, rendered with `{% render 'name' %}`.
- `templates/*.json`: which sections appear on each page type.
- `locales/en.default.json`: all storefront text.
- `assets/`: CSS, JS and images. Reference them with `{{ 'file.css' | asset_url }}`.

## Conventions

- Use `{% render %}`, never `{% include %}`. Pass variables explicitly.
- Storefront text goes in `locales/en.default.json` and is output with the `t` filter.
  Schema labels go in `locales/en.default.schema.json`.
- Anything a merchant might want to change is a section or block setting, not a hard-coded value.
- Images use the `image_url` filter with a `width`, then `image_tag` with `widths` and `sizes`.
  Lazy-load everything below the fold; never lazy-load the first image on the page.
- Keep JavaScript small and scoped to its section. No jQuery. Defer scripts.
- CSS class names follow `<naming convention, e.g. BEM with a section prefix>`.

## Do

- Run `shopify theme check` after every change to a `.liquid` or `.json` file.
- Give every new setting a sensible `default` so the section looks right when added.
- Test new sections with empty settings and with long product titles.

## Don't

- Don't edit `config/settings_data.json` or `templates/*.json` by hand unless asked.
  Merchants change them in the theme editor, so a push can wipe out their work.
  Run `shopify theme pull` first if you must touch them.
- Don't push to or publish the live theme. Push with `--unpublished` and let a person publish.
- Don't add apps, external scripts or tracking pixels without asking.
- Don't put Admin API tokens, storefront passwords or store credentials in any file.
