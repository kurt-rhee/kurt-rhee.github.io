---
layout: post
title: Fast Python, Exploring Python Compilers
description: Blog
summary: Fast Python, Exploring Python Compilers
tags: [python, gpu]
---



[![Star History Chart](https://api.star-history.com/svg?repos=numba/numba,taichi-dev/taichi,exaloop/codon,cython/cython&type=Date)](https://star-history.com/#numba/numba&taichi-dev/taichi&exaloop/codon&cython/cython&Date)

# Motivation

Python is slow.  It is a great language, my language of choice due to the large community, huge amount of available libraries and the speed at which one can create a working program, but when developing algorithms that operate on a large number of objects at scale, the standard implementation of Python may not be the best choice.

One method of increasing the speed of Python is a compiler.  There are two main categories of compilers, static compilers and just-in-time (jit) compilers.  A static compiler compiles a program separately from the operation of the program.  For example a C developer might right a program, compile it, and then run the compiled program.  A jit compiler on the other hand will run the program for a small amount of time and then optimize the rest of the code loops at runtime.  

There are many different libraries which can be used to compile python programs for faster operation, a few well known examples can abe found below with some relevant details

| Compiler    | License     | Static      | JIT         |
| ----------- | ----------- | ----------- | ----------- |
| Cython      | Apache 2.0  | X           |             |
| Numba       | BSD 2       |             | X           |
| Codon       | BSL         | X           | X           |
| Taichi      | Apache 2.0  |             | X           |
