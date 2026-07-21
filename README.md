# Treasure Hunt — Map Overlay & Change Finder

Treasure Hunt is a single-file web app for laying scanned or exported maps over a live
reference map, extracting marked features (like hand-drawn rivers) by colour, lining
everything up to real-world coordinates, and tracing features into exportable GeoJSON.

It runs entirely in your browser. There is nothing to install — it is one HTML file.

---

## What it's for

If you have an **old map** (a scanned aerial photo, a JPG, a PDF) and a **newer map**
(a GeoJSON file), Treasure Hunt lets you superimpose one on the other like tracing paper on
a light table, so you can see how water bodies, coastlines, or roads have changed over time.
You can stack several old maps at once, isolate their coloured markings, align each to true
coordinates, and trace the results into a clean vector file you can open in QGIS.

---

## Getting started

1. Open `treasure-hunt.html` in a modern browser (Chrome, Edge, Firefox, or Safari).
2. Click **Reference (GeoJSON)** in the top bar and load your newer map. The view zooms to fit it.
3. Click **+ Add maps** and select one or more image/PDF files. Each becomes a *layer*.
4. Use the tools (bottom-left) and the layer panel (right) to overlay, extract, align, and trace.

The app needs internet only to fetch the background satellite/street tiles. Your own files
never leave your computer.

---

## The interface

- **Top bar** — load data, open the Claude helper, switch the basemap (Satellite / Streets / None).
- **Tools dock (bottom-left)** — the five tools, a live cursor coordinate readout, and a hint.
- **Layers panel (top-right)** — the list of loaded maps and the settings for the active one.
- **Map** — the workspace. Overlaid maps sit above the basemap; traced shapes sit above everything.

---

## Loading data

**Reference map (GeoJSON).** Loads your baseline map — the "truth" everything is aligned against.

**Overlay maps (JPG / PNG / PDF).** **+ Add maps** accepts several files at once. Images load
directly; PDFs are rasterised in the browser. Each file becomes its own layer.

> Note: PDFs currently import **page 1 only**. Split the PDF first if you need every page.

---

## Layers

Every overlay map appears as a row in the **Layers** panel:

- **Colour swatch** — the colour this map's extracted markings are shown in.
- **Eye icon** — show or hide the layer without deleting it.
- **Name** — click to make the layer *active*. Only the active layer can be dragged or edited.
- **X** — remove the layer.

Layers stack in the order you add them (newer on top).

---

## Layer settings

| Control | What it does |
|---|---|
| Fade | Opacity of the whole layer — your light-table dimmer. |
| Scale | Enlarge or shrink the map over the ground. |
| Rotate | Spin the map to match orientation. |
| Centre here | Snap the layer's centre to the middle of the current view. |
| Align to map | Start the control-point alignment wizard. |

### Extract markings

Isolate coloured features (e.g. deep-blue rivers) from the rest of the map.

- **Mode:** *Map* (original), *Marks only* (just the extracted colour), or *Both*.
- **Colours to extract:** click **Pick a colour from this map**, then click a marking to
  sample it. Add several shades — scanned colours are rarely uniform. Each appears as a
  removable chip.
- **Tolerance:** raise it if parts of a river drop out; lower it if stray pixels get caught.
- **Show marks in:** the output colour. Give each map its own colour so overlaps stay readable.

---

## Tools

**Move.** Drag the active layer around the map to position it by eye.

**Pick colour.** Click markings on the active layer to add them as extraction colours.

**Coordinates.** Shows live latitude/longitude of your cursor. Click to drop a draggable pin
with exact coordinates to six decimals — the true earth position at that spot.

**Align (control-point georeferencing).** Scales, rotates, and places an old map onto real
coordinates from two matched landmarks:

1. Select the layer, then click **Align**.
2. Click a landmark on the old map → the same point on the basemap → a second landmark far
   from the first → its match on the basemap.
3. The app snaps the layer into place.

> Position and scale are always solved correctly. If a layer comes out rotated the wrong way,
> flip it with the **Rotate** slider.

**Draw.** Trace features by hand over the aligned map. Because you draw *on the map*, every
point is real latitude/longitude — already georeferenced.

1. Choose **Line - river** or **Area - lake**, and a colour.
2. Click along the feature to drop vertices.
3. **Undo point** / **Finish** (or Enter) / **Esc** to discard.
4. **Export** downloads a `.geojson`; **Clear** wipes all shapes.

Each exported shape keeps its colour and kind as properties and opens in QGIS in the correct
location.

---

## Ask Claude (alignment helper)

An advisory chat for workflow guidance — good control points, reading a poor fit, tuning
extraction. It does not move your maps.

> The live assistant connects only when the app runs **inside Claude's own environment**.
> Hosted as a plain web page it shows an offline note instead. Every other feature works
> fully offline.

---

## Known limitations

- PDFs import page 1 only.
- Alignment rotation may occasionally need a manual flip.
- Ask Claude is offline on static hosting.
- Overlaying by eye is for *visual* comparison. For *measured* change, align with control
  points first, then trace and export.

---

## Built with

- [Leaflet](https://leafletjs.com/) — interactive mapping
- [pdf.js](https://mozilla.github.io/pdf.js/) — in-browser PDF rasterising
- Esri World Imagery and OpenStreetMap tiles
