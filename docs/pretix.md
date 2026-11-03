# Ticket sales with Pretix

[Handbook](README.md) · [Event roadmap](event-updates.md)

The tickets page embeds your Pretix shop and includes a link to open it directly.
Pricing, inventory, sale windows, payments and checkout are managed in Pretix.

## Connect your shop

1. Set up your event and ticket products in Pretix.
2. Replace the demo shop in `src/_data/pretix.yaml`:

   ```yaml
   host: "https://pretix.eu"
   organizer: "your-organizer"
   event: "your-event"
   locale: "en"
   ```

   For a shop at `https://pretix.eu/your-organizer/your-event/`, the last two
   segments are the organizer and event. Use your own `host` for self-hosted
   Pretix. An empty event lists the organizer's events instead of one shop.

3. Set `billingEmail` in `src/_data/tickets.yaml`. Review the actual help links,
   invoice, delivery and refund wording in `src/tickets.vto` against your shop
   settings; adding a data field alone does not add text to the page.
4. Update the terms in `src/_data/terms/` and the operator/ticketing information
   in `src/imprint.vto`.
5. Remove `draft: true` from `src/tickets.vto`. Publish `src/attending.vto` too
   if you want the “I'm attending” image generator, after checking its branding.
6. Update the homepage hero and menu CTA to promote ticket sales. Links using
   `page: tickets` appear once the page is published.

The template's ticket page already includes `comp.pretix.head()` in its head slot
and `comp.pretix.widget()` in its body. If you move the shop to another page,
include both there too. No Pretix API key is needed for this public widget.

## Early bird, regular tickets and sold out

Use the [early-bird](https://kcd-website-demo.mark-sagikazar.workers.dev/06-early-bird/)
and [regular-ticket](https://kcd-website-demo.mark-sagikazar.workers.dev/08-tickets/)
examples for copy and countdown placement. The starter has no global ticket-price
or sale-status switch: update page wording and any countdown when the offer changes.

Configure the actual price change and availability in Pretix. A website countdown
does not change your shop's prices.

When sold out, change the homepage and ticket page messaging. To retain a menu
CTA label without an active link, use:

```yaml
cta:
  label: "Sold out"
  page: tickets
  icon: "fa-ticket"
  disabled: true
```

This disables only the menu CTA. Update other purchase buttons and the shop's
availability as appropriate. After the event, follow the
[wrap-up guide](after-the-event.md).

## Before announcing sales

Build and open the deployed preview. Verify the embedded shop loads the right
event, the direct-shop fallback works, and the published wording matches the
products, prices and contact details in Pretix. Check the attending image on both
desktop and mobile if you publish it.
