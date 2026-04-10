---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Metrology
section: Retrieving_and_drawing_results
module_tag: 3dmet
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Metrology / Retrieving and drawing results"
---

# Retrieving and drawing results

After successfully performing an [`M3dmetFit`](../../Reference/3dmet/M3dmetFit.md), [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md), [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md), or [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation, you can retrieve the required results from your 3D metrology result buffer using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md). You can also copy certain results using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).

> **Note:** Note that functions suffixed with Ex (for example, [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md)) have a corresponding function without the Ex (for example, [`M3dmetFeature`](../../Reference/3dmet/M3dmetFeature.md)) that return the results directly, instead of returning them in a result buffer. The [`M3dmet...Ex`](../../Reference/3dmet/M3dmetFeatureEx.md) functions calculate more results at once.

To draw the result of a fit or volume operation, use [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

## Possible results

You can retrieve several types of results with the 3D Metrology module. These results provide considerable information about fit, statistics, feature, and volume operations.

### Fit results

For fit operations, you can retrieve numerous details about the fitted geometry, as well as general information.

After a successful fit operation, you can retrieve the following results about a fitted geometry, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md):

- The type of fitted geometry ([`M_GEOMETRY_TYPE`](../../Reference/3dmet/M3dmetGetResult.md)).
- Its center point ([`M_CENTER_...`](../../Reference/3dmet/M3dmetGetResult.md)).
- Its start and end point ([`M_START_POINT_...`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_END_POINT_...`](../../Reference/3dmet/M3dmetGetResult.md), respectively).
- The components of its unit vector ([`M_AXIS_...`](../../Reference/3dmet/M3dmetGetResult.md)), which is, for example, the vector coincident upon a cylinder's central axis or along a fitted line.
- Its closest point to the origin of the working coordinate system ([`M_CLOSEST_TO_ORIGIN_...`](../../Reference/3dmet/M3dmetGetResult.md)).
- For a fitted plane geometry, the coefficients of its equation ([`M_COEFFICIENT_...`](../../Reference/3dmet/M3dmetGetResult.md)).

You can also retrieve the following information about the fit operation itself, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md):

- The status of the operation ([`M_STATUS_FIT`](../../Reference/3dmet/M3dmetGetResult.md)).
- The root-mean-squared (RMS) error for the fit ([`M_FIT_RMS_ERROR`](../../Reference/3dmet/M3dmetGetResult.md)).
- The outlier distance ([`M_OUTLIER_DISTANCE`](../../Reference/3dmet/M3dmetGetResult.md)).
- The number of points used in the fit ([`M_NUMBER_OF_POINTS_TOTAL`](../../Reference/3dmet/M3dmetGetResult.md)), and subsets of this total, such as the number of inlier and outlier points ([`M_NUMBER_OF_POINTS_...`](../../Reference/3dmet/M3dmetGetResult.md)).
- The size of the source range component ([`M_RESULT_IMAGE_SIZE_...`](../../Reference/3dmet/M3dmetGetResult.md)), which is useful for allocating an image buffer of an appropriate size for [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).

To obtain the fitted geometry itself, copy it into a 3D geometry object using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md).

### Statistics results

For an [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) operation, you can retrieve statistics such as the maximum or minimum distance between the specified reference object and the source point cloud or depth map. To retrieve a statistics result, you must have enabled its calculation using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) or you must have called [`M3dmetStat`](../../Reference/3dmet/M3dmetStat.md) with a predefined context.

For the distances between a reference object and the source point cloud or depth map, you can retrieve the following statistics results, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md):

- The maximum and minimum distance ([`M_STAT_MAX`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_STAT_MIN`](../../Reference/3dmet/M3dmetGetResult.md), respectively).
- The maximum and minimum absolute distance ([`M_STAT_MAX_ABS`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_STAT_MIN_ABS`](../../Reference/3dmet/M3dmetGetResult.md), respectively).
- The mean distance ([`M_STAT_MEAN`](../../Reference/3dmet/M3dmetGetResult.md)).
- The mean absolute distance ([`M_STAT_MEAN_ABS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The standard deviation ([`M_STAT_STANDARD_DEVIATION`](../../Reference/3dmet/M3dmetGetResult.md)).
- The RMS error ([`M_STAT_RMS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The sum of all distances ([`M_STAT_SUM`](../../Reference/3dmet/M3dmetGetResult.md)).
- The sum of all absolute distances ([`M_STAT_SUM_ABS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The sum of squared distances ([`M_STAT_SUM_OF_SQUARES`](../../Reference/3dmet/M3dmetGetResult.md)).

### Feature results

For results that are available after an [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md) operation, refer to the tables in [](S03_Calculate_and_Calculate_Scalar.md). Note that, unlike [`M3dmetFeatureEx`](../../Reference/3dmet/M3dmetFeatureEx.md), the [`M3dmetFeature`](../../Reference/3dmet/M3dmetFeature.md) function returns results directly and does not use a result buffer.

### Volume results

For an [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) operation, you can retrieve the computed volume and other data. Note that, unlike [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md), the [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) function returns results directly and does not use a result buffer.

After calling [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md) to calculate a volume, you can retrieve, using [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md):

- The computed volume ([`M_VOLUME`](../../Reference/3dmet/M3dmetGetResult.md)).
- The mode used ([`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetGetResult.md)), which indicates how the volume was calculated relative to the reference object, either above, under, total, difference, or complete.
- The type of reference object used ([`M_VOLUME_REFERENCE_TYPE`](../../Reference/3dmet/M3dmetGetResult.md)).
- The number of elements used ([`M_VOLUME_NB_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The number of positive elements used ([`M_VOLUME_NB_POSITIVE_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The number of negative elements used ([`M_VOLUME_NB_NEGATIVE_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The number of valid elements that were not used ([`M_VOLUME_NB_UNUSED_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md)).
- The status of the volume operation ([`M_STATUS_VOLUME`](../../Reference/3dmet/M3dmetGetResult.md)).
- Whether volume information was saved into the result buffer ([`M_SAVE_VOLUME_INFO`](../../Reference/3dmet/M3dmetGetResult.md)).

Note that you can have positive and negative volume elements on either side of a reference object. For example, for an [`M_DIFFERENCE`](../../Reference/3dmet/M3dmetControl.md) operation, in which the total volume is calculated as the volume above the reference object minus the volume below, both calculated volumes (before the subtraction) could be composed of positive and negative volume elements. In this case, [`M_VOLUME_NB_POSITIVE_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md) and [`M_VOLUME_NB_NEGATIVE_ELEMENTS`](../../Reference/3dmet/M3dmetGetResult.md) return the total positive or negative elements, respectively, regardless of whether those elements were above or below the reference object. See [](S06_Calculate_volume.md) for more information.

You can also obtain source component dimensions, to use for allocating an image buffer of the correct size when retrieving a result image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md). You can retrieve:

- The size of the source range component ([`M_RESULT_IMAGE_SIZE_...`](../../Reference/3dmet/M3dmetGetResult.md)).
- The size of the source mesh component ([`M_RESULT_ELEMENT_IMAGE_SIZE_...`](../../Reference/3dmet/M3dmetGetResult.md)).

An index, mask, or status image created using [`M3dmetCopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) is useful for diagnosing volume operation results. For more information, see [Diagnosing volume results](S06_Calculate_volume.md).

## Drawing results

You can draw the results of a fit or volume operation using [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

Note that points are represented as squares in a 3D display. You can specify the thickness of points in number of pixels, and they will always be shown with sides of this length when displayed, regardless of their distance from the 3D display's viewpoint.

### Drawing fit results

For fit results, you must use a default draw 3D metrology context ([`M_DEFAULT`](../../Reference/3dmet/M3dmetDraw3d.md)) when calling [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md). Default settings of the 3D graphics list are used for the draw. Once you have drawn a fit result in a 3D graphics list, you can access and modify it using [`M3dgraControl`](../../Reference/3dgra/M3dgraControl.md).

When drawing a fitted plane, [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) will draw a 3D plane geometry clipped to the limits of the points that were used to fit the plane, unlike [`M3dgeoDraw3d`](../../Reference/3dgeo/M3dgeoDraw3d.md), which clips the 3D graphic using the 3D graphics list's clipping box.

### Drawing volume results

For volume results, you typically allocate a draw 3D metrology context to hold the settings for the draw, using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_DRAW_3D_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md). You then specify draw operations and options for the draw with successive calls to [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md). Finally, you call [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) to perform the draw.

Note that for volume results, you can skip allocating a draw 3D metrology context and instead use a default draw 3D metrology context with default draw settings. To do so, directly call [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md) with [`M_DEFAULT`](../../Reference/3dmet/M3dmetDraw3d.md). Note that the default settings specify to draw the 3D graphic with a default point thickness of one pixel and a default wireframe bounding box appearance.

The [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) function provides several operations for drawing volume results into a 3D graphics list.

For volume results, you can draw:

- Volume elements that increased the volume ([`M_DRAW_VOLUME_POSITIVE_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)).
- Volume elements that decreased the volume ([`M_DRAW_VOLUME_NEGATIVE_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)).
- All volume elements that contributed to the volume, both positively and negatively ([`M_DRAW_VOLUME_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md)).

By default, Aurora Imaging Library draws all the volume elements that were used to compute the volume, independent of whether the elements contributed positively or negatively to the operation. To draw only positive or negative volume elements, you must explicitly enable the relevant draw operation (using [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md)). You can also specify to draw a single volume element ([`M_GLOBAL_DRAW_SETTINGS`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_INDEX`](../../Reference/3dmet/M3dmetControlDraw.md)).

Use successive calls to [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) to specify how to draw the 3D graphics. For example, you can set the color for each type of drawn graphic. You can also set the thickness with which to draw the graphic, and whether it appears as a solid surface, a wireframe, or as points. By default, Aurora Imaging Library draws each volume element as a distinguishable volume ([`M_VOLUME_ELEMENT_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_VOLUME`](../../Reference/3dmet/M3dmetControlDraw.md)). Alternatively, you can specify to draw only the surfaces of the source and reference objects ([`M_SURFACE`](../../Reference/3dmet/M3dmetControlDraw.md)), only the surfaces of the source ([`M_SURFACE_SOURCE`](../../Reference/3dmet/M3dmetControlDraw.md)), or only the surfaces of the reference ([`M_SURFACE_REFERENCE`](../../Reference/3dmet/M3dmetControlDraw.md)). For a complete description of all possible options for the draw, refer to the description of [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) in the Aurora Imaging Library Reference.

Note that when [`M_VOLUME_ELEMENT_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) is set to [`M_VOLUME`](../../Reference/3dmet/M3dmetControlDraw.md) and you are also rendering transparency ([`M_OPACITY`](../../Reference/3dmet/M3dmetControlDraw.md) set to less than 100.0), a powerful GPU is required, especially when the number of volume elements is large.

The following images show a depth map drawn using [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md).

|   |   |
| --- | --- |
| *[Image: 3dmet_volume_drawing_results_pic1.png]* | Depth map drawn using [`M3dmetDraw3d`](../../Reference/3dmet/M3dmetDraw3d.md). Here, one surface of each volume element that makes up the source object is drawn, at a height above (green) or below (red) the reference plane. [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) settings include [`M_DRAW_VOLUME_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_SURFACE`](../../Reference/3dmet/M3dmetControlDraw.md) and [`M_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_SOLID`](../../Reference/3dmet/M3dmetControlDraw.md). |
| *[Image: 3dmet_volume_drawing_results_pic2.png]* | The same depth map with a single volume element drawn in white. Here, besides the full depth map drawn using surfaces, a single volume element is drawn using the volume option, which shows the faces of the rectangular prism. For this volume element, [`M3dmetControlDraw`](../../Reference/3dmet/M3dmetControlDraw.md) settings include [`M_GLOBAL_DRAW_SETTINGS`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_INDEX`](../../Reference/3dmet/M3dmetControlDraw.md) set to the element's index, [`M_DRAW_VOLUME_ELEMENTS`](../../Reference/3dmet/M3dmetControlDraw.md) with [`M_VOLUME_ELEMENT_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_VOLUME`](../../Reference/3dmet/M3dmetControlDraw.md), [`M_APPEARANCE`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_SOLID`](../../Reference/3dmet/M3dmetControlDraw.md), and [`M_COLOR`](../../Reference/3dmet/M3dmetControlDraw.md) set to [`M_COLOR_WHITE`](../../Reference/3dmet/M3dmetControlDraw.md). |
