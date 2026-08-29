# Dunbar field layout — distance measure

`index.html` is a self-contained web version of the **Plan** tab in
`DunbarLayout.xlsx`. Instead of picking a *From* and *To* survey mark from long
drop-down lists, you get a zoomable map of the field and tap the marks directly.

- Pan (drag), zoom (wheel / pinch / the **+ − Fit** buttons).
- Tap a mark, then tap another — the straight-line distance and the X/Y offsets
  appear in the panel. Keep tapping to chain segments and get a running total.
  Tap the last mark again (or **Remove last**) to step back; **Clear** resets.
- Category chips (Landmarks / Starts / Hurdles / Relays) show or hide each group
  (pack-start marks are grouped under Starts). Default is Landmarks only.
- **Find a mark…** searches by label, then centres and selects it.

All geometry is baked into the page (local planar grid, metres; origin at mark
`X`, home straight along +X). No server, no network — it works offline and opens
straight from disk.

## View it

- **GitHub Pages**: enable Pages for this repo (Settings → Pages → deploy from
  `master`, root). The page is then at `https://<user>.github.io/<repo>/`.
- **Locally**: open `index.html` in a browser. Serving over `http://` (e.g.
  `python3 -m http.server`) is more reliable than `file://` in some browsers.

## Regenerating the data

If the workbook changes, rebuild the embedded dataset:

```sh
python3 tools/extract_track.py      # needs: pip install openpyxl
```

This reads `DunbarLayout.xlsx` and writes `track.json` (also printing sanity
checks — e.g. `C → D`, `A → B`, `X → Y` should each be ~76.89 m). Then paste the
contents of `track.json` into `index.html`, replacing the object inside:

```html
<script id="track-data" type="application/json"> … </script>
```

`tools/extract_track.py` pulls:

- survey marks from the `All` sheet (`Label`, `X`, `Y`, category);
- the nine lane "line of running" curves, the finish line and the home-straight
  lines from the `Lanes` sheet (drawn as map context).
