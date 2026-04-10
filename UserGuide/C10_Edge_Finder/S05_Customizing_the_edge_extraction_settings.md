---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Edge_Finder
section: Customizing_the_edge_extraction_settings
module_tag: edge
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / edge-finder / Customizing the edge extraction settings"
---

# Customizing the edge extraction settings

Once you have allocated an Edge Finder context with the right context type (using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md)), you can customize the image processing algorithm using [`MedgeControl`](../../Reference/edge/MedgeControl.md). For example, to fit the requirements of your application, you can select the appropriate:

- Edge extraction filter ([`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md)).
- Noise reduction factor ([`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md)).
- Sensitivity of the edgel detection ([`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md)).
- Accuracy of the edgel positions ([`M_ACCURACY`](../../Reference/edge/MedgeControl.md)).

You can also choose to fill in the edge gaps, which is the process of linking broken edges together based on a set of criteria ([`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) and [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md)).

Generally, the default control settings described in this section are sufficient for the majority of images. You should therefore adjust these settings only when dealing with very noisy images, extremely low or multi-contrast images, or images with very thin, refined features. In all cases, it is recommended that you try the default settings first and, if necessary, experiment with different settings to achieve the required edge map and accuracy.

For more advanced information on extracting edges, see [Advanced edge extraction](S08_Advanced_edge_extraction.md).

Note that the edge extraction process involves a denoising operation to even out rough edges and remove noise. The filter type, filter mode, kernel depth, noise reduction factor, and threshold mode all work in concert to control this operation, and ultimately determine which edges to extract. Changing any one of these factors will, to varying degrees, alter the edges that are extracted.

## Filter type

The edge extraction filter is used to compute the source image's spatial derivatives, which in turn are used to calculate the edge's magnitude (Gradient or Hessian) and angle. It also establishes the contribution of each neighboring pixel to the edge calculation. You can set the type of filter used when performing the edge extraction using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md). You can choose either an Infinite Impulse Response (IIR) edge extraction filter, or a Finite Impulse Response (FIR) edge extraction filter. The default filter type is the IIR filter, [`M_SHEN`](../../Reference/edge/MedgeControl.md).

IIR filters allow you to extract both crests and contours, while FIR filters only allow you to extract contours. IIR filters take many more settings into account than FIR filters do. For example, IIR filters also allow you to take advantage of Edge Finder's smoothing capabilities, while FIR filters ignore your smoothness setting.

See *Analyse d'images: filtrage et segmentation* (J.-P. Cocquerez and S. Philippe., 1995, Collection: Enseignement de la physique) for more information on IIR and FIR filters.

### Infinite Impulse Response (IIR) filters

Aurora Imaging Library provides you with the following IIR filters to extract edges, which you can set using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md):

- Shen-Castan ([`M_SHEN`](../../Reference/edge/MedgeControl.md)), which is an exponential weighting function, of the general form `_f(n)_ = Ke<sup>-a|_n_|</sup>`.
- Deriche ([`M_DERICHE`](../../Reference/edge/MedgeControl.md)), which is an exponential weighting function, of the general form `_f(n)_ = K(a|_n_| + 1)e<sup>-a|_n_|</sup>`.

These filters generate the weight values for neighboring pixels. In both cases, _K_ corresponds to a normalization coefficient and _a_ depends on the [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md) setting in [`MedgeControl`](../../Reference/edge/MedgeControl.md). Note that the general forms of these filters are not used directly; their first and second derivatives are used for contour and crest extraction, respectively.

The default filter, Shen-Castan, typically performs an effective edge extraction; it achieves a very good localization of edges, which makes it ideal for the majority of images.

However, Shen-Castan can sometimes be unexpectedly sensitive to noise, or can yield inappropriate results if you are extracting unusually thick crests. If this is the case, you should try Deriche, which is better suited to handle these situations, since it places more emphasis on an edge's neighbors than Shen-Castan. The following image illustrates how the first derivative distribution of values can affect results for both Shen-Castan and Deriche:

*[Image: ShenVsDeriche.png]*

### Finite Impulse Response (FIR) filters

The [`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md) setting in [`MedgeControl`](../../Reference/edge/MedgeControl.md) provides you with the following FIR filters to extract edges:

- Frei Chen ([`M_FREI_CHEN`](../../Reference/edge/MedgeControl.md)), which can be represented with the following convolution kernels:
  *[Image: FreiChenKernel.png]*
- Prewitt ([`M_PREWITT`](../../Reference/edge/MedgeControl.md)), which can be represented with the following convolution kernels:
  *[Image: PrewittKernel.png]*
- Sobel ([`M_SOBEL`](../../Reference/edge/MedgeControl.md)), which can be represented with the following convolution kernels:
  *[Image: SobelKernel.png]*

Note that, as mentioned, FIR filters only allow you to extract contours.

## Smoothing for IIR filters

The [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md) setting in [`MedgeControl`](../../Reference/edge/MedgeControl.md) allows you to control the degree of smoothness applied in the edge extraction. The smoothing operation evens out rough edges and reduces noise; in effect, the smoothness factor affects which edges are extracted.

The range of this control varies from 0 (no smooth) to 100 (a very strong smooth). The default setting is 50.0. Note that [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md) is only relevant if [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md) is set to [`M_DERICHE`](../../Reference/edge/MedgeControl.md) or [`M_SHEN`](../../Reference/edge/MedgeControl.md).

For recursive IIR filters, increasing the smoothness control value does not affect the processing time. A very high smoothing level can result in a loss of important detail and a decrease in precision. Note that, the range of smoothing is not linear; that is, the higher the smoothing factor, the greater the difference between settings. For example, changing the smoothing from 30.0 to 50.0 is less significant than changing the smoothing from 50.0 to 70.0.

The following animation demonstrates this non-linear change in smoothing:

*[Image: SmoothnessControl]*

## Thresholding

Edge Finder determines which edges are extracted from the source image based on a thresholding of the magnitude of each edgel. More specifically, a hysteresis thresholding is used to perform the edge extraction. With this type of thresholding, the extracted edge chains are built such that the magnitude values of all connected edgels are stronger than the lower bound threshold value ([`M_THRESHOLD_LOW`](../../Reference/edge/MedgeControl.md)) and at least one edgel in each edge chain has a magnitude that is stronger than the upper bound threshold value ([`M_THRESHOLD_HIGH`](../../Reference/edge/MedgeControl.md)). For more information on edgel magnitude, see [Magnitude type](S05_Customizing_the_edge_extraction_settings.md).

Thresholding can also be applied during post-calculation. In this case, threshold settings are applied to the Edge Finder result buffer, rather than the source image. Except for filtering operations, the edge extraction process is completely recalculated. As a result, all internal processing buffers ([`M_SAVE_...`](../../Reference/edge/MedgeControl.md)) must have been initially saved, using [`MedgeControl`](../../Reference/edge/MedgeControl.md). For more information on post-calculation, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

Edge Finder provides several predefined threshold modes that automatically select appropriate values for the lower and the upper bound thresholds, based on an internal image evaluation and noise estimation. To automatically determine the threshold settings with which to extract edges, call [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) set to one of the following: [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md), [`M_HIGH`](../../Reference/edge/MedgeControl.md), [`M_MEDIUM`](../../Reference/edge/MedgeControl.md), [`M_LOW`](../../Reference/edge/MedgeControl.md), or [`M_DISABLE`](../../Reference/edge/MedgeControl.md). Note that lower threshold modes result in a more sensitive edgel detection.

The default setting ([`M_HIGH`](../../Reference/edge/MedgeControl.md)) is typically sufficient since it offers a robust detection of pertinent edgels, even from images presenting some contrast variations, noise, and non-uniform illumination. However, for multi-contrast images, or for images with a lot of noise or non-uniform illumination, some edges can be missed. In these cases, the setting [`M_MEDIUM`](../../Reference/edge/MedgeControl.md) should be used. If all relevant edges are still not being extracted, then use [`M_LOW`](../../Reference/edge/MedgeControl.md), which will get all edges over a minimum noise-based estimated threshold. Finally, to extract all edgels in the image, use [`M_DISABLE`](../../Reference/edge/MedgeControl.md), which will set the lower and upper bound threshold values to 0. Note that [`M_LOW`](../../Reference/edge/MedgeControl.md) and [`M_DISABLE`](../../Reference/edge/MedgeControl.md) should be used carefully since a large number of unnecessary edgels might be extracted. For images that have strong contrast, little noise, and consistent illumination, the [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md) setting can be used, since it will extract only the strongest edges. Note that [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md) should also be used carefully since pertinent edgels might not be extracted.

*[Image: ThresholdControl.png]*

In the majority of cases, Edge Finder's predefined threshold modes should provide you with sufficient thresholding control. However, for advanced applications, you might want to explicitly set the threshold bounds. To do so, set [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) to [`M_USER_DEFINED`](../../Reference/edge/MedgeControl.md), and use the [`M_THRESHOLD_LOW`](../../Reference/edge/MedgeControl.md) and [`M_THRESHOLD_HIGH`](../../Reference/edge/MedgeControl.md) control types to specify the lower and upper threshold bounds, respectively. The default settings are 0.

> **Note:** If [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) is not set to [`M_USER_DEFINED`](../../Reference/edge/MedgeControl.md), your [`M_THRESHOLD_LOW`](../../Reference/edge/MedgeControl.md) and [`M_THRESHOLD_HIGH`](../../Reference/edge/MedgeControl.md) values have no effect.

Note that the threshold bounds must be provided as positive values. However, for line crests, Edge Finder will consider these values to be negative and/or positive, depending on whether the edges were extracted as darker (negative edgel values), lighter (positive edgel values), or both darker and lighter (negative and positive edgel values) than the image's background. For more information on extracting line crests, see [Object contours versus line crests](S04_Extracting_the_edges.md).

Advanced users might want to customize the behavior of the hysteresis thresholding; to do so, see [Advanced thresholding](S08_Advanced_edge_extraction.md).

Note that when interfacing with the Aurora Imaging Library Geometric Model Finder or Advanced Geometric Matcher module, you should use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) to set the threshold mode. Note that an [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) setting overrides an [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) setting. For more information, see [Interfacing with Geometric Model Finder or Advanced Geometric Matcher](S10_Interfacing_with_the_Geometric_Model_Finder_module.md).

See *Edges: Saliency measures and automatic thresholding* (P. L. Rosin, 1995, Institute for Remote Sensing Applications) for more information on automatic robust thresholding techniques for edge extraction.

## Edgel accuracy

Edgels have a position and an angle. The [`M_ACCURACY`](../../Reference/edge/MedgeControl.md) and the[`M_ANGLE_ACCURACY`](../../Reference/edge/MedgeControl.md) settings in [`MedgeControl`](../../Reference/edge/MedgeControl.md) determine the accuracy used to estimate edgel positions and angles.

[`M_ACCURACY`](../../Reference/edge/MedgeControl.md) determines edgel positional accuracy, and can be set to one of the following: [`M_HIGH`](../../Reference/edge/MedgeControl.md), [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md), or [`M_DISABLE`](../../Reference/edge/MedgeControl.md). The default setting is [`M_HIGH`](../../Reference/edge/MedgeControl.md). When you set the accuracy to high, edgel positions are calculated with subpixel accuracy, which is typically sufficient. When you set the accuracy to very high, a mathematical model is used to compensate for the deformation introduced by the square shape of each pixel, resulting in very precise subpixel edgel accuracy. If you want to disable subpixel accuracy and calculate edgels with pixel precision, set [`M_ACCURACY`](../../Reference/edge/MedgeControl.md) to [`M_DISABLE`](../../Reference/edge/MedgeControl.md).

[`M_ANGLE_ACCURACY`](../../Reference/edge/MedgeControl.md) determines the precision with which to estimate edgel angles. By default, Aurora Imaging Library uses a high degree of accuracy ([`M_HIGH`](../../Reference/edge/MedgeControl.md)), which establishes angles in increments of 360/256 degrees. To speed up the calculation, you can specify a lower degree of accuracy ([`M_LOW`](../../Reference/edge/MedgeControl.md)), which establishes angles in increments of 45 degrees.

By setting [`M_ACCURACY`](../../Reference/edge/MedgeControl.md) to [`M_HIGH`](../../Reference/edge/MedgeControl.md) and [`M_ANGLE_ACCURACY`](../../Reference/edge/MedgeControl.md) to [`M_LOW`](../../Reference/edge/MedgeControl.md), Edge Finder will compute edgel positions with subpixel accuracy and edgel angles to the closest 45 degree increment. This results in a good estimate of the orientation of the maximum gradient with only a slight reduction in computation time.

Edgel accuracy varies with the image's dynamic range, sharpness, noise, and, to a lesser degree, with the orientation of the edge in the image. The best accuracy is achieved in well-contrasted, noise-free images. Theoretically, Edge Finder can provide a subpixel edgel location accuracy of up to 1/128th of a pixel. However, in real situations, it is reasonable to expect an accuracy from 1/10th of a pixel to 1/40th of a pixel, depending on physical limitations due to the transition's dynamic range and the image's noise.

You can notice the accuracy of the edgels if you draw the edges of a zoomed region of the source image. To do so, call [`MgraControl`](../../Reference/gra/MgraControl.md) with appropriate [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md),[`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md),[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and[`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) values, then call [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) with [`M_DRAW_EDGE`](../../Reference/edge/MedgeDraw.md).

*[Image: AccuracyControl.png]*

See *A Fast and Efficient Subpixelic Edge Detector* (F. Devernay, 1993, INRIA. in Quatriemes Journees ORASIS (CNRS)) for more information on extracting the subpixel locations of edgels.

## Magnitude type

You can specify the type of magnitude used to determine edgel positions using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md). The magnitude type can be set to either [`M_NORM`](../../Reference/edge/MedgeControl.md), where the gradient magnitude is used, or [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md), where the square of the gradient magnitude is used. The default setting is [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md).

Since [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md) uses the square of the gradient magnitude, it is faster, though less precise than [`M_NORM`](../../Reference/edge/MedgeControl.md). Typically, [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md) is used since it preserves a very good edgel location accuracy. Note that changing [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md) can slightly modify the resulting edge map.

## Filling the edge gaps

Edge Finder allows you to fill gaps between edges; that is, it allows you to automatically link the extremities of non-closed edges together, depending on their relative positions and orientations. Unexpected broken edges can occur when dealing with noisy images, and connecting them can significantly improve the quality of your results. To help you choose which edges can be linked, Edge Finder provides a series of constraints, described below. Note that edges will only be linked if they adhere to each constraint.

> **Note:** Filling edge gaps can also be done in post-calculation; however, there are some restrictions that must be taken into account. For more information, see [Post-calculation](S07_Calculating_and_retrieving_results.md).

### Candidate constraint

You can choose whether to join an edge extremity with the other extremity of the same edge, or with an extremity of any edge. To connect an edge's extremity with the extremity of any edge, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILL_GAP_CANDIDATE`](../../Reference/edge/MedgeControl.md) set to [`M_ANY`](../../Reference/edge/MedgeControl.md). This is the default value.

To connect the edge extremities of the same edge, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILL_GAP_CANDIDATE`](../../Reference/edge/MedgeControl.md) set to [`M_SAME`](../../Reference/edge/MedgeControl.md). This is the same as closing an open edge. The following animation illustrates candidate constraints.

*[Image: Filling_Edge_Gaps_Candidate]*

### Region constraint

The [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) and [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md) settings in [`MedgeControl`](../../Reference/edge/MedgeControl.md) define the region where two edge extremities can be linked. That is, when searching for edge extremity candidates to fill edge gaps, [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) sets the maximum distance radius, while [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md) sets the aperture angle; the origin of the aperture angle is the tangent line to the line segment at the point of interest. Note that two edge extremities can only be linked if both meet the [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) and [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md) criteria, and if both are included in the search region of the other. The following animation illustrates region constraints.

*[Image: Filling_Edge_Gaps_Region]*

The gap distance ([`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md)) must be specified in pixel units. The gap distance should be set carefully and large values should generally be avoided, otherwise the wrong edges can be connected. Typically, distance values should not exceed 5 pixels; however, if necessary, you can also specify no distance constraint ([`M_INFINITE`](../../Reference/edge/MedgeControl.md)). The default [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) value is 0 (no edge linking) and the default [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md) value is 360 degrees (all angles).

### Polarity constraint

When searching for edge extremity candidates, you can also use the [`M_FILL_GAP_POLARITY`](../../Reference/edge/MedgeControl.md) control type to specify that only edges with a certain polarity can be linked. The edge's polarity indicates whether edges were established as a transition from light to dark, or vice versa. Two adjacent edgels will have "same polarity" if their angles are within a range of 180 degrees, and "reverse polarity" if their angles are outside the range of 180 degrees. By default ([`M_ANY`](../../Reference/edge/MedgeControl.md)), all edges that meet the distance and angle constraints are considered candidates, regardless of the edge's polarity. If [`M_FILL_GAP_POLARITY`](../../Reference/edge/MedgeControl.md) is set to [`M_SAME`](../../Reference/edge/MedgeControl.md), only edges with the same polarity are considered potential candidates. If [`M_FILL_GAP_POLARITY`](../../Reference/edge/MedgeControl.md) is set to [`M_REVERSE`](../../Reference/edge/MedgeControl.md), only edges with reverse polarity are considered potential candidates.

Note that the [`M_SAME`](../../Reference/edge/MedgeControl.md) and [`M_REVERSE`](../../Reference/edge/MedgeControl.md) polarity settings are not available for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. The following animation illustrates polarity constraints.

*[Image: Filling_Edge_Gaps_Polarity]*

### Continuity constraint

It is possible that multiple edges meet the distance ([`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md)), angle ([`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md)), and polarity ([`M_FILL_GAP_POLARITY`](../../Reference/edge/MedgeControl.md)) settings. When this occurs, you must decide which edges should be linked. To help choose, Edge Finder provides a continuity constraint.

The continuity constraint allows you to decide whether the candidate chosen to fill the edge gap is selected according to its proximity or its maximum continuity. To set the continuity constraint, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILL_GAP_CONTINUITY`](../../Reference/edge/MedgeControl.md). The range of this control varies from 0 to 100. When set to 0, the closest edge extremity is chosen to link edges together. When set to 100, the edge that gives the most continuous result (minimum curvature) is chosen. The default value is 50. In the following example, a candidate must be chosen to fill the gap with edge 1. If the continuity constraint is set to 100, edge 2 will be selected instead of edge 3; even though edge 3 is closer, edge 2 is more continuous. The following animation illustrates continuity constraints.

*[Image: Filling_Edge_Gaps_Continuity]*
