---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Metrology
section: Using_external_edgels_and_points
module_tag: met
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / metrology / Using external edgels and points"
---

# Using external edgels and points with constructed features

It can be useful to use the Metrology module to measure features and validate tolerances based on edgels and points established using other Aurora Imaging Library modules (or third-party applications). To do so, you can add to an [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md) constructed edgel feature to a Metrology context, using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_EDGEL`](../../Reference/met/MmetAddFeature.md) and[`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md). Then, you can add a set of related edgels/points (typically chains of edgels or pixels) to the constructed feature using [`MmetPut`](../../Reference/met/MmetPut.md). You can perform multiple calls to [`MmetPut`](../../Reference/met/MmetPut.md) to add multiple sets of edgels/points (edgel/pixel chains). Each call to [`MmetPut`](../../Reference/met/MmetPut.md) maintains the information and positions of previously-added edgel/pixel chains. You can add both edgels and points to an [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md) constructed edgel feature; however, to do so you must use separate calls. Use [`MmetInquire`](../../Reference/met/MmetInquire.md) to retrieve information about an external edgel feature, such as inquiring the total number of external edgels/points that were added ([`M_NUMBER`](../../Reference/met/MmetInquire.md)).

## Adding edge chains (chained edgels)

You can use [`MmetPut`](../../Reference/met/MmetPut.md) to add edge chains to an [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md) constructed edgel feature in a Metrology context. The edge chains could be established, for example, using the Edge Finder module or Model Finder module. The Edge Finder module allows you to identify and extract sets of connected edgels that make up the edges (edge chains) from a source image. Whereas, the Model Finder module allows you to retrieve the set of edgels at the occurrence of a model. You can use the Metrology module to ensure that the specified edgels comply with a required tolerance. For more information on these modules, see [Geometric Model Finder](../C08_Geometric_Model_Finder/ChapterInformation.md) and [Edge Finder](../C10_Edge_Finder/ChapterInformation.md), respectively.

To add edgels using [`MmetPut`](../../Reference/met/MmetPut.md), specify the total number of edgels you are passing, arrays containing both the X and Y-positions of your edgels, an array containing the gradient angles for each edgel, and optionally an array of chain indices that specifies to which chain the edgels belong. Information for one edgel should be at the same index position in the different arrays.

| Edgel | Array of edge indices | Array of X-coordinates | Array of Y-coordinates | Array of gradient angles (in degrees) |
| --- | --- | --- | --- | --- |
| 1 | 1 | 1 | 2 | 45 |
| 2 | 1 | 2 | 3 | 45 |
| 3 | 1 | 2 | 4 | 90 |
| 4 | 1 | 3 | 3 | 315 |
| 5 | 2 | 1 | 9 | 90 |
| 6 | 2 | 2 | 9 | 90 |
| 7 | 2 | 2 | 8 | 35 |
| 8 | 3 | 4 | 6 | 90 |
| 9 | 3 | 5 | 6 | 90 |

## Adding chained pixels (points)

Even if you don't know the gradient angle of a point, you can use [`MmetPut`](../../Reference/met/MmetPut.md) to add points that fall upon an edge (chained pixels) to an [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md) constructed edgel feature in a Metrology context.

The chained pixels could be established, for example, using the Blob Analysis module. The Blob Analysis module allows you to identify the coordinates of all pixels bordering a blob or delimiting holes within a blob in a counterclockwise or clockwise direction. You can then analyze the shape of the blobs using the Metrology module. For more information on establishing the chained pixels on the border of blobs and their holes, see [Chained pixels](../C06_Blob_analysis/S10_Finding_the_blob_location.md).

To add chained pixels using [`MmetPut`](../../Reference/met/MmetPut.md), specify the total number of points you are passing, arrays containing both the X and Y-positions of your points, and optionally, an array of chain indices that specify to which chain the points belong. When adding points that lie along an edge, as is the case when using the Blob Analysis module, there is no gradient angle information to consider. In this case, you can set the control flag in [`MmetPut`](../../Reference/met/MmetPut.md)to [`M_DEFAULT`](../../Reference/met/MmetPut.md) instead of specifying a gradient angle array.

When calculating the gradient angle at different points, [`MmetPut`](../../Reference/met/MmetPut.md)assumes that points in the arrays are consecutive, and lie along an edge. Furthermore, when calculating the gradient angle of chained pixels, [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md) must be set to [`M_SAME_OR_REVERSE`](../../Reference/met/MmetControl.md). Information for one pixel should be at the same index position in the different arrays.

The _MetrologyOnBlobs.cpp_ example shows how to use the Metrology module to analyze chained pixels derived from blob contours. To run this and other examples, use Aurora Imaging Example Launcher.

It might also be useful to add points that are not along an edge to a Metrology context for analysis. For example, you can create a 3D profile from a depth map or point cloud and then add the resulting points using [`MmetPut`](../../Reference/met/MmetPut.md). For more information, see [Analyzing the profile of a 3D cross-section](../C35_3D_Image_processing/S14_Analyzing_the_profile_of_a_3D_cross_section.md).
