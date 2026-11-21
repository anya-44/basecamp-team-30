# Data Cleaning Log — London Road Speed Limits

## Why we chose this dataset

We need the speed limit on every road around London primary schools, to test whether schools surrounded by more 20mph roads have fewer child pedestrian collisions.

- **Reliable:** Ordnance Survey National Geographic Database (OS NGD), May 2026, *Average And Indicative Speed* layer. Speed limits are supplied by Basemap, an OS partner (549,422 road links from Basemap, 2,939 from Ordnance Survey and Basemap combined). Average speeds come from in-vehicle GPS (telematics) data.
- **Suitable:** one row per road link, with its speed limit, its road line, its length, its road type and average traffic speeds at school-run times (Mon–Fri 7–9am and 2–4pm).
- **Columns used:** 12 of the layer's 71 columns (ID, name, road type, speed limit and source, school-run average speeds, length, geometry).
- **Area:** Greater London plus a 400m margin, so schools near the border keep all their nearby roads. The London boundary comes from OpenStreetMap (© OpenStreetMap contributors).
- **Feasible:** free for UCL students through Digimap; one 640 MB GeoPackage covering Greater London.
- **Gaps:** speed limits are *indicative* (the limit that applies over most of each road link) and reflect May 2026, so roads that changed to 20mph during 2021–2025 show their current limit. Average speeds are historic averages, and on quiet roads they may be based on few vehicles.
- **Licence:** educational use only; the data itself is not on GitHub. Contains OS data © Crown copyright and database rights 2026 Ordnance Survey (100025252).

## Cleaning steps

|  | Action | Reason | Rows affected |
|---|---|---|---|
| 1 | Kept only road links within Greater London plus 400m | Download area included parts of Essex, Kent and Surrey; the 400m margin keeps roads near border schools | 118,440 |
| 2 | Checked `indicativespeedlimit_mph` for blanks | A road with no limit can't be counted | 0 |
| 3 | Removed repeated road links (same `osid`) | Each road should be counted once | 0 |
| 4 | Set speed limits that aren't a UK limit to blank (kept 20–70mph, plus 5/10/15mph private-road limits) | Remove only values that can't be true | 0 |
| 5 | Averaged both directions of Mon–Fri 7–9am average speed and converted kph to mph | One comparable speed per road, in the same units as the limits | 433,921 |
| 6 | Set Mon–Fri 7–9am average speeds of 0mph or below, or over 90mph, to blank | Impossible for a London road | 0 |
| 7 | Kept roads with no Mon–Fri 7–9am average speed, leaving it blank | Missing traffic data doesn't mean the road isn't there | 0 |
| 8 | Averaged both directions of Mon–Fri 2–4pm average speed and converted kph to mph | One comparable speed per road, in the same units as the limits | 433,921 |
| 9 | Set Mon–Fri 2–4pm average speeds of 0mph or below, or over 90mph, to blank | Impossible for a London road | 0 |
| 10 | Kept roads with no Mon–Fri 2–4pm average speed, leaving it blank | Missing traffic data doesn't mean the road isn't there | 0 |
