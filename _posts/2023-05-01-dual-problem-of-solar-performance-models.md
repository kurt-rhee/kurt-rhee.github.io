---
layout: post
title: A Physical Shade Shape Calculation Algorithm
description: A Physical Shade Shape Calculation Algorithm
summary: A Physical Shade Shape Calculation Algorithm
tags: [shade, gpu, 3d]
---


# Motivation
There are many different algorithms that can be used to calculate the amount of shade on an object.  Many of them, implemented by the video game industry, can render near photo-realistic scenes at 60 frames per second.  Why then, shouldn't we borrow these algorithms and calculate shade shape the way that they do it?

Unfortunately the algorithms used by the video game industry are **rendering** algorithms, meant to control the pixels on a screen in such a way that they represent a convincingly lit 3D scene.  These algorithms differ from a **simulation** algorithm which is meant to perform a mathematically rigorous calculation.  A quick detour is warranted here to discuss a few algorithms used by the video game industry.

### Shadow Mapping
Shadow mapping is an incredibly fast algorithm which solves the 3D shading problem in an ingenious way.  First, every object in 3D is projected into a plane which is perpendicular to the incoming rays of the sun.  For the sake of his blog post we will consider the incoming rays of the sun as a **directional light**, meaning rays from the light source are all parallel to one another.  By projecting the 3D scene onto a plane perpendicular to the lights incoming rays, the algorithm has created a scene from the perspective of the light source.  Any objects not visible from the perspective of the incoming light rays are in shadow.  



# Solution

