# Customization and file reference

[Handbook](README.md) · [Setup](setup.md) · [Event updates](event-updates.md)

## Where things live

```text
_config.ts          public URL, theme plugin and build pipeline
deno.json           tasks, permissions and pinned theme URL
wrangler.jsonc      Cloudflare Worker name and static asset settings
src/*.vto          your pages: metadata, copy, layout and sections
src/_data/         YAML content and Markdown terms
src/assets/        logos, photos, documents and other media
src/style.css      Tailwind entry and color roles
docs/              this organizer handbook
```

### Content directory

All paths below are under `src/_data/`.

| File | Content |
| --- | --- |
| `event.yaml` | Name, city, year, date and timezone. |
| `share.yaml` | Default link-preview image; descriptions live in page front matter. |
| `social.yaml` | Network/contact link map used throughout the site. |
| `nav.yaml` | Main links, secondary/legal links and the menu CTA. |
| `hero.yaml` | Hero image and crops; the words live in `index.vto`. |
| `venue.yaml` | Venue facts, coordinates, images and transport links; additional prose lives in `venue.vto`. |
| `organizers.yaml` | Organizing team. |
| `cfp.yaml` | Submission link, deadline, open switch and FAQ. |
| `tickets.yaml` | Billing contact; ticket explanations live in `tickets.vto`. |
| `pretix.yaml` | Public ticket shop host, organizer, event and locale. |
| `keynotes.yaml` | Homepage keynote cards, separate from the programme. |
| `sponsors.yaml` | Sponsor logos grouped by tier. |
| `sponsorship.yaml` | Page-owned packages, survey and key dates; other offer copy lives in `sponsors.vto`. |
| `agenda.yaml` | Programme rooms, sessions and breaks. |
| `speakers.yaml` | Speaker profiles linked to sessions by ID. |
| `patches/agenda.yaml`, `patches/speakers.yaml` | Optional persistent programme corrections. |
| `sessionize.yaml` | API endpoint ID used by `deno task sessionize`. |
| `terms/<language>.md` | Terms, one Markdown file per language. |
| `logo.yaml` (optional) | Override the default logo paths. |
| `footer.yaml` (optional) | Override default footer text/logo. The footer always renders. |

Known concepts are validated at build time. Keys use camelCase; dates and
timestamps should be quoted. Custom top-level data, such as the examples'
`recap.yaml` and `sponsorship.yaml`, is allowed but has no built-in schema.
You can add one through `kcd({ schema: { concepts, required } })` in `_config.ts`.

## Pages and navigation

A `.vto` page has front matter, a layout tag and its own sections. For example:

```vto
---
id: accessibility
title: Accessibility
description: Accessibility information for our event.
---
{{ layout "layouts/page.vto" }}
{{ comp ui.section { width: "4xl" } }}
    {{ comp.ui.heading({ level: 1, title: "Accessibility" }) }}
    <p>Contact the organizing team to discuss your access needs.</p>
{{ /comp }}
```

Add a menu entry in `nav.yaml` with `page: accessibility`. Page links resolve
the page's ID, use its title unless you supply a `label`, and wait while it is
a draft. External links use `url` and `label`. A misspelled or duplicated page
ID fails validation. Theme buttons use `page` for internal pages and `href` for
external links or anchors.

The starter includes home, tickets, agenda, speakers, venue, sponsors, CFP FAQ,
terms, imprint, attending and 404 pages. Edit, rearrange or remove them as needed;
when removing a page, remove its navigation entry and any data requirement no
remaining page uses.

### Choose a homepage layout

- `layouts/home.vto`: full-screen hero with a transparent menu. Put hero content
  in `{{ slot hero }}`.
- `layouts/page.vto`: menu bar, content and footer. Good for inner pages and a
  plain homepage.
- `layouts/site.vto`: common shell with a custom `{{ slot header }}`. Use it for
  a menu bar followed by a smaller banner.

Compare [big hero](https://kcd-website-showcase.mark-sagikazar.workers.dev/home/big-hero/),
[banner](https://kcd-website-showcase.mark-sagikazar.workers.dev/home/banner/) and
[plain](https://kcd-website-showcase.mark-sagikazar.workers.dev/home/plain/) examples.
Adapt their layout and header while keeping your own content sections.

## Colors and images

Edit `src/style.css`, the Tailwind CSS v4 entry. The shared roles are `primary`,
`secondary`, `accent`, `primary-strong` and `accent-strong`. Keep `@theme static`
so the attending-image generator can read the colors, and keep
`@import "theme.css"` for the theme's component styles.

The default logo paths are `/assets/logo-<orientation>-<color>.svg`. To use other
paths, add `logo.yaml` with `horizontal.color`, `horizontal.white`,
`vertical.white` and optionally `vertical.color`.

For automatic portrait/hero resizing, see
[Responsive images in the theme reference](https://github.com/sagikazarmark/kcd-website-theme/blob/main/docs/theme-reference.md#responsive-images).
The base template works without these optional image-processing plugins.

## Terms and longer content

Each file in `src/_data/terms/` has front matter with `label`, `title` and optional
`order`, followed by Markdown. The filename is its language tag, for example
`en.md`. Use `###` for sections under the page and language headings. The terms
page provides a language switch; update the imprint's wording to match the
languages you actually publish.

Shorter content, such as FAQ answers, can use YAML Markdown block strings (`|`
or `>-`). Page copy remains directly in `.vto` files where it belongs to the layout.

## Theme overrides

A file under your `src/` at the same path as a theme file replaces it. For
example, `src/_components/ui/card.vto` replaces the theme card, and
`src/_includes/layouts/page.vto` replaces the page layout.

An overridden file stops receiving theme updates. Prefer editing your pages,
data and stylesheet when that is enough. List overrides with:

```sh
deno task overrides
```

Your pages and `style.css` are site-owned files, not theme overrides. The theme
supplies shared layouts, components, CSS, scripts and safe default data.

## Pinning and upgrading the theme

The `kcd-theme/` import-map entry in `deno.json` is the single source of the
theme location. `_config.ts` and the `sessionize`/`overrides` tasks all use it.

The template pins a published theme release using a URL of this form:

```json
"kcd-theme/": "https://cdn.jsdelivr.net/gh/sagikazarmark/kcd-website-theme@v0.1.0/"
```

Check the [theme releases](https://github.com/sagikazarmark/kcd-website-theme/releases)
and use an existing tag. Keep the trailing slash. The `overrides` task uses
jsDelivr's API to list the published theme's files.

To upgrade, read the release notes, change the tag on a branch, run
`deno task overrides` and `deno task build`, and review the preview. Check any
local overrides against the new theme version. Remote files are cached;
`LUME_NOCACHE=true deno task build` forces a refetch when needed.

## Short links and redirects

Put Cloudflare redirect rules in `src/_redirects` and add this before
`export default site` in `_config.ts`:

```ts
site.copy("_redirects");
```

For example, `/register https://pretix.eu/your-organizer/your-event/ 302` creates
a short registration URL. Review these rules when sales end or an edition is
[archived](after-the-event.md).
