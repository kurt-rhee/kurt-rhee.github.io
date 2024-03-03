---
layout: post
title: The Dual Problem of Solar Performance Modeling Software
description: Blog
summary: The Dual Problem of Solar Performance Models
tags: [ux, user, experience, performance, model]
---


# Motivation
Assume that you can categorize all problems into a set of 2 arbitrary types.  Weak link problems, think food safety, airline pilot training, or seatbelt regulations, are best solved by focusing on eliminating scenarios where a person could perform worse than some baseline (the floor).  This is opposed to strong link problems, think olympic sports, startups, or spelling bees where the floor does not matter, the only thing that matters is increasing the performance of the best parts of the system (raising the ceiling).  

In food safety, it does not matter if we can make the safest food safer.  What matters is that we keep all food above some threshold of safety.  In the world of olympic sports it does not matter what the baseline level of athleticism is for the population, what matters is that the top performers increase their performance.

In the context of solar performance modeling software, both a weak link and a strong link problem exist, and they are inextricably tied together.  The strong link problems are easy to imagine.  At the time of writing, what performance modeling software can model geo-spatially distributed weather conditions?  Terrain following trackers with multiple intra-tracker angles? Terrain-aware backtracking?  Snow soiling on a dynamic system?  The weak link problems are perhaps a little less forefront, but are just as meaningful.  The largest cause of model inaccuracy in the last PVPMC blind modeling study was user error.  The number of possible permutations for a performance model must be in the millions.  If solar performance modeling software were only a strong link problem, then every developer, asset owner, and IE would use a combination of pvlib and custom software development.  Unfortunately, the problem is also a weak link problem, some form of software is needed in order to standardize and set the floor for controllable model accuracy.  

# Solution

The solution is complicated.  If you increase the software's capability, often that means introducing additional parameters, which means increasing the number of interconnected parts where something can go wrong, as well as the amount of time a skilled user must put into configuring the tool just right.  The solution at PlantPredict is to think very carefully about the user experience in regards to the integration design of the software product.  This has meant that we make decisions that reduce our throughput in terms of how many strong link problems we can solve in a given week, but I believe will make for a better software tool in the long term.  

A few examples which PlantPredict has worked on which improve our integration design and raises the floor for the performance modeling community:
- We introduced the option to use the Bang Wong color palette (an accessible color palette for those who have some types of color blindness)
- We version control our model so that a user can use any prediction engine logic that they so choose.
- We do regression tests every single sprint to make sure that we have not affected any of the past versions negatively.
- We write unit tests for new features we introduce.

Often it is the strong link problems that get posted about, talked about, marketed, but the weak link problems that make the most systemic difference.  In an era where the clean technology space is growing so rapidly that the average years of experience for the clean technology employee pool as a whole is going down, my hope is that a dual focus positions PlantPredict to be the logical choice for performance modeling in the future, even if it slows us down today.