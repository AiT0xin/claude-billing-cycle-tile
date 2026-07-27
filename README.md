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

There are two cycle shapes:

- **Same day every month** — the classic monthly subscription. Length varies
  28–31 days as the calendar does.
- **Custom range** — a fixed-length cycle tiled from an explicit start date,
  for anything that doesn't follow the calendar month.

### From the tile

Click the date range in the corner to open a two-month calendar.

- **Click one date, then a different date** — sets a custom cycle running
  between them (inclusive), previewed live as you hover the second date
  before you commit.
- **Click one date, then click it again** — sets the simple "renews on this
  day every month" cycle. This is the fast path if you don't need a custom
  length: one date, clicked twice.

Either way the current cycle is highlighted first, so you can see what you're
about to replace before you click.

### From the URL

```
https://<user>.github.io/claude-billing-cycle-tile/?day=15
```
```
https://<user>.github.io/claude-billing-cycle-tile/?start=2026-07-05&end=2026-08-03
```

Keeping it in the URL means your actual billing dates live in your own Notion
page rather than in this public repo. `?start=`/`?end=` are ISO dates
(`YYYY-MM-DD`) and together set a custom range, same as picking two dates in
the tile. `?day=` falls back to the 1st if nothing is set, and clamps in
months too short to contain it (February, for instance).

**The URL wins over the picker.** Either `?day=` or `?start=`/`?end=` is
re-applied on every load, so a choice made in the calendar won't survive a
refresh while the parameter is present. Pick one: keep the parameter in the
embed URL, *or* drop it and use the picker. In Notion the URL is the reliable
choice — embeds render in a sandboxed iframe where `localStorage` is often
unavailable.

To bake in a different default, edit `DEFAULT_ANCHOR_DAY` near the top of the
`<script>` block in `index.html`.

Parameters combine with the theme override, e.g.:

```
?day=15&theme=dark
```

## Notes on accuracy

Anthropic exposes no API for Claude Pro subscription billing, so the cycle is
computed from `ANCHOR_DAY` rather than read from your account. If you change
plans or your renewal date moves, update that one value.

Date arithmetic is anchored at noon local time, which keeps day counts correct
across daylight-saving transitions.
