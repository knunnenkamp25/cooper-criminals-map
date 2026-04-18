# NC Released Inmates — County Map

Interactive map of recently released North Carolina inmates plotted by county of most recent conviction. Built from public NC Department of Adult Correction records.

## What's in the page

- **County shading view** — counties shaded by how many inmates from your dataset were convicted there.
- **Per-inmate dots view** — every inmate gets a colored dot, jittered inside their county boundary. Dot color = tier (homicide / sex / violent / other). Dots are not real addresses; NC DAC does not publish residence.
- **Filters** — toggle tiers on/off; show only inmates who committed a new offense after release (recidivists).
- **Sidebar** — clicking a county filters the list to that county. Each card shows tier, original offense, post-re-entry offense (if any), and a link to the source NC DAC record.

## Files

- `index.html` — the entire app (Leaflet + topojson-client, single page).
- `inmates-data.js` — 960 inmate records. Sets `window.__INMATES` to an array of records. Fields: `n` (inmate #), `f` (full name), `t` (tier 1–4), `tl` (tier label), `o` (original offense), `p` (post-re-entry offense), `s` (inmate status), `ps` (parole status), `c` (county of conviction), `nc` (# convictions on record), `oc` (other counties on record), `r` (recidivist boolean).
- `README.md` — this file.

The map fetches NC county boundaries at page load from `cdn.jsdelivr.net/npm/us-atlas` (~700KB once, then cached). The data file is loaded as a `<script>` (not `fetch`) so you can test the page by opening `index.html` directly from Finder without running a local server.

## Deploy with GitHub Desktop + GitHub Pages

1. **Create a new repo** in GitHub Desktop: File → New repository. Name it whatever you like (e.g. `nc-inmate-map`). Pick a local path.
2. **Drop these three files** into the repo's local folder.
3. In GitHub Desktop, write a commit message ("initial map") and click **Commit to main**, then **Publish repository** (uncheck "Keep code private" if you want the page public, which is required for free GitHub Pages).
4. On github.com, open your repo → **Settings** → **Pages**. Under "Build and deployment", set Source = "Deploy from a branch", Branch = `main`, folder = `/ (root)`. Click Save.
5. Wait ~30 seconds, then your site will be at `https://<your-username>.github.io/<repo-name>/`.

## Updating the data

When you re-pull NC DAC data:

1. Replace `inmates-data.js` with a new version using the same `window.__INMATES = [...]` shape.
2. Commit and push via GitHub Desktop. GitHub Pages re-deploys automatically.

## Source & caveats

- Data source: [NC Department of Adult Correction Offender Public Information](https://webapps.doc.state.nc.us/opi/offendersearch.do?method=view).
- The county shown for each inmate is the **county of most recent conviction**, not residence. NC DAC does not publish home addresses.
- Dot positions on the per-inmate view are **jittered around county centroids** for readability — they are not real addresses.
- "Recidivist" = an inmate's `Post-Re-Entry Offense(s)` field is populated with something other than blank or "N/A".
