# Amazon Sales Estimator - Stack Influence

Free Amazon sales estimator. Pure client-side (no API key, no serverless function).
Deploys to Netlify; embeds into Webflow via iframe with auto-resize.

## How it works

- `index.html` - the whole tool: UI plus the BSR-to-sales math, all in one file
- `netlify.toml` - tells Netlify to publish the current directory

There is no backend and no API key. The estimate runs entirely in the browser
using a BSR-to-sales curve calibrated to a mid-sized high-velocity US category
(Home & Kitchen), then scaled by a per-category multiplier so the same BSR yields
different volumes across categories. Anchor points are interpolated on a log-log
scale. Same BSR, price, and category always return the same numbers. Results are
shown as a range (about +/-30%, widening to +/-50% at extreme ranks) rather than a
single false-precise figure, because Amazon does not publish unit sales.

This is the same deploy pattern as the Amazon FBA Profit Calculator, minus the
serverless function, since nothing here needs a secret key.

## Deploy

1. Push these files to a GitHub repo.
2. In Netlify: **Add new site -> Import an existing project ->** pick the repo.
3. Leave build command empty. Publish directory: `.` (already set in netlify.toml).
4. Deploy. Netlify gives you a URL like `https://your-site.netlify.app`.

No environment variables are required.

## Embed in Webflow

Paste the iframe snippet (see `webflow-embed-snippet.html`) into a Webflow Embed
element, swapping in your real Netlify URL in both places. The tool auto-resizes,
so there are no scrollbars.

## Tuning the estimate

Two levers, both near the top of the `<script>` block in `index.html`:

- `ANCHORS` - the base curve as `[BSR, monthly units]` pairs, calibrated to Home &
  Kitchen. Everything between anchors is interpolated automatically.
- `CATEGORIES` - per-category velocity multipliers applied to the base curve. Each
  entry is `{ id, t, m }` where `m` scales base units (Home & Kitchen = 1.00, Books
  = 0.16, Electronics = 1.45, etc.). Adjust `m` to reshape a category, or add a new
  entry to extend the list.

The +/-30% display band (and the +/-50% widening at very low/high ranks) is set in
the `estimate()` function.
