---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: Depth_from_focus
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / Depth from focus"
---

# Generating an index map from a scene

From a set of two-dimensional images of the same scene taken at different focus distances, the Aurora Imaging Library Registration module can generate an image that conveys depth information. Essentially, each pixel in the resulting image corresponds to the index of the image in the set where the corresponding pixel appears most in focus. The resulting image is referred to as an index map and the operation is referred to as a depth-from-focus registration operation. This operation uses multiple images of the same scene, taken at different focus distances, and calculates the index map, confidence map, and intensity map of the scene.

*[Image: RegDepthFromFocusOverview.png]*

The depth-from-focus registration operation works by considering the sharp portions in the input images. Then, for each pixel, it computes in which image that pixel is found most in focus. It replaces the corresponding pixel entry with the index of the image. You can also apply a regularization operation which ensures that neighboring index map pixels are coherent (smooth transitions between nearby pixels) in your resulting index map, if regularization is enabled. The above image shows a set of source images of the same scene, taken with different focus distances. The source images each contain a portion of the scene in focus; after the depth-from-focus registration operation, the index map for the object is generated based on the best focus images for the object.

In a depth-from-focus registration operation, the order of the source images is important; the source images must either be in an order from longest to shortest focal length (or vice versa).

In addition to generating the index map of a scene, you can also use a depth-from-focus registration operation to calculate a confidence map and/or intensity map of the scene. The confidence map indicates how reliable the index map is for a particular pixel; the higher the grayscale value of a pixel in the confidence map, the more confident you can be that the corresponding pixel in the index map is correct. The intensity map represents the scene in focus; each pixel in the intensity map is set to the intensity of the corresponding pixel in the image where that pixel is found to be most in focus. The following image demonstrates the confidence map and intensity map generated from a set of source images using a depth-from-focus registration operation.

*[Image: RegDepthfromFocusConfidenceIntensityMap.png]*

## Steps to performing a depth-from-focus registration

The following steps provide a basic methodology for performing a depth-from-focus operation on multiple images:

1. Allocate a depth-from-focus registration context, using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_DEPTH_FROM_FOCUS`](../../Reference/reg/MregAlloc.md).
2. If necessary, allocate a depth-from-focus registration result buffer to store the results of the registration operation, using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md). You don't need to allocate a result buffer if you intend on passing an image buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md).
3. If necessary, specify the control settings for your depth-from-focus registration operation using [`MregControl`](../../Reference/reg/MregControl.md).
4. Set up an array with the identifiers of image buffers that hold the input images. All the images should be of the same scene, all taken with the same camera settings except focus, and in buffers of the same size, type, and with a single band.
5. If necessary, allocate an image buffer to hold the depth-from-focus index map image (alternatively, the index map image can be stored in the depth-from-focus result buffer). The image buffer should be of the same size, type, and number of bands as the input images.
6. Perform the depth-from-focus operation, using [`MregCalculate`](../../Reference/reg/MregCalculate.md). You can pass and process the images in batches using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md), or all at once using [`M_COMPUTE`](../../Reference/reg/MregCalculate.md).
7. You can filter the index map using [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) to remove noise while preserving valid index values. Note, this does not preserve edges.
8. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), retrieve the required results from the result buffer, using [`MregGetResult`](../../Reference/reg/MregGetResult.md).
9. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), you can draw the index map image from the result buffer into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_DEPTH_INDEX_MAP`](../../Reference/reg/MregDraw.md). You can use [`MregGetResult`](../../Reference/reg/MregGetResult.md) to establish the size and type of the image buffer required to draw the depth-from-focus index map image. In addition, you can use [`MregDraw`](../../Reference/reg/MregDraw.md)with [`M_DRAW_DEPTH_CONFIDENCE_MAP`](../../Reference/reg/MregDraw.md)to draw the depth confidence map, or with [`M_DRAW_DEPTH_INTENSITY_MAP`](../../Reference/reg/MregDraw.md)to draw the depth intensity map, if those settings were previously enabled using [`MregControl`](../../Reference/reg/MregControl.md).
10. If necessary, save your registration context, using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md).
11. You can clear the result buffer to perform additional operations, using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_RESET`](../../Reference/reg/MregCalculate.md).
12. Free your allocated registration objects, using [`MregFree`](../../Reference/reg/MregFree.md), unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

When using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md), you can repeat steps 6 through 8 as many times as are required.

## Customizing your depth-from-focus context

You can customize a depth-from-focus registration operation using [`MregControl`](../../Reference/reg/MregControl.md).

The coherency of neighboring pixels in an index map can be important, to ensure the accuracy of the generated index map. You can apply a regularization operation to ensure that neighboring index map pixels are coherent. To do so, you must enable regularization and select the mode in which it should be performed using[`MregControl`](../../Reference/reg/MregControl.md) with [`M_REGULARIZATION_MODE`](../../Reference/reg/MregControl.md).

The regularization mode controls how the registration operation treats the smoothing of pixels from the source images, when generating the index map, in terms of their neighbors. By default, the registration operation will establish pixels in the index map using the average dominance of the neighboring pixels.

You can also use an adaptive smoothing regularization mode. This mode uses the local geometry of the content of the images. In this case, you must use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_ADAPTIVE_INTENSITY_DELTA`](../../Reference/reg/MregControl.md)to identify the maximum variation in pixel values for objects in the image. You must also use [`MregControl`](../../Reference/reg/MregControl.md) to specify an [`M_ADAPTIVE_SMOOTHING`](../../Reference/reg/MregControl.md) value, to control the threshold used to perform the smoothing. You can use [`MregControl`](../../Reference/reg/MregControl.md) to set [`M_REGULARIZATION_MODE`](../../Reference/reg/MregControl.md)to [`M_DISABLE`](../../Reference/reg/MregControl.md) to disable regularization, and use the raw index map values. The following image illustrates the different regularization modes that can be used to generate index maps.

*[Image: RegDepthFromFocusRegularizationMethods.png]*

The specified regularization mode affects the processing time of the depth-from-focus registration operation. The [`M_ADAPTIVE`](../../Reference/reg/MregControl.md) and [`M_AVERAGE`](../../Reference/reg/MregControl.md)regularization modes use the values of neighboring pixels to compute the new value of a given pixel in the index map. The efficiency of these modes depends on the [`M_REGULARIZATION_SIZE`](../../Reference/reg/MregControl.md)control type, which controls the size of the neighborhood over which regularization takes place.

The efficiency of a depth-from-focus registration operation also depends on the [`M_FOCUS_DEPTH_SIZE`](../../Reference/reg/MregControl.md)control type, which specifies the range of images in which an object in the image is in focus; that is, the number of images in which an object in the images goes from out of focus, to in focus, to out of focus again. A larger focus depth size indicates a more precise operation, and a longer processing time.

You can reduce processing time by disabling regularization ([`M_REGULARIZATION_MODE`](../../Reference/reg/MregControl.md) set to [`M_DISABLE`](../../Reference/reg/MregControl.md)); however, this can potentially reduce the accuracy of the operation, since nothing will be done to ensure coherence between neighboring pixels in the index map.

Note that since the input images must be of the same scene, it is assumed that the images completely overlap. Consequently, you do not set rough locations with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md).

## Computing a depth-from-focus registration

To calculate the index map from images of a scene at different focus depths, use [`MregCalculate`](../../Reference/reg/MregCalculate.md). The calculation can be done in one operation, or in stages; regardless of the method of calculation, an index map is generated after each call to [`MregCalculate`](../../Reference/reg/MregCalculate.md).

To calculate the index map in stages, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md) one or more times, passing a new array of images in each call. In this case, [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md) computes the depth-from-focus index map using this new information and all of the information previously processed and stored in the depth-from-focus registration result buffer. This kind of operation is useful if you are grabbing and don't have all the source images available at once.

To calculate the index map in one operation, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md), and pass the array of images required to generate the index map. Subsequent calls to [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md) will clear the depth-from-focus registration result buffer before storing new results.

When generating the index map in stages, you can only store the results in a depth-from-focus registration result buffer; however, when generating the index map in one operation, you can store the index map directly in an image buffer or in a result buffer.

When results are stored in a depth-from-focus result buffer, the index map can be drawn into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_DEPTH_INDEX_MAP`](../../Reference/reg/MregDraw.md). Whenever you draw the index map into an image, care must be taken so that the image in which the index map is drawn, is big enough to hold all index values; for instances, an 8-bit image cannot be used to store the index map when the depth-from-focus operation uses 300 images, since each source image requires a unique index in the index map.

In addition to generating an index map for a set of images of a scene, [`MregCalculate`](../../Reference/reg/MregCalculate.md)can also generate a confidence map and the intensity map for that set of images. Use [`MregControl`](../../Reference/reg/MregControl.md)with[`M_CONFIDENCE_MAP`](../../Reference/reg/MregControl.md) or [`M_INTENSITY_MAP`](../../Reference/reg/MregControl.md)to control whether these maps will be computed. When you are computing the confidence map or intensity map for a scene, you must use a depth-from-focus registration result buffer to store the results. You can then use [`MregDraw`](../../Reference/reg/MregDraw.md)to retrieve the resulting confidence map or intensity map image. To do so, you can use [`MregDraw`](../../Reference/reg/MregDraw.md)with[`M_DRAW_DEPTH_CONFIDENCE_MAP`](../../Reference/reg/MregDraw.md)or [`M_DRAW_DEPTH_INTENSITY_MAP`](../../Reference/reg/MregDraw.md), respectively, and the identifier of an image buffer.

The following diagram shows the ways to use [`MregCalculate`](../../Reference/reg/MregCalculate.md) to perform a depth-from-focus registration operation.

*[Image: DepthFromFocusFlowchart.png]*

## Retrieving and using depth-from-focus results

If [`MregCalculate`](../../Reference/reg/MregCalculate.md) used a result buffer as the destination, the index map image is stored in the result buffer along with some results and some information about the source images. You can use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_STATUS`](../../Reference/reg/MregGetResult.md) to check if the index map image is computed. If [`MregGetResult`](../../Reference/reg/MregGetResult.md) returns [`M_COMPLETE`](../../Reference/reg/MregGetResult.md), the preprocessing information and the index map image are available.

Many of the results that you can retrieve using [`MregGetResult`](../../Reference/reg/MregGetResult.md) depend on their availability within the result buffer. For these result types, you should add the combination constant [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md)when calling [`MregGetResult`](../../Reference/reg/MregGetResult.md). If you retrieve [`M_TRUE`](../../Reference/reg/MregGetResult.md), the information is available and can be retrieved using another call to [`MregGetResult`](../../Reference/reg/MregGetResult.md) without [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md). If the function returns [`M_FALSE`](../../Reference/reg/MregGetResult.md), the requested information is not available, and calling [`MregGetResult`](../../Reference/reg/MregGetResult.md) without [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md) will cause an error.

You can use [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) to filter noise in your index map while preserving valid index values. The following image shows how [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md) filters indices that cover very small areas on an image.

*[Image: RegDepthFromFocusMimFilterMajorityExample.png]*

## Depth from focus example

The depth from focus example _DepthFromFocus.cpp_ illustrates how the operation can be used to combine multiple images taken at different focus distances to obtain a resulting index map. To run this example, use Aurora Imaging Example Launcher.
