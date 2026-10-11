---
title: "Wragg Fire Burn Scar Recovery"
course: "Remote Sensing"
method: "dNBR and NDVI"
order: 1
featured: true
thumbnail: /assets/images/projects/remote-sensing/wragg-fire/thumb.jpg
summary: "High severity areas lost the most vegetation after the 2015 fire, with mean NDVI falling from 0.59 to 0.19, yet nearly recovered to pre-fire levels by 2019."
---

## Abstract

The 2015 Wragg Fire burned approximately 8,100 acres of chaparral and oak woodland near Lake Berryessa, California between July 22 and August 5, 2015 (ABC7 News, 2015). This study examines burn severity patterns and post-fire vegetation recovery across four burn severity classes using multi-temporal Landsat 8 satellite imagery acquired between 2014 and 2019. Burn severity was mapped using the Differenced Normalized Burn Ratio (dNBR), calculated from pre- and post-fire Landsat 8 surface reflectance imagery processed in Google Earth Engine. Recovery trajectories were analyzed using the Normalized Difference Vegetation Index (NDVI) across annual summer composites from 2014 through 2019.

Results indicated that high severity burn areas experienced the greatest initial vegetation loss, with mean NDVI dropping from 0.59 in 2014 to 0.19 immediately following the fire, while unburned areas remained relatively stable throughout the study period. By 2019 all severity classes showed measurable recovery, though high severity areas had not fully returned to pre-fire vegetation levels.

These findings demonstrate the utility of freely available Landsat imagery and open source tools for monitoring post-fire ecosystem recovery at a landscape scale.

## Introduction

Wildfires are a natural and increasingly frequent disturbance in California. However, rising temperatures, periods of drought, and expanding wildland-urban interfaces have intensified fire behavior and increased the ecological and economic consequences of large wildfire events (Kozanitas et al., 2026). Understanding how burned landscapes recover following fire is critical for land managers, ecologists, and policymakers working to restore ecosystem function and reduce future fire risk.

Remote sensing offers a powerful and cost-effective approach to monitoring post-fire vegetation recovery at landscape scales (Bright et al., 2019). Freely available satellite imagery from the Landsat program provides a continuous archive of surface reflectance data dating back to 1972, enabling multi-temporal analysis of vegetation change before, during, and after disturbance events. Spectral indices derived from near-infrared and shortwave infrared bands including the Normalized Burn Ratio (NBR) and Normalized Difference Vegetation Index (NDVI), are widely used to map burn severity and track vegetation recovery trajectories following wildfire, (Xu et al., 2024).

This case study focuses on the 2015 Wragg Fire, near Lake Berryessa in Napa and Yolo Counties, California. The following research questions guide this analysis:

1. What was the spatial distribution of burn severity across the Wragg Fire perimeter?
2. How did vegetation recovery trajectories differ across burn severity classes between 2015 and 2019?
3. Had any severity class fully returned to pre-fire NDVI levels by the end of the study period?

<figure class="project-map">
  <a href="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig1-study-area.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig1-study-area.png' | relative_url }}" alt="Map of the Wragg Fire study area near Lake Berryessa in Napa and Yolo Counties, California.">
  </a>
  <figcaption>Figure 1. Wragg Fire study area location, Lake Berryessa, Napa and Yolo Counties, California.</figcaption>
</figure>

## Methods

This study utilized multi-platform remote sensing workflow integrating Google Earth Engine (GEE), RStudio, and ArcGIS Pro to assess burn severity and post-fire vegetation recovery following the 2015 Wragg Fire near Lake Berryessa, California.

### Study area

The Wragg Fire burned 8,051 acres of chaparral and oak woodland in Napa and Solano Counties, California between July 22 and August 5, 2015 (ABC7 News, 2015). The study area was defined using the official fire perimeter obtained from the Monitoring Trends in Burn Severity (MTBS) program (Eidenshink et al., 2007).

### Data acquisition and preprocessing

Landsat 8 Collection 2 Surface Reflectance imagery was accessed and processed using Google Earth Engine. Annual median composites were generated for the July-September window for each year from 2014 through 2019 to maintain seasonal consistency. Scene-level cloud cover was filtered to less than 15 percent, and pixel-level cloud, cloud shadow, and snow masking was applied using the QA\_PIXEL bitmask band prior to composite generation.

### Burn severity mapping

The Normalized Burn Ratio (NBR) was computed for pre-fire (2014) and post-fire (2015) composites using Landsat 8 near-infrared (Band 5) and shortwave infrared (Band 7):

<p class="equation">NBR = (Band 5 − Band 7) / (Band 5 + Band 7)</p>

The Differenced Normalized Burn Ratio (dNBR) was calculated by subtracting post-fire NBR from pre-fire NBR (Key & Benson, 2022):

<p class="equation">dNBR = NBR<sub>prefire</sub> − NBR<sub>postfire</sub></p>

The dNBR raster was exported from GEE and classified into four burn severity categories in RStudio using the terra package based off previous research projects ("The Automated Temporal Burn Index (ATBI) for Accurate and Scalable Burned Area Mapping," 2025), (Zahabnazouri et al., 2025). USGS does not define burn severity thresholds (Mapping Methods \| MTBS, n.d.):

| Burn severity class | dNBR range |
|---|---|
| Unburned | < 0.10 |
| Low severity | 0.10 – 0.27 |
| Moderate severity | 0.27 – 0.66 |
| High severity | > 0.66 |

### Vegetation recovery analysis

Annual NDVI composites were computed for each year from 2014 through 2019 using Landsat 8 red (Band 4) and near-infrared (Band 5) bands:

<p class="equation">NDVI = (Band 5 − Band 4) / (Band 5 + Band 4)</p>

Zonal statistics were computed in RStudio using the terra package to extract mean NDVI values for each burn severity class across all six annual composites, enabling direct comparison of recovery trajectories across severity classes. Recovery trends were visualized using ggplot2.

### Cartographic presentation

All final map products were designed and exported in ArcGIS Pro 3.6.3.

<figure class="project-map">
  <a href="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig-methods.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig-methods.png' | relative_url }}" alt="[Describe the methods map in a sentence]">
  </a>
  <figcaption>[Figure number and caption]</figcaption>
</figure>

## Results

### Burn severity distribution

Classification of the dNBR raster revealed distinct spatial patterns of burn severity across the Wragg Fire perimeter.

### Pre-fire vegetation condition

Pre-fire NDVI values across the study area in summer 2014 indicated moderate to high vegetation density consistent with healthy vegetation. These values established the baseline against which post-fire recovery was assessed.

### Post-fire vegetation response

Immediately following the fire, mean NDVI values declined across all burned severity classes. High severity areas experienced the greatest vegetation loss with mean NDVI dropping from 0.59 in 2014 to 0.19 in 2015. Moderate severity areas declined from 0.45 to 0.28 and low severity areas showed a smaller decline from 0.50 to 0.41. Unburned areas remained stable at approximately 0.46 across both years confirming the validity of the severity classification.

### Vegetation recovery trajectories

All burn severity classes exhibited positive NDVI recovery trends from 2015 through 2019 as shown in Figure 7. Recovery trajectories varied by severity class. Low severity areas returned to near pre-fire NDVI levels by 2019 (0.49 vs. 0.50 in 2014). Moderate severity areas approached but did not fully reach pre-fire levels (0.41 in 2019 vs. 0.45 in 2014). High severity areas showed the most robust recovery in absolute NDVI gain, rising from 0.19 in 2015 to 0.57 by 2019, nearly returning to pre-fire levels of 0.59.

<figure class="project-map">
  <a href="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig4-burn-severity.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig4-burn-severity.png' | relative_url }}" alt="Map of dNBR burn severity classes within the Wragg Fire perimeter, 2015.">
  </a>
  <figcaption>Figure 4. Wragg Fire dNBR burn severity classification (2015). Classes follow standard USGS severity thresholds (Key & Benson, 2006).</figcaption>
</figure>

<figure class="project-map">
  <a href="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig5-ndvi-2014.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig5-ndvi-2014.png' | relative_url }}" alt="Map of pre-fire NDVI across the Wragg Fire study area, summer 2014.">
  </a>
  <figcaption>Figure 5. Pre-fire NDVI baseline (Summer 2014) derived from Landsat 8 surface reflectance imagery. Color ramp stretched from -0.1 to 0.7 to maintain consistent symbology across all annual NDVI composites. Mean NDVI within the fire perimeter was 0.46.</figcaption>
</figure>

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig6-ndvi-composites.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig6-ndvi-composites.png' | relative_url }}" alt="Six-panel series of annual NDVI maps of the Wragg Fire area from 2014 through 2019.">
  </a>
  <figcaption>Figure 6. Annual NDVI composites (2014–2019) illustrating pre-fire vegetation condition, immediate post-fire vegetation loss, and multi-year recovery trajectory within the Wragg Fire perimeter. All panels use an identical color stretch (-0.1 to 0.7) to ensure visual comparability across years. Mean NDVI values per severity class are reported in Figure 7.</figcaption>
</figure>

<figure class="project-map">
  <img src="{{ '/assets/images/projects/remote-sensing/wragg-fire/fig7-ndvi-trajectories.png' | relative_url }}" alt="Line chart of mean NDVI by burn severity class from 2014 through 2019, with a dashed line marking the fire.">
  <figcaption>Figure 7. Mean NDVI recovery trajectories by burn severity class (2014–2019). The dashed red line indicates the fire event (August 2015).</figcaption>
</figure>

<figure class="project-map">
  <iframe class="embed" src="{{ '/assets/images/projects/remote-sensing/wragg-fire/sankey.html' | relative_url }}" title="[Describe the Sankey chart in a few words]" loading="lazy"></iframe>
  <figcaption>[Figure number and caption for the Sankey chart]</figcaption>
</figure>

## Discussion

This study demonstrated the application of a multi-platform remote sensing workflow to map burn severity and monitor post-fire vegetation recovery following the 2015 Wragg Fire near Lake Berryessa, California.

The dNBR classification effectively captured spatial variability in burn severity across the fire perimeter, with high severity burning concentrated in the main fire area and lower severity burning along perimeter edges. The notable finding was that high severity burn areas exhibited the most NDVI recovery between 2015 and 2019, rising from 0.19 to 0.57 - nearly returning to pre-fire levels of 0.59. Low severity areas returned to near pre-fire levels while moderate severity areas showed the least recovery within the study. Limitations include the study period ending in 2019, the 30-meter spatial resolution of Landsat 8, and potential influence of climate variability on NDVI values. Future studies could extend the time series and incorporate higher resolution imagery such as Sentinel-2 to capture higher resolution recovery dynamics.

## Conclusion

This study successfully applied a multi-platform remote sensing workflow to assess burn severity and post-fire vegetation recovery following the 2015 Wragg Fire near Lake Berryessa, California. Using Landsat 8 imagery processed in Google Earth Engine, dNBR burn severity classification in RStudio, and multi-year NDVI time series analysis, results showed that all burn severity classes exhibited measurable vegetation recovery between 2015 and 2019 with high severity areas demonstrating the most recovery by the end of the study period. This workflow demonstrates the utility of freely available satellite imagery and open source tools for monitoring post-fire recovery at meaningful spatial and temporal scale (Gorelick et al., 2017).

## References

<div class="references" markdown="1">

ABC7 News. (2015, August 6). 8,000-acre Wragg Fire near Lake Berryessa 100 percent contained. <https://abc7news.com/large-wildfire-south-of-lake-berryessa-evacuations-lifted-firefighters-battle-wragg-fire-jumps-containment/907445/>

Xu, E., Wei, M., Li, T., Lei, V., Gao, J., Wang, N., & He, Y. (2024). Assessing burn severity and vegetation restoration in Alberta's boreal forests following the 2016 Fort McMurray wildfire – a remote sensing time-series study. *Sustainable Environment, 10*(1). <https://doi.org/10.1080/27658511.2024.2361569>

Eidenshink, J., Schwind, B., Brewer, K., Zhu, Z.-L., Quayle, B., & Howard, S. (2007). A Project for Monitoring Trends in Burn Severity. *Fire Ecology, 3*(1), 3–21. <https://doi.org/10.4996/fireecology.0301003>

Gorelick, N., Hancher, M., Dixon, M., Ilyushchenko, S., Thau, D., & Moore, R. (2017). Google Earth Engine: Planetary-scale geospatial analysis for everyone. *Remote Sensing of Environment, 202*, 18–27. <https://doi.org/10.1016/j.rse.2017.06.031>

Key, C. H., & Benson, N. C. (2022). Landscape Assessment (LA). In: Lutes, Duncan C.; Keane, Robert E.; Caratti, John F.; Key, Carl H.; Benson, Nathan C.; Sutherland, Steve; Gangi, Larry J. 2006. FIREMON: Fire Effects Monitoring and Inventory System. Gen. Tech. Rep. RMRS-GTR-164-CD. Fort Collins, CO: U.S. Department of Agriculture, Forest Service, Rocky Mountain Research Station. P. LA-1-55, 164. <https://research.fs.usda.gov/treesearch/24066>

Kozanitas, M., Rosenblad, K. C., & Ackerly, D. D. (2026). Resprouting and mortality following mixed-severity fire in California mixed-evergreen and oak woodland communities: Individual and community responses. *Forest Ecology and Management, 612*, 123706. <https://doi.org/10.1016/j.foreco.2026.123706>

Bright, B.C., Hudak, A.T., Kennedy, R.E. et al. Examining post-fire vegetation recovery with Landsat time series analysis in three western North American forest types. *fire ecol 15*, 8 (2019). <https://doi.org/10.1186/s42408-018-0021-9>

The automated temporal burn index (ATBI) for accurate and scalable burned area mapping. (2025). *International Journal of Applied Earth Observation and Geoinformation, 144*, 104866. <https://doi.org/10.1016/j.jag.2025.104866>

Mapping Methods \| MTBS. (n.d.). Www.mtbs.gov. <https://www.mtbs.gov/mapping-methods>

Zahabnazouri, S., Belmont, P., David, S., Wigand, P. E., Elia, M., & Capolongo, D. (2025). Detecting Burn Severity and Vegetation Recovery After Fire Using dNBR and dNDVI Indices: Insight from the Bosco Difesa Grande, Gravina in Southern Italy. *Sensors, 25*(10), 3097. <https://doi.org/10.3390/s2510309>

</div>