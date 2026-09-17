# GeoPing Live

A browser-based **Location-Based Services (LBS)** app for mobile and desktop. It shows your
live position on an OpenStreetMap basemap, finds real nearby places, and fires geofence
enter/exit alerts as you move. Opens on the **Vijayanagar, Velachery (Chennai)** area and
includes place/POI search.

**Live demo:** _add your GitHub Pages URL here after publishing_

---

## Features

- **Live positioning** — HTML5 Geolocation `watchPosition`; the marker and every distance
  update continuously as you move.
- **Real map** — Leaflet with OpenStreetMap tiles (no API key). Leaflet is embedded in the
  page, so the app has no CDN dependency.
- **Live nearby places** — queried from the **Overpass API** (OpenStreetMap's live database):
  restaurants, cafés, ATMs, pharmacies, hospitals, colleges, shops, hotels, temples, museums,
  and more. Refetches as you move ~150 m or widen the fence.
- **Search** — two kinds in one box:
  1. **Named POI / brand search** via Overpass (e.g. `MedPlus`) within ~8 km of the map focus.
  2. **Place / address geocoding** via OpenStreetMap **Nominatim** (neighbourhoods, roads,
     landmarks).
- **Geofencing** — adjustable radius (100 m – 3 km). Places inside turn green; crossing the
  boundary raises a green *"Entered range"* or red *"Left range"* toast per place.
- **Proximity ranking** — everything measured with the haversine formula; inside/outside counts
  update live.

## LBS concepts demonstrated

| Concept | Where it lives in the code |
|---|---|
| Device positioning | `startTracking()` → `navigator.geolocation.watchPosition` |
| Distance (great-circle) | `haversine(a, b)` |
| Geofencing + enter/exit events | `updateProximity()` compares each place's distance to `st.geofence` |
| Live POI retrieval | `fetchPOIs()` / `buildQuery()` → Overpass API |
| Named-POI search | `searchPOIByName()` → Overpass `name~"…"` regex |
| Address geocoding | `geocodePlaces()` → Nominatim |

## Running locally

Geolocation only works over `https://` or `http://localhost`, so serve the folder rather than
opening the file directly:

```bash
python -m http.server 8000
# then open http://localhost:8000/  in Chrome/Firefox
```

## Deploying to GitHub Pages

1. Create a new repository (e.g. `geoping-live`) on GitHub.
2. Upload `index.html` and `README.md`.
3. Repo **Settings → Pages → Build and deployment → Source: Deploy from a branch**, branch
   `main`, folder `/ (root)`, **Save**.
4. Wait ~1 minute; your app is live at `https://<username>.github.io/geoping-live/`.
   Open it on your phone for full GPS.

## Notes & limitations

- Results reflect **whatever is mapped in OpenStreetMap** — well-known chains and landmarks are
  usually present; some small or new shops may not be. Missing results mean the place isn't in
  OSM yet, not an app bug.
- Overpass and Nominatim are free community endpoints with fair-use rate limits — fine for a
  demo. For guaranteed coverage/quotas, swap in a commercial Places API (Google, Mapbox) — the
  fetch logic is isolated in `fetchPOIs`/`buildQuery`/`searchPOIByName`.

## Tech

Vanilla JavaScript, Leaflet 1.9.4, OpenStreetMap tiles, Overpass API, Nominatim. No build step,
no framework, single self-contained `index.html`.

---

Built by Packiarajan Siva · M.Tech Geoinformatics, University of Madras.
