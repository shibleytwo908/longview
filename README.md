# Longview

A medium-range weather outlook for anywhere in Europe. Single-page app, no
build step, no backend, no API key — same recipe as CarbIn (formerly Grid
Watt).

## What it does

- Search any place by name (Open-Meteo's free geocoding API) or use your
  current location.
- Switch the forecast source between Open-Meteo's "best match" blend and
  named national models — ECMWF IFS HRES, DWD ICON, NOAA GFS, Météo-France,
  UK Met Office, MeteoSwiss ICON, KNMI, DMI, MET Norway, GEM, JMA — via the
  dropdown in the header.
- iOS Weather-style layout: current conditions, a scrolling 48-hour strip,
  and a 16-day outlook with low–high range bars.
- The footer always states which model produced the numbers on screen, plus
  the last-updated time.

All data comes from [Open-Meteo](https://open-meteo.com) — the geocoding API
(`geocoding-api.open-meteo.com`) and the forecast API
(`api.open-meteo.com/v1/forecast`), fetched directly from the browser. No key
required, and nothing is sent to any other server.

## Files

```
longview/
├── index.html       — the app (HTML, CSS, JS in one file)
├── manifest.json     — PWA manifest (installable, home-screen icon)
├── icons/
│   ├── icon-192.png
│   ├── icon-512.png
│   ├── apple-touch-icon.png
│   ├── favicon-32.png
│   └── favicon-16.png
└── README.md          — this file
```

## Running it locally

No build step. Either:

- Open `index.html` directly in a browser, or
- Serve the folder so the manifest and icons resolve correctly:
  ```
  npx serve .
  ```

## Deploying

Same as CarbIn — drag-and-drop the `longview/` folder onto Netlify (or any
static host). Because the manifest's `start_url` and icon paths are
relative, the whole folder needs to be deployed together — don't upload
`index.html` on its own if you want the installable/PWA behaviour.

## Notes on model coverage

Some national models only forecast their home region (e.g. MeteoSwiss ICON
is only meaningful over Switzerland and nearby Alpine areas). If you pick a
model that doesn't cover the selected location, Open-Meteo's API returns an
error, which the app shows as a plain on-screen message rather than failing
silently.

## Limits

- Forecast horizon is 16 days (Open-Meteo's maximum for the daily block);
  medium-range accuracy — especially for named models other than the
  ensemble blend — drops off past about day 7–10.
- The geocoding search isn't restricted to Europe; place names elsewhere in
  the world will also resolve and forecast correctly, since Open-Meteo's
  coverage is global.
