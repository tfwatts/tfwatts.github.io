---
title: "Line-of-Sight Optimization for Counter-UAS Radar Siting"
course: "Raster Spatial Analysis"
method: "Viewshed-Based Analysis"
order: 2
featured: true
thumbnail: /assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/thumb.jpg
banner: /assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/banner.png
banner_alt: "Satellite Photo of Levi Stadium"
summary: "A viewshed-based siting analysis that uses a 1 m terrain-and-building surface model to place counter-UAS radars around Levi’s Stadium, evaluating how much of a drone’s low-altitude approach corridor a small set of sites can cover and whether coverage is limited by terrain or by sensor count."
---

<p class="project-byline">
  By Thomas Watts · Raster Spatial Analysis
</p>

## Introduction

Small unmanned aircraft systems (UAS) are a persistent, low-cost threat to fixed assets such as stadiums and critical infrastructure, able to fly low and approach along concealed routes. Defending against them depends on detection. Radar, one of the primary counter-UAS sensors, is a line-of-sight instrument: at counter-UAS frequencies, any building or ridge between the radar and a low-flying drone casts a detection shadow. Radar placement is therefore a spatial problem governed by the surrounding surface.
Siting counter-UAS radars is a location-allocation problem: which subset of possible sites achieves the required coverage? The value of each site depends on those already chosen, a radar duplicating coverage adds little, one filling a gap may be critical, so the problem can be solved iteratively against a combined picture of the threat corridor and the terrain-plus-building surface that controls visibility.
This project asked: for a defined asset and the low-altitude corridor a small UAS would use to approach it, what set of radar sites and mast heights achieves line-of-sight coverage of that corridor, and which coverage gaps are terrain- and structure-imposed and therefore unavoidable? Using Levi's Stadium (Santa Clara, CA) as the asset, the analysis applies raster methods, a digital surface model, viewshed analysis, and an iterative coverage-selection heuristic in ArcGIS Pro and ModelBuilder. Because building heights are modeled (discussed in limitations), the work is a reproducible methodology demonstration, rather than an operational recommendation.

## Study area

The study area is a 20 km-radius region centered on Levi's Stadium in Santa Clara, California, in the southern San Francisco Bay Area. It spans dense urban and commercial development through Santa Clara and San Jose, open low-lying and reclaimed ground toward the bay, and South Bay salt marsh and open water to the north, over generally flat valley-floor terrain rising to low hills at the edges. This mix of built-up areas, open flats, and coastal margin suits a viewshed-based siting problem, where coverage is governed by buildings in the urban core and by open approaches over flat and coastal ground. The surface model covers the full 20 km extent; the siting analysis focuses on a threat corridor within roughly 5 km of the stadium.

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig1.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig1.png' | relative_url }}" alt="Fig.1: Study area: a 20 km radius around Levi's Stadium, Santa Clara, California, in the southern San Francisco Bay Area.">
  </a>
  <figcaption>Fig.1: a 20 km radius around Levi's Stadium, Santa Clara, California, in the southern San Francisco Bay Area.</figcaption>
</figure>

## Data

<!-- ============================================================
     DATA SECTION — uses your stylesheet classes (no inline styling)
     Place inside <main class="site-main"> ... </main>
     ============================================================ -->

<div class="table-scroll">
  <table class="data-table">
    <thead>
      <tr>
        <th>Dataset</th>
        <th>Source</th>
        <th>Type / Format</th>
        <th>Resolution / Units</th>
        <th>Coordinate System</th>
        <th>Role in Project</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>USGS 3DEP 1&nbsp;m DEM (bare earth)</td>
        <td>USGS / The National Map (3DEP)</td>
        <td>Raster (elevation)</td>
        <td>1&nbsp;m, meters</td>
        <td>NAD83 UTM 10N (EPSG&nbsp;26910)</td>
        <td>Base terrain surface (42 tiles)</td>
      </tr>
      <tr>
        <td>UT-GLOBUS building footprints + modeled heights</td>
        <td>Zenodo record 11156602 (Kamath et al., 2024)</td>
        <td>Vector polygons w/ height attribute</td>
        <td>Height above ground, meters</td>
        <td>WGS84 UTM 10N (EPSG&nbsp;32610)</td>
        <td>Building layer for the DSM</td>
      </tr>
      <tr>
        <td>NLCD Land Cover</td>
        <td>MRLC / NLCD</td>
        <td>Raster (categorical)</td>
        <td>30&nbsp;m</td>
        <td>Albers (EPSG&nbsp;5070)</td>
        <td>Feasibility masking (water / wetland)</td>
      </tr>
      <tr>
        <td>IRIS C-UAS radar specifications</td>
        <td>Robin Radar Systems product sheet</td>
        <td>Reference (parameters)</td>
        <td class="na">&mdash;</td>
        <td class="na">&mdash;</td>
        <td>Sensor parameters (range, elevation, mast)</td>
      </tr>
    </tbody>
  </table>
</div>

<h3>Main characteristics</h3>
<p>
  The <strong>USGS 3DEP 1&nbsp;m DEM</strong> is a lidar-derived bare-earth elevation product distributed
  through The National Map at 1&nbsp;m resolution in NAD83 UTM 10N; 42 tiles span the study area. It provides
  the ground surface on which the digital surface model is built.
</p>
<p>
  <strong>UT-GLOBUS</strong> (Kamath et al., 2024) is a global dataset of building footprints with modeled
  height-above-ground, obtained from Zenodo. Heights are expressed in meters relative to the ground rather
  than as absolute elevations, and are modeled rather than surveyed, which carries a known per-building error,
  discussed in the limitations. It supplies the building form for the surface model.
</p>
<p>
  <strong>NLCD land cover</strong> (30&nbsp;m, MRLC) is a categorical raster classifying land into cover types
  such as developed, open water, and wetland. Its open-water and wetland classes define ground that is not
  plausibly sitable for a radar, supporting the feasibility mask.
</p>
<p>
  The <strong>Robin Radar IRIS C-UAS</strong> product sheet describes a 360&deg; azimuth, 60&deg;
  elevation-coverage radar whose detection range scales with drone size. This project adopts the C1
  &ldquo;Micro&rdquo; class (0.9&nbsp;kg) detection range of <strong>3,100&nbsp;m</strong> as the radar search
  radius and a fixed <strong>3&nbsp;m mast</strong> reflecting a realistic tripod or vehicle deployment. These
  specifications supply the sensor parameters used in the viewshed analysis.
</p>

<h3>Data citations</h3>
<div class="references">
  <p>
    Kamath, H., Singh, M., Malviya, N., Martilli, A., He, L., Aliaga, D., He, C., Chen, F., Magruder, L.,
    Yang, Z.-L., &amp; Niyogi, D. (2024). <em>GLObal Building heights for Urban Studies (UT-GLOBUS)</em>
    [Data set]. Zenodo. https://doi.org/10.5281/zenodo.11156602 (CC-BY-4.0)
  </p>
  <p>
    Kamath, H., et al. (2024). GLObal Building heights for Urban Studies (UT-GLOBUS).
    <em>Scientific Data</em>, 11, 886. https://doi.org/10.1038/s41597-024-03719-w
  </p>
</div>

## Methods

<!-- ============================================================
     METHODS SECTION — uses your stylesheet (no inline styling)
     Centered subheaders via one small inline rule only.
     Place inside <main class="site-main"> ... </main>
     ============================================================ -->

<p>
  The analysis was implemented in ArcGIS Pro using ModelBuilder automation. It proceeded in two stages:
  construction of a digital surface model (DSM) and definition of a threat corridor, followed by
  viewshed-based candidate scoring and radar site selection. All processing was performed in
  NAD83 UTM 10N (EPSG&nbsp;26910) at 1&nbsp;m resolution.
</p>

<h3 style="text-align:center;">Surface preparation (DSM)</h3>
<p>
  A bare-earth surface was assembled by mosaicking 42 USGS 3DEP 1&nbsp;m DEM tiles. Building form was added
  from UT-GLOBUS footprints carrying modeled height-above-ground. Because these heights are relative to the
  ground, the building layer was reprojected to match the DEM, rasterized to the DEM grid, and combined
  additively: where a building footprint exists, its height was added to the underlying ground elevation,
  producing rooftop surface elevations; elsewhere the bare-earth values were retained. The combined DSM was
  clipped to a 20&nbsp;km-radius study area centered on Levi&rsquo;s Stadium. The 20&nbsp;km extent deliberately
  exceeds the analysis area so that terrain and structures beyond the corridor are available to occlude
  line-of-sight, avoiding an artificial horizon at the study edge.
</p>

<h3 style="text-align:center;">Threat corridor</h3>
<p>
  The asset was represented by the stadium footprint. The threat corridor was defined as an annulus around it,
  from a 2&nbsp;km inner standoff (the &ldquo;detect-before&rdquo; line, affording reaction time against an
  inbound small UAS) to a 5&nbsp;km outer edge (the standoff distance plus approximately one sensor detection
  range, beyond which coverage from a corridor-adjacent sensor is not expected). Low-altitude threat was
  represented by a target height of 30&nbsp;m above the surface.
</p>

<h3 style="text-align:center;">Radar parameters</h3>
<p>
  Parameters were drawn from the Robin Radar IRIS C-UAS specification. The sensor was modeled with a
  3,100&nbsp;m search radius (the C1 &ldquo;Micro&rdquo; class detection range), a 3&nbsp;m observer offset
  representing a fixed deployable mast, a 30&nbsp;m target offset (drone altitude), and a 60&deg; vertical upper
  angle reflecting the radar&rsquo;s elevation coverage limit. Geodesic Viewshed was used, which applies
  earth-curvature correction inherently; a refractivity coefficient of 0.13 was applied.
</p>

<h3 style="text-align:center;">Feasibility masking and candidate generation</h3>
<p>
  Candidate radar sites were restricted to plausibly sitable ground by excluding open water and wetland
  (from NLCD land cover, converted to vector exclusion zones to avoid resampling a categorical raster) and the
  2&nbsp;km standoff interior. Candidate sites were generated as a systematic point grid. An initial 500&nbsp;m
  grid over the full feasible area produced far more candidates than the problem required, with most positioned
  too far from the corridor to achieve line-of-sight to it. The candidate set was therefore restricted to points
  within one detection range (3,100&nbsp;m) of the corridor and regenerated at 1&nbsp;km spacing appropriate to
  the sensor&rsquo;s range, yielding 141 corridor-relevant candidates.
</p>

<h3 style="text-align:center;">Candidate scoring (viewshed)</h3>
<p>
  For each of the 141 candidates, a Geodesic Viewshed was computed over the DSM and clipped to the corridor,
  producing a per-candidate raster of the corridor cells that site can see. This was automated with a
  ModelBuilder iterator. Summing all candidate viewsheds identified the coverable corridor &mdash; cells visible
  to at least one candidate &mdash; and, by difference, the cells visible to none: the terrain- and
  structure-imposed shadows that constitute unavoidable gaps.
</p>

<h3 style="text-align:center;">Radar site selection</h3>
<p>
  A running &ldquo;uncovered&rdquo; raster was created as the full coverable corridor &mdash; the cells visible
  to at least one candidate. In the first round, each of the 141 candidate viewsheds was scored by the number of
  corridor cells it covered, and the highest-scoring candidate was selected as the first sensor. Its coverage was
  then subtracted from the uncovered raster. In each subsequent round, every remaining candidate&rsquo;s coverage
  was intersected with the current uncovered raster to compute its marginal contribution, and the candidate with
  the greatest marginal coverage was selected, after which its coverage was subtracted and the process repeated.
</p>

## Notes on methods

  Two practical constraints shaped how the process was run. First, because each scoring round required
  re-evaluation of every candidate against the updated uncovered surface at 1&nbsp;m resolution, a full round was
  computationally expensive. As a result, candidates contributing negligible coverage in the first round were
  excluded from subsequent rounds, and scoring in later rounds was restricted to the strongest candidates by
  initial coverage. This is an approximation, as a low-scoring candidate is assumed not to become a leading
  marginal contributor later.

<p>
  Second, rather than iterating until the entire coverable corridor was covered, selection was run for a fixed
  budget of three radars. This reframes the analysis from &ldquo;the minimum sensors for complete coverage&rdquo;
  to &ldquo;the coverage achievable within a defined sensor budget, and where the residual gaps are&rdquo; &mdash;
  a framing consistent with real siting decisions, which are typically budget-constrained. After the three sensors
  were placed, the cumulative coverage was computed as a percentage of the coverable corridor, and the residual
  uncovered corridor was mapped alongside the terrain- and structure-imposed shadows identified during candidate
  scoring. Together these constitute the project&rsquo;s coverage-gap output.
</p>

## Results

The iterative site-selection routine placed three radars, each chosen as the site adding the most previously uncovered corridor at the time of its selection. The three sites together cover 72.6% of the coverable corridor - 49,464,259 of 68,155,276 coverable cells - leaving 27.4% uncovered within a three-radar budget.
Coverage grew with diminishing returns as sites were added. The first radar covered 26.2% of the corridor on its own, the second raised cumulative coverage to 51.7%, and the third brought it to 72.6%. Each successive site added slightly less than the last (roughly 17.9, 17.4, and 14.2 million cells), reflecting increasing overlap with corridor already covered, the expected pattern when each site is selected for the coverage it adds beyond those already placed.
Candidate scoring, performed before any site was selected, established how much of the corridor is coverable at all. Of the full corridor (68,766,160 cells), 99.1% (68,155,276 cells) can be seen by at least one candidate radar, while only 0.9% (610,884 cells) is shadowed from every candidate position by terrain or buildings. The corridor is therefore almost entirely coverable, with permanent terrain- and structure-imposed shadows accounting for less than one percent of its area.

<!-- ============================================================
     RESULTS — All three tables
     Uses your stylesheet (.data-table.numeric, .table-scroll, caption)
     Place inside <main class="site-main"> ... </main>
     ============================================================ -->

<!-- ---------- TABLE 1: Per-radar coverage ---------- -->
<h3>Coverage by radar (cumulative selection)</h3>

<div class="table-scroll">
  <table class="data-table numeric">
    <thead>
      <tr>
        <th>Radar</th>
        <th>Site (candidate)</th>
        <th>Coverage added (cells)</th>
        <th>Cumulative coverage (cells)</th>
        <th>Cumulative&nbsp;%</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>1</td>
        <td><code>cov_95</code></td>
        <td>17,887,708</td>
        <td>17,887,708</td>
        <td>26.2%</td>
      </tr>
      <tr>
        <td>2</td>
        <td><code>cov_72</code></td>
        <td>17,364,260</td>
        <td>35,251,968</td>
        <td>51.7%</td>
      </tr>
      <tr>
        <td>3</td>
        <td><code>cov_126</code></td>
        <td>14,212,291</td>
        <td>49,464,259</td>
        <td>72.6%</td>
      </tr>
    </tbody>
    <caption>
      Cumulative % is relative to the coverable corridor (68,155,276 cells). Cell counts are 1&nbsp;m
      raster cells (1 cell = 1&nbsp;m&sup2;).
    </caption>
  </table>
</div>

<!-- ---------- TABLE 2: Corridor coverability summary ---------- -->
<h3>Corridor coverability and coverage summary</h3>

<div class="table-scroll">
  <table class="data-table numeric">
    <thead>
      <tr>
        <th>Measure</th>
        <th>Cells</th>
        <th>% of corridor</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Total threat corridor</td>
        <td>68,766,160</td>
        <td>100.0%</td>
      </tr>
      <tr>
        <td>Coverable (visible to &ge;1 candidate)</td>
        <td>68,155,276</td>
        <td>99.1%</td>
      </tr>
      <tr>
        <td>Permanent shadow (visible to no candidate)</td>
        <td>610,884</td>
        <td>0.9%</td>
      </tr>
    </tbody>
    <caption>
      Percentages are relative to the total threat corridor (68,766,160 cells). Cell counts are 1&nbsp;m
      raster cells (1 cell = 1&nbsp;m&sup2;).
    </caption>
  </table>
</div>

<!-- ---------- TABLE 3: Three-radar outcome ---------- -->
<h3>Three-radar coverage outcome</h3>

<div class="table-scroll">
  <table class="data-table numeric">
    <thead>
      <tr>
        <th>Outcome</th>
        <th>Cells</th>
        <th>% of coverable corridor</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>Covered by 3 radars</td>
        <td>49,464,259</td>
        <td>72.6%</td>
      </tr>
      <tr>
        <td>Uncovered within 3-radar budget</td>
        <td>18,691,017</td>
        <td>27.4%</td>
      </tr>
    </tbody>
    <caption>
      Percentages are relative to the coverable corridor (68,155,276 cells). Cell counts are 1&nbsp;m
      raster cells (1 cell = 1&nbsp;m&sup2;).
    </caption>
  </table>
</div>

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig2.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig2.png' | relative_url }}" alt="Three map panel showing each selected site.">
  </a>
  <figcaption>Fig.2: Sequential selection of three radar sites (cov_95, cov_72, cov_126) over the digital surface model and threat corridor, each chosen for the coverage it adds beyond those already placed.</figcaption>
</figure>

<figure class="project-map wide">
  <a href="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig3.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/raster-spatial-analysis/line-of-sight-optimization-for-counter-uas-radar-siting/fig3.png' | relative_url }}" alt="Zoomed view of the threat corridor over the digital surface model, showing the coverage-count raster. Cell values indicate how many candidate radar sites can see each location, illustrating overlapping viewshed coverage and how it varies with terrain and buildings.">
  </a>
  <figcaption>Fig.3: Combined line-of-sight coverage from the three selected radars over the threat corridor, with residual uncovered areas shown as gaps.</figcaption>
</figure>

## Limitations

The project's main limitation is that UT-GLOBUS building heights are modeled rather than surveyed, carrying roughly 9 m RMSE per building. Because line-of-sight is height-sensitive, this error propagates into the viewshed results, a modeled building may cast a shadow that does not exist, or miss a real one, which is the main reason the work is framed as a methodology demonstration rather than an operational recommendation. The conclusions concern the method and the overall coverage pattern rather than any single building.
Other surface limitations include the bare earth plus flat-topped building extrusions omit trees, construction cranes, signage, and other vertical obstructions. Real roofline geometry (pitched roofs, parapets, rooftop equipment) is likewise not represented.
Scope reductions from the original design. The project was initially designed to evaluate coverage across three altitude bands (30, 75, and 120 m above the surface), representing the vertical extent of the low-altitude airspace a drone might use. Due to the computational cost of viewshed analysis at 1 m resolution and the time available, these were reduced to a single 30 m band which represents the most "at risk" and therefore valuable case, with the additional analyses deferred to future work. Coverage is therefore evaluated at one representative drone height rather than across the full low-altitude volume; a drone could in principle fly above or between the modeled bands.
The threat and sensor parameters, the 2–5 km corridor, the 2 km standoff, and the radar specifications (3,100 m detection range, 3 m mast, 60° elevation limit) are realistic but theoretical, chosen to define a problem rather than derived from a specific threat assessment. Candidate sites were placed on a systematic 1 km grid restricted to ground within one detection range of the corridor, so a finer or terrain-aware placement might surface better sites than the grid sampled. Finally, for tractability, weak candidates were dropped from later selection rounds and the routine was run for a fixed budget of three radars rather than iterated to full corridor coverage; the three-sensor result demonstrates the selection method and reports the coverage achievable within that budget, not a proven minimum for complete coverage.
Future work. The most valuable extensions are restoring the multi-band (30/75/120 m) evaluation, allowing for candidate sites at building heights to simulate radar placement on buildings, and iterating the selection to full corridor coverage to report a true minimum sensor count.

## Conclusion

This project set out to apply an iterative, coverage-driven process for siting counter-UAS radars by selecting each site to maximize the low-altitude corridor it covers beyond those already selected and to assess how effectively a small number of radars, chosen this way, can cover the approach corridor to a defined asset. Using Levi's Stadium and a viewshed analysis over a combined terrain-and-building surface, the answer came in two parts. Three radars covered roughly three-quarters of the corridor, each adding less than the last, indicating further radars would keep closing gaps at a declining rate. More tellingly, over 99% of the corridor proved coverable, with under 1% permanently shadowed by terrain or structures. The limiting factor is therefore sensor budget, not terrain: the flat valley floor and open, coastal approaches let candidate radars see nearly the entire corridor, so coverage depends chiefly on how many radars are deployed.
The project demonstrated a reproducible workflow including surface construction, corridor definition, viewshed-based candidate scoring, and iterative site selection that distinguishes gaps a larger budget could close. Because building heights are modeled rather than surveyed, the results are best read as a methodology demonstration; run on surveyed heights and across multiple altitude bands, the same pipeline would support an operational siting decision.
