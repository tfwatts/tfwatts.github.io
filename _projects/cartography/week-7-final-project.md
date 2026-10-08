---
title: "550 Miles of Virginia"
subtitle: "The Appalachian Trail from Harpers Ferry to the Tennessee line."
course: "Cartography"
method: "Final Project"
order: 7
featured: true
theme: ogilby
thumbnail: /assets/images/projects/cartography/week-7/thumb.jpg
summary: "The Appalachian Trail from Harpers Ferry to the Tennessee line."
---

About a quarter of the Appalachian Trail runs through Virginia, more than through any other state. From Harpers Ferry on the Potomac River to the Tennessee line south of Damascus, Virginia's section covers 554.5 miles, climbing from river crossings a few hundred feet above sea level to nearly 5,500 feet in the highlands around Mount Rogers.

This project maps that journey as one long strip, inspired by John Ogilby's Britannia of 1675, which drew England's roads as continuous ribbons running down the page. Scroll through five maps and walk Virginia's trail southbound, from Harpers Ferry to Tennessee, feeling both the distance and the climbs.

<figure class="project-map">
  <img src="{{ '/assets/images/projects/cartography/week-7/overview.png' | relative_url }}" alt="Map of the full Appalachian Trail from Georgia to Maine, with Virginia's 554.5 miles highlighted.">
  <figcaption>The whole Appalachian Trail, Georgia to Maine, with Virginia's 554.5 miles highlighted.</figcaption>
</figure>

<p class="button-row"><a class="button button-large" href="https://tfwatts.github.io/550-miles-of-virginia/">Begin the walk &rarr;</a></p>

## How to read the maps

Read each map from top to bottom; scrolling down is walking south. Mile markers count from Harpers Ferry (mile 0), with a small dot every 10 miles and a larger one every 50.

Beside each map, an elevation profile shows the climbs and descents along the same stretch of trail. Match its mile numbers to the mile markers on the map: the deep dip near mile 233, for example, is the trail dropping to the James River Foot Bridge.

Like Ogilby's road maps, each map follows the trail rather than the compass, so north shifts slightly from map to map. The compass rose on each one shows which way north lies. Every map also has its own key, scale bar, and small locator, so any one of them can be read on its own.

On a phone, tap any map to open it full size and zoom in.

## About the map panels

This project is built around one idea: letting a viewer feel the length of Virginia’s Appalachian Trail and its constant climbs and descents. Every design choice serves that goal.

**The strip.** The format comes from John Ogilby’s Britannia (1675), which drew roads as long, narrow strips turned to follow their route. The trail is cut into five overlapping panels using ArcGIS Pro’s strip map tools and a map series. All five share one scale, 1:250,000, so a mile of trail takes the same space on every panel and the distance is shown honestly; changing the scale between panels would make some sections feel shorter than they are. Because each panel follows the trail rather than the compass, each has its own compass rose, and ringed markers show exactly where one panel hands off to the next.

**The terrain.** The land is drawn with a multidirectional hillshade, which lights the terrain from several directions at once. A single light source would have shifted direction on the rotated panels and made some ridges look like valleys. Muted elevation colors, from sage green in the valleys to light tan on the ridges, sit beneath the shading, and the same colors fill the elevation profiles so the map and profile read as one.

**Projection.** The panels use the NAD 1983 Virginia Lambert projection, a state-based projection that keeps distances accurate within Virginia, which matters for a map built around trail miles. The locator map uses a Lambert Conformal Conic projection suited to the eastern United States, which keeps the shapes of the states familiar across the trail’s whole length.

**Visual hierarchy.** The trail is the most important feature, so it is drawn white with a dark outline, a nod to the white blazes that mark it, and it sits above everything else. Mile markers, shelters, and towns come next, while roads, rivers, and park boundaries stay quiet in the background. Roads appear only as short, straight stubs where they cross the trail, a simplification suited to the scale and borrowed from the side roads on Ogilby’s maps. Land outside Virginia is faded so the eye stays in the state.

**Distance and elevation.** The trail was measured from Harpers Ferry (mile 0) to the Tennessee line (mile 554.5), and the mile markers, shelters, road crossings, and places were all located by that measured distance. That is what lets the elevation profile beside each panel match the map mile for mile.

**Type.** Titles use IM Fell English, a revival of type from Ogilby’s era. Labels use bold Georgia, a serif designed for reading on screens, with a thin light outline so names stay readable over shaded relief.

**The locator map.** The overview of the whole trail is turned so the trail runs straight down the page, echoing the strip, and it uses the same trail symbols, so Virginia’s quarter of the journey stands out at a glance. Its trail line was simplified to suit the much smaller scale.

**Methods and tools.** Maps were made in ArcGIS Pro. The trail was measured along its length from Harpers Ferry (mile 0) to the Tennessee line (mile 554.5), and mile markers, shelters, road crossings, and places were located by that measured distance. The strip was cut into five panels at a single scale using ArcGIS Pro’s strip map tools and a map series. Elevation was prepared in Google Earth Engine. The elevation profiles and the straight road stubs were drawn with custom Python scripts. Mileage is measured from the map data and may differ slightly from the Appalachian Trail Conservancy’s official figures.

## Sources and notes

### Trail and park data

- **Appalachian Trail centerline.** National Park Service, Appalachian National Scenic Trail. [APPA Official Centerline](https://www.arcgis.com/home/item.html?id=71975f7fc14347c7a6c1059fdb593f91). ArcGIS Online (accessed 09/24/2026).
- **Shelters.** National Park Service, Appalachian National Scenic Trail. [Appalachian National Scenic Trail – Official Features and Facilities](https://www.arcgis.com/home/item.html?id=2739a451a90c4a3283be4ccd6a6a12a9). ArcGIS Online (accessed 09/24/2026).
- **National Park Service land.** National Park Service, Land Resources Division. [Administrative Boundaries of National Park System Units](https://www.arcgis.com/home/item.html?id=e62a2420170b4452b4a334db92130220). ArcGIS Online (accessed 09/24/2026).
- **National forests.** USDA Forest Service. [Administrative Forest Boundaries](https://data.fs.usda.gov/geodata/edw/datasets.php?dsetCategory=boundaries) (accessed 09/24/2026).
- **Skyline Drive.** National Park Service, Shenandoah National Park. [SHEN\_TRANS\_SkylineDrive\_ln](https://services1.arcgis.com/fBc8EJBxQRMcHlei/arcgis/rest/services/SHEN_TRANS_SkylineDrive_ln/FeatureServer/40) (accessed 10/03/2026).

### Terrain, roads, and water

- **Elevation.** U.S. Geological Survey, [3D Elevation Program (3DEP)](https://www.usgs.gov/3d-elevation-program), 1/3 arc-second (10 m) digital elevation model, accessed through Google Earth Engine and resampled to 30 m (accessed 09/24/2026).
- **Roads and states.** U.S. Census Bureau. [TIGER/Line Shapefiles, 2025](https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-line-file.html): Primary and Secondary Roads (Virginia, West Virginia); States (accessed 09/24/2026).
- **Rivers.** Esri. [USA Rivers and Streams](https://hub.arcgis.com/datasets/esri::usa-rivers-and-streams/about) (derived from the USGS National Hydrography Dataset). ArcGIS Living Atlas (accessed 09/24/2026).

### Locator map and places

- **Canada (locator map).** Esri. [World Countries (Generalized)](https://hub.arcgis.com/datasets/esri::world-countries-generalized/about). ArcGIS Living Atlas (accessed 10/03/2026).
- **Place locations.** Located with the Esri World Geocoding Service in ArcGIS Pro.

### Type and inspiration

- **Title typeface.** IM Fell English, digitized by Igino Marini. [Google Fonts](https://fonts.google.com/specimen/IM+Fell+English).
- **Inspiration.** Ogilby, John. *Britannia, Volume the First.* London, 1675.