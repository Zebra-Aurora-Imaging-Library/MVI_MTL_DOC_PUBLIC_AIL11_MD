---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Calculating_and_retrieving_results
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Calculating and retrieving results"
---

# Calculating and retrieving results

Once you have allocated an Edge Finder context and result buffer ([`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) and [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md)), and have calculated the edge features ([`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md)), you can retrieve the results from your Edge Finder result buffer, using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md).

After the initial calculation, you can select edges that meet a specified criterion with [`MedgeSelect`](../../Reference/edge/MedgeSelect.md), and post-calculate new features for them. By doing so, you can avoid unnecessarily calculating time-consuming edge features for all edges in a source image.

For optimum performance, the source image buffer on which you make calculations should be a 1-band 8-bit unsigned buffer. Other buffer depths and types are generally accepted, but can slightly decrease performance. 3-band source image buffers are also supported, but only when extracting edge contours. When the source image buffer is a 32-bit floating-point buffer, Edge Finder uses floating-point precision calculations. In all cases, you can force the edge processing operations to use floating-point precision by enabling [`M_FLOAT_MODE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md). Note that changing this processing mode can slightly affect the resulting edge map; also, floating-point precision calculations will typically take more time.

Occasionally, an operation can take an unexpectedly long time to calculate. To prevent this, you can use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_TIMEOUT`](../../Reference/edge/MedgeControl.md) to set a maximum edge extraction and calculation time. By default, there is no time limit.

## Sorting keys

The results obtained from [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) can be sorted in ascending or descending order, by a maximum of three features assigned as sorting keys. To specify a feature as a sorting key, add [`M_SORTn_UP`](../../Reference/edge/MedgeControl.md) or [`M_SORTn_DOWN`](../../Reference/edge/MedgeControl.md) to the feature when selecting it for calculation using [`MedgeControl`](../../Reference/edge/MedgeControl.md). Assign the numbers 1, 2, or 3 to indicate the sorting precedence of the feature(s). Note that features will only be sorted after an [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).

You can also post-sort edges by assigning a sorting key to a previously calculated edge feature. Note that, although the post-calculation changes the order in which the edges are sorted, it does not change their label values, which were assigned at the edges' initial calculation. For more information on post-calculation, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

## Retrieving the results

Edge Finder offers several types of results that provide considerable information on the nature of the extracted edges, such as magnitude and Feret values. This information can be retrieved for all edges or for a specified edge, from your result buffer.

Typically, before retrieving any edge feature result, you should first retrieve the number of edges found in the image to determine the size of the result array needed to hold the results. To do so, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_NUMBER_OF_CHAINS`](../../Reference/edge/MedgeGetResult.md). Similarly, if you need to retrieve edgel results, such as the edgels' coordinates, magnitude, or angle values, you should also determine the size of the result array needed to hold the results. To do so, you must retrieve the number of extracted edgels for each edge using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_NUMBER_OF_CHAINED_EDGELS`](../../Reference/edge/MedgeGetResult.md).

Edge results are indexed as positive integers starting at 0 and, if applicable, are ordered with respect to the sorting keys. You can also label edges by enabling [`M_LABEL_VALUE`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md) (enabled by default). The edge's label value is a positive integer greater than or equal to 1. Each edge in an image is given a unique label value; that is, unlike an edge's index value, an edge's label value will not change, regardless of future operations.

Edge Finder allows you to retrieve results for a particular edge or for all edges, using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md). To retrieve results for a particular edge, you must specify either the edge's index or label value. However, since an edge's index value can change, it is recommended that you specify the label of the edge, especially if you want to perform selection and post-calculation operations. To retrieve results for all edges, you must specify [`M_ALL`](../../Reference/edge/MedgeGetResult.md) as the edge's index or label value.

Note that before an edge feature can be returned with [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md), you must first enable the feature for calculation, using [`MedgeControl`](../../Reference/edge/MedgeControl.md), and then call [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). Every edge feature, except for the label value ([`M_LABEL_VALUE`](../../Reference/edge/MedgeControl.md)), is disabled by default. For more information on edge features, see [Edge features](S06_Edge_features.md).

To verify the availability of an edge feature (if it has been calculated), use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md), and combine [`M_AVAILABLE`](../../Reference/edge/MedgeGetResult.md) to the specified result type. For a complete description of all possible results, see [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) in the _Aurora Imaging Library Reference_.

In general, results are returned in pixels, and coordinates are relative to the center of the top-left pixel in the source image. If you calculate edges using a calibrated source image, results are automatically returned in the output units specified by the associated camera calibration context of the source image. However, you can change the output units to pixel units or world units by setting [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) to either [`M_PIXEL`](../../Reference/edge/MedgeControl.md) or [`M_WORLD`](../../Reference/edge/MedgeControl.md), respectively. For more information, see [Camera Calibration](../C28_Calibration/S01_Calibration_overview.md).

Note that if your image is calibrated, results are calculated in the real world; therefore, retrieving results in pixel units instead of world units will typically be slower. Also note that, in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, the Feret angles). For example, if an edge appears warped in the source image, but the camera calibration context of the source image compensates for this during the extraction, the resulting Feret angles are meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

## Selecting the results

If you do not have many unwanted edges, it is usually faster to simply calculate all required features for all edges. However, calculating many features on a large number of unwanted edges can be unnecessarily time-consuming. To speed up this process, you can use [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) to select a subset of edges for calculation and result retrieval.

Edges that meet the [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) selection criteria can be included, excluded, or deleted from the result buffer. Further calculations are subsequently applied to included edges only. Excluded edges can be re-included at any time, while deleted edges are permanently removed from the result buffer.

Typically, if you have many unwanted edges, you calculate (for all edges) only those features that allow you to distinguish between wanted and unwanted edges. Then, you exclude the unwanted edges from further calculations, and calculate all the required features for the remaining included edges. To arrive at the required set of results, you can make as many calls as necessary to [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) and [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).

Any calculation made to refine results after an initial call to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) is considered a post-calculation. Post-calculating edges can be an effective way of speeding up processing time, especially if advanced features are only calculated on a small subset of selected edges. For more information on post-calculation, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

Note that, if at any time you calculate edges in a new source image, all current results will be discarded and replaced by the newly calculated edges. In addition, all selected features will be recalculated for all edges in the new source image. This means that you will have to restart the selection procedure.

The [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) function can be used to select edges based on one of the following:

- A calculated edge feature, where the edge selection depends on whether the specified edge feature meets the specified condition.
- The inter-relationship of edges, where the edge selection depends on whether edges meet the specified box or chain condition of the specified edge or group of edges.
- The current status of edges, where the edge selection depends on a specific edge, all edges, all included edges, or all excluded edges. These will be included, included only, excluded, excluded only, or deleted. For example, you can include only ([`M_INCLUDE_ONLY`](../../Reference/edge/MedgeSelect.md)) the excluded edges ([`M_EXCLUDED_EDGES`](../../Reference/edge/MedgeSelect.md)), which will essentially swap the previously included and excluded edges.
- The proximity of an edge or edges to a specified point, where the edge selection depends on the specified radius, and the nearest neighbor condition.

Note that you can also use [`MedgeSelect`](../../Reference/edge/MedgeSelect.md) to crop and select a portion of a specified edge. For more information, see [Advanced edge extraction](S08_Advanced_edge_extraction.md).

If the edges have been calculated using a calibrated source image, you must specify relevant values in the real world. If the edges have not been calculated using a calibrated source image, you must specify relevant values in pixels.

### Edge selection based on edge features

You can include, exclude, or delete edges based on whether the specified edge feature meets the specified condition. The edge feature used to select the edges must have been previously calculated for the included edges. After several selection and post-calculation operations, an edge feature might not be available or might only be available for a few edges of the included subset. To verify the availability of an edge feature, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md), and combine [`M_AVAILABLE`](../../Reference/edge/MedgeGetResult.md) to the specified feature result type.

The following code shows you how to apply a selection based on edge features:

> **Code example:** [userguide.edge_finder.Calculating_and_retrieving_results01](userguide.edge_finder.Calculating_and_retrieving_results01)

In this example, all edges with a moment elongation that is less than 0.8 are deleted.

*[Image: DeleteElongated.png]*

### Edge selection based on the inter-relationship of edges

You can include, exclude, or delete edges based on whether edges meet the specified box or chain condition. That is, you can include, exclude, or delete all edges that are inside or outside the bounding box of a specified edge or group of edges; similarly, you can include, exclude, or delete all edges that are inside or outside a specified edge or group of edges.

> **Note:** Inside and outside chain conditions only take effect when the edge is closed.

The following code shows you how to apply a selection based on the inter-relationship of edges:

> **Code example:** [userguide.edge_finder.Calculating_and_retrieving_results02](userguide.edge_finder.Calculating_and_retrieving_results02)

In this example, all edges inside included edges are excluded.

*[Image: ExcludeInsideIncluded.png]*

### Edge selection based on the proximity of the edges to a point

You can include, exclude, or delete edges based on their proximity to a specified point. You can select either the closest edge to a point, or all the edges in the specified vicinity of a point. Use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NEAREST_NEIGHBOR_RADIUS`](../../Reference/edge/MedgeControl.md) to specify the maximum radius distance from the point.

The following code shows you how to apply a selection based on the proximity of the edges to a point:

> **Code example:** [userguide.edge_finder.Calculating_and_retrieving_results03](userguide.edge_finder.Calculating_and_retrieving_results03)

In this example, only the edges closest to the point A (Ax, Ay), and within the radius r, are included.

*[Image: IncludeOnlyAllClosest.png]*

## Internal processing buffers

When performing edge extraction, Edge Finder uses internal processing buffers. These can be saved in the Edge Finder result buffer. The internal processing buffers are used when extracting edges. They include image derivative, angle, magnitude, source image, and mask buffers.

Some results can only be post-calculated if the appropriate internal buffer(s) have been previously saved in the result buffer. For example, you can only post-calculate the strength of an edge feature if the internal magnitude buffer was initially saved. For more information, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

Using [`MedgeControl`](../../Reference/edge/MedgeControl.md), you can set whether the internal processing buffers are saved. By default, the internal processing buffers are not saved ([`M_DISABLE`](../../Reference/edge/MedgeControl.md)). You must enable saving them in the result buffer, using [`MedgeControl`](../../Reference/edge/MedgeControl.md), before the first call to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). Saving these buffers does not affect processing time.

Even if an internal processing buffer has been saved in the result buffer, you cannot access it directly. You can, however, allocate a new buffer with the same properties as the internal buffer and use [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) to copy the contents of the internal buffer into the newly allocated buffer. To copy an internal result buffer you must:

1. Use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) to get the required information from the internal buffer. You will need the following information about the internal buffer to be cloned:
   - The buffer range and bit depth of the buffer (add [`M_SIGN`](../../Reference/edge/MedgeGetResult.md) or [`M_SIZE_BIT`](../../Reference/edge/MedgeGetResult.md) to the internal buffer, respectively). Alternatively you can add [`M_TYPE`](../../Reference/edge/MedgeGetResult.md), which returns both.
   - The width and height of the buffer (add [`M_SIZE_X`](../../Reference/edge/MedgeGetResult.md) and [`M_SIZE_Y`](../../Reference/edge/MedgeGetResult.md) to the internal buffer).
2. Allocate your own buffer, which should have the same properties as the internal buffer you want to copy.
3. Use [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) to copy the contents of the internal buffer into your own buffer.

### Derivatives

You can save the internal derivative buffers used when extracting edges, in the Edge Finder result buffer. To save the buffers, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md).

The following internal derivative buffers are stored within the Edge Finder result buffer:

- [`M_CROSS_DERIVATIVE_ID`](../../Reference/edge/MedgeGetResult.md). This buffer contains the cross derivatives calculated from the source buffer.
- [`M_FIRST_DERIVATIVE_X_ID`](../../Reference/edge/MedgeGetResult.md). This buffer contains the first derivatives in X-direction calculated from the source buffer.
- [`M_FIRST_DERIVATIVE_Y_ID`](../../Reference/edge/MedgeGetResult.md). This buffer contains the first derivatives in the Y-direction calculated from the source buffer.
- [`M_SECOND_DERIVATIVE_X_ID`](../../Reference/edge/MedgeGetResult.md). This buffer contains the second derivatives in the X-direction calculated from the source buffer.
- [`M_SECOND_DERIVATIVE_Y_ID`](../../Reference/edge/MedgeGetResult.md). This buffer contains the second derivatives in the Y-direction calculated from the source buffer.

Note that you can retrieve the first derivatives in the X- and Y-direction in one call using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_FIRST_DERIVATIVES_ID`](../../Reference/edge/MedgeGetResult.md). You can also retrieve the second derivatives in the X- and Y-direction in one call using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_SECOND_DERIVATIVES_ID`](../../Reference/edge/MedgeGetResult.md).

Typically, you will let Edge Finder manage its own derivative images to perform the edge extraction. However, you can specify your own derivative image buffers, so that the edge extraction is done with them instead. For more information on derivative images, and how they are used to extract edges, see [Providing the image's derivatives](S08_Advanced_edge_extraction.md).

### Angles

You can save the internal angle buffer used when extracting edges, in the Edge Finder result buffer, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md). To retrieve information about the internal angle buffer, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_ANGLE_ID`](../../Reference/edge/MedgeGetResult.md). You can also save the angle value of the edge at each edgel position, in the Edge Finder result buffer, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_ANGLE`](../../Reference/edge/MedgeControl.md); you can retrieve this information using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md)with[`M_CHAIN_ANGLE`](../../Reference/edge/MedgeGetResult.md). Angle values refer to the angle between the horizontal axis and the edge's perpendicular direction at each edgel location.

If you save the chain angle, the angle values of the extracted edges are saved. If you save the angle buffer, the angle values of all the edges in the source image are saved; in this case, the only relevant angle values are those whose edgels have a magnitude that is above the lower-bound threshold value.

Angles are returned counter-clockwise and mapped in the range of 0 to 255. That is, 0° corresponds to 0, and 360° corresponds to 256.

*[Image: AngleConventionDef.png]*

Note that for edge contours, the ascending gradient angle is returned. For line crests, the strongest gradient angle between 0 and 180 (either ascending or descending) is returned.

When extracting edge contours from a color image, the angle of the polarity cannot be obtained. For example, it is impossible to determine if red is darker or lighter than green; therefore, it is impossible to ascertain the direction of the transition.

### Magnitude

You can save the internal magnitude buffer used when extracting edges, in the Edge Finder result buffer, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_MAGNITUDE`](../../Reference/edge/MedgeControl.md). To retrieve information about the internal magnitude buffer, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_MAGNITUDE_ID`](../../Reference/edge/MedgeGetResult.md). You can also save the magnitude value of the edge at each edgel position, in the Edge Finder result buffer, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_MAGNITUDE`](../../Reference/edge/MedgeControl.md); you can retrieve this information using [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md)with[`M_CHAIN_MAGNITUDE`](../../Reference/edge/MedgeGetResult.md).

For object contours, the magnitude is the norm of the gradient vector at the edgel position. For line crests, the magnitude is equal to the maximum eigenvalue of the Hessian matrix at the edgel position.

If you save the chain magnitude, the magnitude values of the extracted edges are saved. If you save the magnitude buffer, the magnitude values of all the edges in the image are saved.

For more information on magnitude, and how it is used to extract edges, see [Customizing the edge extraction settings](S05_Customizing_the_edge_extraction_settings.md).

### Source image and mask

You can save both the source image, and the image used to mask the source image, in the Edge Finder result buffer. To do so, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_IMAGE`](../../Reference/edge/MedgeControl.md) and [`M_SAVE_MASK`](../../Reference/edge/MedgeControl.md), respectively. To retrieve information about the source image buffer, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_IMAGE_ID`](../../Reference/edge/MedgeGetResult.md). To retrieve information about the internal mask buffer, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_MASK_ID`](../../Reference/edge/MedgeGetResult.md). For more information on masking, see [Masking the edges](S08_Advanced_edge_extraction.md).

## Post-calculation

To avoid repeatedly calculating time-consuming edge features for all edges in a source image, you can perform post-calculations on included edges of an Edge Finder result buffer to decrease processing time. The operation of selecting edges and adding new features can be repeated until the required result is calculated.

Post-calculations are done with the result buffer only; that is, you should not provide a source image to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md). If you do provide a source image, all current results will be discarded and replaced by the newly calculated edges. In addition, all selected features will be recalculated for all edges in the new source image.

### Restrictions

Typically, anything that can be calculated can also be post-calculated. However, some post-calculations are not valid unless certain values have been initially saved.

The internal magnitude buffer ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_MAGNITUDE`](../../Reference/edge/MedgeControl.md)) must have been initially saved if you want to post-calculate the following:

- [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_AVERAGE_STRENGTH`](../../Reference/edge/MedgeControl.md).
- [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_STRENGTH`](../../Reference/edge/MedgeControl.md).
- [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_MAGNITUDE`](../../Reference/edge/MedgeControl.md).

Note that, if any of the values above were initially calculated, and you intend to fill edge gaps during post-calculation, the internal magnitude buffer ([`M_SAVE_MAGNITUDE`](../../Reference/edge/MedgeControl.md)) must have been initially saved.

The internal angle buffer ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md)) must have been initially saved if you want to post-calculate the following:

- [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_ANGLE`](../../Reference/edge/MedgeControl.md).
- The [`M_FILL_GAP_POLARITY`](../../Reference/edge/MedgeControl.md) settings [`M_SAME`](../../Reference/edge/MedgeControl.md) or [`M_REVERSE`](../../Reference/edge/MedgeControl.md).

Note that, if any of the values above were initially calculated, and you intend to fill edge gaps during post-calculation, the internal angle buffer ([`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md)) must have been initially saved.

Thresholding can also be applied during post-calculation. In this case, all internal processing buffers ([`M_SAVE_...`](../../Reference/edge/MedgeControl.md)) must have been initially saved, in [`MedgeControl`](../../Reference/edge/MedgeControl.md). For more information, see [Thresholding](S05_Customizing_the_edge_extraction_settings.md).

## Annotating the results

The [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) function offers many drawing operations for annotating your results. You can, for example, draw a zoomed region of the edge map that was used to calculate results by specifying the appropriate values for [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md), [`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md), [`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md), in [`MgraControl`](../../Reference/gra/MgraControl.md). The relative origin values must be specified in pixels, and are relative to the coordinates of the top-left corner of the region in the source image, while the scale values specify the X- and Y-scaling factors used to fill the destination buffer. For example:

*[Image: medgezooming.png]*

You can also perform many other drawing operations with [`MedgeDraw`](../../Reference/edge/MedgeDraw.md), such as draw a bounding box around each edge, a cross at each edge's center of gravity, or draw a cross at each edgel. In addition, you can draw the operation's numerical value by adding [`M_DRAW_VALUE`](../../Reference/edge/MedgeDraw.md) to the following drawing operations: [`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md), [`M_DRAW_POSITION`](../../Reference/edge/MedgeDraw.md), [`M_DRAW_FERET_MIN`](../../Reference/edge/MedgeDraw.md), [`M_DRAW_FERET_MAX`](../../Reference/edge/MedgeDraw.md), and [`M_DRAW_FERET_GENERAL`](../../Reference/edge/MedgeDraw.md). For example, if you add [`M_DRAW_VALUE`](../../Reference/edge/MedgeDraw.md) to [`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md), the center of gravity's coordinates are drawn within parenthesis, and centered above the drawing cross.

Typically, all drawing operations in [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) can be combined; you can, therefore, draw multiple effects simultaneously. For example, to draw the edge's chains, center of gravity, bounding box, and the source image's internal cross-derivative buffer, you would specify [`M_DRAW_EDGE`](../../Reference/edge/MedgeDraw.md) + [`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md) + [`M_DRAW_BOX`](../../Reference/edge/MedgeDraw.md) + [`M_DRAW_CROSS_DERIVATIVE`](../../Reference/edge/MedgeDraw.md).

> **Note:** Note that only one internal drawing buffer can be drawn at a time; for example, you cannot combine [`M_DRAW_ANGLE`](../../Reference/edge/MedgeDraw.md) and [`M_DRAW_MAGNITUDE`](../../Reference/edge/MedgeDraw.md) in the same operation.

You can either use a previously allocated 2D graphics context to control the drawing color, or you can use the default ([`M_DEFAULT`](../../Reference/edge/MedgeDraw.md)) 2D graphics context. You can either draw directly into the selected image buffer, or non-destructively annotate in a display's overlay buffer. For more information on Aurora Imaging Library graphics, see [Overview](../C25_Displaying_an_image/S01_Overview.md).

Note that, if the edges are calculated using a calibrated source image, Edge Finder takes the camera calibration into account; that is, drawings might be distorted, according to the camera calibration. For example, a straight line (in the world) might be drawn as a curve.

For a complete list and explanation of each drawing operation, see [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) in the _Aurora Imaging Library Reference_.
