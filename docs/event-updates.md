# Updating your event website

[Handbook](README.md) · [Setup](setup.md) · [Recap and archive](after-the-event.md)

The website follows your announcements. You decide when each page and section
goes live; there is no single event-status setting to change.

## Find your next update

The [live demo](https://kcd-website-demo.mark-sagikazar.workers.dev/) shows a complete
fictional event at each milestone. Use its bottom navigation to compare versions.
The order is an example: sponsorship, ticket sales and speaker announcements can
happen in whatever order fits your event.

| Milestone | What to update | Example |
| --- | --- | --- |
| Announce the event | Identity, venue, team, homepage and social links. See [setup](setup.md). | [Announcement](https://kcd-website-demo.mark-sagikazar.workers.dev/01-announced/) |
| Open the CFP | Submission URL, deadline, FAQ, menu and homepage. See [CFP](cfp.md). | [CFP open](https://kcd-website-demo.mark-sagikazar.workers.dev/02-cfp-open/) |
| Invite sponsors | Packages, prospectus, contact details; publish `sponsors.vto`. | [Sponsorship](https://kcd-website-demo.mark-sagikazar.workers.dev/03-sponsorship/) |
| Welcome sponsors | Logos and tiers in `sponsors.yaml`. | [Sponsors](https://kcd-website-demo.mark-sagikazar.workers.dev/04-sponsors/) |
| Announce keynotes | Cards in `keynotes.yaml`, portraits and homepage copy. | [Keynotes](https://kcd-website-demo.mark-sagikazar.workers.dev/05-keynotes/) |
| Open early-bird sales | Shop, ticket page, attending image and pricing copy. See [Pretix](pretix.md). | [Early bird](https://kcd-website-demo.mark-sagikazar.workers.dev/06-early-bird/) |
| Reveal speakers | Import speaker/talk data, publish `speakers.vto`. See [Sessionize](sessionize.md). | [Speakers](https://kcd-website-demo.mark-sagikazar.workers.dev/07-speakers/) |
| Switch to regular tickets | Update Pretix prices and your homepage/ticket wording. | [Tickets](https://kcd-website-demo.mark-sagikazar.workers.dev/08-tickets/) |
| Publish the timetable | Import scheduled talks, verify times/rooms, publish `agenda.vto`. | [Agenda](https://kcd-website-demo.mark-sagikazar.workers.dev/09-agenda/) |
| Thank attendees | Retire sales calls to action and publish a thank-you homepage. | [Wrap-up](https://kcd-website-demo.mark-sagikazar.workers.dev/10-wrap/) |
| Share photos and resources | Publish the gallery, recordings, slides and recap as they arrive. See [after the event](after-the-event.md). | [Photos](https://kcd-website-demo.mark-sagikazar.workers.dev/11-photos/) / [Recap](https://kcd-website-demo.mark-sagikazar.workers.dev/12-recap/) |
| Launch the next edition | Preserve the finished site at `/year/`, put the new edition at `/`. | [Archive guide](after-the-event.md#archive-the-finished-edition-at-year) |

## The publishing workflow

1. Create a branch for the announcement.
2. Update the data **and** page copy. A changed data file does not rewrite a
   headline, a ticket price written in a page or a menu label.
3. Run `deno task serve` to work on it, then `deno task build` to check the
   production output.
4. Push and review the [Cloudflare branch preview](deployment.md#preview-changes-before-publishing).
5. Merge when the announcement is ready. Cloudflare publishes the production build.

### How content becomes visible

- **Pages:** remove `draft: true` from their front matter to publish. Local serve
  includes drafts; production and branch-preview builds exclude them.
- **Links:** `page: agenda` refers to a page's `id`. The menu and theme buttons
  omit it while that page is a draft. Hard-coded URLs do not get this behavior.
- **Sections:** pages check collection lengths directly. For example,
  `keynotes.speakers.length` reveals the keynote section once cards are added.
  The venue and footer render directly; there is no generic section-level
  `enabled` switch. To remove a section, edit the page that owns it.
- **Explicit switches:** `cfp.open` controls CFP content; its external menu link
  has its own `enabled` switch in `nav.yaml`.
- **Deadlines:** timed components change their own copy in the browser, and a
  later rebuild produces the expired state. They do not update external menus,
  Pretix prices or other page copy for you.

Keep unpublished announcements on a branch until they are ready to merge.

## Sponsorship and keynote announcements

For the sponsorship offer, fill `src/_data/sponsorship.yaml`, rewrite the
headline numbers, contact address and prospectus link in `src/sponsors.vto`,
add any prospectus asset, and remove that page's `draft: true`.
`sponsorship.yaml` is page-owned data; it does not have a built-in theme schema,
so inspect the rendered page as well as running a build.

When a sponsor joins, add its logo and tier entry to `src/_data/sponsors.yaml`.
The homepage shows populated tiers. The examples use logos on a 500×300 canvas
for consistent sizing.

Keynote announcements use `src/_data/keynotes.yaml`, independently of the
Sessionize programme. Update these cards when announcing a keynote even if you
also import the talk from Sessionize.

## Adapt an example

Browse [the theme repository](https://github.com/sagikazarmark/kcd-website-theme)
for the demo source under `demo/`. Each milestone is a complete site.
Compare neighboring `src/` directories, then adapt only the files and sections
you need. Keep your own config, assets and event facts.

For layouts rather than event content, use the
[feature showcase](https://kcd-website-showcase.mark-sagikazar.workers.dev/).
