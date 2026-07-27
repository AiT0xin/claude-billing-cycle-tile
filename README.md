# Claude Pro Billing Cycle tile

A single-file, embeddable tile showing where you are in your Claude Pro billing
cycle as a two-month calendar — days already passed are outlined, days still to
come are filled solid, and today is ringed. Designed to be dropped into a
Notion page.

Nothing is fetched and nothing is tracked. The tile computes the cycle in your
browser from one config value, so it stays correct on its own.

## Embed in Notion

1. Enable GitHub Pages for this repo: **Settings → Pages → Source: `main` / root**.
2. Copy the published URL, e.g. `https://<user>.github.io/claude-billing-cycle-tile/`
3. In Notion, type `/embed`, paste the URL, and drag the block to about
   380px tall.

### Theme

The tile follows the viewer's light/dark preference automatically. To pin it,
append a query parameter to the embed URL:

- `?theme=dark`
- `?theme=light`

## Configuration

The one value that matters is the day of the month your subscription renews
on. Set it in the embed URL:

```
https://<user>.github.io/claude-billing-cycle-tile/?day=15
```

Keeping it in the URL means your actual billing date lives in your own Notion
page rather than in this public repo. Months too short to contain the day
(February, for instance) clamp to their last day.

You can also set it from the tile itself with **Edit billing date**, which
saves to `localStorage`.

**The URL wins over the button.** If `?day=` is present it is re-applied on
every load, so an edit made with the button won't survive a refresh. Pick one:
put `?day=` in the embed URL, *or* leave it off and use the button. In Notion
the URL is the reliable choice — embeds render in a sandboxed iframe where
`localStorage` is often unavailable.

To change the built-in fallback, edit `DEFAULT_ANCHOR_DAY` near the top of the
`<script>` block in `index.html`.

Everything else is derived: the cycle runs from the anchor day to the day
before the next anchor day, and the chevrons step through neighbouring cycles.

Parameters combine, so a fully specified embed looks like:

```
?day=15&theme=dark
```

## Notes on accuracy

Anthropic exposes no API for Claude Pro subscription billing, so the cycle is
computed from the renewal day rather than read from your account. If you change
plans or your renewal date moves, update that one value.

Date arithmetic is anchored at noon local time, which keeps day counts correct
across daylight-saving transitions.
