---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Basic_concepts
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Measurement module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Measurement module are:

- **Characteristic**. An identifying quality of a marker, such as its position.
- **Edge**. A curve that delineates a boundary, which can be established from intensity transitions in an image.
- **Edge peak**. The highest edgevalue among those for which the edge profile rises above and then falls below the minimum edgevalue threshold on both sides.
- **Edge profile**. The normalized first derivative of the intensity profile over the length of the search region for marker.
- **Edge strength**. The greatest edgevalue of an edge.
- **Edgevalue**. A value that represents the change at a given location in the intensity profile. Aurora Imaging Library establishes edgevalues by applying a first derivative filter to the intensity profile of the pixels in the search region. Aurora Imaging Library normalizes the filter's output according to the number of pixels and the maximum pixel value possible.
- **Intensity profile**. A projection of the pixels bounded by the two-dimensional search region into a one-dimensional pixel intensity summation. In the case of the Measurement module, the region is the search region.
- **Marker**. The set of image characteristics defining what to search for, or what to explicitly place, in a target image. The Aurora Imaging Library Measurement module allows you to search for edge markers, stripe markers, and circle markers. You can also explicitly place point markers.
- **Marker occurrence**. An instance of the marker found in the target image.
- **Orientation**. The expected alignment of the marker's occurrences, relative to the search region. The orientation can be horizontal, vertical, or any.
- **Search direction**. The direction in which to search for the marker's occurrences in the search region. The search direction is perpendicular to the specified orientation of the marker.
- **Search region**. The general region of the target image in which the find operation can search for a marker occurrence.
- **Subregions**. Sections that subdivide the search region. If subregions exist, they define where to search for a marker occurrence.
- **Target image**. The image in which to define the search region and find the marker.
