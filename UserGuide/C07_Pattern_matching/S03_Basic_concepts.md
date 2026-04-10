---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Basic_concepts
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Pattern Matching module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Pattern Matching module are:

**Normalized grayscale correlation (NGC)**. A technique that finds a pattern by looking for a similar spatial distribution of intensity.

**Model image**. The model, when drawn, is a model image.

**Model occurrence**. The model, when found in a target image.

**Model's source image**. The image from which the model is defined/extracted.

*[Image: Model.png]*

**Reference position**. A point that is relative to the model origin, that indicates which position to return when an occurrence of the model is found in the image. The position of an occurrence is the model's reference position transformed at the model occurrence (that is, relative to the target image's origin). The reference position does not have to be in the model, but it is, by default, located at the model's center.

**Search region**. The area in which to find the model's reference position of a model occurrence within the target image.

**Target image**. The image being searched.
