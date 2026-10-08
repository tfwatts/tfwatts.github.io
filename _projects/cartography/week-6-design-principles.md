---
title: "Cartographic Generalization Across Scale: Florida"
course: "Cartography"
method: "Generalization Techniques"
order: 6
featured: false
thumbnail: /assets/images/projects/cartography/week-6/thumb.jpg
summary: "A three map layout showing how cartographic generalization simplifies Florida and its surroundings as map scale decreases from 1:10,000,000 to 1:100,000,000."
---

<figure class="project-map">
  <a href="{{ '/assets/images/projects/cartography/week-6/map.png' | relative_url }}">
    <img src="{{ '/assets/images/projects/cartography/week-6/map.png' | relative_url }}" alt="A three map layout showing how cartographic generalization simplifies Florida and its surroundings as map scale decreases from 1:10,000,000 to 1:100,000,000.">
  </a>
  <figcaption>Florida at 1:10M, 1:50M, and 1:100M: coastlines simplify, small lakes drop away, and urban areas merge and then collapse to points as the scale gets smaller.</figcaption>
</figure>

## Overview

This layout presents Florida at three scales, 1:10,000,000, 1:50,000,000, and 1:100,000,000, built in ArcGIS Pro with Natural Earth data and projected in NAD 1983 Florida GDL Albers. The assignment for this graduate cartography course called for three side by side maps of the same region to show how much detail a map can hold as its scale decreases. Each map applies generalization techniques suited to its scale: coastlines simplify, lakes are kept or removed based on the smallest size a reader can see, and urban areas are merged at 1:50M and reduced from shapes to points at 1:100M. Labels follow the same logic, with city names at the largest scale and only country and ocean names at the smallest. Matching symbols and scale bars of equal length keep the comparison focused on what changes from one scale to the next.