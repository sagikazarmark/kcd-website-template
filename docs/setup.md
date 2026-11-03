# Setting up a website

[Handbook](README.md) → Set up → [Deploy](deployment.md)

## 1. Create your repository and preview the starter

Open the [template repository](https://github.com/sagikazarmark/kcd-website-template)
and choose **Use this template → Create a new repository**. Clone your new
repository and open a terminal in it.

Install [Deno 2.8](https://docs.deno.com/runtime/getting_started/installation/)
(use 2.8.3 to match the deployment examples), then run:

```sh
deno task serve
```

Open <http://localhost:3000/>. The first run downloads the build dependencies;
subsequent edits reload automatically. No separate dependency installation is needed.

Change the event name in `src/_data/event.yaml` and save. You now have a working
site you can edit.

## 2. Add your event's identity

Start with these files. Each YAML file includes field notes and examples.

| File | What to change |
| --- | --- |
| `src/_data/event.yaml` | Event name, city, year, date and timezone. Keep ISO dates quoted; update both `date.iso` and `date.label`. |
| `src/_data/social.yaml` | Your community and contact links. |
| `src/_data/venue.yaml` | Venue, address, coordinates, transport links and images. |
| `src/_data/organizers.yaml` | Your organizing team and portraits. |
| `_config.ts` | `location`: your public URL, initially your Cloudflare URL if you don't have a custom domain yet. |
| `wrangler.jsonc` | `name`: a unique Worker name, such as `kcd-yourcity`. |

The starter assumes a known date and venue. If yours are still being confirmed,
adapt the homepage and keep unfinished pages as drafts rather than publishing
fictional details. Keep required data files while pages still read them.

## 3. Replace the branding and page copy

- Put your logo lockups at `src/assets/logo-horizontal-color.svg`,
  `logo-horizontal-white.svg`, `logo-vertical-color.svg` and
  `logo-vertical-white.svg`. For different paths, add `src/_data/logo.yaml`.
- Replace the hero image and update its paths, dimensions and alt text in
  `src/_data/hero.yaml`.
- Replace the share image in `src/_data/share.yaml` with a 1200×630 JPEG or PNG.
- Add `src/favicon.svg` to replace the default KCD wheel.
- Set your colors in `src/style.css`.
- Rewrite the homepage in `src/index.vto`: tagline, introduction, venue copy,
  community links and calls to action. Replace or remove the placeholder video.
- Review each page's front-matter `description`, `src/venue.vto`,
  `src/imprint.vto` and the terms in `src/_data/terms/en.md`.

Some copy lives directly in `.vto` pages, so editing YAML alone will not replace
every “Your City”, example URL or fictional claim. Search `src/` for `Your City`,
`2099`, `example.com`, `example.org`, `#` links and `AAAAAAAAAAA` before publishing.

## 4. Publish only what is ready

Tickets, the attending image, sponsorship, CFP FAQ, speakers and agenda start as
draft pages. `draft: true` in their front matter excludes them from a production
build. Links using their page IDs wait with them.

**Local preview includes drafts.** A page visible in `deno task serve` is not
necessarily published. Remove `draft: true` when its content is ready, then run:

```sh
deno task build
```

The production files are in `_site/`. Data validation reports the file and key
to fix. Check the build output and resolve any template errors as well. If you
delete a page, remove its navigation links and remove only the required data
concepts no remaining page needs from `_config.ts`.

## 5. Get it online

Commit your changes and follow **[Deploying to Cloudflare](deployment.md)**.
Check the deployed homepage, navigation, mobile menu, images and share metadata.

Next: use the **[event update roadmap](event-updates.md)** for each new announcement.
For coding-agent-assisted edits, point the agent at this handbook, the relevant
data files and page templates; have it build the site and review the resulting
diff and preview before publishing.
