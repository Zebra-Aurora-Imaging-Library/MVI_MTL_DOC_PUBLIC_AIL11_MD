---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Pattern_matching_in_general
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Pattern matching in general"
---

# Pattern matching - in general

To find occurrences of patterns (models) in your target image, you can use the Aurora Imaging Library Pattern Matching module. This module finds the degree of similarity between the specified model and the neighborhood of the pixels in the target image, on a pixel-by-pixel basis, using normalized grayscale correlation (NGC). The module then returns the position of pixels whose neighborhoods have the highest degree of similarity and meet other specified criteria. When dealing with non-rotated models or models that have broken edges, the Pattern Matching module offers a faster search than the Aurora Imaging Library Model Finder module ([`Mmod...`](../../Reference/mod/MmodAlloc.md)).

*[Image: mpatwafer.png]*

The Pattern Matching module allows you to search for any number of different models simultaneously, through a range of angles, but only when using 8-bit, grayscale, unsigned image buffers.

The module provides some support for camera calibration. Although the results are calculated in pixels, if the target image has been associated with a camera calibration context, positional results can be returned in calibrated real-world units.

The module also allows you to restore a Pattern Matching context from a file or memory stream, or save a Pattern Matching context to a file or memory stream.
