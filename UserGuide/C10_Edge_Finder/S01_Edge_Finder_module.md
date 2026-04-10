---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Edge_Finder_module
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Edge Finder module"
---

# Aurora Imaging Library Edge Finder module

The Aurora Imaging Library Edge Finder module is a set of powerful functions that allow you to extract and analyze edges in a source image.

Many applications can be implemented using the Edge Finder module, such as complex measurement operations, defect detection, shape recognition, and shape analysis. You can also perform advanced edge manipulations, such as closing broken edges.

Edge Finder allows you to calculate a large number of edge features that can provide you with useful information, such as the edge's minimum and maximum Feret diameter. You can either calculate all required features for all edges, or perform a post-calculation on a refined subset of selected edges, which can speed up your processing time considerably.

The Edge Finder module can also be used in conjunction with the Aurora Imaging Library Geometric Model Finder and Advanced Geometric Matcher modules to define models from the result of an edge extraction or to find model occurrences in the result of an edge extraction. For more information, see [Geometric Model Finder module](../C08_Geometric_Model_Finder/S01_Geometric_Model_Finder_module.md) and [Advanced Geometric Matcher module](../C09_Advanced_Geometric_Matcher/S01_Advanced_Geometric_Matcher_module.md).

If your source image is calibrated, results are calculated in the world coordinate system, otherwise they are calculated in the pixel coordinate system. Also, if your source image is calibrated, you can retrieve results in either real-world or pixel units. For more information about camera calibration, see [Camera Calibration](../C28_Calibration/S01_Calibration_overview.md).

## Edges and Edge Finder

Edges are curves that delineate a boundary. These can be established from intensity transitions in an image. Well-defined edges come from sharp transitions in value, typically found in highly-contrasted images. Conversely, weak edges come from gradual transitions in value, typically found in smooth images.

Depending on your settings, Edge Finder will extract one of two edge types: object contours or line crests. An object contour is a type of edge that defines the outline of objects in an image. A line crest is a type of edge that defines the skeleton of objects in an image. Note that line crests should be used to extract edges from objects consisting of thin curvilinear shapes, such as fingerprints.

In either case, edges are extracted from the source image and used to form the image's edge map, which represents how the image is defined as a set of edges.

*[Image: EdgeExtractionExample.png]*

All Edge Finder operations, such as annotations and feature calculations are performed using the image's edge map.

You can only extract line crests from grayscale images. However, you can extract object contours from both grayscale and color images. When operating on color images, Edge Finder takes into consideration all three bands (RGB) to establish the presence of an edge.
