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
**Figure 2:**  A rendering of a flat terrain.  Astronomical tracking was intended for use on flat surfaces.

Astronomical tracking involves aligning solar panels with the sun's position throughout the day. This approach by definition maximizes the transposed irradiance on the surface of the PV modules during clear-sky conditions, but can cause inter-row shading because it does not take into account the system geometry.  Readers interested in developing their own tracking or performance modeling software may want to consult the further reading section below.  

**Further reading:** 
- [Marion and Dobos 2013](https://github.com/kurt-rhee/pv-model-comparison/blob/main/tracking_angle_models/Marion_and_Dobos.pdf)
- [Surface Angles Primer](https://github.com/pvlib/pvlib-python/issues/1911)


# 2. Backtracking

![Flat]({{ site.url }}/assets/images/flat.jpeg#center)**Figure 3:**  A rendering of a flat terrain.   Backtracking was intended for use on flat surfaces.

Backtracking adjusts the position of solar panels to prevent shading from adjacent rows assuming that all the module surfaces are horizontal and co-planar. The tracked surface can be made horizontal and co-planar either by building on flat land, or adjusting the tracker pile heights to bring the above surface into level.  Backtracking systems receive less transposed irradiance during clear-sky conditions than astronomically tracked systems, but also receive less row-to-row shading.  

The choice of between astronomical tracking and backtracking often comes down to whether or not the modules being tracked experience non-linear energy losses due to shade.  Modules that experience linear shade losses are often astronomically tracked instead of backtracked.  I am currently unaware if there is an original paper describing backtracking, if you are looking for additional content on the topic, I would recommend the "further reading" section under the slope-aware backtracking section.  The Anderson and Mikofski paper there does a good job explaining how backtracking in general works.

An aside to talk about modeling:  In PVsyst version 7.0, cirum-solar irradiance was included in the beam portion of irradiance rather than the diffuse portion.  This change increases the amount of energy predicted for tracking systems and also increases the relative performance of backtracking systems relative to astronomical tracking ones.  There are no studies that I am aware at the time of writing which discuss the trade-offs between modeling circumsolar as direct or diffuse, but if you are reading this sometime in the future and would like to contribute to this blog post please feel free to send published research my way.

**Further reading:**
- [PVSyst 7.0 Announcement](https://forum.pvsyst.com/topic/2135-new-circumsolar-treatment-in-v-70/)
- [Perez Components](https://pvpmc.sandia.gov/modeling-guide/1-weather-design-inputs/plane-of-array-poa-irradiance/calculating-poa-irradiance/poa-sky-diffuse/perez-sky-diffuse-model/)

.
# 3. Slope-Aware Backtracking

![Sloped]({{ site.url }}/assets/images/slope.jpeg#center)
**Figure 4:**  A rendering of a sloped terrain.  Slope-aware backtracking is intended for sloping terrain.

Slope-aware backtracking is an improvement on standard backtracking which considers the slope of the terrain in addition to the ground coverage ratio (GCR) of the system.  This approach reduces the amount of row-to-row shading on sloped systems relative to a backtracking algorithm which does not take into account slope.  The slope aware backtracking algorithm assumes that there is only one slope experienced by the system and that the trackers are co-planar (aka not terrain-following).  Since not all site terrains can be characterized by only one single slope, advanced users of the algorithm may set up multiple sub-zones in their system so that each zone uses a different slope-aware backtracking strategy.  As the number of zones increases the more this algorithm approximates the terrain-aware backtracking algorithms that follow.

**Further reading:**
- [Anderson and Mikofski 2020](https://github.com/kurt-rhee/pv-model-comparison/blob/main/tracking_angle_models/Anderson_Mikofski_2020.pdf)
- [Leung et. al.. 2021](https://ieeexplore.ieee.org/abstract/document/9573469)

# 4. Terrain-Aware Backtracking

![Terrain]({{ site.url }}/assets/images/terrain.jpeg#center)
**Figure 5:**  These algorithms were intended for use on variable terrain surfaces

There are a few different approaches to backtracking on variable terrain.  I will define variable terrain here as unconstrained in terms of how many slopes it can posplacesess, as well as in which direction those slopes can point.  Of course, there are mechanical limits inherent to each tracker design, but for sake of time I am not going to talk about individual tracker architectures in this post.

Terrain-aware backtracking, at the time of writing, is relatively new and less understood than the other types of tracking algorithms.  To make this more confusing, a tracker company's proprietary tracking algorithm is often comprised of both a terrain backtracking component and a diffuse irradiance optimization component  (We are only discussing the backtracking component here).  On top of that even, there are three different sub-approaches to backtracking on terrain, used by different tracker companies which are often called "terrain-aware" even though the underlying concept between them is quite different.


## 4. a. Increased GCR Backtracking

The simplest approach to reducing shading losses on variable terrain is to start with the as-built ground coverage ratio (GCR), let's say 33%, and then gradually increase the tracking algorithm GCR until shading losses decrease to an acceptable level.  A system with this type of algorithm may be built with a 33% GCR, but will be controlled as if it were built with a 45% GCR.  This approach leaves some energy on the table since a single input (GCR) cannot optimize for variable terrain.  An increase in GCR which narrowly avoids shade in one part of the plant, may be too aggressive in another and cause less than optimal angles for transposition.

One can improve upon this approach by using the slope-aware backtracking algorithm and/or using multiple slope-aware backtracking algorithms for different geographic zones in the project depending on the slope in a given zone.

## 4. c. Current Sensor Backtracking

Another possible approach is to use live data from the plant when choosing when to backtrack.  For example, one could measure the current from a string of modules, or the current coming from a reference module as long as that reference module spans the width of the modules on the tracker.  If the current is decreased by shading, then a more backtracked angle can be taken by each individual tracker until no shading is experienced.  

This strategy is an improvement on increased GCR backtracking since it takes the data from each tracker into account.  A caveat:  With terrain following trackers, the reference module approach would not be able to eliminate all shade effects since the referene module only exists in one bay along a tracker which may have multiple differently angled bays.

## 4. d. Digital Twin (Ray-Casting) Backtracking

The last approach is to create a digital 3D model of the site and then use the digital model to determine what angle each tracker must be at for each sun-angle in order to avoid shading.  This method allows individual tracker control much like current sensor backtracking, makes modeling easier since you do not need to install the system to determine what angles the trackers should go to, is robust for terrain following (articulating) tracker architectures, but requires that the digital 3d model of the plant be accurate and representative of the as-built system.  

**Further Reading**
- [Terrain Aware Backtracking via Forward Ray-Tracing](https://ieeexplore.ieee.org/document/9938554)

# Conclusion

I hope that this blog post helps clear up confusion around some of the different types of horizontal single-axis tracking algorithms that exist.  As always, if you have any insights you would like me to add please feel free to reach out to me, and let me know if there are any posts you want to see in the future.