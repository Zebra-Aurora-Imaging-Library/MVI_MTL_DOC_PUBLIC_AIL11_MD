---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Optimizing_edge_extractions
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Optimizing edge extractions"
---

# Optimizing edge extractions

There are some things that you can do to optimize the edge extraction and/or speed up the calculation process:

- Specify an area to process.
- Perform post-calculations to calculate expensive features.
- Optimize your control type ([`MedgeControl`](../../Reference/edge/MedgeControl.md)) settings.
- Retrieve calibrated results in world units.

## Specify an area to process

Specify an area to process that contains all necessary edges. You can specify an area using a child buffer. All edgels that fall outside the area will be ignored, speeding up the edge extraction. For non-rectangular areas, you can specify the appropriate area using a mask ([`MedgeMask`](../../Reference/edge/MedgeMask.md)).

For more information on setting an area, see [Using child buffers, ROIs, or a copy to manipulate specific data areas](../C23_Data_buffers/S06_Using_child_buffers_ROIs_or_a_copy_to_manipulate_specific_data_areas.md), or [Masking the edges](S08_Advanced_edge_extraction.md).

## Perform post-calculation

To compute features as efficiently as possible, you should calculate a few features first (preferably, the fastest), and eliminate as many unnecessary edges as possible. Then, post-calculate expensive features on the remaining edges. The process of calculating features and eliminating unnecessary edges can be repeated until the required result is calculated. Post-calculation greatly decreases processing time since Aurora Imaging Library won't have to calculate the most time-consuming edge features for all edges.

For more information on post-calculation, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

## Optimize your control type settings

To compute features as efficiently as possible, you should optimize your [`MedgeControl`](../../Reference/edge/MedgeControl.md) control type settings. Some settings result in faster calculations ([`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md)). Note that faster calculations do not mean better results and your application might require settings that take longer to calculate.

### Adjust the accuracy

Use the minimum required accuracy ([`M_ACCURACY`](../../Reference/edge/MedgeControl.md)); decreasing the accuracy increases the speed of the calculation process. Disabling [`M_ACCURACY`](../../Reference/edge/MedgeControl.md) results in pixel precision, which although not very precise, is typically sufficiently accurate when extracting the edges of large objects. By default, the accuracy is set to [`M_HIGH`](../../Reference/edge/MedgeControl.md); this results in subpixel edgel accuracy, but typically a slower calculation process.

For more information on setting the accuracy, see [Edgel accuracy](S05_Customizing_the_edge_extraction_settings.md).

### Building edge chains

If possible, reduce the detail level of edge chains by disabling [`M_CHAIN_ALL_NEIGHBORS`](../../Reference/edge/MedgeControl.md). Disabling this control type increases the speed of the calculation but reduces the level of detail present in the edge chains. Enabling this control type results in slower calculations; however, the edge chains contain the most amount of edgel information possible. Note that by default, this control type is disabled.

For more information on edge chains, see [Basics of edge extraction](S04_Extracting_the_edges.md).

### Adjust the extraction scale

If possible, reduce the scale of the image when extracting edges, by setting [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) to a value between 0.0 and 1.0. Reducing the extraction scale can increase the speed at which edges are found and extracted. Note however, this can result in less reliable results, including the loss of important details and/or reduced accuracy of the extraction results. An extraction scale greater than 1.0 will slow down the extraction. Once the edge extraction is complete, the results are scaled to the original scale of the image. Note that the default setting of 1.0 usually provides the most accurate search results.

### Specifying the IIR filter mode

When using an IIR filter, it is generally faster to implement the filtering operation recursively, unless you implement it using kernel mode with a very small convolution kernel (for example, 3x3). If you use kernel mode with a large kernel, the calculation will be slower, although more accurate. Note that if you have dedicated hardware performing the operation, it is typically faster to use a convolution kernel.

For more information on filters, see [Customizing the edge extraction settings](S05_Customizing_the_edge_extraction_settings.md).

### Specify the smoothness

If your image contains a lot of noise, increase the smoothness factor ([`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md)). Increasing the smoothness factor reduces the noise, resulting in the edge extraction operation extracting fewer unwanted edges. This will typically decrease the time it takes to extract the edges. Be aware that in kernel mode, increasing the smoothness factor can increase the size of the convolution kernel, which increases the processing time. Note that a very high smoothing level can result in a loss of important detail and a decrease in precision.

For more information on specifying the smoothness factor, see [Smoothing for IIR filters](S05_Customizing_the_edge_extraction_settings.md).

### Specify the magnitude

Use the square of the gradient magnitude, by setting [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md) to [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md). This optimizes the edge extraction while preserving a very good edgel result. [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md) is calculated faster than [`M_NORM`](../../Reference/edge/MedgeControl.md), but is less accurate. Note that for [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md), the default is [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md) for contour Edge Finder contexts, and the default is [`M_NORM`](../../Reference/edge/MedgeControl.md) for crest Edge Finder contexts.

For more information on the magnitude type, see [Magnitude type](S05_Customizing_the_edge_extraction_settings.md).

### Setting the threshold

Use the highest possible threshold ([`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md)) that still detects all pertinent edgels. The lower the threshold, the more sensitive the edgel detection; that is, more edgels are detected. The default threshold mode is [`M_HIGH`](../../Reference/edge/MedgeControl.md).

For more information on thresholding, see [Thresholding](S05_Customizing_the_edge_extraction_settings.md).

### Calculating the length of each edge

Calculate the length of each edge using [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md). [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md) gives a coarser but faster approximation of the edge's length than [`M_LENGTH`](../../Reference/edge/MedgeControl.md).

For more information on calculating the length, see [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md) and [`M_LENGTH`](../../Reference/edge/MedgeControl.md) in [`MedgeControl`](../../Reference/edge/MedgeControl.md).

### Limiting the search radius

When retrieving the coordinates of edgels using [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md), limit the search region using [`M_SEARCH_RADIUS_MAX`](../../Reference/edge/MedgeControl.md), [`M_SEARCH_RADIUS_MIN`](../../Reference/edge/MedgeControl.md), and [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md). All edgels that fall outside this region will be ignored, speeding up the edge extraction.

For more information on setting the search radius, see [Location-based constraints](S08_Advanced_edge_extraction.md).

## Retrieving calibrated results

If your image is calibrated, retrieve results in world units. Retrieving results in pixel units will typically be slower. Also note that, in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, the Feret angles).

For more information, see [Camera calibration - overview](../C28_Calibration/S01_Calibration_overview.md).
