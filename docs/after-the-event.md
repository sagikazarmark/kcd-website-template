# After the event: recap and archive

[Handbook](README.md) · [Event roadmap](event-updates.md)

Keep the finished event useful: thank attendees, publish resources as they arrive,
then preserve that edition when next year's website takes over the homepage.

## Publish a thank-you homepage

Use the [wrap-up demo](https://kcd-website-demo.cloudnativehu.workers.dev/10-wrap/)
as a starting point:

1. Rewrite `src/index.vto` with a thank-you message and the event's highlights.
2. Set `cfp.open: false` and disable the submission link in `nav.yaml`.
3. Remove or replace ticket and sponsorship calls to action. Put `tickets.vto`,
   `attending.vto` and the sponsorship-offer page back into draft if they are no
   longer useful; retain the sponsor acknowledgments.
4. Keep agenda, speakers, venue and relevant legal pages available.
5. Add resource cards for photos, recordings and slides. A resource with no URL
   can show “Coming soon”; publish each link when it is ready.

The demo's wrap-up, photos and recap directories contain complete examples. They
are page compositions to adapt, not a setting that changes your homepage for you.
Their `recap.yaml` is site-owned data, without a built-in theme schema.

## Add photos, recordings and slides

The [photos example](https://kcd-website-demo.cloudnativehu.workers.dev/11-photos/)
adds a preview gallery and full-album link. Each `comp.gallery` image needs `src`,
descriptive `alt`, `width` and `height`; optional `full` links to a larger image.
Use a unique gallery `id` per page. Add credits and an album link in the page.

`comp.recap.resource` renders a resource card from a title, description, action
label and `resource: { url?, note?, enabled? }`:

- No URL: a non-clickable “Coming soon” state.
- URL supplied: the resource is published.
- `note`: explain partial availability, such as “More recordings next week.”
- `enabled: false`: hide the card.

Follow the [complete recap](https://kcd-website-demo.cloudnativehu.workers.dev/12-recap/)
for a film, recordings, slides and an optional transparency report. Replace its
sample illustrations, KCD Budapest links and fictional figures with your own.
Check the resources, build and deploy after each update.

## Archive the finished edition at `/year/`

When next year's site is ready, keep the new edition at `/` and the finished one
at, for example, `/2026/`. Preserve the **built HTML and assets** so future theme
and content changes cannot rewrite the old edition.

There is no automatic archive command in the template. The following workflow
uses the existing Lume build and static-copy support. Substitute your domain,
year, tag and paths throughout.

### 1. Save a reproducible final source

Commit the completed recap, including all imported programme data and assets.
Tag that commit, for example `edition-2026-final`, and keep its pinned theme
version. Build the archive from a separate checkout or worktree of that commit,
so preparing next year's content does not overwrite the old edition.

### 2. Build for the archive URL

In that final-edition checkout, run:

```sh
deno task lume --location https://kcd-yourcity.example/2026/
```

This builds into `_site/`. The template's `basePath()` plugin prefixes local
links and assets with `/2026/`, while `location` sets canonical and share URLs.
Simply moving a build originally made for `/` would leave links pointing at the
new edition, so build with the archive URL first.

Review hard-coded absolute links, redirect rules and scripts that construct URLs;
they may still point at the old root. Fix edition-specific URLs in the archive
source and rebuild. Keep links to external albums and recordings as external links.

### 3. Include the snapshot in next year's site

Copy that `_site/` output into **next year's** repository at `src/2026/`, keeping
its pages and assets together. Do not copy previous years' archive directories
inside it: those should remain siblings such as `src/2025/` and `src/2026/`.

In the new site's `_config.ts`, before `export default site`, add:

```ts
site.copy("2026"); // Frozen HTML and assets, copied without rendering them again.
```

Repeat for each archived year. Commit the snapshot and config change so every
Cloudflare build includes it. Keep the new site's `location` at the root URL,
`https://kcd-yourcity.example/`.

Cloudflare reads `_redirects` and `_headers` from the deployment root. Review the
old build's copies separately: merge any needed rules into the new site's root
files with year-prefixed paths, rather than relying on nested configuration files.
Do not carry over a blanket redirect from `/2026/*` to `/`.

### 4. Launch the new edition and verify both

Personalize the new edition as described in [setup](setup.md): date/year, team,
venue, shop, Sessionize endpoint, branding and announcement copy. Add an external-style
navigation entry (`url: /2026/`, `label: "2026 edition"`) for the archive; copied
HTML has no current-site page ID to use with `page:`.

Run `deno task build` and review the Cloudflare branch preview before publishing:

- `/` shows the new edition.
- `/2026/`, `/2026/agenda/` and `/2026/speakers/` show the finished edition.
- Archive menus, logos, CSS, scripts, images and gallery dialogs use `/2026/`
  where appropriate, rather than opening current-year pages or assets.
- Canonical and share URLs identify the archived edition.
- Older archives remain available, with no accidentally nested years.
- Ticket/CFP links no longer imply the archived event is open for registrations.

Merge and deploy the new homepage and archive together. To correct the archive
later, edit its saved source, rebuild with the same `/2026/` location and replace
the snapshot; avoid hand-editing generated HTML.
