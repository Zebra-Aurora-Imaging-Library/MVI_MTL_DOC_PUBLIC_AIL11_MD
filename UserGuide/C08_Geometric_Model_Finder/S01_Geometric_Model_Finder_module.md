---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Geometric_Model_Finder_module
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Geometric Model Finder module"
---

# Geometric Model Finder module

The Geometric Model Finder module, also referred to as the Model Finder module, is a set of functions to find patterns, or models, based on geometric features. The module finds models using edge-based geometric features (geometric features from extracted edges) instead of a pixel-to-pixel correlation (which is used by the Pattern Matching ([`Mpat...`](../../Reference/pat/MpatAlloc.md)) module). As such, the Model Finder module offers several advantages over the Pattern Matching module. These advantages include a greater tolerance of lighting variations (including specular reflection), model occlusion, as well as variations in scale and angle.

*[Image: IntroImage.png]*

The Model Finder module allows you to tailor your search to fit the requirements of your application. You can search for any number of different models simultaneously, through a range of angles and scales. You can also search for different kinds of models with Model Finder including models created from images (image-type), models created from Edge Finder result buffers (Edge Finder-type), models created from Model Finder result buffers (Model Finder-type), models merged from two previous models (merge-type), and synthetic models. Synthetic models are either models that have a predefined shape, or models defined from CAD files.

The module provides complete support for camera calibration. Searches can be performed in the calibrated real-world such that, without physically correcting your images, occurrences can be found even in the presence of complex distortions, and results calculated in real-world units.

The module also allows you to restore a Model Finder context from a file or memory stream, or save a Model Finder context to a file or memory stream.
