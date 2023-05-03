---
layout: post
title: Terrain Aware Backtracking via Forward Ray Tracing 
description: Project
summary: Terrain Aware Backtracking via Forward Ray Tracing 
tags: [variability, uncertainty]
---

![forward ray casting]({{ site.url }}/assets/images/forward-ray-tracing.png#center)


# Motivation

Terrain following horizontal single axis trackers can introduce a bend or a dislocation in the torque-tube.  This feature allows them to conform to the underlying land, reducing many environmental and economic issues associated with standard trackers which must remain co-planar along the tracker axis.  Unfortunately, this lack of co-planarity renders historic backtracking algorithms less effective, since the algorithms developed for co-planar trackers assumes co-planarity along the torque-tube axis.  

At the time of writing, some advancements have been made for backtracking algorithms for terrain following trackers, but none have filtered downwards to performance modeling tools. This is because many of these methods are implemented in-situ, once the plant has already been constructed.  For example, during the commissioning step, a tracker's backtracking algorithm can be tuned with a tighter spaced input ground coverage ratio than is physically true in order to tell the trackers to back-track sooner and more aggressively.  This can help avoid some shading and improve performance, but cannot take into account the unique terrain and geometry of each individual terrain following tracker.

The algorithm developed at Nevados to backtrack terrain following trackers lends itself well to the performance modeling realm, because it does not require implementation in-situ.  An adequate digital model of the tracker geometries in 3D can be used to determine what angle each individual tracker should go to, in order to avoid shading its neighbors.

# Solution

The solution given a 3D model of the tracker geometries is physically simple, but computationally complex.  The naive solution to the problem, to consider each tracker and compare it to the shadows cast by every other tracker in the system is solvable in a insufficient O(n^2) time.  Therefore a set of cutting algorithms must be chosen to speed up the calculation.  These algorithms are not discussed here in order to maintain Nevados IP.  

Once the minimal set of shadow casting trackers are created, a ray casting operation can take place for every top corner of every bay object in the shadow casting tracker.  If any ray lands on or above the tracker under consideration, both trackers are backtracked by one degree.  Doing so across the entire site for every timestep (5 minute move intervals for Nevados) will render a time-series of tracker angles which entirely avoid shade across the system.
