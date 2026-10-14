---
title: "James River Bathymetric Interpolation Study"
course: "Raster Spatial Analysis"
method: "IDW, Spline, and Kriging"
order: 4
featured: false
thumbnail: /assets/images/projects/raster-spatial-analysis/james-river/thumb.jpg
summary: "IDW had the lowest error, but Kriging produced the most realistic bathymetric surface for the James River."
---

## Abstract

This study evaluates three spatial interpolation methods: Inverse Distance Weighting (IDW), Spline, and Kriging; applied to bathymetric point data extracted from NOAA hydrographic survey H13768 in the James River, Virginia. Depth values were extracted from a 10-meter resampled Bathymetric Attributed Grid (BAG) raster and used to generate continuous depth surfaces across unsurveyed gaps between and around multibeam sonar swaths. Accuracy was assessed using 39,345 withheld test points. IDW achieved the lowest RMSE (0.167m), followed by Spline (0.170m) and Kriging (0.196m). Despite its higher RMSE, Kriging was selected as the most appropriate method due to its statistical foundation in spatial autocorrelation, surface smoothness, and adherence to the observed depth range. Spline was eliminated from consideration due to significant divergence beyond observed values. Results suggest that for continuous physical phenomena such as bathymetric depth, geostatistical methods offer advantages over purely distance-based approaches.

## Introduction

Bathymetric surveys of navigable waterways are collected using multibeam sonar along discrete vessel tracklines, resulting in measured swaths separated by unsurveyed gaps. The NOAA hydrographic survey H13768 provides this data as a gridded raster surface for the southwestern Chesapeake Bay region, including the James River, Virginia. This study extracts point values from that raster and evaluates three spatial interpolation methods, Inverse Distance Weighting (IDW), Spline, and Kriging, to determine which method most accurately predicts bathymetric depth across unmeasured areas.

> Which spatial interpolation method - IDW, Spline, or Kriging - produces the most accurate continuous bathymetric surface from discrete point values extracted from a multibeam sonar raster in the James River, Virginia?

## Study area

The study area encompasses two adjacent estuarine segments of the tidal James River in Virginia, as defined by the Virginia DEQ 2024 Water Quality Assessment. The first segment extends from the tidal freshwater/oligohaline boundary at approximately river mile 51.94 downstream to the watershed limit at approximately river mile 42.7. The second segment extends from the confluence with the Chickahominy River at river mile 48.40 downstream to a line between Hog Point and the mouth of College Creek on the north shore of the James River. Together these two polygons define the spatial extent within which bathymetric data from NOAA hydrographic survey H13768 was extracted and analyzed. This reach of the James River is a tidal estuary with depths ranging from near zero at the shoreline to approximately 22.7 meters in the navigation channel.

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-study-area.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-study-area.png' | relative_url }}" alt="Map of the two tidal James River segments that form the study area.">
  </a>
  <figcaption>Study area showing the two James River estuary segments and extracted bathymetric survey data.</figcaption>
</figure>

## Data

Bathymetric data was obtained from NOAA hydrographic survey H13768, a multibeam sonar survey of the southwestern Chesapeake Bay conducted between April 2023 and February 2024. The survey was delivered as a Bathymetric Attributed Grid (BAG) raster at 1-meter resolution, referenced vertically to Mean Lower Low Water (MLLW) and horizontally to the North American Datum of 1983 (NAD83 2011), UTM Zone 18N. The study area boundary polygons were obtained from the Virginia DEQ Estuaries dataset (2024 Final Water Quality Assessment Integrated Report), accessed via the Virginia DEQ ArcGIS Hub, and used as a mask to extract bathymetric values within the two James River segments of interest.

### Source datasets

<div class="table-scroll">
<table class="data-table">
  <thead>
    <tr><th>Field</th><th>NOAA H13768 BAG</th><th>VA DEQ Estuaries</th></tr>
  </thead>
  <tbody>
    <tr><td>Official Name</td><td>NOS Hydrographic Survey H13768</td><td>Estuaries (2024 Final WQA IR Assessment)</td></tr>
    <tr><td>Year Published/Updated</td><td>2024</td><td>2024</td></tr>
    <tr><td>Author/Owner</td><td>NOAA National Ocean Service, Office of Coast Survey</td><td>Virginia Department of Environmental Quality</td></tr>
    <tr><td>URL/Repository</td><td><a href="https://www.ngdc.noaa.gov/nos/H12001-H14000/H13768.html" target="_blank">ngdc.noaa.gov – H13768</a></td><td><a href="https://geohub-vadeq.hub.arcgis.com/datasets/abac9b1d68fd494b9c7e34158283fd99_4/about" target="_blank">VA DEQ ArcGIS Hub</a></td></tr>
    <tr><td>Description</td><td>Multibeam sonar bathymetric survey of southwestern Chesapeake Bay, delivered as a gridded BAG raster with elevation and uncertainty bands</td><td>Polygon dataset of estuarine water body segments identified in Virginia's 2024 305(b)/303(d) Water Quality Assessment Integrated Report</td></tr>
    <tr><td>Coordinate System</td><td>UTM Zone 18N</td><td>WGS 1984 Web Mercator Auxiliary Sphere</td></tr>
    <tr><td>Geodesic Model</td><td>GRS80</td><td>WGS84</td></tr>
    <tr><td>Horizontal Datum</td><td>NAD83 (2011)</td><td>WGS84</td></tr>
    <tr><td>Vertical Datum</td><td>MLLW</td><td><span class="na">N/A</span></td></tr>
    <tr><td>EPSG Code</td><td>6347</td><td>3857</td></tr>
    <tr><td>Geometry Type</td><td>Raster</td><td>Polygon</td></tr>
  </tbody>
</table>
</div>

### Derived data products

<div class="table-scroll">
<table class="data-table">
  <thead>
    <tr><th>Product</th><th>Dataset Name</th><th>Derived From</th><th>Resolution / Cell Size</th><th>Point Count</th><th>Coordinate System</th><th>EPSG</th><th>Geometry</th></tr>
  </thead>
  <tbody>
    <tr><td>Resampled Elevation Raster</td><td><code>Study_Area_Extract</code></td><td>H13768 BAG</td><td>10 m</td><td><span class="na">N/A</span></td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Raster</td></tr>
    <tr><td>Training Points</td><td><code>JR_Training</code></td><td><code>Study_Area_Extract</code></td><td><span class="na">N/A</span></td><td>157,379</td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Point</td></tr>
    <tr><td>Test Points</td><td><code>JR_Test</code></td><td><code>Study_Area_Extract</code></td><td><span class="na">N/A</span></td><td>39,345</td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Point</td></tr>
    <tr><td>IDW Surface</td><td><code>JR_IDW</code></td><td><code>JR_Training</code></td><td>25 m</td><td><span class="na">N/A</span></td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Raster</td></tr>
    <tr><td>Spline Surface</td><td><code>JR_Spline</code></td><td><code>JR_Training</code></td><td>25 m</td><td><span class="na">N/A</span></td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Raster</td></tr>
    <tr><td>Kriging Surface</td><td><code>JR_Kriging</code></td><td><code>JR_Training</code></td><td>25 m</td><td><span class="na">N/A</span></td><td>NAD83(2011) UTM Zone 18N</td><td>6347</td><td>Raster</td></tr>
  </tbody>
</table>
</div>

## Methods

All data used in this analysis was processed in NAD83(2011) UTM Zone 18N (EPSG 6347), the native coordinate system of the NOAA H13768 bathymetric survey. UTM Zone 18N is appropriate for this study area as the James River falls well within the zone's extent, minimizing distortion of distance and area. The metric unit system is suited to bathymetric interpolation, where depth and spatial relationships are generally expressed in meters.

A total of 196,724 points were extracted from `Study_Area_Extract`, a 10-meter resampled derivative of the NOAA H13768 BAG raster, using the Raster to Point tool in ArcGIS Pro. Each point represents the center of a 10-meter raster cell containing a bathymetric depth measurement, recorded in meters below Mean Lower Low Water (MLLW). Points were generated only where valid sonar measurements existed; cells corresponding to the unsurveyed gaps between sonar swaths produced no points, as NoData cells are automatically excluded during raster to point conversion.

The 196,724 extracted points were randomly divided into training and test datasets using the Subset Features tool in ArcGIS Pro. A total of 157,379 points (80%) were assigned to the training dataset (`JR_Training`) and used as input for all three interpolation methods. The remaining 39,345 points (20%) were withheld as the test dataset (`JR_Test`) and reserved exclusively for accuracy assessment. Random sampling was used for the split, meaning spatial distribution of training and test points were not explicitly controlled.

### IDW

Inverse Distance Weighting (IDW) estimates unknown values at unsampled locations by calculating a weighted average of surrounding known points, where the influence of each point decreases as distance increases. Points closer to the prediction location are assigned greater weight than those farther away. The significance of the distance is controlled by a power parameter; a higher power increases the significance of nearby points relative to distant ones. IDW assumes that spatial similarity decreases with distance and does not account for the statistical trends in the data.

### Spline

Spline interpolation estimates unknown values by using an algorithm (Spline equation) that results in a smooth mathematical curve through known data points, minimizing the curvature of the surface. Spline does not rely solely on distance because it considers the shape of the surface, producing a smooth continuous result that passes exactly through each input point. Two variants exist: Regularized and Tension, regularized was selected for this analysis.

### Kriging

Kriging is a geostatistical interpolation method that estimates unknown values by modeling the spatial autocorrelation of the data through a semivariogram. The semivariogram quantifies autocorrelation, allowing Kriging to assign weights to surrounding points based on both distance and the statistical structure of the dataset. Unlike IDW and Spline, Kriging provides not only a predicted value surface but also an estimate of prediction uncertainty. Ordinary Kriging was used for this analysis, with a Spherical semivariogram model applied as the default fitting method.

The original 1-meter BAG raster was resampled to 10 meters prior to point extraction using bilinear resampling. This decision was made to reduce computational load while preserving the spatial structure of the bathymetric data, as the 1-meter resolution produced an unmanageable point count of approximately 19.6 million points. Bilinear resampling was selected over nearest neighbor for continuous depth data, producing smooth transitions between cell values rather than abrupt steps.

### Assumptions

The 196,724 extracted points were split 80/20 by percentage into training (157,379) and test (39,345) datasets using random sampling via the Subset Features tool. An output cell size of 25 meters was applied consistently across all three interpolation methods. This resolution is lower than the 10-meter input point spacing; however, the interpolation target (unsurveyed gaps between sonar paths) spans distances far greater than 25 meters. Consistent cell size ensures output rasters are directly comparable in the results analysis. Default search radius and neighbor parameters were accepted across all methods, using a variable search radius with a minimum of 12 points. For IDW, a power value of 2 was used as the standard default. Spline was set to Regularized, suitable for gradually varying depth conditions in a tidal river environment. For Kriging, the default Spherical semivariogram model and lag size were applied.

<div class="figure-row wide">
  <figure>
    <a href="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-idw.png' | relative_url }}"><img src="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-idw.png' | relative_url }}" alt="[Describe the first image]"></a>
    <figcaption>IDW tool inputs used to generate the interpolated depth surface.</figcaption>
  </figure>
  <figure>
    <a href="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-spline.png' | relative_url }}"><img src="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-spline.png' | relative_url }}" alt="[Describe the second image]"></a>
    <figcaption>Spline tool inputs used to generate the interpolated depth surface.</figcaption>
  </figure>
  <figure>
    <a href="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-kriging.png' | relative_url }}"><img src="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-kriging.png' | relative_url }}" alt="[Describe the third image]"></a>
    <figcaption>Kriging tool inputs used to generate the interpolated depth surface.</figcaption>
  </figure>
</div>

## Results

The three interpolation methods produced notably different results in both surface statistics and predictive accuracy. IDW and Kriging generated surfaces closely aligned with the observed depth range of -22.72 to -0.85 meters, while Spline produced values significantly outside the observed range, with minimum and maximum values of -105.09 and 483.60 meters respectively. This is consistent with ESRI's Spline documentation, which states that a Regularized Spline will produce a gradually changing surface with values that may lie outside the sample data range, in this case, considerably so. The mean depth values for IDW (-4.14m) and Kriging (-4.14m) were nearly identical, both slightly shallower than the observed mean of -5.21m, while Spline produced a mean of -1.98m, reflecting the distorting effect of the outlier values in the statistics.

[Interpolation Results Comparison table]

Accuracy assessment using 39,345 withheld test points revealed that all three methods performed similarly, with RMSE values under 0.20 meters. IDW achieved the lowest RMSE of 0.167 meters, followed closely by Spline at 0.170 meters and Kriging at 0.196 meters. The low RMSE values across all three methods indicate that each approach was capable of predicting depth at unsampled locations with reasonable accuracy at the point level. It is worth noting that, despite Spline's divergence in range, its point-level RMSE remained competitive, suggesting the extremes may have been concentrated in areas distant from test points rather than uniformly distributed across the surface.

Visual comparison of the three output rasters reinforces these findings. IDW produced a surface consistent with expected results for this method and dataset. Kriging produced a smoother surface than IDW while remaining within the observed depth range. Spline produced a surface that accurately reflected most depth characteristics of the other two methods, while exhibiting more pronounced artifacts in areas of sparse point coverage where interpolation distances were greatest. Further analysis would be required to determine whether these artifacts represent real bottom structure or errors introduced by the interpolation. Taken together, the accuracy metrics and visual outputs suggest that IDW and Kriging are the most reliable methods for this dataset, with Spline's range deviation presenting a significant limitation despite its competitive point-level accuracy.

### Accuracy assessment: predicted vs. observed depth (m)

Table displays a random sample of 25 test points from the full test dataset. The complete dataset of 39,345 points used for accuracy assessment is available [here](https://docs.google.com/spreadsheets/d/1OydfFk-M-yC_Yfde8VIoA7CUcvO6qcnj/edit?usp=sharing&ouid=114301637572248807406&rtpof=true&sd=true).

[Accuracy Assessment table]

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-final-map.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/raster-spatial-analysis/james-river/fig-final-map.png' | relative_url }}" alt="[Describe the final map in a sentence]">
  </a>
  <figcaption>Comparison of IDW, Spline, and Kriging interpolated bathymetric surfaces, James River, Virginia.</figcaption>
</figure>

## Conclusion

While IDW achieved the lowest RMSE of the three methods evaluated, Kriging is selected as the most appropriate interpolation method for this analysis. The difference in RMSE between IDW (0.167m) and Kriging (0.196m) is approximately 0.029 meters, a difference that, while measurable, is potentially insubstantial for bathymetric applications where depth values range nearly 22 meters or more. At this scale, both methods demonstrate comparable predictive accuracy at the point level, and the RMSE advantage of IDW alone is insufficient justification for selecting it over a more theoretically robust approach.

Kriging is the only method among the three that is grounded in spatial statistics. Rather than assuming that distance alone determines the influence of surrounding points, Kriging models the actual spatial autocorrelation structure of the data through a semivariogram, assigning weights based on both distance and the statistical properties of the dataset. This arguably makes Kriging the more defensible choice from a geostatistical standpoint, particularly for a continuous physical phenomenon like bathymetric depth where spatial structure is meaningful. Additionally, Kriging produced a smoother surface than IDW while remaining within the observed depth range, making it more physically realistic for a tidal river environment where gradual depth transitions are expected. Spline was eliminated from consideration due to its significant deviation beyond the observed depth range, rendering its surface unreliable despite its competitive point-level RMSE.

For hydrographic and navigational applications, surface quality and physical realism are potentially as important as raw predictive accuracy. A smooth, statistically grounded surface that stays within observed depth values is more suitable for practical use than one optimized purely for point-level error minimization. On this basis, Kriging is recommended as the preferred interpolation method for predicting bathymetric depth in unmeasured areas between sonar swaths in the James River study area.

### Limitations

Several limitations affect the conclusions of this analysis. The training and test datasets were generated using random sampling via the Subset Features tool, meaning spatial distribution of points was not explicitly controlled. This introduces the possibility that certain areas of the study region were underrepresented in the training data, potentially affecting interpolation accuracy in those locations. Additionally, the original 1-meter BAG raster was resampled to 10 meters prior to point extraction, reducing the spatial resolution of the input data and potentially smoothing fine-scale depth variation that may be present in the original survey. Default parameters were accepted for all three interpolation methods without optimization, and a custom semivariogram was not fitted for Kriging, which may have improved its performance. Finally, the accuracy assessment table presented in this report represents a sample of 25 points drawn from the full test dataset of 39,345 points and should be interpreted as illustrative rather than exhaustive.

Of the three spatial interpolation methods evaluated, Kriging produced the most appropriate continuous bathymetric surface from discrete point values extracted from the NOAA H13768 multibeam sonar raster in the James River, Virginia, based on its statistical foundation, surface smoothness, and adherence to the observed depth range.