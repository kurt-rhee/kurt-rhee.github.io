---
layout: post
title: A Physical Shade Shape Calculation Algorithm
description: A Physical Shade Shape Calculation Algorithm
summary: A Physical Shade Shape Calculation Algorithm
tags: [shade, gpu, 3d]
---


# Motivation
There are many different algorithms that can be used to calculate the amount of shade on an object.  Many of them, implemented by the video game industry, can render near photo-realistic scenes at 60 frames per second.  Why then, shouldn't we borrow these algorithms and calculate shade shape the way that they do it?

Unfortunately the algorithms used by the video game industry are **rendering** algorithms, meant to control the pixels on a 2D screen in such a way that they represent a convincingly lit 3D scene.  These algorithms differ from a **simulation** algorithm which is meant to perform a mathematically rigorous calculation.  A quick detour is warranted here to discuss a few algorithms used by the video game industry.

**Disclaimer** Each of these explanations are extremely high level and ignore many of concepts necessary for algorithm implementation.  In this post I have focused on brevity and simplicity over robustness.  Many notable algorithms such as 3D shadow volumes, rasterization, painters algorithm have been left out.

### Shadow Mapping

Shadow mapping is an incredibly fast algorithm which solves the 3D shading problem in an ingenious way.  First, every object in 3D is projected into a plane which is perpendicular to the incoming rays of the sun.  For the sake of his blog post we will consider the incoming rays of the sun as a **directional light**, meaning rays from the light source are all parallel to one another.  By projecting the objects in the 3D scene onto a plane perpendicular to the lights incoming rays, the algorithm has created a 2D scene from the perspective of the light source.  Any objects not visible from the perspective of the incoming light rays are in shadow.  

Why doesn't this algorithm work for solar performance engineers out of the box?  In the video game realm, the actual shadow shape in terms of geometry does not matter.  What that means is that the shadow geometries are not calculated in terms of vertices, edge lengths, or other constructs that may be recognizable to a mathematician.  Instead they are stored in computer memory as a matrix of grid cells where a cell can be either 1 (visible) or 0 (not visible).  This shortcut reduces the number of calculations to n where n is the number of pixels required for rendering.  This results in an undesired **artifact** called aliasing, where the shadow shape depends on the number of pixels computed.  As the number of pixels increases towards infinity, this algorithms becomes more precise, but unacceptably slow.

![aliasing example]({{ site.url }}/assets/images/3d-aliasing.png#center)

### Ray Tracing

Ray Tracing is a well known algorithm which comes in many different flavors.  We will distinguish ray tracing from ray casting here by defining ray tracing as a superset of ray casting where a ray can be cast from the light source and then have n number of reflective bounces off of whatever material that it hits.  In contrast, ray casting will be defined as a ray shooting operation from the light source with no additional reflective bounces.  Ray Tracing and Ray Casting can produce remarkable images, especially as the number of rays shot tends towards infinity, but therein lies the same problem.  The fidelity of the results is not fixed, it depends on the number of rays cast.   With a large enough number of rays, the results from the ray tracing family of algorithms could theoretically be good enough for detailed performance modeling.

A back of the napkin calculation shows that if a ray was cast for each square centimeter of module surface on a 100 MWac PV plant with a 1.3 DC/AC ratio you might have approximately 330,000 modules on the site.  A square centimeter is likely below the threshold of geometric uncertainty, knowing that tracker rotation angle measurements, digital elevation models and construction tolerances all contribute to inaccuracies in the 3D model.  If each module is apprpoximately 20,500 cm^2, this means approximately 7 billion rays must be cast to get square centimeter level of precision.  Each of these 7 billion rays would need to be individually intersection tested against any possible obstructions between the sun and the pv surface in question.  Assuming 6 modules per bay on a terrain following tracker (the smallest unit geometry we can consider while still getting physically realistic results) we would expect ~385 billion operations required for the naive case where we must test for intersection across all objects in the scene.  The naive case is obviously not true since we do not need to test objects that are behind the square centimeter of pv surface area we want to check for shading, or far away from the light vector's path, but it is useful for understanding the order of magnitude we need to consider when selecting algorithms.  We need to calculate this for every timestep in a performance model timeseries.  Them minimum timeseries is often 8760 scenes, possibly more in PlantPredict which can handle timesteps down to 1 minute.  

Even very advanced (and expensive) GPU's which support real time ray tracing render around 60 frames per second.  An 8760 would take 146s or 2.43 minutes assuming only 33 million rays shot.  This maps to 4 rays per pixel on a 4k resolution screen.  Even though there are complexity reduction algorithms which can be used to reduce the numbers of rays we would actually need to shoot well below the 7 billion mark, the great distance between 33 million and 7 billion makes this sort of algorithm unlikely candidates for performance models today.  

A counterpoint, if we were to take a set of 5 representative points for each cell on a module and cast 1 ray per representative point, we would need to cast ~24 million rays per scene.  A much more feasible number.  This approach would be a nice starting point, because as GPU performance increases and costs come down, we can easily increase the number of representative points without large software development overhead.


### Collision Detection

Perhaps we remove ourselves from the constraint of finding solutions within the graphics processing sphere and instead search for solutions within the realm of physics engines.  
