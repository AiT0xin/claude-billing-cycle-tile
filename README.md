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
   **460px** tall.

The tile itself is only ~120px, but the date picker opens downwards and
Notion's iframe clips anything past the block's edge. The page background is
transparent, so the extra room below is invisible until the picker is open.

### Theme

The tile follows the viewer's light/dark preference automatically. To pin it,
append a query parameter to the embed URL:

- `?theme=dark`
- `?theme=light`

**Triple-click the tile** to cycle its background: normal → solid `#161616`
→ transparent → back. Solid pins the dark palette and fills the whole embed;
transparent keeps following the viewer's theme, so it stays readable on a
light or dark page. The choice is saved in `localStorage`, which Notion's
sandboxed iframe may not persist — reach for `?theme=` when it must stick.

## Configuration

The one value that matters is the day of the month your subscription renews
on. Set it in the embed URL:

```
https://<user>.github.io/claude-billing-cycle-tile/?day=15
```

Keeping it in the URL means your actual billing date lives in your own Notion
page rather than in this public repo. The value is remembered in
`localStorage` once seen, and falls back to the 1st if nothing is set. Months
too short to contain the day (February, for instance) clamp to their last day.

To bake in a default instead, edit `DEFAULT_ANCHOR_DAY` near the top of the
`<script>` block in `index.html`.

You can also set it from the tile: click the date range in the corner to open
a two-month calendar and pick any date. The day-of-month you click becomes the
renewal day, and the current cycle is highlighted so you can see what you are
choosing.

**The URL wins over the picker.** If `?day=` is present it is re-applied on
every load, so a date chosen in the calendar won't survive a refresh. Pick one:
put `?day=` in the embed URL, *or* leave it off and use the picker. In Notion
the URL is the reliable choice — embeds render in a sandboxed iframe where
`localStorage` is often unavailable.

Everything else is derived: the cycle runs from the anchor day to the day
before the next anchor day.

Parameters combine, so a fully specified embed looks like:

```
?day=15&theme=dark
```

## Notes on accuracy

Anthropic exposes no API for Claude Pro subscription billing, so the cycle is
computed from `ANCHOR_DAY` rather than read from your account. If you change
plans or your renewal date moves, update that one value.

Date arithmetic is anchored at noon local time, which keeps day counts correct
across daylight-saving transitions.
