---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Basic_concepts
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Edge Finder module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Edge Finder module are:

- **Edge**. A curve that delineates a boundary, which can be established from intensity transitions in an image. In Edge Finder, an edge is considered to be an edge chain and its features.
- **Edgel**. An elementary point (or edge element) within an edge.
- **Edge angle**. The angle between the horizontal axis and the edge's perpendicular direction at each edgel location.
- **Edge chain**. The set of connected edgels that construct an edge.
- **Edge feature**. Information describing an edge, such as its length.
- **Edge magnitude**. The strength of the edge at each edgel location. For object contours, the magnitude is the norm of the gradient vector at the edgel position. For line crests, the magnitude is equal to the maximum eigenvalue of the Hessian matrix at the edgel position.
- **Edge map**. The set of edges extracted from the source image.
- **Filter support region**. The number of neighboring pixels that are taken into account when computing the output pixel. The number of pixels taken into account can be finite or infinite, and is determined by the type of filter used to extract edges.
- **Line crest edge**. A type of edge that defines the skeleton of objects in an image. Typically, objects should be thin curvilinear shapes.
- **Object contour edge**. A type of edge that defines the outline of objects in an image. Typically, objects should not be thin curvilinear shapes.
- **Post-calculation**. Any calculation made to refine results after an initial edge calculation.
- **Source image**. The image from which the edges are extracted.

> **Note:** After edge chains have been extracted, the terms edge chain and edge are used interchangeably.
