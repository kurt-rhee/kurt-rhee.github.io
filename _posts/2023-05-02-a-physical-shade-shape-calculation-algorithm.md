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

**Disclaimer** Each of these explanations are extremely high level and ignore many of concepts necessary for algorithm implementation.  In this post I have focused on brevity and simplicity over robustness.  Many notable algorithms such as 3D shade volumes, rasterization, painters algorithm have been left out for the sake of brevity.

### Shadow Mapping

Shadow mapping is an incredibly fast algorithm which solves the 3D shading problem in an ingenious way.  First, every object in 3D is projected into a plane which is perpendicular to the incoming rays of the sun.  For the sake of his blog post we will consider the incoming rays of the sun as a **directional light**, meaning rays from the light source are all parallel to one another.  By projecting the objects in the 3D scene onto a plane perpendicular to the lights incoming rays, the algorithm has created a 2D scene from the perspective of the light source.  Any objects not visible from the perspective of the incoming light rays are in shadow.  

Why doesn't this algorithm work for solar performance engineers out of the box?  In the video game realm, the actual shadow shape in terms of geometry does not matter.  What that means is that the shadow geometries are not calculated in terms of vertices, edge lengths, or other constructs that may be recognizable to a mathematician.  Instead they are stored in computer memory as a matrix of grid cells where a cell can be either 1 (visible) or 0 (not visible).  This shortcut reduces the number of calculations to n where n is the number of pixels required for rendering.  This results in an undesired **artifact** called aliasing, where the shadow shape depends on the number of pixels computed.  As the number of pixels increases towards infinity, this algorithms becomes more precise, but unacceptably slow.

![aliasing example]({{ site.url }}/assets/images/3d-aliasing.png#center)

### Ray Tracing

Ray Tracing also 


# Solution

