---
title: "High-Hazard Dams in North Carolina"
course: "Vector Spatial Analysis"
method: "Attribute Queries, Spatial Queries, Spatial Joins, and Combination"
order: 1
featured: false
thumbnail: /assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/banner.jpg
banner: /assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/banner.jpg
banner_alt: "Fontana hydroelectric dam"
summary: "A national-to-county analysis identifying where high-hazard, poor-condition dams concentrate and how that burden intersects with population across North Carolina’s counties."
---

<p class="project-byline">
  By Thomas Watts · Vector Spatial Analysis ·
  <a href="https://storymaps.arcgis.com/stories/ab2168bbab0741e7a8c60cdc95e90701" target="_blank" rel="noopener">View the original ArcGIS StoryMap ↗</a>
</p>

## Introduction

This analysis is organized around eight questions spanning four query types. The analysis follows a national-to-county "funnel": broad attribute selections establish the national picture, then spatial queries and joins narrow the focus to the most at-risk state and counties.

Attribute Queries

1.  Nationally, which dams carry a High hazard potential classification?
2.  Nationally, which dams are classified High or Significant hazard potential?

Spatial Queries

3. Which high-hazard, poor-condition dams are located within North Carolina?
4. Which North Carolina counties contain at least one at-risk dam?

Spatial Joins

5. How many at-risk dams are located in each state?
6. How many at-risk dams are located in each North Carolina county?

Combination (Attribute + Spatial + Join)

7. Which North Carolina counties combine a high at-risk-dam burden with a large county population (potential risk)?
8. Which North Carolina counties carry a high dam burden despite small populations, indicating high exposure but low local capacity?

## Study area

This analysis begins at the national scale, examining dams across the contiguous United States before narrowing to a single state and its counties. The funnel identified North Carolina as the focus area: a southeastern state spanning roughly 500 miles from the Atlantic coastal plain, across the central Piedmont, to the Blue Ridge and Great Smoky Mountains in the west. North Carolina contains 100 counties and a population of over 10 million, distributed unevenly between dense Piedmont metros such as Raleigh (Wake County) and Charlotte (Mecklenburg County) and sparsely populated rural and mountain counties. The state's landscape includes thousands of small, privately owned dams, many built for recreation, agriculture, or water supply, concentrated in the Piedmont and mountain regions. This combination of variable terrain, uneven population distribution, and a large inventory of dams makes North Carolina a meaningful setting for examining where dam risk and population intersect.

## Data

This analysis uses three primary datasets. Dam locations and attributes come from the U.S. Army Corps of Engineers National Inventory of Dams (NID, 2026), a point dataset of approximately 92,000 dams nationwide with attributes including hazard potential classification, condition assessment, and ownership. County population come from the U.S. Census Bureau's American Community Survey (ACS) 2019–2023 five-year estimates (table B01003), joined to 2024 Census Cartographic Boundary census tract polygons (cb_2024_us_tract_500k) and dissolved to county level. State and national boundaries were drawn from the Census Bureau's 2024 TIGER/Line national geodatabase.

<figure class="project-map wide">
  <a href="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_1.png' | relative_url }}">
    <img src="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_1.png' | relative_url }}" alt="Fig.1: At-risk dams by state (choropleth) with North Carolina at-risk dams shown in the inset. North Carolina leads all states with 247 at-risk dams">
  </a>
  <figcaption>Fig.1: At-risk dams by state (choropleth) with North Carolina at-risk dams shown in the inset. North Carolina leads all states with 247 at-risk dams</figcaption>
</figure>

## Projection

All layers were projected to USA Contiguous Albers Equal Area Conic (EPSG 5070). Because this analysis counts and compares dams across states and counties and aggregates population within those polygons an equal-area projection was selected: it preserves the relative sizes of areal units so that spatial joins, counts, and population comparisons are not distorted by the areal exaggeration common to conformal or unprojected geographic coordinate systems. Using a single projected coordinate system from the national scale down to the county level also kept every layer spatially aligned, ensuring that point-in-polygon selections and joins matched features accurately throughout.

## Attribute Queries

Attribute queries were performed on the National Inventory of Dams point layer using Select by Attributes. The first query selected all dams nationally with a High hazard potential classification (publicHazardId = 'High'). The second selected dams classified as either High or Significant hazard potential. These single-layer selections rely only on stored attribute values, with no geometry involved. Each selection was exported to its own feature class to preserve the result. The High-hazard selection also established the starting population of dams that later attribute filtering "by condition assessment" would narrow into the "at-risk" subset used throughout the remainder of the analysis.

## Spatial Queries

Spatial queries were performed using Select by Location, which selects features based on their geometric relationship to another layer rather than their attributes. The first query selected at-risk dams (High hazard and Poor or Unsatisfactory condition) located within the North Carolina state boundary, isolating the 247 dams inside the study area. The second selected North Carolina counties that contain at least one at-risk dam, using a "contains" relationship between county polygons and dam points. Both queries were run on layers already projected to EPSG 5070 to ensure accurate spatial alignment, and results were exported for mapping.

## Spatial Joins

Spatial joins transferred and aggregated information between layers based on location. The first join spatially joined at-risk dam points to state polygons using a one-to-one "contains" relationship, producing a count of at-risk dams per state; sorting this count identified North Carolina as the national leader. The second join applied the same method within North Carolina, joining the state's at-risk dam points to its 100 county polygons to count dams per county. County population was then summarized from Census tract data and joined by GEOID, producing a single county layer carrying both at-risk dam counts and total population.

## Combination

The combination questions integrated attribute filtering, spatial relationships, and joins. Starting from the county layer containing both at-risk dam counts and summarized population, counties were classified by dam burden and population using manual breaks. Select by Attributes then isolated counties meeting combined criteria; for example, a high at-risk-dam count together with a large population (Question 7), or a high dam count paired with a small population (Question 8). Because dam counts derived from a spatial join and population from a tabular join, answering these questions required all three operation types working together on a single integrated layer.

## Assumptions

This analysis defines an "at-risk" dam as one with a High hazard potential classification and a condition assessment of Poor or Unsatisfactory. This assumes hazard potential and condition together approximate risk—hazard captures the consequence of failure (probable loss of life), while condition captures the likelihood of a problem. It is important to note that hazard potential does not, by itself, indicate that a dam is failing; it describes downstream consequences if failure occurred. A key assumption is that dams lacking a condition rating are excluded rather than assumed safe: of roughly 17,000 High-hazard dams nationally, only about 2,800 carry a Poor or Unsatisfactory rating, and the remainder are unrated or better-rated. The at-risk set therefore reflects only dams with reported poor conditions and undercounts true exposure.

Second, each dam is treated as a single point, and population is measured as the total population of the county containing it. This assumes county population is a reasonable proxy for potential exposure, though it does not model the actual downstream inundation area, which may cross county lines or affect only part of a county. Finally, ACS population and income values are five-year estimates carrying margins of error and are treated as approximations.

## Results

The first attribute query returned 16,959 dams nationally classified as High hazard potential dams whose failure would probably cause loss of human life. The second query, selecting dams classified as either High or Significant hazard potential, returned 28,275 dams, adding those where failure would cause significant economic, environmental, or lifeline damage but no expected loss of life. Together these results establish the national scope of consequential dams and provide the starting point for the funnel. The High-hazard result is particularly important: it defines the population of dams that subsequent condition-based filtering narrows into the "at-risk" subset. Notably, when the High-hazard set was further filtered to those in Poor or Unsatisfactory condition, only 2,789 dams remained, roughly one in six, because the majority of High-hazard dams are either unrated or in better-reported condition. This attribute filtering alone reveals a meaningful national pattern: while tens of thousands of dams carry serious potential consequences, a much smaller, identifiable subset combines high consequence with documented poor condition, making them great candidates for closer geographic examination.

### Attribute Queries

<figure class="project-table">
  <table>
    <thead>
      <tr>
        <th>Selection</th>
        <th>Criteria</th>
        <th>Dam count</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>High hazard potential</td>
        <td><code>publicHazardId = 'High'</code></td>
        <td>16,959</td>
      </tr>
      <tr>
        <td>High or Significant hazard</td>
        <td><code>publicHazardId IN ('High','Significant')</code></td>
        <td>28,275</td>
      </tr>
      <tr>
        <td>At-risk (High + Poor/Unsatisfactory)</td>
        <td>Hazard = High <em>and</em> condition in (Poor, Unsatisfactory)</td>
        <td>2,789</td>
      </tr>
    </tbody>
  </table>
  <figcaption>Table 1: National attribute-query results. Filtering High-hazard dams to those in Poor or Unsatisfactory condition isolates the 2,789 "at-risk" dams that anchor the funnel.</figcaption>
</figure>

### Spatial Queries

The spatial queries narrowed the analysis from national scope to North Carolina. The first query, selecting at-risk dams located within the North Carolina boundary, returned 247 dams, the largest at-risk count of any state, ahead of Texas (217) and Georgia (199). This confirmed North Carolina as the focus of the funnel (see fig.1)The second spatial query identified which North Carolina counties contain at least one at-risk dam: 62 of the state's 100 counties, meaning at-risk dams are widespread rather than confined to a few locations. Their distribution is not uniform, however; the selected dams concentrate in the Piedmont and western mountain regions, consistent with North Carolina's large inventory of small, privately owned dams in those areas. These spatial selections demonstrate how geometry alone, without reference to attributes can isolate features by location: dams falling inside a state polygon, and counties containing dam points. The results set up the spatial joins that follow by establishing both the study area (North Carolina) and the units of aggregation (its counties) for counting and comparison.

### Spatial Joins

The spatial joins aggregated at-risk dams to two levels of geography. Joining at-risk dam points to state polygons produced a count per state, ranking North Carolina first with 247, followed by Texas (217), Georgia (199), South Carolina (162), and New Mexico (145); a distinct southeastern cluster. This join is what quantitatively selected North Carolina as the funnel's focus. Joining the state's 247 dams to its 100 counties then revealed how that burden distributes internally. The counts range widely: Macon and Wake counties tie for the highest at-risk-dam burden with 17 each, followed by Transylvania (15), Caldwell (10), and a group of counties with 8. The county join also confirmed that all 247 dams fell within North Carolina counties, with no points lost to misalignment. A separate tabular join then attached county population, summarized from Census tract data, to each county. The result is a single integrated layer carrying both at-risk-dam counts and total population per county, the analytical foundation for the combination questions, which pair dam burden against the number of people in each county.

<figure class="project-map wide">
  <a href="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_2.png' | relative_url }}">
    <img src="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_2.png' | relative_url }}" alt="At-risk dams by North Carolina county. Macon and Wake counties carry the highest burden with 17 each. Data: USACE NID (2026); U.S. Census (2024)">
  </a>
  <figcaption>Fig.2: At-risk dams by North Carolina county. Macon and Wake counties carry the highest burden with 17 each. Data: USACE NID (2026); U.S. Census (2024).</figcaption>
</figure>

### Combination

The combination analysis paired each county's at-risk-dam burden against its population, revealing a pattern that neither variable shows alone. Two counties carry the highest burden, Macon and Wake, both with 17 at-risk dams. Notably they sit at opposite extremes of population: Wake holds 1,151,009 residents, Macon just 37,533. Classifying all counties by dam burden and population exposed two distinct high-burden groups. The first pairs many at-risk dams with large populations: Wake (17 dams) and Cumberland (8 dams; 336,749 residents) represent high-consequence settings where failures could affect large numbers of people. The second pairs equally high dam burdens with small populations: Macon (17), Transylvania (15; 33,243 residents), and Polk (8; 19,689 residents), rural, largely mountainous counties carrying heavy dam responsibility with limited population and, by extension, limited local tax base and emergency-management capacity. Notably, Mecklenburg County (Charlotte), despite 1.13 million residents, has only 4 at-risk dams, showing that population size alone does not predict dam burden. The combination thus distinguishes counties facing a consequence problem from those facing a capacity problem.

<figure class="project-map wide">
  <a href="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_3.png' | relative_url }}">
    <img src="{{ 'assets/images/projects/vector-spatial-analysis/high-hazard-dams-in-north-carolina/Fig_3.png' | relative_url }}" alt="Bivariate choropleth of North Carolina counties crossing at-risk-dam burden (x-axis) with total population (y-axis). Counties in the upper-right combine many at-risk dams with large populations (Wake, Cumberland); counties in the lower-right combine many at-risk dams with small populations (Macon, Transylvania, Polk). Data: USACE NID (2026); U.S. Census Bureau (2024).">
  </a>
  <figcaption>Fig.3: Bivariate choropleth of North Carolina counties crossing at-risk-dam burden (x-axis) with total population (y-axis). Counties in the upper-right combine many at-risk dams with large populations (Wake, Cumberland); counties in the lower-right combine many at-risk dams with small populations (Macon, Transylvania, Polk). Data: USACE NID (2026); U.S. Census Bureau (2024).</figcaption>
</figure>

## Conclusion

This analysis used a national-to-county funnel to identify where dam risk and population intersect in the United States, ultimately focusing on North Carolina. The attribute queries established the national scope: 16,959 dams carry a High hazard potential classification, and 28,275 are classified High or Significant. Filtering the High-hazard set to those in Poor or Unsatisfactory condition isolated 2,789 "at-risk" dams nationally, dams combining serious potential consequences with documented poor condition. This subset, roughly one in six High-hazard dams, became the basis for the geographic analysis.

The spatial queries and joins answered where these at-risk dams concentrate. North Carolina contains 247 at-risk dams, more than any other state, ahead of a broader southeastern cluster including Texas, Georgia, and South Carolina. Within North Carolina, at-risk dams appear in 62 of 100 counties, concentrated in the Piedmont and western mountains. Joining dams to counties showed that Macon and Wake counties carry the highest burden, with 17 at-risk dams each, followed by Transylvania and Caldwell. These operations answered the core "where" questions: at-risk dams are widespread but unevenly distributed, with identifiable hotspots.

The combination analysis produced the most important finding by pairing dam burden with population. Although Macon and Wake tie for the highest dam count, they represent fundamentally different risk profiles. Wake County, with over 1.1 million residents, and Cumberland County (336,749) exemplify a consequence problem: many at-risk dams sitting in large populations. Macon (37,533), Transylvania (33,243), and Polk (19,689) exemplify a capacity problem: equally high dam burdens in small, rural, mountainous counties with far fewer people and, by extension, more limited local resources to inspect, maintain, and manage those dams. Mecklenburg County, despite being one of the state's most populous, has only four at-risk dams, confirming that population size alone does not predict dam burden. Taken together, the analysis shows that prioritizing dam risk requires looking beyond raw counts: the same numeric burden can signify either widespread exposure or concentrated local strain, and effective triage must distinguish between them.

## Limitations

The most significant limitation is incomplete condition data. Only 2,789 of roughly 17,000 High-hazard dams carry a Poor or Unsatisfactory rating; the majority are unrated or reported in better condition. Because unrated dams were excluded from the at-risk set, the analysis may undercount true risk. A county shown here with few at-risk dams may simply have many unrated dams rather than genuinely safe ones. The state and county rankings therefore reflect where poor conditions are documented, not necessarily where risk is greatest, an important caveat for any prioritization drawn from these results.

Second, each dam is represented as a single point, and exposure is measured as total county population rather than the actual downstream inundation area. A dam's flood path may cross county lines or threaten only part of a county, so county population is a coarse proxy for who is genuinely at risk. This particularly affects the combination findings: the consequence-versus-capacity distinction is meaningful at the county scale but would require downstream inundation modeling to identify truly exposed populations. 

## References

U.S. Army Corps of Engineers. (2026). *National Inventory of Dams.* https://nid.sec.usace.army.mil

U.S. Army Corps of Engineers. (2026). *National Inventory of Dams: Data Dictionary.* https://doi.org/10.21949/1529016

U.S. Census Bureau. (2023). *American Community Survey 5-Year Estimates, 2019–2023, Table B01003: Total Population.* https://data.census.gov

U.S. Census Bureau. (2023). *American Community Survey 5-Year Estimates, 2019–2023, Table B19013: Median Household Income in the Past 12 Months.* https://data.census.gov

U.S. Census Bureau. (2024). *Cartographic Boundary Files — Census Tracts (cb_2024_us_tract_500k).* https://www.census.gov/geographies/mapping-files/time-series/geo/cartographic-boundary.html

U.S. Census Bureau. (2024). *TIGER/Line Geodatabase — National.* https://www.census.gov/geographies/mapping-files/time-series/geo/tiger-geodatabase-file.html