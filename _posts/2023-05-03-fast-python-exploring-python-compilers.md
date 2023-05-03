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

# Solution

One method of increasing the speed of Python is a compiler.  There are two main categories of compilers, static compilers and just-in-time (jit) compilers.  A static compiler compiles a program separately from the operation of the program.  For example a C developer might right a program, compile it, and then run the compiled program.  A jit compiler on the other hand will run the program for a small amount of time and then optimize the rest of the code loops at runtime.  

There are many different libraries which can be used to compile python programs for faster operation, a few well known examples can be found below with some relevant details.  Assuming you want to develop a massively parallel simulation at scale for commercial use and not just for fun, and also assuming you work for an organization where paying for a license is prohibitive, you are left with Cython, Numba and Taichi since Codon employs a non-permissive license for commercial applications.  Generally, it can also be advantageous to work with free open source software (FOSS), since it is likely that more people will gravitate towards and contribute to software with permissive licensing over software with restrictive licensing.   Warp-lang, by Nvidia has a custom license which is commercially permissive.


| Compiler    | License     | Static      | JIT         | GPU         |
| ----------- | ----------- | ----------- | ----------- | ----------- |
| Cython      | Apache 2.0  | X           |             |             |
| Numba       | BSD 2       |             | X           | X           |
| Codon       | BSL         | X           | X           | X           |
| Taichi      | Apache 2.0  |             | X           | X           |
| Warp-lang   | Custom      |             | X           | X           |

At this point, the field of possible frameworks has been narrowed to Numba, Taichi and Warp-lang.  

Numba has the longest developmeht history, but a slower trajectory and a lower number of total Github stars.  While github stars are an imperfect memasure of popularity, they are an easily viewable metric to give readers a high level indication of what libraries people may be using and if that excitement is growing over time. 

Taichi (by their own benchmarks) benchmarks well in time tests against Numba.

Warp-lang is the newest entry to the space and is backed by Nvidia.  

The solution?  There is no solution.  No satisfying one size fits all compiler, but hopefully this blog post helps others on the path to creating efficient algorithm deployments with Python.