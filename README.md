# Amazon Sales Estimator - Stack Influence

Free Amazon sales estimator. Pure client-side (no API key, no serverless function).
Deploys to Netlify; embeds into Webflow via iframe with auto-resize.

## How it works

- `index.html` - the whole tool: UI plus the BSR-to-sales math, all in one file
- `netlify.toml` - tells Netlify to publish the current directory

There is no backend and no API key. The estimate runs entirely in the browser
using a blended, category-agnostic BSR-to-sales curve (anchor points interpolated
on a log-log scale). Same BSR and price always return the same numbers.

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

The model lives in the `ANCHORS` array near the top of the `<script>` block in
`index.html`. Each entry is `[BSR, monthly units]`. Add, remove, or adjust anchor
points to reshape the curve; everything between anchors is interpolated
automatically. If you later add category selection, swap in a different anchor
set per category.
