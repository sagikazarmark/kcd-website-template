# Opening and closing the call for papers

[Handbook](README.md) · [Event roadmap](event-updates.md) · [Sessionize programme](sessionize.md)

## Open submissions

1. Create your call for papers in Sessionize or your chosen submission service.
2. Edit `src/_data/cfp.yaml`:

   ```yaml
   open: true
   url: "https://sessionize.com/your-event/"
   deadline: "2099-02-28T23:59:59+01:00"
   faq:
     - question: "Can I submit my first conference talk?"
       answer: "Yes! We welcome first-time speakers and can help you prepare."
   ```

   Use your actual deadline and UTC offset. Set the display timezone in
   `event.yaml`. FAQ answers support Markdown.

3. In `src/_data/nav.yaml`, set the **Submit a talk** URL to the same submission
   URL and remove `enabled: false` (or set it to `true`).
4. Remove `draft: true` from `src/cfp-faq.vto` after writing your FAQ.
5. Rewrite the CFP copy and speaker support links in `src/index.vto`, including
   the hero and deadline messages. Build, preview and publish.

The starter homepage already reads `cfp.open` to show the submission button,
countdown and CFP section. The demo's
[open CFP](https://kcd-website-demo.cloudnativehu.workers.dev/02-cfp-open/)
shows the result.

## At the deadline

The homepage's timed copy and buttons switch at `cfp.deadline`. The external
navigation link is separate: set its `enabled` to `false` in `nav.yaml` and deploy.
Make sure your submission service closes on the same schedule.

You can keep `cfp.open: true` temporarily to show the expired “closed” messages.
When you want to retire the whole CFP section and countdown, set `open: false`
and rewrite the hero for the next announcement. Keep the FAQ published if it is
still useful, or restore its `draft: true` to remove it from the build.

For an extension, change the deadline in both the submission service and your
website, update any written dates, and re-enable the menu link if needed.

Opening the CFP does not import accepted talks. That comes later with
**[Sessionize speakers and agenda](sessionize.md)**.
