# Changelog

## osm-liberty-dark 1.0.0

Dark variant of [OSM Liberty](https://github.com/maputnik/osm-liberty) (based on upstream
`gh-pages` at `40a0d19`), meant as the background for a route overlay in a dark web UI.

Only paint properties and icon colors changed. Every layer, filter, layout property, zoom range,
`source-layer` and the layer order are identical to OSM Liberty, so the level of detail and the
label density are unchanged. Three paint keys were added where OSM Liberty relied on defaults
that only work on a light map: `road_shield` `text-color` (default black), `road_label`
`text-halo-color` (default transparent) and two dimming keys on the `natural_earth` raster.

### Style metadata

- `name` / `id`: `osm-liberty-dark`
- `sources.openmaptiles.url`: `https://tiles.openfreemap.org/planet` (keyless OpenMapTiles 3.x)
- `sprite`: `.../sprites/osm-liberty-dark` (built sprites committed on this branch)
- `glyphs`: unchanged; only the fontstacks `Roboto Regular`, `Roboto Medium` and
  `Roboto Condensed Italic` are used, as before

### Colors by layer group

| Layer group | Layers | Color(s) |
|---|---|---|
| Background / land | `background` | `#0e141b` |
| Residential (z ≤ 8 only, as upstream) | `landuse_residential` | `rgba(20,28,38, 0.84 → 0.6)` ≈ `#141c26` |
| Parks | `park`, `park_outline` | fill `#15241c` @ 0.7, outline / dashed edge `#1f3227` |
| Wood / grass | `landcover_wood`, `landcover_grass` | `#16271d` @ 0.7, `#18291f` @ 0.5 |
| Sport, cemetery | `landuse_pitch`, `landuse_track`, `landuse_cemetery` | `#15231e`, `#15231e`, `#151f1a` |
| Hospital, school | `landuse_hospital`, `landuse_school` | `#1e1a21`, `#1a1c17` |
| Ice, sand, wetland | `landcover_ice`, `landcover_sand`, `landcover_wetland` | `#1a222b`, `#1c1d18`, pattern `#132019` / `#1c2b22` |
| Water | `water` | `#0f1e2c` |
| Waterways | `waterway_river`, `waterway_other`, `waterway_tunnel` | `#15293b`, tunnels `#132230` (dashed) |
| Aeroways | `aeroway_fill`, `aeroway_runway`, `aeroway_taxiway` | `#161d26` @ 0.7, lines `#232c37` |
| Motorway (+ links) | `*_motorway*` | fill `#6b5a3a` (`#7a6340` at z5), casing `#3a3123` |
| Trunk / primary | `*_trunk_primary*` | fill `#46515e`, casing `#262f3a` |
| Secondary / tertiary, links | `*_secondary_tertiary*`, `*_link*` | fill `#3a4552`, casing `#222b35` |
| Minor streets | `road_minor*`, `tunnel_minor`, `tunnel_street_casing`, `bridge_street*` | fill `#2b3542`, casing `#1b232d` |
| Service / track | `*_service_track*` | fill `#242d38`, casing `#171e27` |
| Paths / pedestrian | `*_path_pedestrian*` | dashes `#36414d`, bridge casing `#1b232d`, tunnel `#28313c` |
| Tunnels (all classes) | `tunnel_*` | road fill blended 35 % toward the background, dashed casings as roads |
| Pedestrian areas | `road_area_pattern` | hatch pattern `#1f2832` |
| Rail | `*_major_rail*` | `#535d69` (line + hatching), tunnels `#333c46` |
| Transit (tram, subway, light rail) | `*_transit_rail*` | `#48525d`, tunnels `#333c46` |
| One-way arrows | `road_one_way_arrow*` | sprite `arrow` `#66727f` |
| Buildings z13–14 | `building` | fill `#1b2531`, outline `#25303d` |
| Buildings z14+ | `building-3d` | `#0e151f`, which renders as ≈ `#1b2431` (see note) |
| Boundaries | `boundary_3`, `boundary_2_*` | `#4a4659` (dashed), `#5b5569` |
| Place labels | `place_village`, `place_town`, `place_city`, `country_*` | `#e6edf3`, halo `rgba(11,16,22,0.8)` |
| Minor place labels | `place_other` (suburbs, quarters), `state`, `continent` | `#b3bec9`, halo `rgba(11,16,22,0.8)` |
| Road labels, shields | `road_label`, `road_shield` | `#9aa7b4`, halo `#0b1016`; shields `#18212b` with `#3a4552` border |
| POI labels | `poi_z14`, `poi_z15`, `poi_z16` | `#9aa7b4`, halo `#0b1016` |
| Transit labels | `poi_transit` | `#93a8bd`, halo `#0b1016` |
| Water labels | `water_name_*` | `#6f8aa5`, halo `rgba(11,16,22,0.8)` |
| Relief (dropped by the tile server) | `natural_earth` | `raster-brightness-max` 0.35, `raster-saturation` -0.6 |

Note on `building-3d`: MapLibre (GL JS and GL Native) lights the roofs of fill extrusions with the
default light. For dark colors that is about a 1.42× gain plus ambient, so `#1b2531` would show up
as `#293442`, the same value as minor streets. The base color is darkened so that roofs render
close to the intended `#1b2531`.

### Icons (sprites/osm-liberty-dark)

All SVGs in `svgs/` and the group colors in `iconset.json` were recolored, and the sprites were
rebuilt with `npm run build_sprites`. They hold the same 323 icons with the same sizes as upstream.

- POI discs: glyph `#9aa7b4` on a `#18212b` disc with a `#2e3946` ring (the category colors
  blue / orange / green / red / brown were all replaced by the one gray)
- Transport squares: glyph `#9aa7b4` on `#232e3b` (was white on `#4898ff`)
- Bare glyphs (Unsorted group): `#9aa7b4`; the one-way `arrow` is dimmer at `#66727f`
- Road shields `default_1..6`: `#18212b` with a `#3a4552` border
- Place dots `dot_9/10/11`: `#c9d3dc` core with a `#0b1016` halo
- Patterns: `pedestrian_polygon` hatch `#1f2832`; `wetland_bg_11` `#132019` / `#1c2b22`

### Contrast with the route overlay

The overlay is drawn as a casing `#6b7885` (11 px, 0.8 opacity), a track `#17212c` (7 px), and the
driven part as a `#38bdf8 → #34d399` gradient. WCAG contrast ratios:

| Route element | vs water `#0f1e2c` | vs park ≈ `#131f1c` | vs primary `#46515e` | vs secondary `#3a4552` | vs minor `#2b3542` | vs motorway `#6b5a3a` |
|---|---|---|---|---|---|---|
| gradient start `#38bdf8` | 7.9 | 7.9 | 3.8 | 4.6 | 5.8 | 3.1 |
| gradient end `#34d399` | 8.8 | 8.8 | 4.2 | 5.1 | 6.5 | 3.5 |
| casing (composited) | 2.9 | 2.9 | 1.6 | 1.9 | 2.3 | 1.4 |
| track `#17212c` | 1.0 | 1.0 | 2.0 | 1.7 | 1.3 | 2.4 |

On every surface, the casing or the track separates the undriven route from the map by at
least 1.6:1. The undriven route reads as a light outline around a dark core, the inverse of map
roads (dark casing, lighter fill). Map colors stay low in chroma: the highest are the motorway
(0.19, and 0.23 for its z5 stop) and the water labels (0.21), against 0.75 for `#38bdf8` and 0.62
for `#34d399`. There are no blue, teal or green road colors.
