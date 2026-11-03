# KCD website template

Launch your **Kubernetes Community Day** website, then keep it current from the
first announcement through the final recap. This starter is the fictional
**KCD Your City 2099**, ready for your event's content and branding.

**[🌍 Live demo](https://kcd-website-demo.mark-sagikazar.workers.dev/)** ·
**[🎨 Feature showcase](https://kcd-website-showcase.mark-sagikazar.workers.dev/)** ·
**[🚀 Use this template](https://github.com/sagikazarmark/kcd-website-template/generate)**

[Preview the starter](https://kcd-website-template.mark-sagikazar.workers.dev/) or
[browse the template repository](https://github.com/sagikazarmark/kcd-website-template).

## Highlights

- 🗓️ **Ready-made event milestones:** announcement, CFP, sponsorship, tickets,
  speakers, agenda and recap examples.
- 🎟️ **Pretix ticket shop** embedded in your site.
- 🎤 **Sessionize imports** for speakers, talks, timetable and photos.
- 🎨 **Tailwind CSS v4** for your colors and page layouts.
- 🤖 **Coding-agent-friendly editing:** commented YAML and page templates, with
  content validation on every build.
- ☁️ **Cloudflare deployment** from your own Git repository.

## Get your first preview

1. Click **[Use this template](https://github.com/sagikazarmark/kcd-website-template/generate)**,
   create your repository and clone it.
2. Install [Deno 2.8](https://docs.deno.com/runtime/getting_started/installation/)
   (the deployment guide pins 2.8.3).
3. From your repository, run:

   ```sh
   deno task serve
   ```

4. Open <http://localhost:3000/>. Change `src/_data/event.yaml` and see it reload.
5. Follow **[Setting up a website](docs/setup.md)**, then
   **[Deploying to Cloudflare](docs/deployment.md)**.

Run all commands from the root of your new website repository.

## Your organizer handbook

| When you need to… | Guide |
| --- | --- |
| Put your name, date, venue and branding on the site | [Set up your website](docs/setup.md) |
| Get it online and review changes before publishing | [Deploy to Cloudflare](docs/deployment.md) |
| Keep the site in step with event planning | [Update your event website](docs/event-updates.md) |
| Invite talk proposals | [Open the call for papers](docs/cfp.md) |
| Sell tickets | [Connect Pretix](docs/pretix.md) |
| Announce speakers and publish the timetable | [Import from Sessionize](docs/sessionize.md) |
| Share photos and recordings, then prepare next year | [Recap and archive](docs/after-the-event.md) |
| Change layouts, colors or the theme version | [Customize your site](docs/customization.md) |

[All documentation](docs/README.md) ·
[Theme repository and technical reference](https://github.com/sagikazarmark/kcd-website-theme)
