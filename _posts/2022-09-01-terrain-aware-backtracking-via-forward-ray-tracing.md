---
layout: post
title: Terrain Aware Backtracking via Forward Ray Tracing 
description: Terrain Aware Backtracking via Forward Ray Tracing 
summary: Terrain Aware Backtracking via Forward Ray Tracing 
tags: [variability, uncertainty]
---

![useful image]({{ site.url }}/assets/images/forward-ray-tracing.png)


# Motivation

Terrain following horizontal single axis trackers can introduce a bend or a dislocation in the torque-tube.  This feature allows them to conform to the underlying land, reducing many environmental and economic issues associated with standard trackers which must remain co-planar along the tracker axis.  Unfortunately, this lack of co-planarity renders historic backtracking algorithms less effective, since the algorithms developed for co-planar trackers assumes co-planarity along the torque-tube axis.  

At the time of writing, some advancements have been made for backtracking algorithms for terrain following trackers, but none have filtered downwards to performance modeling tools. This is because many of these methods are implemented in-situ, once the plant has already been constructed.  For example, during the commissioning step, a tracker's backtracking algorithm can be tuned with a tighter spaced input ground coverage ratio than is physically true in order to tell the trackers to back-track sooner and more aggressively.  This can help avoid some shading and improve performance, but cannot take into account the unique terrain and geometry of each individual terrain following tracker.

The algorithm developed at Nevados to backtrack terrain following trackers lends itself well to the performance modeling realm, becuase it does not require implementation in-situ.  An adequate digital model of the tracker geometries can be used to determine what angle each individual tracker should go to, in order to avoid shading its neighbors.

# Solution
Writing in progress.

# Results
Writing in progress.