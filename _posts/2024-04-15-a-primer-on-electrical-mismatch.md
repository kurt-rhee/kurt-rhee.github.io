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

Electrical mismatch due to non-uniform shading can be a confusing topic for both new and experienced PV performance modeling professionals.  The confusion likely arises from the multiple sources. For one, there are multiple concepts competing for the term "mismatch."  But, the main reason is that understanding electrical mismatch due to non-uniform shading requires building up layers of intuition from multiple fields including PV performance modeling, PV systems design, and electrical engineering.  In this blog post, we will guide you through some definition of electrical mismatch, some necessary context, and will guide you through some common misconceptions.

# Definitions and Scope

### Electrical Mismatch due to Module Power Tolerance

There are two types of mismatch that people generally talk about in a PV performance model.  The first type occurs when modules with difference efficiencies are included in the same string.  For example, a company may buy a set of 500W modules and install them in a string of 28 modules each.  If each of these modules actually performed at 500W at STC, there would be no mismatch, but in reality each will perform slightly differently due to differences in materials and manufacturing.  In order to reduce this effect, manufacturers bin their modules so that they come in a set of a given tolerance.  Using our 500W module example from before, the company may receive 10,000 modules all within a 500W to 505W bin.  Large PV projects may receive modules across multiple bins and must be careful to string modules with similar power bins together.  In a PV performance model, the distribution of actual power ratings is generally considered to be gaussian and the loss (or gain if the range of modules installed has a positive power bin) associated is generally a flat percentage across all time-steps.  This mismatch we will ignore in this blog post in favor of electrical mismatch due to non-uniform shading.

### Electrical Mismatch due to Non-Uniform Shading

The most discussed, debated and probably misunderstood form of electrical mismatch is due to differences in the spatial distribution of irradiance that hit the surface of a module.  The rest of this blog post is dedicated to building up the necessary intuition to make sense of electrical mismatch losses in a performance model.

### Other Types of Electrical Mismatch

A quick aside, there are other types of electrical mismatch that can occur at an operating solar power plant.  For example, electrical mismatch due to non-uniform soiling and electrical mismatch due to non-uniform wire distances.  Electrical mismatch due to non-uniform soiling, for example snow soiling which can collect on certain parts of the module but not others.  Most performance modeling software do not yet consider losses due to these types of mismatch and require the users to implement an increased flat percentage loss instead.  We will ignore these sources of mismatch loss in this blog post as well for the sake of time.  


# Building up the Intuition

### Electrical Engineering

This blog post assumes that the reader has taken entry level electrical engienering curriculum at the university level, but that a refresher may be useful.

Some rules to remember:

- Voltage adds in series.
- Current adds in parallel.
- Diodes allow current to pass in one direction during normal operation.
- Diodes allow current to pass in the opposite direction at high negative voltage.

![Diode Curve]({{ site.url }}/assets/images/diode.png#center)
**Figure 2:**  A general diode IV curve from LibreTexts [2]

### PV Systems Design

Some terminology:

- Cell:  For the purposes of this blog post, silicon doped to create a PN junction and separated by other cells by wires.
- Module:  A collection of electrically connected cells (generally 72 for utility scale plants)
- Sub-Module:  a set of cells which are connected in series, and connected to other sub-modules in parallel (generally 3 sub-modules per module)

Starting from the very beginning.  What happens if we shade a single solar cell?  Since current is proportional to irradiance, current is decreased.  As long as the partial shade does not block the path of the electrons in the cell to the wires, the effect should have a roughly linear effect on power.

Now what happens if we connect this shaded cell to another unshaded cell in series?  

In large utility scale solar power plants, many strings of module are generally connected to an inverter with a single maximum power point tracker (MPPT) or very few MPPTs relative to the number of strings.  This single point of common coupling for many strings of modules requires that all strings perform at the same level of current due to Kirchoffs current law. 





### Shading

In a photovoltaic performance model, there are 3 different types of irradiance that hit the front side of a solar module:  beam, diffuse and ground reflected.  For now we will ignore the circumsolar portion (which generally gets bucketed into either the beam or diffuse portions), as well as the ground reflected irradiance which is generally considered to have an isotropic-ish effect much like the diffuse portion.  

Beam irradiance is the most intuitive of the irradiance types and can be thought of as light which takes a direct (straight vector) path from the sun to the solar module.  Beam irradiance can be blocked by any object that sits between an object and the straight vector pointing to the sun position.  Blocking objects may include trees, buildings, or other solar modules.  

Diffuse irradiance on the other hand is less intuitive and encapsulates all of the light that hits a module, that does not take a direct line from the sun.  For example, a photon may bounce off of the ground, then a molecule in the atmosphere, and then be directed towards the module (horizon brightening).  The cumulative effect of many photons all taking circuitous paths to the module results in a "diffuse dome" of irradiance which can be integrated to determine how much diffuse light is hitting the module.  It is this diffuse irradiance which makes it so that if so that objects in shade are still visible.  It is also the reason that our first misconception can be debunked.

**Misconception:  As soon as a module is shaded, production goes to zero**

This cannot be true, since some diffuse light still shines on the module.  In Desert Center, CA (a location with a very low fraction of diffuse light energy relative to direct light energy) the proportion of total irradiance due to diffuse light in the plane of the array on a tracker system is around ________%.

### Electrical Mismatch


# Conclusion



**Further reading:** 
1. [Engineering Libre Texts:  Diode Primer](https://eng.libretexts.org/Bookshelves/Materials_Science/Supplemental_Modules_%28Materials_Science%29/Solar_Basics/A._Introductory_Physics_for_Solar_Application/II._Electricity/5._Diodes)
2. [PVEducation:  IV Curve Primer](https://www.pveducation.org/pvcdrom/solar-cell-operation/iv-curve)



