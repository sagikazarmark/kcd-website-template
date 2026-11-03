# Deploying to Cloudflare

[Handbook](README.md) · [Set up your website](setup.md) · [Event updates](event-updates.md)

Your site is static: Lume builds `_site/`, and **Cloudflare Workers static
assets** serves it. The template includes `wrangler.jsonc`; no Worker application
code is needed.

## Deploy the repository you just created

1. Commit and push your website to **your own GitHub repository**.
2. Set `name` in `wrangler.jsonc` to your Worker name, for example
   `kcd-yourcity`. Use the same name when creating the Worker.
3. In the [Cloudflare dashboard](https://dash.cloudflare.com/), open
   **Workers & Pages**, create a Worker and choose the option to import/connect
   a Git repository. Authorize GitHub and select **your website repository**.
4. Configure Workers Builds:

   | Setting | Value |
   | --- | --- |
   | Production branch | `main` (or your chosen publishing branch) |
   | Root directory | Repository root |
   | Build command | `npm run build` |
   | Deploy command | `npm run deploy` |
   | Preview command | `npm run preview` |

   Cloudflare's build image has Node/npm and installs the dependencies in
   `package.json` (Deno and Wrangler) before building. The `npm run` scripts
   there run `deno task build`, `wrangler deploy` and `wrangler preview`.

5. Deploy and open the resulting `https://<worker>.<account>.workers.dev/` URL.
6. Put that URL in `_config.ts`'s `location` (or use your custom domain below),
   commit and push. Canonical and share URLs are generated from `location`.

The asset directory is already `./_site` in `wrangler.jsonc`, with directory-style
HTML URLs and a 404 page. You don't need a Pages output-directory setting.

## Preview changes before publishing

In **Settings → Build**, enable builds for non-production branches and set the
preview command above. Open a pull request, inspect its preview URL, then merge
to the production branch to publish. Preview builds run `deno task build`, so
they exclude drafts just like production. To review a page's publication, remove
`draft: true` on the branch.

If an existing Worker uses `npx wrangler versions upload`, that produces a
version URL rather than the newer branch Preview. It is also suitable for this
static site; enable version preview URLs in Cloudflare when using that workflow.

## Add your domain

In the Worker's **Settings → Domains & Routes**, add your custom domain and
follow Cloudflare's DNS instructions. Set `_config.ts`'s `location` to the final
HTTPS URL and redeploy. Verify both the domain and page metadata after the change.

## What about a Deploy to Cloudflare button?

Cloudflare's button **clones a public repository into a new repository** before
deploying. If you already created your site from the GitHub template, use the
repository connection above to keep deploying that same repository.

To start by cloning and deploying the starter instead:

[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/sagikazarmark/kcd-website-template)

Set the build command from the table during setup, then clone the repository
Cloudflare creates and follow the [setup guide](setup.md) to personalize it.
This route initially publishes the fictional starter content.

A README button cannot automatically discover the repository it was copied into.
To offer other people a deployable copy of **your** public site, replace the
repository URL in this snippet:

```md
[![Deploy to Cloudflare](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/YOUR-ORG/YOUR-REPO)
```

## Manual deployment

With Node/npm installed, run from your website repository:

```sh
npm install
npm run build
npx wrangler login
npm run deploy
```

For ongoing updates, connecting Git avoids needing to run these commands each time.

## If a build fails

| Symptom | Check |
| --- | --- |
| `deno` not found | Use `npm run build` as the build command, so the `deno` npm package from `package.json` is used. |
| Cannot resolve `kcd-theme/` | An independent repo needs a published URL in `deno.json`; see [theme pinning](customization.md#pinning-and-upgrading-the-theme). |
| `_site` missing | Set a build command and the correct root directory. |
| Missing data or invalid key | Fix the named file in `src/_data/`, then build locally. |
| A page exists locally but not online | Check `draft: true`; local serve includes drafts. |
| Links point at an example domain | Update `location` in `_config.ts` and any hard-coded page links. |

References: [Workers Builds settings](https://developers.cloudflare.com/workers/ci-cd/builds/configuration/),
[deploy button behavior](https://developers.cloudflare.com/workers/platform/deploy-buttons/).
