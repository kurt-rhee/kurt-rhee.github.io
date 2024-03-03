---
layout: post
title: A Disambiguation of Horizontal Single Axis Tracking Algorithms
description: Blog
summary: A Disambiguation of Horizontal Single Axis Tracking Algorithms
tags: [hsat, horizontal, tracker, angle, gcr, truetracking, backtracking]
---




![Flat]({{ site.url }}/assets/images/trackers.jpeg#center)
**Figure 2:**  A rendition of a horizontal single-axis tracking system made by Google's Gemini (formerly Bard)

# Introduction

Horizontal single-axis trackers (HSAT) play a crucial role in maximizing solar energy production, especially where I work in North America. Understanding the differences between the four main types of tracking modes:  astronomical tracking, GCR-based backtracking, slope-aware backtracking and terrain-aware backtracking can help solar performance engineers accurately model modern tracker systems and make informed system optimization decisions.  The following post is a high level primer on the four main types of tracking modes with references for those that might want to dive deeper.  

In order to keep the post reasonably long, I've elected to skip over some algorithms which some tracker companies may employ.  If these algorithms interest you, let me know and I may write another blog post about them some time in the future.

**Algorithms for a future blog post:**

- Diffuse Irradiance Optimization
- Wind Stow
- Hail Stow
- Flood Stow

**Disclaimer:**
I am a former employee of Nevados Engineering which makes terrain following trackers and I own stock in the company.   I try to write an un-biased blog, but no person exists completely outside of the influence of biases, and I encourage readers to take everything written here with a grain of salt. 

# 1. Astronomical Tracking AKA True Tracking

![Flat]({{ site.url }}/assets/images/flat.jpeg#center)
**Figure 2:**  This algorithm was intended for use on flat surfaces.

Astronomical tracking involves aligning solar panels with the sun's position throughout the day. This approach by definition maximizes the transposed irradiance on the surface of the PV modules during clear-sky conditions, but can cause inter-row shading because it does not take into account the system geometry.  Readers interested in developing their own tracking or performance modeling software may want to consult the further reading section below.  

**Further reading:** 
- [Marion and Dobos 2013](https://github.com/kurt-rhee/pv-model-comparison/blob/main/tracking_angle_models/Marion_and_Dobos.pdf)
- [Surface Angles Primer](https://github.com/pvlib/pvlib-python/issues/1911)


# 2. Backtracking

![Flat]({{ site.url }}/assets/images/flat.jpeg#center)**Figure 3:**  This algorithm was intended for use on flat surfaces.

Backtracking adjusts the position of solar panels to prevent shading from adjacent rows assuming that all the module surfaces are horizontal and co-planar. The tracked surface can be made horizontal and co-planar either by building on flat land, or adjusting the tracker pile heights to bring the above surface into level.  Backtracking systems receive less transposed irradiance during clear-sky conditions than astronomically tracked systems, but also receive less row-to-row shading.  

The choice of between astronomical tracking and backtracking often comes down to whether or not the modules being tracked experience non-linear energy losses due to shade.  Modules that experience linear shade losses are often astronomically tracked instead of backtracked.  I am currently unaware if there is an original paper describing backtracking, if you are looking for additional content on the topic, I would recommend the "further reading" section under the slope-aware backtracking section.  The Anderson and Mikofski paper there does a good job explaining how backtracking in general works.

An aside to talk about modeling:  In PVsyst version 7.0, cirum-solar irradiance was included in the beam portion of irradiance rather than the diffuse portion.  This change increases the amount of energy predicted for tracking systems and also increases the relative performance of backtracking systems relative to astronomical tracking ones.  There are no studies that I am aware at the time of writing which discuss the trade-offs between modeling circumsolar as direct or diffuse, but if you are reading this sometime in the future and would like to contribute to this blog post please feel free to send published research my way.

**Further reading:**
- [PVSyst 7.0 Announcement](https://forum.pvsyst.com/topic/2135-new-circumsolar-treatment-in-v-70/)
- [Perez Components](https://pvpmc.sandia.gov/modeling-guide/1-weather-design-inputs/plane-of-array-poa-irradiance/calculating-poa-irradiance/poa-sky-diffuse/perez-sky-diffuse-model/)

.
# 3. Slope-Aware Backtracking

![Sloped]({{ site.url }}/assets/images/slope.jpeg#center)
**Figure 4:**  This algorithm was intended for use on sloped surfaces.

Slope-aware backtracking is an improvement on standard backtracking which considers the slope of the terrain in addition to the ground coverage ratio (GCR) of the system.  This approach reduces the amount of row-to-row shading on sloped systems relative to a backtracking algorithm which does not take into account slope.  The slope aware backtracking algorithm assumes that there is only one slope experienced by the system and that the trackers are co-planar (aka not terrain-following).  Since not all site terrains can be characterized by only one single slope, advanced users of the algorithm may set up multiple sub-zones in their system so that each zone uses a different slope-aware backtracking strategy.  As the number of zones increases the more this algorithm approximates the terrain-aware backtracking algorithms that follow.

**Further reading:**
- [Anderson and Mikofski 2020](https://github.com/kurt-rhee/pv-model-comparison/blob/main/tracking_angle_models/Anderson_Mikofski_2020.pdf)
- [Leung et al 2021](https://ieeexplore.ieee.org/abstract/document/9573469)

# 4. Terrain-Aware Backtracking

![Terrain]({{ site.url }}/assets/images/terrain.jpeg#center)
**Figure 5:**  These algorithms were intended for use on variable terrain surfaces

There are a few different approaches to backtracking on variable terrain, meaning terrain which changes 


## 4. a. Increased GCR Backtracking on Terrain

## 4. c. Current Sensor Backtracking

## 4. d. Digital Twin (Ray-Casting) Backtracking

