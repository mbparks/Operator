# OPERATOR

Field Instrument 031

A modular console for the amateur radio operator. Nine instruments behind one bezel,
with a day and night panel. Single file, no build step. Online features where they help,
with offline fallbacks so nothing breaks when the network is gone.

## Modules

- GREYLINE (031A): world terminator map with your station plotted under the day and
  night line, subsolar point, and your local sun elevation. Coastlines are real, loaded
  online from Natural Earth and cached for offline use, with coarse outlines as a fallback.
- FOOTPRINT (031B): plots where your signal is landing from reception reports on the same
  world map. Add spots by grid, load a demo footprint, or fetch live from PSK Reporter
  through a CORS proxy.
- SMITH (031C): impedance and reflection. Enter a load and read SWR, return loss,
  reflection coefficient, and mismatch loss, then get a two-element L-network match to
  your system impedance. Match solutions are self-verified before they are shown.
- FEEDLINE (031D): cut dipoles, verticals, and end-fed wires to length, and estimate
  coax loss by type, length, frequency, and load SWR, including power actually delivered.
- AZIMUTH (031E): grid to grid distance, short-path and long-path beam headings, reverse
  heading, and a compass rose pointing the way.
- REPEATER (031F): find repeaters near your position on a real street map, click any one
  for programming and connection details, and keep a net schedule for it. The map tiles
  follow your day and night setting. A north-up plan scope is the offline fallback when
  tiles cannot load. Position comes from GPS, your grid, or typed coordinates. Repeater
  data comes from RepeaterBook through a CORS proxy, filtered to the states nearest you and
  sorted by distance. A sample set is bundled so the module works offline.
- TELEGRAPH (031G): send text as Morse with adjustable speed, Farnsworth spacing, and
  tone, watch the rhythm light up in time, or hold the key and listen.
- LEXICON (031H): Q codes, phonetics, prosigns, RST, and common abbreviations, searchable.
- LOGBOOK (031I): log contacts, auto-fill the band from frequency, and export ADIF or CSV.

## Operating notes

- Set your callsign and grid square in the top status rail. GREYLINE, FOOTPRINT, and
  AZIMUTH all use your grid as the origin.
- The day and night switch lives in the top right. Night is the default.
- LOGBOOK and your station settings persist in the browser through localStorage. When the
  page is hosted normally this just works. In a sandboxed preview that blocks storage, the
  instrument falls back to memory for the session and loses nothing while it is open.
- FOOTPRINT live fetch needs a permissive CORS proxy because PSK Reporter does not allow
  direct browser requests. Paste a proxy prefix that takes a URL parameter, for example
  the Cloudflare Worker pattern ending in ?url=. Without a proxy, work by hand or load the
  demo footprint.
- REPEATER pulls live data from RepeaterBook, which also blocks direct browser requests, so
  it uses the same CORS proxy field. The proxy is saved once and shared with FOOTPRINT. The
  states nearest your position are detected from a built-in bounding-box table and filled in
  for you, with a control to widen how many states are pulled. Results are cached per state
  for seven days so the module keeps working offline. Net schedules are scarce in open data:
  any net mentioned in a repeater's RepeaterBook notes is surfaced automatically, and you can
  add your own net entries that persist for that repeater. RepeaterBook asks that requests
  carry a descriptive User-Agent, so set one in your proxy, and the on-air data is credited to
  RepeaterBook in the interface.

## Online and offline

OPERATOR uses online data where it adds value, and falls back cleanly when offline.

- Map tiles for REPEATER come from CARTO (dark at night, light by day). If they cannot
  load, REPEATER switches to the plan scope automatically.
- Coastlines for GREYLINE and FOOTPRINT load once from Natural Earth and are cached in the
  browser, so they stay sharp offline after the first online visit. If they never load, the
  built-in coarse outlines are used.
- Repeater data and reception reports still need a CORS proxy, and both cache locally.
- Everything else, the math instruments, TELEGRAPH, LEXICON, and LOGBOOK, is fully offline.

## Data sources and credits

- RepeaterBook for repeater locations, programming, nodes, and notes (REPEATER).
- PSK Reporter for reception reports (FOOTPRINT).
- Map tiles by CARTO, map data by OpenStreetMap contributors (REPEATER).
- Coastlines from Natural Earth via the natural-earth-geojson project (GREYLINE, FOOTPRINT).
- Browser geolocation for GPS position (REPEATER).
- Local solar math for the terminator and sun elevation (GREYLINE).
- Your own annotations for net schedules, saved in the browser (REPEATER).

## Conventions

- Single file HTML, no build step.
- Online features where they help, with offline fallbacks so nothing breaks.
- Dark mode default with a day mode toggle.
- No em dashes anywhere in prose or code.
- Vintage scientific instrument aesthetic.

## Validation performed

- JavaScript syntax checked, both per file and assembled.
- Em dash, en dash, and horizontal bar sweep returned none.
- Core math unit tested: grid to lat and lon, round trip, great circle distance and
  bearing, and subsolar point against known references.
- Smith L-network solver confirmed to return self-verifying matches across several loads.
- Nearest-state detection checked against several border locations.
- Coastline data verified to load, project inside the canvas, and split cleanly at the
  antimeridian.

## License

GPL-3.0

This program is free software: you can redistribute it and/or modify it under the terms
of the GNU General Public License as published by the Free Software Foundation, either
version 3 of the License, or (at your option) any later version. This program is
distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY, without even
the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the GNU
General Public License for more details. You should have received a copy of the GNU
General Public License along with this program. If not, see the GNU licenses page at
gnu.org.
