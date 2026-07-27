# Claude Pro Billing Cycle tile

A single-file, embeddable tile that shows where you are in your Claude Pro
billing cycle — days used, days left, and the renewal date. Designed to be
dropped into a Notion page.

Nothing is fetched and nothing is tracked. The tile computes the cycle in your
browser from one config value, so it stays correct on its own.

## Embed in Notion

1. Enable GitHub Pages for this repo: **Settings → Pages → Source: `main` / root**.
2. Copy the published URL, e.g. `https://<user>.github.io/claude-billing-cycle-tile/`
3. In Notion, type `/embed`, paste the URL, and drag the block to about
   120px tall.

### Theme

The tile follows the viewer's light/dark preference automatically. To pin it,
append a query parameter to the embed URL:

- `?theme=dark`
- `?theme=light`

## Configuration

The one value that matters is the day of the month your subscription renews
on. Set it in the embed URL:

```
https://<user>.github.io/claude-billing-cycle-tile/?day=30
```

Keeping it in the URL means your actual billing date lives in your own Notion
page rather than in this public repo. The value is remembered in
`localStorage` once seen, and falls back to the 1st if nothing is set. Months
too short to contain the day (February, for instance) clamp to their last day.

To bake in a default instead, edit `DEFAULT_ANCHOR_DAY` near the top of the
`<script>` block in `index.html`.

Everything else is derived: the cycle runs from the anchor day to the day
before the next anchor day, and the picker in the corner lets you look at the
three previous and two upcoming cycles.

Parameters combine, so a fully specified embed looks like:

```
?day=30&theme=dark
```

## Notes on accuracy

Anthropic exposes no API for Claude Pro subscription billing, so the cycle is
computed from `ANCHOR_DAY` rather than read from your account. If you change
plans or your renewal date moves, update that one value.

Date arithmetic is anchored at noon local time, which keeps day counts correct
across daylight-saving transitions.
