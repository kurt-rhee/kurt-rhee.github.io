---
layout: post
title: Layout Automation and Electrical Clustering
description: Layout automation project
summary: Layout automation project
tags: [layout, graph, clustering]
---

# Motivation
Designing a photovoltaic powerplant takes a long time.  Generally it also takes a lot of skilled labor, but sometimes it doesn't make sense to design an entire power plant from start to finish before making a decision regarding the plant's design.  If we can iterate very quickly over a set of indicative designs and then feed these designs into both a performance model and a financial model, we can improve project economics by searching a parameter space that would be too large, too slow and too error prone to create by humans alone.  The solution space below shows a set of programs that can be used individually or in a pipeline depending on the project's maturity to very quickly determine project economics.

# Solution
In order to quickly generate designs and then feed these design parameters into a financial model, the layout of the photovoltaic panels on given parcels of land must be determined. 

### 1. Capacity Check
Often, the first question asked when acquiring a piece of land is how much capacity can we fit?  For this problem, it is relatively simple to model trackers as rectangular polygons, paste them in an array over a land surface and then remove any trackers which overlap any constraints or fall outside of the acquired land area.  Each time we ran this program we tried a set of different starting positions for the tracker array and picked the initialization condition which maximized the number of trackers that fit on the given piece of land.

### 2. Removing Over-Capacity
Stopping at the capacity check step will result in an over-estimation of the real capacity that a piece of acquired land will fit since even though the tracker polygons might fit geometrically, they might not fit electrically or economically.  Following those constraints, the question becomes which trackers should we remove from the geometric layout?  The answer we came up with is to remove the trackers which are the most difficult to connect electrically.  For example the ones that are furthest away from other trackers and would require longer wire runs.  In order to determine which trackers are the least connected, we converted the geometric layout to a graph and ranked trackers by their number of nearest neighbors.

![nearest neighbors graph ranking]({{ site.url }}/assets/images/layout-nearest-neighbors.png)

The above graph abstracts the tracker polygon into a representative 2D point which is the centroid of the tracker object.  The color of the point indicates how many nearest neighbors the tracker has (nnn) and the y axis shows the y position of the centroid on the geographically projected land surface.  

![removal of excess trackers]({{ site.url }}/assets/images/layout-remove-excess.png)

We then removed the tracker with the least nearest neighbors and then recomputed the number of nearest neighbors for each node that was connected to the removed tracker.  Leaving us with a geometrically and electrically plausible layout of trackers.  

### 3. Deciding Inverter Placement Locations

The next high level question we must answer when it comes to plant design is the where should we place inverters relative to the available tracker objects?  The answer 

![useful image]({{ site.url }}/assets/images/layout-initial-clustering.png)