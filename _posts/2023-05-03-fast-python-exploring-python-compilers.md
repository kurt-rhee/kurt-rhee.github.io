---
layout: post
title: Fast Python, Exploring Python Compilers
description: Blog
summary: Fast Python, Exploring Python Compilers
tags: [python, gpu]
---



[![Star History Chart](https://api.star-history.com/svg?repos=cython/cython,exaloop/codon,taichi-dev/taichi,numba/numba,nvidia/warp&type=Date)](https://star-history.com/#cython/cython&exaloop/codon&taichi-dev/taichi&numba/numba&nvidia/warp&Date)

# Motivation

Python is slow.  It is a great language, my language of choice due to the large community, huge amount of available libraries and the speed at which one can create a working program, but when developing algorithms that operate on a large number of objects at scale, the standard implementation of Python may not be the best choice.  

A note that introducing a compiler to your code introduces complexity.  In general there are no "drop-in" replacements.  In practice it is often best to write the program in question for the compiler in mind, rather than write a program in standard python and then decide that it needs to run faster after the fact.  

# Solution

| Compiler    | License     | Static      | JIT         | GPU         |
| ----------- | ----------- | :---------: | :---------: | :---------: |
| Cython      | Apache 2.0  |      X      |             |             |
| Numba       | BSD 2       |             |      X      |       X     |
| Codon       | BSL         |      X      |      X      |       X     |
| Taichi      | Apache 2.0  |             |      X      |       X     |
| Warp-lang   | Custom      |             |      X      |       X     |

<br />


One method of increasing the speed of Python is a compiler.  There are two main categories of compilers, static compilers and just-in-time (jit) compilers.  A static compiler compiles a program separately from the operation of the program.  For example a C developer might write a program, compile it, and then run the compiled program.  A jit compiler on the other hand will run the program for a small amount of time and then optimize the rest of the code loops at runtime.  

There are many different libraries which can be used to compile python programs for faster operation, a few well known examples can be found above with some relevant details.  In the following sections we will go through a few compilers and eliminate some from consideration.  This process of elimination and the thought processes behind each elimination will hopefully narrow the crowded field for readers that might be unfamiliar with compilers in general.  In order, the sections below represent the likelihood that the compiler will be the best choice for a given project with earlier sections representing lower likelihood that the compiler represents the best choice.


## Codon
Assuming you want to develop a massively parallel simulation at scale for commercial use and not just for fun, and also assuming you work for an organization where paying for a license is prohibitive, you are left with Cython, Numba and Taichi since Codon employs a non-permissive license for commercial applications.  Generally, it can also be advantageous to work with free open source software (FOSS), since it is likely that more people will gravitate towards and contribute to software with permissive licensing over software with restrictive licensing.   Warp-lang, by Nvidia has a custom license which is commercially permissive.

## Cython
If you have reached this point in the blog post, congrats the field of possible frameworks has been narrowed by exactly 1.  Given the ability of some compilers to operate on both the CPU and GPU, the question then becomes why choose a compiler which can only target one processing architecture?  In my opinion this lack of functionality disqualifies Cython as the choice compiler for most modern compiled python projects.  

## Warp-lang
Warp-lang is the newest entry to the space and is backed by Nvidia.  Warp-lang interacts with Nvidia Omniverse which acts as a GUI.  At the time of writing, warp-lang is relatively new and does not have as large of a community as Numba or Taichi.  

## Numba
Numba has the longest development history, but a slower trajectory and a lower number of total Github stars.  While github stars are an imperfect measure of popularity, they are an easily viewable metric to give readers a high level indication of what libraries people may be using and if that excitement is growing over time. 


## Taichi
![taichi]({{ site.url }}/assets/images/compiler-taichi.png#center)

Taichi (by their own tests) benchmarks well in time tests against Numba.  It also includes some components right out of the box such as support for classes and a GUI which may help for some workloads such as those that require rendering.  Workloads like collision detection, ray casting, etc. may be easier to troubleshoot in Taichi than Numba.  


# Conclusion  

The solution?  There is no solution.  No satisfying one size fits all compiler, but hopefully this blog post helps others on the path to creating efficient algorithm deployments with Python.