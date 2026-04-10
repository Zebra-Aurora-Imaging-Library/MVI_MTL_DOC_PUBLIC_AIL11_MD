---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Advanced_edge_extraction
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Advanced edge extraction"
---

# Advanced edge extraction

In addition to the fundamental edge extraction settings, Edge Finder provides advanced settings that allow you to write customized applications. Edges can be approximated, masked, cropped, and extracted using advanced thresholding settings. You can provide your own derivative images, and you can put data from user-supplied arrays into an Edge Finder result buffer. You can also get edgels, from an Edge Finder result buffer, which are the closest neighbors to a list of user-specified edgel coordinates.

## Approximating the edges

If you are interested in a basic, geometric representation of the edge map, you can approximate your edges by specifying a polygonal segmentation. To do so, set [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) to [`M_LINE`](../../Reference/edge/MedgeControl.md). The default setting is [`M_DISABLE`](../../Reference/edge/MedgeControl.md).

When [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) is set to [`M_LINE`](../../Reference/edge/MedgeControl.md), each edge is piecewise approximated using a connected series of straight line segments, commonly called polylines. The polylines are defined by the set of automatically calculated vertex coordinates, which determine the location of the line segment ending points. Use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) to retrieve the X- ([`M_VERTICES_X`](../../Reference/edge/MedgeGetResult.md)) and Y- ([`M_VERTICES_Y`](../../Reference/edge/MedgeGetResult.md)) coordinates of the vertices. This information can be retrieved for either a group of edges or a particular edge. You can also get the index of each vertices' corresponding edge or edgel, using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_VERTICES_CHAIN_INDEX`](../../Reference/edge/MedgeGetResult.md) and [`M_VERTICES_INDEX`](../../Reference/edge/MedgeGetResult.md), respectively.

The resolution of the edge approximation can be adjusted using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_APPROXIMATION_TOLERANCE`](../../Reference/edge/MedgeControl.md). The range of this control varies from 0 to 100. When set to 0, a very fine approximation of the edges is performed. When set to 100, a coarse approximation of the edges is performed. The default value is 50. The following animation shows the effects of chain approximation.

*[Image: ApproximatingEdges]*

When [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) is set to [`M_LINE`](../../Reference/edge/MedgeControl.md), both the edge map and the edge approximation is calculated. When [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) is disabled, only the edge map is calculated.

## Masking the edges

The [`MedgeMask`](../../Reference/edge/MedgeMask.md) function allows you to apply a mask to the source image of the Edge Finder operation. A mask is a binary image used to define irrelevant, inconsistent, or featureless areas in the source image. As a result, when a mask is set, subsequent calls to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) will only extract edges in the source image's unmasked regions.

To set a mask, you must specify an image buffer (also known as a **mask buffer**) that identifies the masked pixels, using [`MedgeMask`](../../Reference/edge/MedgeMask.md). A masked pixel in the source image corresponds to a non-zero value in the mask buffer. Mask buffers are useful for creating non-rectangular areas to be processed. For rectangular areas, it is more effective to set a mask with child buffers. For more information, see [Using child buffers, ROIs, or a copy to manipulate specific data areas](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md).

If necessary, you can remove the mask by setting the mask buffer identifier to [`M_NULL`](../../Reference/edge/MedgeMask.md).

You can also mask edges during [post-calculation](S07_Calculating_and_retrieving_results.md). To do so, you must apply the mask after the initial call to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). In this case, masked edges are excluded from the result buffer and subsequent calculations. Note that partially masked edges are cropped.

## Cropping the edges

Selecting an edge or a group of edges for calculation and result retrieval is typically sufficient for the majority of cases. However, in some advanced applications, you might only be interested in one part of a single edge. In this case, you can use [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) with [`M_CROP_CHAIN`](../../Reference/edge/MedgeSelect.md).

Cropping an edge essentially means splitting one edge into two, thereby allowing you to choose only one of the edges to be included, excluded, or deleted from the Edge Finder result buffer. The selected portion receives a new label value, while the other portion keeps the original label value.

You can make as many cropping operations as necessary to arrive at the required set of results. Note that all calculated edge features are updated after each cropping operation.

To split an edge into two edges, you must specify the following:

- The edge to split. This is identified by the edge's label value.
- The point at which to split the edge. This is identified by the edgel's index value.
- The portion of the edge to select. This is identified by all edgels in the specified edge with index values that are greater than, less than, greater than or equal to, or less than or equal to, the point at which the edge was split.

The following code shows you how to crop and exclude a portion of the currently included edge labeled 5. The code also shows how to crop and include a portion of the currently excluded edge labeled 3:

> **Code example:** [userguide.edge_finder.advanced_edge_extraction02](userguide.edge_finder.advanced_edge_extraction02)

The following animation demonstrates the above code snippet:

*[Image: Edge_Crop]*

The edge labeled 5 is split into two edges at edgel 25. One edge consists of all edgels ranging from the start of edge 5 to the 24th edgel; this edge keeps the label 5 and is not selected for exclusion. The other edge consists of all edgels ranging from the 25th edgel to the end of edge 5; this edge gets a new label value and is selected for exclusion.

The edge labeled 3 is split into two edges at edgel 18. One edge consists of all edgels ranging from the start of edge 3 to the 18th edgel; this edge keeps the label 3 and is not selected for inclusion. The other edge consists of all edgels ranging from the 18th edgel to the end of edge 3; this edge gets a new label value and is selected for inclusion.

## Advanced thresholding

Edge Finder uses a hysteresis thresholding method to perform the edge extraction. That is, edge chains are extracted such that the magnitude values of all connected edgels are stronger than the lower bound threshold value, and at least one edgel in each edge chain has a magnitude that is stronger than the upper bound threshold value. For more information on thresholding, and on setting these bounds, see [Thresholding](S05_Customizing_the_edge_extraction_settings.md).

Edge Finder also allows you to customize the behavior of the hysteresis thresholding using the [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md). That is, [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md) allows you to set how the upper and lower bound threshold values will be used (as opposed to setting the bounds themselves).

If you set [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md) to [`M_HYSTERESIS`](../../Reference/edge/MedgeControl.md), both the lower bound threshold value and the upper bound threshold value will be used. This is the default setting.

If you set [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md) to [`M_NO_HYSTERESIS`](../../Reference/edge/MedgeControl.md), the lower bound threshold value is ignored, and is considered to be equal to the upper bound threshold value.

If you set [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md) to [`M_FULL_HYSTERESIS`](../../Reference/edge/MedgeControl.md), only the upper bound threshold value is used; the lower bound threshold value is ignored, and is considered to be 0. In this case, the extracted edges are sets of connected edgels such that the magnitude of at least one edgel in each edge is stronger than the upper bound threshold value. This type of thresholding can be used to extract edges in noise-free highly multi-contrast images.

## Providing the image's derivatives

Typically, you will let Edge Finder manage its own derivative images to perform the edge extraction. However, you can specify your own derivative image buffers, so that the edge extraction is done with them instead. In this case, when you call [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md), set the source image to [`M_NULL`](../../Reference/edge/MedgeCalculate.md), and specify the required derivative image buffers instead. For example:

> **Code example:** [userguide.edge_finder.advanced_edge_extraction01](userguide.edge_finder.advanced_edge_extraction01)

Note that, when specifying your own derivative image buffers, the majority of the processing control settings remain in effect, except for filter settings (for example, [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md)).

Derivative image buffers must be 1-band 16-bit signed or 1-band 32-bit floating-point buffers. In addition, all derivative image buffers must be consistent (for example, they must have the same scaling factor), and they must be of the same type and size. When the derivative image buffers are 32-bit floating-point buffers, Edge Finder uses floating-point precision calculations.

When extracting object contours, you must provide the image's first derivatives in both the X- and Y-directions. Note that the image derivatives must be consistent with the image axis; that is, if the intensity transition in the source image increases (from black to white) with the axis directions, the associated derivative values must be positive. Similarly, if the intensity transition in the source image decreases (from white to black) with the axis directions, the associated derivative values must be negative.

*[Image: EdgeFirstDerivativeSample.png]*

When extracting line crests, you must provide the image's second derivatives in both the X- and Y-direction, as well as the image's cross derivative. Note that the image derivatives must be consistent with the image axis; that is, a light line crest in the source image must result in negative second derivative values. Similarly, a dark line crest in the source image must result in positive second derivative values.

*[Image: EdgeSecondDerivativeSample.png]*

## Putting data into an Edge Finder result buffer

Typically, you will use the Edge Finder result buffer to hold edge extraction information after an edge selection and calculation. However, Edge Finder also allows you to copy data from user-supplied arrays into an Edge Finder result buffer, using [`MedgePut`](../../Reference/edge/MedgePut.md). This can be useful if you want to construct an Edge Finder result buffer that cannot be obtained from one image. You can therefore combine results from multiple Edge Finder result buffers to, for example, build a complete model to add to a Model Finder context. For more information, see [Defining and adding models to your Model Finder context](../C08_Geometric_Model_Finder/S05_Defining_and_adding_models_to_your_model_finder_context.md).

Note that once edges are added to the Edge Finder result buffer, their features can be calculated with [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).

If edgels are calculated with pixel accuracy, you must provide edgels with coordinates that are integer values; in this case, the distance between consecutive edgels should be one pixel. If edgels are calculated with subpixel accuracy, you can provide edgels with coordinates that are double values; in this case, the distance between consecutive edgels is such that, each pixel touched by the edge corresponds to one edgel. To change the accuracy with which edgels are calculated, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_ACCURACY`](../../Reference/edge/MedgeControl.md).

To add data to an Edge Finder result buffer, you must use [`MedgePut`](../../Reference/edge/MedgePut.md) to specify the following user-supplied arrays:

- The X-coordinate(s) of the edgel(s).
- The Y-coordinate(s) of the edgel(s).

You must also specify the number of edgels that you want to add to the Edge Finder result buffer. If necessary, you can also provide arrays containing the index of the edge each edgel belongs to, the orientation of the edgel and the magnitude of the edgel.

The information that you provide about the edges to add to the Edge Finder result buffer must be ordered on an edge by edge basis. For example, the following table illustrates how to supply X- and Y-coordinates for three distinct edges; note that the index provided is that of the edge each edgel belongs to:

| Array of X-coordinates | Array of Y-coordinates | Array of edge indices |
| --- | --- | --- |
| 1 | 2 | 1 |
| 2 | 3 | 1 |
| 2 | 4 | 1 |
| 3 | 3 | 1 |
| 1 | 9 | 2 |
| 2 | 9 | 2 |
| 2 | 8 | 2 |
| 4 | 6 | 3 |
| 5 | 6 | 3 |

## Finding the closest edgels to a list of points

From an Edge Finder result buffer, you can retrieve the coordinates of edgels that correspond to the closest neighbors from a list of user-specified source point coordinates. You must use [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) to specify your input data, which must include the source points and, if necessary, a corresponding angle for each point.

You can also set a series of constraints to which potential results (edgel candidates) must adhere before being returned. Only edgels that meet all constraints (if any are set) can be returned as the closest neighbors. To set the constraint(s), use [`MedgeControl`](../../Reference/edge/MedgeControl.md).

You can either set simple constraints, advanced constraints, or both. Simple constraints are based on the location of the edgel candidate (the search region), while advanced constraints are typically based on the inner properties of the edgel candidate (the gradient angle).

When applying constraints, you should first set the ones based on location. This will establish the search region in the Edge Finder result buffer, and generate a group of edgel candidates. If you do not apply any edgel-based constraints, then the candidates in this region that are closest to the source points will be returned. However, if you set advanced constraints, then only the edgels in this region that also adhere to these constraints can be returned.

Advanced constraints are typically based on the source edgel's gradient angle. In this case, you must provide an angle value for each source point as part of your input data for [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). Therefore, the candidate edgel will only be returned if its gradient angle adheres to the constraint applied with the source angle. Note that an edgel's gradient angle is perpendicular to the edge, and is dependent on the edge extraction process. For more information on an edgel's gradient angle, see [How edges are calculated](S04_Extracting_the_edges.md).

### Location-based constraints

You can use location-based constraints to limit the area within the Edge Finder result buffer from which edgels matching source points can be found. All edgels that do not fall within this area will be ignored. By default, the search region encompasses all edgels within the Edge Finder result buffer.

The following [`MedgeControl`](../../Reference/edge/MedgeControl.md) control types can be used to specify location-based constraints:

- [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md).
- [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md).
- [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md).
- [`M_SEARCH_RADIUS_MAX`](../../Reference/edge/MedgeControl.md).
- [`M_SEARCH_RADIUS_MIN`](../../Reference/edge/MedgeControl.md).

The maximum ([`M_SEARCH_RADIUS_MAX`](../../Reference/edge/MedgeControl.md)) and minimum ([`M_SEARCH_RADIUS_MIN`](../../Reference/edge/MedgeControl.md)) search radius constraints set the maximum and minimum distance radii in the Edge Finder result buffer that will be searched for the closest edgels. These values must be set in pixels. You must use [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) to specify the source point from which the radii are measured.

Together, [`M_SEARCH_RADIUS_MAX`](../../Reference/edge/MedgeControl.md) and [`M_SEARCH_RADIUS_MIN`](../../Reference/edge/MedgeControl.md) define a ring-like region, in the Edge Finder result buffer, that will be used to find the closest edgels. All edgels that fall outside this ring will be ignored. By default, the minimum distance radius is 0 pixels, and the maximum distance radius is an infinite number of pixels ([`M_INFINITE`](../../Reference/edge/MedgeControl.md)); that is, by default, the entire Edge Finder result buffer will be searched. Note that the [`M_INFINITE`](../../Reference/edge/MedgeControl.md) setting will retrieve all the closest edgels (one edgel per edge).

*[Image: NearestNeighborSearchRadiusConstraints.png]*

The Edge Finder result buffer region that will be searched for the closest edgels can be further constrained to an angular sector within the area defined by the radius constraints ([`M_SEARCH_RADIUS_...`](../../Reference/edge/MedgeControl.md)). You can use [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) to define the search angle, and [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to define the tolerance. The search angle is relative to the source angle set with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md), and the tolerance is applied to the resulting angle evenly; that is, half of the tolerance angle is applied in the positive direction, and half is applied in the negative direction. All edgels that are located within this angular sector (tolerance) can be returned. Any value between 0.0° to 360.0° can be set for both the search angle and tolerance. By default, [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) is set to 0.0° and [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) is set to 360.0 degrees; therefore, all edgels will be considered.

For example, if you set [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) to 30.0°, [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to 30.0°, and the source angle to 15°, only edgels that fall between 30.0° and 60.0° will be considered.

*[Image: NearestNeighborDirectionAngleConstraints.png]*

You can also set the sign of the angle that will be used to find the closest edgels with [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md). The sign of the angle can be the same as ([`M_SAME`](../../Reference/edge/MedgeControl.md)) or the reverse of ([`M_REVERSE`](../../Reference/edge/MedgeControl.md)), the source angle set with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). The default is [`M_SAME`](../../Reference/edge/MedgeControl.md). The sign can also be set to [`M_SAME_OR_REVERSE`](../../Reference/edge/MedgeControl.md), which means that it can be the same as, or the reverse of, the source angle.

For example, if you set the source angle to 15°, and the search angle and tolerance to 30.0°, then edgels that fall within an angle that has the same sign would be located between 30.0° and 60.0°, edgels that fall within an angle that has a reverse sign would be located between 210° and 240°, and edgels that fall within an angle that has any sign would be located at either between 30.0° and 60.0°, or between 210° and 240°.

*[Image: NearestNeighborConstraints.png]*

Note that, if necessary, you can allow a point between two edgels to be returned, using [`M_GET_SUBEDGELS`](../../Reference/edge/MedgeGetNeighbors.md) in [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). In this case, an edgel candidate is returned, as long as an edge (and not necessarily an edgel) is located within the search region.

*[Image: NearestNeighborAccuracy.png]*

### Edgel-based constraints

As mentioned, location-based constraints limit the region in the Edge Finder result buffer that will be searched for the closest edgels. Edgel-based constraints, on the other hand, are applied to the edgels themselves. Only edgels that adhere to these constraints can be returned. Note that, if you have applied location-based constraints, only edgels within the defined search region will be considered.

The following [`MedgeControl`](../../Reference/edge/MedgeControl.md) control types are used to specify edgel-based constraints:

- [`M_NEIGHBOR_MAXIMUM_NUMBER`](../../Reference/edge/MedgeControl.md).
- [`M_NEIGHBOR_MINIMUM_SPACING`](../../Reference/edge/MedgeControl.md).
- [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md).
- [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md).

The maximum number constraint ([`M_NEIGHBOR_MAXIMUM_NUMBER`](../../Reference/edge/MedgeControl.md)) sets the maximum number of closest edgels (that is, the _N_ closest edgels) that can be returned, for each source point. The default maximum number is 1; therefore, the edgel closest to each source point is returned.

The minimum spacing constraint ([`M_NEIGHBOR_MINIMUM_SPACING`](../../Reference/edge/MedgeControl.md)) sets the minimum distance separating closest edgel candidates within the same edge. Therefore, edgels that are too close together will not be returned. The spacing is set in number of edgels and must be greater than or equal to 1. The default minimum spacing is infinite ([`M_INFINITE`](../../Reference/edge/MedgeControl.md)), which means that there is no minimum distance (number of edgels) that must separate edgel candidates.

The neighbor's angle constraint ([`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md)) sets the gradient angle that an edgel must have, before being considered a candidate. [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) is based on a source angle (for each source point), set with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). The gradient angle of the edgel candidate can be the same ([`M_SAME`](../../Reference/edge/MedgeControl.md)), the reverse ([`M_REVERSE`](../../Reference/edge/MedgeControl.md)), or the same or reverse ([`M_SAME_OR_REVERSE`](../../Reference/edge/MedgeControl.md)) as the angle of the source point. The constraint can also be set to any angle ([`M_ANY`](../../Reference/edge/MedgeControl.md)); in this case, the source angle and the source point are ignored. The default is [`M_ANY`](../../Reference/edge/MedgeControl.md). Note that [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) can only be used if the [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md) internal buffer was initially saved.

You can also set an angular tolerance for [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) using [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md). In this case, candidate edgels that have a gradient angle that falls within the specified angular range (tolerance) can be returned. The tolerance is applied to the [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) value evenly; that is, half of the tolerance angle is applied in the positive direction, and half is applied in the negative direction. Any angle between 0.0° to 360.0° can be set as a tolerance. By default, [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) is set to 180.0°; therefore, only candidate edgels that have a gradient angle that is the same as the source angle will be considered. Note that setting [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to 360.0° is equivalent to setting the neighbor angle constraint to [`M_ANY`](../../Reference/edge/MedgeControl.md); in either case, edgels with any gradient angle are considered.

For example, if you set the angle of the source point to 45°, [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) to [`M_SAME_OR_REVERSE`](../../Reference/edge/MedgeControl.md), and [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to 90°, only edgels that have an angle between 0° and 90° or between 180° and 270° can be returned. In the following example, edgels 1 and 3 are considered the closest edgel candidates.

*[Image: NeighborAngle.png]*
