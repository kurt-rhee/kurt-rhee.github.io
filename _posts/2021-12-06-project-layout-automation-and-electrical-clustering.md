---
layout: post
title: Layout Automation and Electrical Clustering
description: Project
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

![nearest neighbors graph ranking]({{ site.url }}/assets/images/layout-nearest-neighbors.png#center)

The above graph abstracts the tracker polygon into a representative 2D point which is the centroid of the tracker object.  The color of the point indicates how many nearest neighbors the tracker has (nnn) and the y axis shows the y position of the centroid on the geographically projected land surface.  

![removal of excess trackers]({{ site.url }}/assets/images/layout-remove-excess.png#center)

The program then removed the tracker with the least nearest neighbors and then recomputed the number of nearest neighbors for each node that was connected to the removed tracker.  Leaving us with a geometrically and electrically plausible layout of trackers.  

### 3. Deciding Inverter Placement Locations

The next high level question we must answer when it comes to plant design is the where should we place inverters relative to the available tracker objects?  The answer should be at the location which minimizes the amount of wire needed to connect all of the trackers to the inverter.  Besides the minimization function, a set of constraints must also be obeyed.  Each grouping of trackers should have equal number of trackers, for example we would not want one inverter to have 10 trackers while another has 234 trackers connected to it electrically, and each group of trackers should form a contiguous shape.  

The project context switches for the next few images, apologies for using different examples.  I am writing about this years after implementing the solution and only have access to photos from an old publication.

![spectral clustering]({{ site.url }}/assets/images/layout-initial-clustering.png#center)

The first pass guess our program takes to determine inverter-tracker electrical groupings is to attempt a clustering operation.  We tried many different clustering algorithms including K-means and others that exist in the sci-kit python library, but decided that feeding the graph form of the layout to a spectral clustering algorithm gave the best initial results.  Above you can see two clusters that were guessed by the algorithm and the resulting centroids of all tracker objects in each individual cluster.  We believe that spectral clustering performed better than location (x, y) based clustering due to its graph based knowledge of nearest neighbors.

![adjacency chain]({{ site.url }}/assets/images/layout-adjacency-chain.png#center)

The initial guess with spectral clustering, while usually good often did not meet the constraint of equal number of trackers per cluster.  By converting the graph form of the layout into an adjacency matrix, the program can then reassign trackers from overfull clusters to underfull clusters.  If we combine this idea with Djikstra's shortest path algorithm, it can efficiently do this operation even if overfull clusters are far away from underfull ones.

![layout]({{ site.url }}/assets/images/layout-final.png#center)

Once these clusters were created, then the program could comb through the layout and calculate indicative wire run lengths for each cluster (manhattan distance calculation), fencing considerations (bounding concave polygon problem), and other bill of quantities line items that could be fed into a capital expenditures calculator, as well as an API for performance modeling (PlantPredict) in order to calculate revenues.

# Conclusion

The finished program creates slightly less feasible layouts, multiples of times faster than a human engineer.  This allows for applications built on top of it to iterate through a large matrix of possible plant designs, feeding the resulting capex, opex and generation profiles into a financial model and selecting the best performing plant in terms of a given financial metric. The tool decreases EDFR plant LCOE, increases competitiveness in bids and allows us to build better more profitable solar projects.


### Acknowledgements

Thanks to Cameron Nielsen who white boarded with me for many days on this project, and also created much of the user interface, and James Alfi and James Christopherson who saw the value of working on R&D projects while I worked at EDF Renewables.