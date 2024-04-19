---
layout: post
title: A Primer on Electrical Mismatch DRAFT
description: Blog
summary: A Primer on Electrical Mismatch DRAFT
tags: [mismatch]
---


![Solar Panels]({{ site.url }}/assets/images/solar-panels-retrotransposition.png#center)
**Figure 1:**  A placeholder image, replace in final draft

# Introduction

Electrical mismatch due to non-uniform shading can be a confusing topic for both new and experienced PV performance modeling professionals.  The confusion likely arises from the multiple sources. For one, there are multiple concepts competing for the term "mismatch."  But, the main reason is that understanding electrical mismatch due to non-uniform shading requires building up layers of intuition from multiple fields including PV performance modeling, PV systems design, and electrical engineering.  In this joint(!) blog post, Mark Mikofski and I will present some context, and guide you around some common misconceptions that arise when talking about electrical mismatch.

# Definitions and Scope

### Electrical Mismatch due to Module Power Tolerance

There are two types of mismatch that people generally talk about in a PV performance model.  The first type occurs when modules with difference efficiencies are included in the same string.  For example, a company may buy a set of 500W modules and install them in a string of 28 modules each.  If each of these modules actually performed at 500W at STC, there would be no mismatch, but in reality each will perform slightly differently due to differences in materials and manufacturing.  In order to reduce this effect, manufacturers bin their modules so that they come in a set where all modules in the set fall within a  given tolerance.  Using our 500W module example from before, the company may receive 10,000 modules all within a 500W to 505W bin.  Large PV projects may receive modules across multiple bins and must be careful to string modules with similar power bins together.  In a PV performance model, the distribution of actual power ratings is generally considered to be gaussian and the loss (or gain if the range of modules installed has a positive power bin) associated is generally a flat percentage across all time-steps.  This mismatch we will ignore in this blog post in favor of electrical mismatch due to non-uniform shading.

### Electrical Mismatch due to Non-Uniform Shading

The most discussed, debated, and probably misunderstood form of electrical mismatch is due to differences in the spatial distribution of irradiance that hits the surface of a module.  The rest of this blog post is dedicated to building up the necessary intuition to make sense of electrical mismatch losses due to partial shading in a performance model.

### Other Types of Electrical Mismatch

A quick aside, there are other types of electrical mismatch that can occur at an operating solar power plant.  For example, electrical mismatch due to non-uniform soiling and electrical mismatch due to non-uniform wire distances.  Electrical mismatch due to non-uniform soiling, for example snow soiling which can collect on certain parts of the module but not others.  Most performance modeling software do not yet consider losses due to these types of mismatch and require the users to implement an increased flat percentage loss instead.  We will ignore these sources of mismatch loss in this blog post for the sake of time.  


# Building up the Intuition

### PV Systems Design

Some terminology:

- Cell:  For the purposes of this blog post, silicon doped to create a PN junction and separated by other cells by wires.
- Module:  A collection of electrically connected cells (generally 72 for utility scale plants)
- Sub-Module:  a set of cells which are connected in series, and connected to other sub-modules in parallel (generally 3 sub-modules per module)

In large utility scale solar power plants, many strings of module are generally connected to an inverter with a single maximum power point tracker (MPPT) or very few MPPTs relative to the number of strings. This point of common coupling will be 


### Electrical Engineering

This blog post assumes that the reader has taken entry level electrical engienering curriculum at the university level, but that a refresher may be useful.

Some rules to remember:

- Voltage adds in series.
- Current adds in parallel.
- Current must be the same across components in series.
- Diodes allow current to pass in one direction during normal operation.
- Diodes allow current to pass in the opposite direction at a threshold negative voltage.
- Both PV cells and bypass diodes are modeled as diodes.

![Diode Curve]({{ site.url }}/assets/images/diode.png#center)
**Figure 2:**  A general diode IV curve from LibreTexts [1]

Quoting Mark Mikofski [2]:

> The cells in a PV module can be considered roughly as a current source in parallel with a diode and some resistive elements. Diodes are semiconductors. In other words, they only conduct current in one direction, called the forward bias. When a negative voltage, or a reverse bias, is applied to the cell, the semiconductor won't conduct a current. However, if enough reverse bias is applied, all semiconductors will eventually breakdown, and carry a current. This phenomema is called reverse bias breakdown, and the breakdown voltage varies between cell technology. A typical front contact p-type silicon solar cell may breakdown at around -20 volts, while a back-contact n-type silicon solar cell may breakdown at -5 volts. There are many factors, beyond the scope of this primer, that affect reverse bias breakdown, such as purity of the substrate as well as type and concentration of dopant. The most important thing to understand about reverse bias breakdown is this:  When a cell is in reverse breakdown, it can carry nearly any current, but because the voltage is negative, then the cell will dissipate energy and will get hot as it exchanges heat with the environment around it.

So why would a negative voltage be applied to a cell?  



### Shading

In a photovoltaic performance models, there are 3 different types of irradiance that hit the front side of a solar module:  beam, diffuse and ground reflected.  For now we will ignore the circumsolar portion (which generally gets bucketed into either the beam or diffuse portions), as well as the ground reflected irradiance which is generally considered to have an isotropic-ish effect much like the diffuse portion.  

Beam irradiance is the most intuitive of the irradiance types and can be thought of as light which takes a direct (straight vector) path from the sun to the solar module.  Beam irradiance can be blocked by any object that sits between an object and the straight vector pointing to the sun position.  Blocking objects may include trees, buildings, or other solar modules.  

Diffuse irradiance on the other hand is less intuitive and encapsulates all of the light that hits a module, that does not take a direct line from the sun.  For example, a photon may bounce off of the ground, then a molecule in the atmosphere, and then be directed towards the module (horizon brightening).  The cumulative effect of many photons all taking circuitous paths to the module results in a "diffuse dome" of irradiance which can be integrated to determine how much diffuse light is hitting the module.  It is this diffuse irradiance which makes it so that if so that objects in shade are still visible.  It is also the reason that our first misconception can be debunked.

**Misconception:  As soon as a module is shaded, production goes to zero**

This cannot be true, since some diffuse light still shines on the module.  In Desert Center, CA (a location with a very low fraction of diffuse light energy relative to direct light energy) the proportion of total irradiance due to diffuse light in the plane of the array on a tracker system is around ________%.

# Putting it all together

### Electrical Mismatch

Starting from the very beginning.  What happens if we shade a single solar cell?  Since current is proportional to irradiance, current is decreased.  As long as the partial shade does not block the path of the electrons in the cell to the wires, the effect should have a roughly linear effect on power.

Now what happens if we connect this shaded cell to another unshaded cell in series?  





# Conclusion



**Further reading:** 
1. [Engineering Libre Texts:  Diode Primer](https://eng.libretexts.org/Bookshelves/Materials_Science/Supplemental_Modules_%28Materials_Science%29/Solar_Basics/A._Introductory_Physics_for_Solar_Application/II._Electricity/5._Diodes)
2. [BreakingBytes by Mark Mikofski: Blog on Mismatch](https://breakingbytes.github.io/electric-mismatch-in-silicon-cell-pv-is-not-intuitive.html#electric-mismatch-in-silicon-cell-pv-is-not-intuitive)
9. [PVEducation:  IV Curve Primer](https://www.pveducation.org/pvcdrom/solar-cell-operation/iv-curve)



