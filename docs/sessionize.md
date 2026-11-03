# Speakers and agenda with Sessionize

[Handbook](README.md) · [Event roadmap](event-updates.md) · [CFP](cfp.md)

Sessionize supplies the programme; your site stores a local copy. A normal site
build never contacts Sessionize. Run an import whenever the accepted talks,
speaker details or timetable change, then review and commit the result.

## Import your event

1. In Sessionize, configure your event's **API/Embed** endpoint to expose the
   speakers and sessions you want to publish.
2. Copy the endpoint ID from
   `https://sessionize.com/api/v2/<endpoint>/view/All` into
   `src/_data/sessionize.yaml`:

   ```yaml
   endpoint: "your-endpoint-id"
   ```

   Replace the starter's sample endpoint, not just its CFP submission URL.

3. Run from your website repository:

   ```sh
   deno task sessionize
   deno task build
   ```

4. Review the changes to:
   - `src/_data/agenda.yaml`: rooms, sessions and breaks.
   - `src/_data/speakers.yaml`: speaker profiles.
   - `src/assets/speakers/`: mirrored profile photos.
5. Commit those files together with the announcement's page changes.

The import overwrites the two generated YAML files. It also removes previously
mirrored portraits no longer used by a speaker, leaving unrelated files alone.

## Publish speakers before the timetable

Remove `draft: true` from `src/speakers.vto` when the speaker announcement is
ready. Keep `src/agenda.vto` as a draft until the timetable is ready.

Speaker listings are derived from talks: make sure the imported programme
includes the accepted sessions and their speaker IDs. Review the
[speakers example](https://kcd-website-demo.mark-sagikazar.workers.dev/07-speakers/).
The homepage's keynote cards are separate content in `keynotes.yaml`.

## Publish the agenda

Set rooms and talk times in Sessionize, run the import again, then verify the
programme in your preview. Times in `agenda.yaml` are event-local times; check
them against `event.yaml` and your published date/timezone.

Remove `draft: true` from `src/agenda.vto`. The page's navigation, cross-links
and homepage teaser can now appear. Check every room, break and session against
the schedule before merging.

For Sessionize flags, add checkbox questions named **Keynote** and **Cancelled**
to the session form and include their answers in the API output. The converter
maps these to talk flags. You can also set flags through local patches.

## Keep corrections between imports

Put changes that must survive a re-import in `src/_data/patches/agenda.yaml` or
`src/_data/patches/speakers.yaml`. Use the imported IDs, keeping their type and
spelling; the following examples assume a session `talk-42` and speaker `speaker-7`.

`src/_data/patches/agenda.yaml`:

```yaml
sessions:
  - id: "talk-42"
    title: "A corrected talk title"
    cancelled: true
```

`src/_data/patches/speakers.yaml`:

```yaml
- id: "speaker-7"
  photo: "/assets/speakers/custom-portrait.jpg"
```

Add the portrait file too. Patches are partial records matched on `id`; supplied
fields replace the corresponding values, and `null` clears an optional field.
Unknown IDs and misspelled keys fail validation. If an import removes a record,
remove or update its patch as well.

A cancelled talk remains marked in its timetable slot and is excluded from the
homepage teaser. A speaker with only cancelled talks is not listed.

## Without Sessionize

You can maintain `agenda.yaml` and `speakers.yaml` directly. Their field comments
describe the format, and the demo provides populated examples. Keep unique IDs,
valid speaker/room references and start times before end times. The build checks
these relationships after applying patches. Remove `sessionize.yaml` if unused
and do not run the import task over hand-maintained files.
