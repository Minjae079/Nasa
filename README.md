# SARSCOPE — interactive 3D Earth explorer (learning prototype)

A **working interactive globe + detailed map prototype**. It demonstrates the user interaction requested for the NASA Space Apps **Dancing with the SARs** idea, but it is **not a validated SAR change-detection tool** and should not be represented as a finished contest submission.

## Features

- Drag the Earth to rotate it; scroll to zoom.
- Click anywhere on the globe for a camera fly-in and a detailed local map.
- Use the **Explore Illinois Tech** shortcut to fly to IIT's campus in Chicago.
- Toggle **Streets** / **Satellite** basemaps; pan and zoom the location.
- Use the **Back to globe**, **Reset**, and zoom controls.
- The SAR panel honestly shows that measured change detection has **not yet been loaded**.
- A clearly separated configuration hook exists for **future, verified georeferenced before/after SAR rasters**.

## Running locally

Requires internet access: JS libraries, globe texture and map tiles load from CDNs and tile providers.

```bash
cd sarscope_globe_demo
python3 -m http.server 8000
```

Open http://localhost:8000 in your browser. You may also preview `index.html` by opening it directly, but using a local server is recommended for future raster configuration.

To deploy later, upload this directory to any static site host (Netlify, Vercel static, GitHub Pages). No API key is required for this prototype.

## Stack

- [Globe.GL](https://globe.gl/) — 3D globe, rotation and `onGlobeClick`, `pointOfView` fly-to interaction.
- [Leaflet](https://leafletjs.com/) — detailed, zoomable 2D local map.
- [OpenStreetMap](https://www.openstreetmap.org/copyright) map tiles and Esri World Imagery for reference basemaps. Respect provider usage terms and required attribution.
- Globe texture from Globe.GL/three-globe examples (Blue Marble appearance).

## Connecting real NISAR data, *after* validating rasters

The site does not fetch large HDF5 files in the visitor's browser. Instead:

1. Find two compatible NISAR GCOV observations for the same location / orbit / polarization / processing version.
2. Extract and reproject the same geographical AOI. Ensure values, masks, radar geometry and pixel grids are comparable.
3. Render 32-bit radar numerical results into matched, **north-up**, **EPSG:4326** image overlays for *visualization*. Preserve numeric rasters outside the browser for actual analysis.
4. Copy `data/sar_layers.example.json` to `data/sar_layers.json`; replace the image URLs, time stamps and bounds with real outputs. The web app checks for this file and enables an opacity comparison slider when files are available.
5. Validate any change-detection claims against environmental/geospatial data; do not infer flooding or construction directly from a simple intensity difference.

The bounds in the example file are **placeholders**, not a validated footprint.

## Competition timing

NASA Space Apps 2026 runs **November 14–15, 2026**. Its historical FAQ instructs participants to begin actual challenge project work only when the hackathon starts. This prototype is a general technology exercise to understand globe interactions. **Review the 2026 rules and ask your local organizers before reusing pre-event code or design in an official entry.**

## Files

- `index.html`: page elements, information cards, navigation
- `styles.css`: responsive dark UI
- `app.js`: globe interaction, map behavior, optional raster layer hook
- `data/sar_layers.example.json`: template for adding **real** imagery later
