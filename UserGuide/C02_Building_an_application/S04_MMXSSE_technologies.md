---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: MMXSSE_technologies
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / MMXSSE technologies"
---

# Aurora Imaging Library and the Intel SIMD technologies

Aurora Imaging Library's processing operations have been optimized to take advantage of Intel's single-instruction, multiple-data (SIMD) technologies to accelerate multimedia (and multimedia-like) applications.

SIMD technologies can handle computation-intensive algorithms that perform repetitive operations. These technologies cover a wide range of capabilities, such as basic arithmetic operations, logical operations, shift operations, comparison operations, and data transfer instructions. These instructions can assist in processing multiple pieces of data in parallel.

SIMD technologies include streaming SIMD extensions (SSE/SSE2/SSE3/SSE4) and advanced vector extensions (AVX/AVX2/AVX-512). AVX is an improvement to SSE. With the improvement, the amount of data that can be processed simultaneously increased (from 128-bit with SSE to 512-bit with AVX). Each new SIMD technology provides additional instructions that Aurora Imaging Library selectively uses to improve overall image processing performances.
