# OG image renderer

Source HTML for the social-share cards at `../../og.png` (EN),
`../../og-ar.png` (AR), and `../../og-fr.png` (FR). All are 1200×630.

## Regenerating

1. Serve this folder locally:

   ```bash
   cd /path/to/Awaab-Public/tools/og-renderer
   python3 -m http.server 8000
   ```

2. Open `http://localhost:8000/og-en.html`, `og-ar.html`, and `og-fr.html`
   in a headless browser at viewport `1200×630` and screenshot to `og.png`
   / `og-ar.png` / `og-fr.png` respectively. Crop to top-left 1200×630 if the
   browser produces a taller canvas:

   ```bash
   ffmpeg -y -i raw.png -vf "crop=1200:630:0:0" og.png
   ```

3. Copy results to the repo root: `cp og.png og-ar.png og-fr.png ../../`.

## Why the bottom-right shows `elshazlyyy.github.io`

The site is hosted at `https://elshazlyyy.github.io/Awaab-Public/`.
The marketing domain `awaab.app` is currently not owned. The OG card
shows the host so the share preview matches the URL someone is
clicking through.

If the project ever moves to a custom domain, update the `.url`
text in both HTML files and regenerate.
