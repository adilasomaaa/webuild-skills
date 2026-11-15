# Variants and Components

The full inventory (every component, variant, option, max columns, footer placement and required
feature) lives in the theme repo at `docs/guide/pembuatan-tema/02-inventaris-komponen.md`. Read it
for the component you're working on. This file covers the rules for changing that inventory.

## Two kinds of variant

| Kind | Where the options come from | Can a theme add options? |
| --- | --- | --- |
| **Base variant** | Theme Kit contract (`DefaultThemeContract::getBaseVariants()`) | **Yes.** webuild-app merges the theme's `component_variants`, flags contract options as `base`, and shows the rest under "Variant tambahan dari tema" with their `label` and `description` |
| **Fixed variant** | Hard-coded in the webuild-app admin form (e.g. `profile_video.layout_orientation`, `pranala.variant`, `audio.layout_style`) | **No.** A new option never appears in the admin until webuild-app changes. It is a proposal |

Every option of both kinds must render correctly. The admin can pick any of them.

### Base variants

| Component | configuration_key | Base options |
| --- | --- | --- |
| `header_logo` | `mode` | `logo_name`, `logo_only`, `name_only` |
| `header_button` | `button_variant` | `primary`, `secondary`, `outline` |
| `contents` | `layout_variant` | `grid`, `list`, `featured_list` |
| `media_gallery` | `layout_variant` | `asymmetric_grid`, `marquee` |
| `agenda` | `timeframe` | `today`, `this_week`, `this_month` |
| `documents` | `layout_variant` | `grid`, `list` |
| `faq` | `layout_orientation` | `vertical`, `horizontal` |
| `slider` | `aspect_ratio` | `21:9`, `16:10`, `4:3`, `16:9` |

For `contents`, webuild-app already knows the labels of `headline`, `latest_list` and `by_category`.
Prefer those names if the design matches them.

Legacy values that must still render: `media_gallery: editorial_grid` → `asymmetric_grid`,
`contents: sorotan` → `featured_list`. Legacy section keys (`gallery_media`, `links`, `contact_info`,
`news`, `articles`, `geo_maps`) are translated by Theme Kit. Don't create views for them.

## Adding an option to a base variant

1. Add it under `theme.json → component_variants.<component>.options`:
   ```json
   "magazine": {
       "label": "Majalah",
       "description": "Satu berita utama besar dan empat berita kecil dalam dua kolom."
   }
   ```
   `label` and `description` are shown to the admin, so write them in Indonesian. Optional:
   `"view": "sections/main/contents/components/magazine.blade.php"` (relative to `views/`; the file
   must exist if listed).
2. If `component_settings` has a `select` with the same key, add the option there too.
3. Render it in the section view. Unknown values fall back to the default:
   ```php
   $v = $config['layout_variant'] ?? $variant ?? 'grid';
   // ... @if($v === 'magazine') ... @elseif($v === 'list') ... @else (grid) ... @endif
   ```
4. Update the view's header comment with the new option.
5. Log it in `CHANGELOG.md` → `### New variants`.
6. Test it in the customizer, then run `theme:check`.

## Proposals (fixed variants and new data)

Don't implement these on your own:
- a new option for a fixed variant,
- a new `configuration_key` for a component without `component_variants`,
- a data field webuild-app doesn't send.

Log each one in `CHANGELOG.md` → `### Proposals` with what it is, why the design needs it, and
which webuild-app form or data source would change. Tell the user.

## Adding component settings

`theme.json → component_settings.<component>.<field>`: requires `type` and `label`. A `select`
requires `options`. Provide a `default`, and always use a fallback when reading it in the view.
For catalog components, webuild-app uses its own form, so extra settings only reach the view if
that form sends them. Treat new settings on catalog components as proposals.

## Adding a new component

Use this path only when no existing component (or base-variant option) covers the need.

1. Add the key to `theme.json → sections.<area>`.
2. Add a definition under `component_definitions`:
   ```json
   "announcement_bar": {
       "label": "Pengumuman",
       "category": "content",
       "icon": "IconSpeakerphone",
       "data_source": ["static"],
       "default_configuration": { "text": "", "url": "" }
   }
   ```
   `data_source` may only contain: `static`, `news`, `articles`, `gallery`, `agenda`, `documents`,
   `faqs`, `sliders`. The admin edits it through a generic form built from `component_settings`.
3. Create `views/sections/<area>/<key>/<key>.blade.php` with a header comment (description,
   variants, settings) like the other sections.
4. Log it in `CHANGELOG.md` → `### New components`.
5. Run `theme:check`. All three parts (sections, definition, view) must agree.

## Compatibility across versions

When a theme is updated, Theme Kit compares the old and new manifests. A release is rejected if it
**removes** an existing section or variant option. Adding is always safe; removing never is.
