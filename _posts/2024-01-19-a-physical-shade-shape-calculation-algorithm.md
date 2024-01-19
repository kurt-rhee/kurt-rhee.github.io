---
layout: post
title: A Physical Shade Shape Calculation Algorithm
description: Blog
summary: A Physical Shade Shape Calculation Algorithm
tags: [shade, gpu, 3d]
---

![3d-shade example]({{ site.url }}/assets/images/3d-shade.png#center)
Figure 1:  Relative shading percentage of different tracker geometries on a 3D site using a polygon clipping algorithm.

# Caveat
At the time of writing I work for a software tool which chose to go with a continuous space algorithm.  Readers should know that discrete space algorithms are totally viable if done correctly and I don't mean at all to say that one method is the right method.  This is merely a write-up on how we made the decisions that we did.


# Motivation
There are many different algorithms that can be used to calculate the amount of shade on an object.  Many of them, implemented by the video game industry, can render near photo-realistic scenes at 60 frames per second.  Why then, shouldn't we borrow these algorithms and calculate shade shape the way that they do it?

Unfortunately the algorithms used by the video game industry are **rendering** algorithms, meant to control the pixels on a 2D screen in such a way that they represent a convincingly lit 3D scene.  I call this category of algorithms "discrete space" algorithms, since they calculate whether or not some point in space is shaded or not.  Discrete space algorithms differ from what I will call "continuous space" algorithms which do not bin physical space to simplify the problem.  

To get a better understanding of different algorithms, it is often useful to think about their behavior at the limits.  At the bottom limit of discrete space algorithms, we could represent a solar panel by a single point and then decide whether or not that point is shaded. If the point is shaded, then we would consider the solar panel 100% shaded.  If the point was not shaded, then we would consider the solar panel 0% shaded.  This would be an easy computer program to write, and it would run quickly, but this is not a fine-grained enough representation for the purposes of solar performance modeling.  At the top limit with infinte points the algorithm would calculate the amount of shading on the panel exactly, but would also take infinite amount of time to complete.  Somewhere, in-between these limits, there is an optimal trade-off between computational complexity and accuracy.  Finding this optimal point and convincing the community that the point you have found is correct is the art of discrete space shading algorithms.

A quick detour is warranted here to discuss a few algorithms used by the graphics processing industry.

# Discrete Space Algorithms

**Disclaimer** Each of these explanations are extremely high level and ignore many of concepts necessary for algorithm implementation.  In this post I have focused on brevity and simplicity over robustness.  Many notable algorithms such as 3D shadow volumes, rasterization, painters algorithm have been left out.

### Shadow Mapping

![aliasing example]({{ site.url }}/assets/images/3d-aliasing.png#center)
Figure 2:  Alias problems with the shadow map technique

Shadow mapping is an incredibly fast algorithm which solves the 3D shading problem in an ingenious way.  First, every object in 3D is projected into a plane which is perpendicular to the incoming rays of the sun.  For the sake of his blog post we will consider the incoming rays of the sun as a **directional light**, meaning rays from the light source are all parallel to one another.  By projecting the objects in the 3D scene onto a plane perpendicular to the lights incoming rays, the algorithm has created a 2D scene from the perspective of the light source.  Any objects not visible from the perspective of the incoming light rays are in shadow.  

Why doesn't this algorithm work for solar performance engineers out of the box?  In the video game realm, the actual shadow shape in terms of geometry does not matter.  What that means is that the shadow geometries are not calculated in terms of vertices, edge lengths, or other constructs that may be recognizable to a mathematician.  Instead they are stored in computer memory as a matrix of grid cells where a cell can be either 1 (visible) or 0 (not visible).  This shortcut reduces the number of calculations to n where n is the number of pixels required for rendering.  This results in an undesired **artifact** called aliasing, where the shadow shape depends on the number of pixels computed.  Video game players may be familiar with this effect which is seen as a stair-step shaped shadow as displayed in the image above.  As the number of pixels increases towards infinity, this algorithms becomes more precise, but unacceptably slow.

### Ray Tracing

![ray tracing]({{ site.url }}/assets/images/3d-ray-tracing.png#center)
Figure 3:  Conceptual example of ray tracing

Ray Tracing is a well known algorithm which comes in many different flavors.  We will distinguish ray tracing from ray casting here by defining ray tracing as a superset of ray casting where a ray can be cast from the light source and then have n number of reflective bounces off of whatever material that it hits.  In contrast, ray casting will be defined as a ray shooting operation from the light source with no additional reflective bounces.  Ray Tracing and Ray Casting can produce remarkable images, especially as the number of rays shot tends towards infinity, but therein lies the same problem.  The fidelity of the results is not fixed, it depends on the number of rays cast.   With a large enough number of rays, the results from the ray tracing family of algorithms could theoretically be good enough for detailed performance modeling.

#### A back of the napkin calculation: 

If a ray was cast for each square centimeter of module surface on a 100 MWac PV plant with a 1.3 DC/AC ratio you might have approximately 330,000 modules on the site.  A square centimeter is likely below the threshold of geometric uncertainty, knowing that tracker rotation angle measurements, digital elevation models and construction tolerances all contribute to inaccuracies in the 3D model.  If each module is approximately 20,500 cm^2, this means approximately 7 billion rays must be cast to get square centimeter level of precision.  Each of these 7 billion rays would need to be individually intersection tested against any possible obstructions between the sun and the pv surface in question.  Assuming 6 modules per bay on a terrain following tracker (the smallest unit geometry we can consider while still getting physically realistic results) we would need 55,000 bays and we would expect ~385 billion operations required for the naive case where we must test for intersection across all objects in the scene.  The naive case is obviously not true since we do not need to test objects that are behind the square centimeter of pv surface area we want to check for shading, or far away from the light vector's path, but it is useful for understanding the order of magnitude we need to consider when selecting algorithms.  We need to calculate this for every time-step in a performance model time-series.  The minimum time-series is often 8760 scenes, but there can be more in performance modeling software which can handle sub-hourly calculations.  

Even very advanced (and expensive) GPU's which support real time ray tracing render around 60 frames per second.  An 8760 would take 146s or 2.43 minutes assuming only 33 million rays shot.  This maps to 4 rays per pixel on a 4k resolution screen.  Even though there are complexity reduction algorithms which can be used to reduce the numbers of rays we would actually need to shoot well below the 7 billion mark, the great distance between 33 million and 7 billion makes this sort of algorithm unlikely candidates for performance models today. 

A counterpoint, if we were to take a set of 5 representative points for each cell on a module and cast 1 ray per representative point, we would need to cast ~24 million rays per scene.  A much more feasible number.  This approach would be a nice starting point, because as GPU performance increases and costs come down, we can easily increase the number of representative points without large software development overhead.

### Hemi-Cube

![hemi cube]({{ site.url }}/assets/images/hemi-cube.png#center)
Figure 4:  Conceptual example of a hemicube

The hemi-cube algorithm involves choosing a select number of target points on the module surface and then taking a 3D digital (discrete) picture of the world from each target point.  Each wall of the hemi-cube "camera" sees a projection of the world, and you can tell how much that point is shaded. With enough hemi-cubes you should get a good idea of how much shade is hitting your modules.  People who are interested in the hemi-cube approach should read DNV's recent papers by Hamer et. al.  they are well written and instructive.

# Continuous Space Algorithms

### Polygon Clipping

![polygon clipping]({{ site.url }}/assets/images/boolean-geometry.png#center)
Figure 5:  Conceptual example of a polygon clipping algorithm


Thus far we have been exploring algorithms that have been developed for the graphics processing industry.  Another community we can borrow ideas from is the computational geometry community.

One of the calculations one might expect a computational geometry engine to provide is **polygon clipping **.  A polygon clipping algorithm takes into account the geometry and spatial positions of two objects and then returns the shape of their intersection.  At first glance, this operation may not seem useful for 3D shading, but by combining it with an idea from shadow mapping we can produce an algorithm which may perform in reasonable time.  

If we take every object in the 3D scene and project it into the 2 dimensional place perpendicular to the incoming rays of light, we now end up with a set of 2 dimensional polygons.  If these polygons overlap, that indicates that shade is being cast on one of the polygons.  Let's say we once again take the naive approach and test polygon intersection for each bay (4 edges * 55,000 bays) against every other bay in the site.  For our 100 MW example we would need ~48 million calculations.  This gives us perfectly precise  polygon shade shapes with less operations than the 7 billion calculations we would need to do in order to get cm^2 precision using ray casting.

# Conclusion

3D shade algorithms are hard.  There is a reason that not every solar performance software has one, and it is clear that there are still plenty of improvements that need to be made for us as an industry to truly feel confident when modeling on terrain.  

Writing the 3d shade algorithm for PlantPredict was one of the most difficult problems I have worked on in my career thus far, and took me far outside of my performance modeling comfort zone into the realms of computation geometry, linear algebra, boolean geometry, the Rust programming language and algorithm design.  Not very many jobs let you explore so far off the rails of your written job description and I will always be grateful for such an opportunity.

If you are still reading, I hope the difference between discrete space algorithms and continuous space algorithms is clear and you learned something new about the underlying algorithms used by different performance modeling software tools.  If there are any questions still lingering after reading the post please feel free to reach out to me via my email in the about section of this website and I would be happy to add additional context.