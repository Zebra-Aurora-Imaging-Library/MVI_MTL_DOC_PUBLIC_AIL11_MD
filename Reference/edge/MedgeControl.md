---
doctype: Reference
module: edge
function: MedgeControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeControl"
---

# MedgeControl

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Control an Edge Finder context or an Edge Finder result buffer setting.

## Syntax

```c
void MedgeControl(
    AIL_ID     ContextOrResultId,  //out
    AIL_INT64  ControlType,        //in
    AIL_DOUBLE ControlValue        //in
)
```

## Description

This function sets the specified control for either an Edge Finder context or an Edge Finder result buffer. For Edge Finder contexts, these settings control the execution of [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) operations and select which edge features [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) should calculate. For Edge Finder result buffers, these settings control the post manipulation of results. For example, to draw a zoomed region of the source image that was used to calculate results, the drawing control values must be appropriately set. Similarly, to select edges based on the proximity of an edge or edges to a specified point, [`M_NEAREST_NEIGHBOR_RADIUS`](../../Reference/edge/MedgeControl.md) must be appropriately set. For more information, see [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) or [`MedgeSelect`](../../Reference/edge/MedgeSelect.md).

All control settings can typically be inquired with [`MedgeInquire`](../../Reference/edge/MedgeInquire.md).

For new context settings to take effect, you must calculate the settings, using [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).

Note that some control settings have post-calculation restrictions. For more information, see [Post-calculation](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md).

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies either the Edge Finder context or the Edge Finder result buffer whose settings you want to modify. The Edge Finder context or the Edge Finder result buffer must have been previously allocated on the required system using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) or [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md), respectively.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### For operation settings for object contours and line crests

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used to control the Edge Finder context operation settings and can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, unless otherwise specified.

---

### `M_ACCURACY`

Sets the edgel accuracy of the edge extraction. Accuracy depends on the image's dynamic range, sharpness, and noise. The best accuracy is achieved in well-contrasted noise-free images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that edgel accuracy will be disabled. Edgels will be calculated with pixel accuracy. |
| `M_HIGH` *(default)* | Specifies high accuracy. Edgels will be calculated with subpixel accuracy. |
| `M_VERY_HIGH` | Specifies very high accuracy. Edgels will be calculated with very precise subpixel accuracy. [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md) uses a classical camera model to compensate for any pixel-distortion aberration. |

---

### `M_ANGLE_ACCURACY`

Sets the precision with which to estimate edgel angles when extracting edges.  [`M_ANGLE_ACCURACY`](../../Reference/edge/MedgeControl.md) can only be used with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies high precision. The angle estimation is performed in increments of 360/256 degrees. |
| `M_LOW` | Specifies low precision. The angle estimation is performed in increments of 45 degrees. |

---

### `M_CHAIN_ALL_NEIGHBORS`

Sets how edge chains are built. Edge chains are built using an 8-connected lattice.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that edge chains are built with the least amount of edgel information possible.  *[Image: chainneighborsdisable.png]* |
| `M_ENABLE` | Specifies that edge chains are built with as much edgel information as possible.  Enabling [`M_CHAIN_ALL_NEIGHBORS`](../../Reference/edge/MedgeControl.md) can result in slightly longer calculations, however, the edge chain will contain more edgel information.  *[Image: chainneighborsenable.png]* |

---

### `M_DETAIL_LEVEL`

Sets the level of details to extract from the image. The detail level determines what is considered an edge/background. A higher detail level will include more edges than a lower detail level.  Essentially, [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) sets the threshold mode of the Edge Finder context. Note that an [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) setting overrides an [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) setting.  Typically, [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) is used when interfacing with the Aurora Imaging Library Geometric Model Finder module. Otherwise, [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) should be used instead.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` | Sets the detail level to high. |
| `M_MEDIUM` *(default)* | Sets the detail level to medium. |
| `M_VERY_HIGH` | Sets the detail level to very high. |

---

### `M_EXTRACTION_SCALE`

Sets the scale of the image at which to do the edge extraction. Once the extraction is complete, the results are scaled to the original scale of the image.  An extraction scale less than one speeds up the calculation or the search but can result in a less reliable result, including, the loss of important details and/or a reduction in the accuracy of the search results.  [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) is for advanced users of the EdgeFinder module. The default setting usually provides the most accurate search results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the extraction scale. |

---

### `M_FILTER_SMOOTHNESS`

Sets the degree of smoothness (strength of denoising) applied by the filter during the neighborhood operation.  [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md) only has an effect if [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_TYPE`](../../Reference/edge/MedgeControl.md) is set to [`M_DERICHE`](../../Reference/edge/MedgeControl.md) or [`M_SHEN`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value.  A value of 100.0 results in a strong noise reduction effect, while a value of 0.0 has almost no noise reduction effect. |

---

### `M_FILTER_TYPE`

Sets the type of filter used when performing the neighborhood operation used to extract edges. The type of filter determines the distribution of the neighborhoods' influence.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DERICHE` | Specifies a Deriche infinite support filter. This is an exponential weighting function, of the general form:  *[Image: DericheFunction.png]*  Deriche is an Infinite Impulse Response (IIR) filter.  For the Deriche filter, the neighborhoods' influence decreases much slower as the distance from the central pixel increases, compared to the Shen-Castan filter ([`M_SHEN`](../../Reference/edge/MedgeControl.md)).  [`M_DERICHE`](../../Reference/edge/MedgeControl.md) can be used with both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Typically, [`M_DERICHE`](../../Reference/edge/MedgeControl.md) is used for unusually thick crests.  [`M_DERICHE`](../../Reference/edge/MedgeControl.md) allows you to use Edge Finder's smoothing capabilities. To do so, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md). |
| `M_FREI_CHEN` | Specifies a Frei Chen filter. This is a Finite Impulse Response (FIR) filter that can be represented with the following convolution kernels:  *[Image: FreiChenKernel.png]*  [`M_FREI_CHEN`](../../Reference/edge/MedgeControl.md) can only be used with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Also, when using [`M_FREI_CHEN`](../../Reference/edge/MedgeControl.md), you cannot smooth images using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md). |
| `M_PREWITT` | Specifies a Prewitt filter. This is a Finite Impulse Response (FIR) filter that can be represented with the following convolution kernels:  *[Image: PrewittKernel.png]*  [`M_PREWITT`](../../Reference/edge/MedgeControl.md) can only be used with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Also, when using [`M_PREWITT`](../../Reference/edge/MedgeControl.md), you cannot smooth images using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md). |
| `M_SHEN` *(default)* | Specifies a Shen-Castan infinite support exponential filter. This is an exponential weighting function, of the general form:  *[Image: ShenFunction.png]*  Shen-Castan is an Infinite Impulse Response (IIR) filter.  For the Shen-Castan filter, the neighborhoods' influence decreases much faster as the distance from the central pixel increases, compared to the Deriche filter ([`M_DERICHE`](../../Reference/edge/MedgeControl.md)).  [`M_SHEN`](../../Reference/edge/MedgeControl.md) can be used with both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Typically, [`M_SHEN`](../../Reference/edge/MedgeControl.md) performs an excellent edge extraction on most images; however, if you are extracting unusually thick crests that yield inappropriate results, you should use [`M_DERICHE`](../../Reference/edge/MedgeControl.md).  [`M_SHEN`](../../Reference/edge/MedgeControl.md) allows you to use Edge Finder's smoothing capabilities. To do so, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md). |
| `M_SOBEL` | Specifies a Sobel filter. This is a Finite Impulse Response (FIR) filter that can be represented with the following convolution kernels:  *[Image: SobelKernel.png]*  [`M_SOBEL`](../../Reference/edge/MedgeControl.md) can only be used with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Also, when using [`M_SOBEL`](../../Reference/edge/MedgeControl.md), you cannot smooth images using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_FILTER_SMOOTHNESS`](../../Reference/edge/MedgeControl.md). |

---

### `M_FLOAT_MODE`

Sets whether to force all the processing of edge extraction operations to be performed using floating-point precision calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edge extractions are not forced to be performed using floating-point precision calculations. |
| `M_ENABLE` | Specifies that all edge extractions are forced to be performed using floating-point precision calculations. |

---

### `M_MAGNITUDE_TYPE`

Sets how to calculate the magnitude of the edge at each edgel position.  Note that for [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the magnitude is the norm of the gradient vector at the edgel position. For [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the magnitude is equal to the maximum eigenvalue of the Hessian matrix at the edgel position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value.  For [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the default is [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md). For [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the default is [`M_NORM`](../../Reference/edge/MedgeControl.md). |
| `M_NORM` | Specifies that the magnitude will be used. |
| `M_SQR_NORM` | Specifies that the square of the magnitude will be used.  This value optimizes the edge extraction operation while still preserving a very good edgel position accuracy. [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md) is calculated faster than [`M_NORM`](../../Reference/edge/MedgeControl.md), though it is less accurate. |

---

### `M_OVERSCAN`

Sets the type of overscan used to handle the source image's bordering pixels. Note that [`M_OVERSCAN`](../../Reference/edge/MedgeControl.md) is ignored if using an IIR filter (e.g. Shen and Deriche filter types).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that no overscan will be used, unless processing the border pixels is faster than ignoring them; in the latter case, Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified operation and the target system. |
| `M_MIRROR` *(default)* | Specifies that the border pixels of a source image are processed using overscan pixel values that mirror the source buffer pixel values. That is, the overscan pixel values will be a mirror copy of the source buffer's borders. For example:  *[Image: KernelOverscan.png]* |
| `M_REPLACE` | Specifies that the border pixels of a source image are processed using overscan pixel values set to the overscan replacement value ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeControl.md)). |
| `M_TRANSPARENT` | Specifies that the border pixels of a source image are processed using transparent overscan pixel values. That is, the overscan pixel values will be those of the parent buffer. If they are not available, a mirror type overscan is used instead. |

---

### `M_OVERSCAN_REPLACE_VALUE`

Sets a replacement value for the overscan pixel values. Note that [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeControl.md) is ignored unless [`M_OVERSCAN`](../../Reference/edge/MedgeControl.md) with [`MedgeControl`](../../Reference/edge/MedgeControl.md) is set to [`M_REPLACE`](../../Reference/edge/MedgeControl.md). In addition, [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeControl.md) is ignored if using an IIR filter (e.g. Shen and Deriche filter types).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REPLACE_MAX` | Specifies that the overscan neighborhood pixel values will be set to the maximum value of the source image buffer. |
| `M_REPLACE_MIN` | Specifies that the overscan neighborhood pixel values will be set to the minimum value of the source image buffer. |
| `Value` *(default)* | Specifies the value of the overscan neighborhood pixels. |

---

### `M_THRESHOLD_HIGH`

Sets the upper bound of the hysteresis threshold value. [`M_THRESHOLD_HIGH`](../../Reference/edge/MedgeControl.md) is ignored unless [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) is set to [`M_USER_DEFINED`](../../Reference/edge/MedgeControl.md). Note that lower threshold values result in a more sensitive edgel detection; that is, a higher (or equal) number of edgels are detected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the upper bound of the hysteresis threshold. |

---

### `M_THRESHOLD_LOW`

Sets the lower bound of the hysteresis threshold value. [`M_THRESHOLD_LOW`](../../Reference/edge/MedgeControl.md) is ignored unless [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) is set to [`M_USER_DEFINED`](../../Reference/edge/MedgeControl.md). Note that lower threshold values result in a more sensitive edgel detection; that is, a higher (or equal) number of edgels are detected.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the lower bound of the hysteresis threshold. |

---

### `M_THRESHOLD_MODE`

Sets the threshold of the edge extraction. The Edge Finder module uses a classical hysteresis threshold to extract relevant edges in the image. Threshold values can either be set manually or determined automatically by the Edge Finder module. Note that lower threshold values result in a more sensitive edgel detection; that is, a higher (or equal) number of edgels are detected.  When interfacing with the Aurora Imaging Library Geometric Model Finder module, you should use [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) to set the threshold mode. Note that an [`M_DETAIL_LEVEL`](../../Reference/edge/MedgeControl.md) setting overrides an [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies no threshold. All edges are extracted. |
| `M_HIGH` *(default)* | Specifies a high threshold. [`M_HIGH`](../../Reference/edge/MedgeControl.md) should be used for images with some contrast variations, noise, and non-uniform illumination. [`M_HIGH`](../../Reference/edge/MedgeControl.md) always results in a lower (or equal) number of edgels than [`M_MEDIUM`](../../Reference/edge/MedgeControl.md). |
| `M_LOW` | Specifies a low threshold. All edges over a minimum noise-based estimated threshold are extracted. [`M_LOW`](../../Reference/edge/MedgeControl.md) always results in a lower (or equal) number of edgels than [`M_DISABLE`](../../Reference/edge/MedgeControl.md). |
| `M_MEDIUM` | Specifies a medium threshold. [`M_MEDIUM`](../../Reference/edge/MedgeControl.md) should be used for multi-contrast images, or for images with a lot of noise or non-uniform illumination. [`M_MEDIUM`](../../Reference/edge/MedgeControl.md) always results in a lower (or equal) number of edgels than [`M_LOW`](../../Reference/edge/MedgeControl.md). |
| `M_USER_DEFINED` | Specifies that the threshold values will be user-defined. Set the threshold values with [`M_THRESHOLD_LOW`](../../Reference/edge/MedgeControl.md) and [`M_THRESHOLD_HIGH`](../../Reference/edge/MedgeControl.md). |
| `M_VERY_HIGH` | Specifies a very high threshold. Only the strongest edges in the image are extracted. [`M_VERY_HIGH`](../../Reference/edge/MedgeControl.md) always results in a lower (or equal) number of edgels than [`M_HIGH`](../../Reference/edge/MedgeControl.md). |

---

### `M_THRESHOLD_TYPE`

Sets the type of hysteresis threshold used when performing the edge extraction.  Edge chains are built such that the magnitude values of all connected edgels are stronger than a lower bound threshold value and such that at least one of the edgel's magnitude in each edge chain is stronger than a upper bound threshold value.  Note that [`M_THRESHOLD_TYPE`](../../Reference/edge/MedgeControl.md) sets how to use the lower and upper threshold bounds, while [`M_THRESHOLD_MODE`](../../Reference/edge/MedgeControl.md) defines what the bounds are.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FULL_HYSTERESIS` | Specifies that the lower bound threshold value is 0. |
| `M_HYSTERESIS` *(default)* | Specifies that both the lower bound threshold value and the upper bound threshold value will be used. |
| `M_NO_HYSTERESIS` | Specifies that the lower bound threshold value is equal to the upper bound threshold value. |

---

### `M_TIMEOUT`

Sets the maximum edge extraction and calculation time for [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of edge extraction and calculation time. |
| `Value > 0` | Specifies the maximum edge extraction and calculation time, in msec. |

### For operation settings for line crests

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) settings are used to control the Edge Finder context operation settings and can only be specified for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_FOREGROUND_VALUE`

Sets the color of the line crests to extract from the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies that the line crests are both lighter and darker than the image's background color. This corresponds to both valley-like and ridge-like lines extracted from the image. |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that the line crests are darker than the image's background color. This corresponds to valley-like lines extracted from the image (a valley-like Gaussian profile). |
| `M_FOREGROUND_WHITE` | Specifies that the line crests are lighter than the image's background color. This corresponds to ridge-like lines extracted from the image (a ridge-like Gaussian profile). |

### For saving internal buffers in the Edge Finder result buffer

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used to save internal buffers in the Edge Finder result buffer and can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_SAVE_ANGLE`

Sets whether the internal angle buffer, used when extracting edges, is saved in the Edge Finder result buffer. Note that the angle value is not meaningful in the whole image; it is only meaningful for edges whose magnitude value is above the lower bound threshold value.  Note that the angle is measured between the horizontal axis and the perpendicular direction of the edge chain at each edgel location. For more information on angle convention in Aurora Imaging Library, see [Internal processing buffers](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle buffer will not be saved. |
| `M_ENABLE` | Specifies that the angle buffer will be saved. |

---

### `M_SAVE_CHAIN_ANGLE`

Sets whether the angle value of the edge at each edgel position is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle values will not be saved. |
| `M_ENABLE` | Specifies that the angle values will be saved. |

---

### `M_SAVE_CHAIN_MAGNITUDE`

Sets whether the magnitude value of the edge at each edgel position is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the magnitude values will not be saved. |
| `M_ENABLE` | Specifies that the magnitude values will be saved. |

---

### `M_SAVE_DERIVATIVES`

Sets whether the internal derivative buffers used when extracting edges are saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the derivative buffers will not be saved. |
| `M_ENABLE` | Specifies that the derivative buffers will be saved. |

---

### `M_SAVE_IMAGE`

Sets whether the source image is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the image will not be saved. |
| `M_ENABLE` | Specifies that the image will be saved. |

---

### `M_SAVE_MAGNITUDE`

Sets whether the internal magnitude buffer, used when extracting edges, is saved in the Edge Finder result buffer. For [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the magnitude is the norm of the gradient vector at the edgel position. For [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, the magnitude is equal to the maximum eigenvalue of the Hessian matrix at the edgel position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the magnitude buffer will not be saved. |
| `M_ENABLE` | Specifies that the magnitude buffer will be saved. |

---

### `M_SAVE_MASK`

Sets whether the mask buffer is saved in the Edge Finder result buffer. For more information, see [`MedgeMask`](../../Reference/edge/MedgeMask.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the mask will not be saved. |
| `M_ENABLE` | Specifies that the mask will be saved. |

### For calculating edge features

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used to control the edge features calculated for each edge and can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_AVERAGE_STRENGTH`

Sets whether to calculate the average strength of each edge. This is the average energy of each edge, which is defined as the energy of the edge ([`M_STRENGTH`](../../Reference/edge/MedgeControl.md)) divided by its number of edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average strength will not be calculated. |
| `M_ENABLE` | Specifies that the average strength will be calculated. |

---

### `M_BOX_X_MAX`

Sets whether to calculate the extreme right edgel coordinate of each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme right edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme right edgel coordinate will be calculated. |

---

### `M_BOX_X_MIN`

Sets whether to calculate the extreme left edgel coordinate of each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme left edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme left edgel coordinate will be calculated. |

---

### `M_BOX_Y_MAX`

Sets whether to calculate the extreme bottom edgel coordinate of each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme bottom edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme bottom edgel coordinate will be calculated. |

---

### `M_BOX_Y_MIN`

Sets whether to calculate the extreme top edgel coordinate of each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme top edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme top edgel coordinate will be calculated. |

---

### `M_CENTER_OF_GRAVITY_X`

Sets whether to calculate the X-position of each edge's center of gravity. This is equal to the average position of edgel positions in X.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the X-position of the center of gravity will be calculated. |

---

### `M_CENTER_OF_GRAVITY_Y`

Sets whether to calculate the Y-position of each edge's center of gravity. This is equal to the average position of edgel positions in Y.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position of the center of gravity will be calculated. |

---

### `M_CIRCLE_FIT_CENTER_X`

Sets whether to calculate the X-coordinate of the center of the circle that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |

---

### `M_CIRCLE_FIT_CENTER_Y`

Sets whether to calculate the Y-coordinate of the center of the circle that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |

---

### `M_CIRCLE_FIT_COVERAGE`

Sets whether to calculate the coverage of the circle that is the best fit for each edge. The circle fit coverage indicates what angular fraction of the fitted circle is subtended by the radii going to the endpoints of the edge. If the edge is closed, the coverage will be 1. For a quarter circle the coverage would be 0.25. A perfectly straight line has a coverage of 0, it doesn't subtend much of an infinitely large circle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the circle will be calculated. |

---

### `M_CIRCLE_FIT_ERROR`

Sets whether to calculate the fit error of the circle that is the best fit for each edge. This is calculated as the average quadratic error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the circle will be calculated. |

---

### `M_CIRCLE_FIT_RADIUS`

Sets whether to calculate the radius of the circle that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the radius will not be calculated. |
| `M_ENABLE` | Specifies that the radius will be calculated. |

---

### `M_CLOSURE`

Sets whether to calculate the closure state of each edge. The closure state identifies whether the edge forms a closed chain.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge's closure state will not be calculated. |
| `M_ENABLE` | Specifies that the edge's closure state will be calculated. |

---

### `M_CONVEX_PERIMETER`

Sets whether to calculate the convex elongation of each edge. This is an approximation of the perimeter of the convex hull of an edge. It is derived from several Feret diameters; therefore, a larger number of Ferets gives a more accurate result (see [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the convex elongation will not be calculated. |
| `M_ENABLE` | Specifies that the convex elongation will be calculated. |

---

### `M_ELLIPSE_FIT_ANGLE`

Sets whether to calculate the angle of the ellipse that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle will not be calculated. |
| `M_ENABLE` | Specifies that the angle will be calculated. |

---

### `M_ELLIPSE_FIT_CENTER_X`

Sets whether to calculate the X-coordinate of the center of the ellipse that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |

---

### `M_ELLIPSE_FIT_CENTER_Y`

Sets whether to calculate the Y-coordinate of the center of the ellipse that is the best fit for each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |

---

### `M_ELLIPSE_FIT_COVERAGE`

Sets whether to calculate the coverage of the ellipse that is the best fit for each edge. The coverage describes the portion of the ellipse covered by the edge. The value returned is between 0.0 and 1.0, inclusive, where 0.0 equals no coverage, 0.5 equals 50 percent coverage, and 1.0 equals 100 percent coverage.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the ellipse will be calculated. |

---

### `M_ELLIPSE_FIT_ERROR`

Sets whether to calculate the fit error of the ellipse that is the best fit for each edge. This is calculated as the average quadratic error.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the ellipse will be calculated. |

---

### `M_ELLIPSE_FIT_MAJOR_AXIS`

Sets whether to calculate the major axis of the ellipse that is the best fit for each edge. The major axis is the line passing through the foci, center, and vertices of an ellipse. It is also the principal axis of symmetry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the major axis will not be calculated. |
| `M_ENABLE` | Specifies that the major axis will be calculated. |

---

### `M_ELLIPSE_FIT_MINOR_AXIS`

Sets whether to calculate the minor axis of the ellipse that is the best fit for each edge. The minor axis is the line through the center of an ellipse that is perpendicular to the major axis. The minor axis is an axis of symmetry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minor axis will not be calculated. |
| `M_ENABLE` | Specifies that the minor axis will be calculated. |

---

### `M_FAST_LENGTH`

Sets whether to calculate a coarse approximation for the length of each edge. [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md) gives a less accurate but faster approximation of the edge's length than [`M_LENGTH`](../../Reference/edge/MedgeControl.md). [`M_FAST_LENGTH`](../../Reference/edge/MedgeControl.md) is equal to:  *[Image: medgeFastLengthDef.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that a fast length will not be calculated. |
| `M_ENABLE` | Specifies that a fast length will be calculated. |

---

### `M_FERET_ELONGATION`

Sets whether to calculate the Feret elongation of each edge. This is a measure of the shape of each edge. It is equal to [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md) / [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md). It is accurate for reasonably compact objects, but becomes less accurate for very elongated objects (because [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md) becomes less accurate).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret elongation will not be calculated. |
| `M_ENABLE` | Specifies that the Feret elongation will be calculated. |

---

### `M_FERET_GENERAL`

Sets whether to calculate the general Feret of each edge. This is the Feret diameter calculated at [`M_FERET_GENERAL_ANGLE`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the general Feret will not be calculated. |
| `M_ENABLE` | Specifies that the general Feret will be calculated. |

---

### `M_FERET_MAX_ANGLE`

Sets whether to calculate the maximum Feret angle of each edge. This is the angle at which the maximum Feret diameter is found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret angle will be calculated. |

---

### `M_FERET_MAX_DIAMETER`

Sets whether to calculate the maximum Feret diameter of each edge. This is the largest Feret diameter found after checking a certain number of angles (see [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md)). More angles will give a more accurate result, but will take longer to calculate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret diameter will be calculated. |

---

### `M_FERET_MEAN_DIAMETER`

Sets whether to calculate the average Feret diameter at all the angles checked. See [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md) for more information.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the average Feret diameter will be calculated. |

---

### `M_FERET_MIN_ANGLE`

Sets whether to calculate the minimum Feret angle of each edge. This is the angle at which the minimum Feret diameter is found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret angle will be calculated. |

---

### `M_FERET_MIN_DIAMETER`

Sets whether to calculate the minimum Feret diameter of each edge. This is the smallest Feret diameter found after checking a certain number of angles (see [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md)). More angles will give a more accurate result, but will take longer to calculate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret diameter will be calculated. |

---

### `M_FERET_X`

Sets whether to calculate the X-Feret value of each edge. This is the dimension of the minimum bounding box of an edge in the horizontal direction; that is, [`M_BOX_X_MAX`](../../Reference/edge/MedgeControl.md)   - [`M_BOX_X_MIN`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the X-Feret value will be calculated. |

---

### `M_FERET_Y`

Sets whether to calculate the Y-Feret value of each edge. This is the dimension of the minimum bounding box of an edge in the vertical direction; that is, [`M_BOX_Y_MAX`](../../Reference/edge/MedgeControl.md)   - [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the Y-Feret value will be calculated. |

---

### `M_FIRST_POINT_X`

Sets whether to calculate the X-coordinate of each edge's first point (starting point). Together with [`M_FIRST_POINT_Y`](../../Reference/edge/MedgeControl.md), these values define a unique point for each edge, which is always the first point of each edge chain.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's X-coordinate will be calculated. |

---

### `M_FIRST_POINT_Y`

Sets whether to calculate the Y-coordinate of each edge's first point (starting point). Together with [`M_FIRST_POINT_X`](../../Reference/edge/MedgeControl.md), these values define a unique point for each edge, which is always the first point of each edge chain.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's Y-coordinate will be calculated. |

---

### `M_LABEL_VALUE`

Sets whether to calculate the label value of each edge in an image. The label value is a positive integer greater or equal to one; each edge in an image has a unique label value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the label value will not be calculated. |
| `M_ENABLE` *(default)* | Specifies that the label value will be calculated. |

---

### `M_LENGTH`

Sets whether to calculate the length of each edge. [`M_LENGTH`](../../Reference/edge/MedgeGetResult.md) gives a more accurate but slower approximation of the edge's length than [`M_FAST_LENGTH`](../../Reference/edge/MedgeGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the length of each edge will not be calculated. |
| `M_ENABLE` | Specifies that the length of each edge will be calculated. |

---

### `M_LINE_FIT_A`

Sets whether to calculate the coefficient_A_ of the line that is the best fit for each edge. The line fit approximation is based on the following equation: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _A_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _A_ will be calculated. |

---

### `M_LINE_FIT_B`

Sets whether to calculate the coefficient _B_of the line that is the best fit for each edge. The line fit approximation is based on the following equation: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _B_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _B_ will be calculated. |

---

### `M_LINE_FIT_C`

Sets whether to calculate the coefficient _C_ of the line that is the best fit for each edge. The line fit approximation is based on the following equation: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _C_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _C_ will be calculated. |

---

### `M_LINE_FIT_ERROR`

Sets whether to calculate the fit error of the line that is the best fit for each edge. This is calculated as the average quadratic error. The line fit approximation is based on the following equation: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error will not be calculated. |
| `M_ENABLE` | Specifies that the fit error will be calculated. |

---

### `M_MOMENT_ELONGATION`

Sets whether to calculate the moment elongation of each edge. [`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md) specifies the ratio of the principal values of the edge's inertial matrix, which corresponds to the principal directions of the edge shape. This can be approximately defined as the ratio between the edge's minimum and maximum moment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the moment elongation will be calculated. |

---

### `M_MOMENT_ELONGATION_ANGLE`

Sets whether to calculate the angle of the principal axis along each edge's moment elongation ([`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle of the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the angle of the moment elongation will be calculated. |

---

### `M_POSITION_X`

Sets whether to calculate the X-position of each edge. The position of the edge is defined by the middle edgel of the edge chain.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position will not be calculated. |
| `M_ENABLE` | Specifies that the X-position will be calculated. |

---

### `M_POSITION_Y`

Sets whether to calculate the Y-position of each edge. The position of the edge is defined by the middle edgel of the edge chain.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position will be calculated. |

---

### `M_SIZE`

Sets whether to calculate the number of edgels of each edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the number of edgels will not be calculated. |
| `M_ENABLE` | Specifies that the number of edgels will be calculated. |

---

### `M_STRENGTH`

Sets whether to calculate the strength of each edge. This is the energy of each edge, which is defined as the sum of the square of the edgels' magnitude values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the strength will not be calculated. |
| `M_ENABLE` | Specifies that the strength will be calculated. |

---

### `M_TORTUOSITY`

Sets whether to calculate the tortuosity measure of each edge. The tortuosity measure is equal to the diagonal length of the edge's bounding box ([`M_BOX`](../../Reference/edge/MedgeControl.md)), divided by the length of the edge ([`M_LENGTH`](../../Reference/edge/MedgeControl.md)). Therefore, a non-tortuous edge (a straight line) will have a tortuosity of 1.0 while a tortuous edge will have its tortuosity decreasing toward zero.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the tortuosity measure will not be calculated. |
| `M_ENABLE` | Specifies that the tortuosity measure will be calculated. |

---

### `M_X_MAX_AT_Y_MAX`

Sets whether to calculate the maximum X-coordinate at the maximum Y-coordinate of each edge. Together with [`M_BOX_Y_MAX`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-maximum at Y-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the X-maximum at Y-maximum will be calculated. |

---

### `M_X_MIN_AT_Y_MIN`

Sets whether to calculate the minimum X-coordinate at the minimum Y-coordinate of each edge. Together with [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-minimum at Y-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the X-minimum at Y-minimum will be calculated. |

---

### `M_Y_MAX_AT_X_MIN`

Sets whether to calculate the maximum Y-coordinate at the minimum X-coordinate of each edge. Together with [`M_BOX_X_MIN`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-maximum at X-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-maximum at X-minimum will be calculated. |

---

### `M_Y_MIN_AT_X_MAX`

Sets whether to calculate the minimum Y-coordinate at the maximum X-coordinate of each edge. Together with [`M_BOX_X_MAX`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-minimum at X-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-minimum at X-maximum will be calculated. |

### Combination Constants — For specifying a sorting key

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify a sorting key for result retrieval.

Note that only one edge feature can be selected as the first, second, or third sorting key.

| Value | Description |
| --- | --- |
| `M_NO_SORT` | Removes the specified sorting key. |
| `M_SORTn_DOWN` | Specifies the feature as the _n_ <sup>th</sup> sorting key (in descending order), where _n_ stands for an integer between 1 and 3. |
| `M_SORTn_UP` | Specifies the feature as the _n_ <sup>th</sup> sorting key (in ascending order), where _n_ stands for an integer between 1 and 3. |

### For calculating Feret values

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used to calculate Feret values and can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_FERET_ANGLE_SEARCH_END`

Sets the end of the angular range at which to search for Feret diameters. The angular range is used to calculate the following Feret features: [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), and [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md). The search is done in the counter-clockwise direction in steps of ([`M_FERET_ANGLE_SEARCH_END`](../../Reference/edge/MedgeControl.md) - [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md)) / [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the end of the angular region. |

---

### `M_FERET_ANGLE_SEARCH_START`

Sets the start of the angular range at which to search for Feret diameters. The angular range is used to calculate the following Feret features: [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), and [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md). The search is done in the counter-clockwise direction in steps of ([`M_FERET_ANGLE_SEARCH_END`](../../Reference/edge/MedgeControl.md) - [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md)) / [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the start of the angular region. |

---

### `M_FERET_GENERAL_ANGLE`

Sets the angle at which to calculate the [`M_FERET_GENERAL`](../../Reference/edge/MedgeControl.md) value.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MOMENT_ELONGATION_ANGLE + n` | Specifies to use the moment elongation angle plus the offset _n_. The moment elongation angle is the angle of the principal axis for an edge's moment elongation ([`M_MOMENT_ELONGATION`](../../Reference/edge/MedgeControl.md)).  Adding an offset is optional. For example, if you want to get the general Feret measure to be perpendicular to the moment elongation angle, set the offset to 90. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_NUMBER_OF_FERETS`

Sets the number of Feret angles to use to calculate [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_FERET_MEAN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_ELONGATION`](../../Reference/edge/MedgeControl.md), and [`M_CONVEX_PERIMETER`](../../Reference/edge/MedgeControl.md). The first Feret angle used is established using [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md), and the difference between successive angles is ([`M_FERET_ANGLE_SEARCH_END`](../../Reference/edge/MedgeControl.md) - [`M_FERET_ANGLE_SEARCH_START`](../../Reference/edge/MedgeControl.md)) / [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md).  Note that increasing the number of Ferets increases the accuracy of the results; however, it also increases the processing time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the number of Ferets. |

### For selecting groups of edge features

The following values allow you to select groups of edge features for calculation in a single call.

---

### `M_ALL_FEATURES`

Sets whether to calculate all edge features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edge features will not be calculated. |
| `M_ENABLE` | Specifies that all edge features will be calculated. |

---

### `M_BOX`

Sets whether to calculate the four extreme edgel coordinates of each edge ([`M_BOX_X_MIN`](../../Reference/edge/MedgeControl.md), [`M_BOX_X_MAX`](../../Reference/edge/MedgeControl.md), [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md), and [`M_BOX_Y_MAX`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all the extreme edgel coordinates will not be calculated. |
| `M_ENABLE` | Specifies that all the extreme edgel coordinates will be calculated. |

---

### `M_CENTER_OF_GRAVITY`

Sets whether to calculate the X- and Y-coordinates of the center of gravity of each edge ([`M_CENTER_OF_GRAVITY_X`](../../Reference/edge/MedgeControl.md) and [`M_CENTER_OF_GRAVITY_Y`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that both the X- and Y-coordinates of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that both the X- and Y-coordinates of the center of gravity will be calculated. |

---

### `M_CIRCLE_FIT`

Sets whether to calculate the circle fit values of each edge ([`M_CIRCLE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_RADIUS`](../../Reference/edge/MedgeControl.md), [`M_CIRCLE_FIT_ERROR`](../../Reference/edge/MedgeControl.md), and [`M_CIRCLE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all the circle fit values will not be calculated. |
| `M_ENABLE` | Specifies that all the circle fit values will be calculated. |

---

### `M_CONTACT_POINTS`

Sets whether to calculate the minimum/maximum X-coordinate at the minimum/maximum Y-coordinate of each edge ([`M_X_MIN_AT_Y_MIN`](../../Reference/edge/MedgeControl.md), [`M_X_MAX_AT_Y_MAX`](../../Reference/edge/MedgeControl.md), [`M_Y_MAX_AT_X_MIN`](../../Reference/edge/MedgeControl.md), and [`M_Y_MIN_AT_X_MAX`](../../Reference/edge/MedgeControl.md)). Together with its corresponding extreme edgel coordinate, these points represent four contact points on the convex perimeter of the edge. For example, [`M_X_MIN_AT_Y_MIN`](../../Reference/edge/MedgeControl.md) and [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md) is one of the four contact points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the four contact points will not be calculated. |
| `M_ENABLE` | Specifies that the four contact points will be calculated. |

---

### `M_ELLIPSE_FIT`

Sets whether to calculate the ellipse fit values of each edge ([`M_ELLIPSE_FIT_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_ELLIPSE_FIT_CENTER_X`](../../Reference/edge/MedgeControl.md), [`M_ELLIPSE_FIT_CENTER_Y`](../../Reference/edge/MedgeControl.md), [`M_ELLIPSE_FIT_COVERAGE`](../../Reference/edge/MedgeControl.md), [`M_ELLIPSE_FIT_ERROR`](../../Reference/edge/MedgeControl.md), [`M_ELLIPSE_FIT_MINOR_AXIS`](../../Reference/edge/MedgeControl.md), and [`M_ELLIPSE_FIT_MAJOR_AXIS`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all the ellipse fit values will not be calculated. |
| `M_ENABLE` | Specifies that all the ellipse fit values will be calculated. |

---

### `M_FERET_BOX`

Sets whether to calculate the X- and Y-Feret values of each edge ([`M_FERET_X`](../../Reference/edge/MedgeControl.md) and [`M_FERET_Y`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X- and Y-Feret values will not be calculated. |
| `M_ENABLE` | Specifies that the X- and Y-Feret values will be calculated. |

---

### `M_FIRST_POINT`

Sets whether to calculate the X- and Y-coordinate of each edge's first point ([`M_FIRST_POINT_X`](../../Reference/edge/MedgeControl.md), and [`M_FIRST_POINT_Y`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X- and Y-coordinate of each edge's first point will not be calculated. |
| `M_ENABLE` | Specifies that the X- and Y-coordinate of each edge's first point will be calculated. |

---

### `M_LINE_FIT`

Sets whether to calculate the line fit values of each edge ([`M_LINE_FIT_A`](../../Reference/edge/MedgeControl.md), [`M_LINE_FIT_B`](../../Reference/edge/MedgeControl.md), [`M_LINE_FIT_C`](../../Reference/edge/MedgeControl.md), and [`M_LINE_FIT_ERROR`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all the line fit values will not be calculated. |
| `M_ENABLE` | Specifies that all the line fit values will be calculated. |

---

### `M_POSITION`

Sets whether to calculate both the X- and Y-position of each edge ([`M_POSITION_X`](../../Reference/edge/MedgeControl.md) and [`M_POSITION_Y`](../../Reference/edge/MedgeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that both the X- and Y-position will not be calculated. |
| `M_ENABLE` | Specifies that both the X- and Y-position will be calculated. |

### For performing post-calculations on extracted edges

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used when performing post-calculations on extracted edges and can typically be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_APPROXIMATION_TOLERANCE`

Sets the resolution of edge approximation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the resolution. When set to 0.0, a very fine edge approximation is performed. When set to 100.0, a coarse edge approximation is performed. |

---

### `M_CHAIN_APPROXIMATION`

Sets the simple geometric feature used when performing the edge approximation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge approximation will not be performed. In this case, only the edge map is calculated. |
| `M_LINE` | Specifies that the edge approximation will be performed using a polygonal segmentation of each edge, in pixel units. In this case, both the edge map and the edge approximation are calculated. |
| `M_WORLD_LINE` | Specifies that the edge approximation will be performed using a polygonal segmentation of each edge, in world units. In this case, both the edge map and the edge approximation are calculated. If [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) is set to [`M_WORLD_LINE`](../../Reference/edge/MedgeControl.md), calling [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) to perform the calculation generates an error if the operation is not performed on a calibrated image. |

---

### `M_FILL_GAP_ANGLE`

Sets the aperture angle, starting at the line's tangent at the point of interest, where Edge Finder searches for edge extremity candidates when filling edge gaps. That is, [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md), along with [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md), define a region where two chain extremities can be linked. Note that two chain extremities can only be linked if both are included in the search region of the other.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the aperture angle, in degrees. |

---

### `M_FILL_GAP_CANDIDATE`

Sets whether to join an edge extremity with the other extremity of the same edge, or with an extremity of any edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the extremity of an edge can be connected with the extremity of any edge. |
| `M_SAME` | Specifies that the extremity of an edge can only be connected with the other extremity of the same edge. |

---

### `M_FILL_GAP_CONTINUITY`

Sets the continuity constraint used when performing edge gap filling, when more than one edge extremity candidate is present. Among the whole set of extremity candidate edgels found, the one which fulfills the continuity criteria best is chosen to fill the gap.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the continuity constraint. When set to 0.0, the closest candidate is chosen to link edge segments together. When set to 100.0, the candidate that gives the most continuous result (minimum curvature) is chosen. |

---

### `M_FILL_GAP_DISTANCE`

Sets the maximum distance radius where Edge Finder searches for edge extremity candidates when filling edge gaps. That is, [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md), along with [`M_FILL_GAP_ANGLE`](../../Reference/edge/MedgeControl.md), define a region where two chain extremities can be linked. Note that two chain extremities can only be linked if both are included in the search region of the other.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite maximum distance radius. |
| `Value` *(default)* | Specifies the maximum distance radius, in pixels. Note that generally, large values should not be set. Typically, you would set [`M_FILL_GAP_DISTANCE`](../../Reference/edge/MedgeControl.md) to a value in the range of 2 to 5 pixels. |

---

### `M_FILL_GAP_POLARITY`

Sets the use of the edge polarity to perform the filling of edge gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that gaps between edges will be filled, regardless of their polarity. |
| `M_REVERSE` | Specifies that gaps between edges that have reverse polarity will be filled.  Note that [`M_REVERSE`](../../Reference/edge/MedgeControl.md) is not available for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |
| `M_SAME` | Specifies that gaps between edges that have the same polarity will be filled.  Note that [`M_SAME`](../../Reference/edge/MedgeControl.md) is not available for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. |

### For a result buffer

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings can be specified for an Edge Finder result buffer.

---

### `M_AGM_COMPATIBLE`

Sets whether the Edge Finder result buffer can be used with an AGM context. When used together, you can define models from the result of an edge extraction, or you can find model occurrences in the result of an edge extraction. Note that you must call [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) for this setting to take effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Edge Finder result buffer cannot be used with an AGM context. |
| `M_ENABLE` | Specifies that the Edge Finder result buffer can be used with an AGM context. |

---

### `M_DRAW_CROSS_SIZE`

Sets the size of the cross used to identify certain features when drawing with [`MedgeDraw`](../../Reference/edge/MedgeDraw.md). This cross is used to draw the edge's edgels ([`M_DRAW_EDGELS`](../../Reference/edge/MedgeDraw.md)), mid-point ([`M_DRAW_POSITION`](../../Reference/edge/MedgeDraw.md)), and center of gravity ([`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md)).  For more information, see [`MedgeDraw`](../../Reference/edge/MedgeDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the size of the cross, in pixels. |

---

### `M_MODEL_FINDER_COMPATIBLE`

Sets whether the Edge Finder result buffer can be used with a Model Finder context. When used together, you can define models from the result of an edge extraction, or you can find model occurrences in the result of an edge extraction. Note that you must call [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) for this setting to take effect.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Edge Finder result buffer cannot be used with a Model Finder context. |
| `M_ENABLE` | Specifies that the Edge Finder result buffer can be used with a Model Finder context. |

---

### `M_NEAREST_NEIGHBOR_RADIUS`

Sets the radius distance used to select the closest edge or edges from a point. For more information, see the [`MedgeSelect`](../../Reference/edge/MedgeSelect.md)   [`Condition`](../../Reference/edge/MedgeSelect.md) parameter values [`M_NEAREST_NEIGHBOR`](../../Reference/edge/MedgeSelect.md) and [`M_ALL_NEAREST_NEIGHBORS`](../../Reference/edge/MedgeSelect.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the radius distance, in pixels. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specifed, calling [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) or [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) generates an error if the result was not calculated on a calibrated image. |

### For setting closest-edgel constraints

The following [`ControlType`](../../Reference/edge/MedgeControl.md) and corresponding [`ControlValue`](../../Reference/edge/MedgeControl.md) parameter settings are used to set closest-edgel constraints for [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). This function retrieves the coordinates of edgels, from an Edge Finder result buffer, that meet the constraints set below, and correspond to the closest neighbors from a list of user-specified source points.  The following values can only be specified for an Edge Finder result buffer.

---

### `M_NEIGHBOR_ANGLE`

Sets the gradient angle that an edgel must have, before being considered a candidate, when finding the closest edgels to a list of points.  [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) is based on a source angle, which you must provide for each source point, with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). Note that you can set a tolerance for this angle, using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md).  > **Note:** [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) can only be used if the internal angle buffer ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md)) was initially saved.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the edgel candidate can have any gradient angle. Note that [`M_ANY`](../../Reference/edge/MedgeControl.md) is equivalent to setting [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to 360.0°. In this case, you need not provide the source angle(s) in [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). |
| `M_REVERSE` | Specifies that the gradient angle of the edgel candidate must have the reverse angle (+ 180°) of the source point. |
| `M_SAME` | Specifies that the gradient angle of the edgel candidate must have the same angle as the source point. |
| `M_SAME_OR_REVERSE` | Specifies that the gradient angle of the edgel candidate must either have the same, or the reverse angle of the source point. |

---

### `M_NEIGHBOR_ANGLE_TOLERANCE`

Sets the angular tolerance to use for the angle constraint ([`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md)), when finding the closest edgels to a list of points. Only edgels that have a gradient angle that falls within this angular range can be returned.  Note that [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) is ignored if [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) is set to [`M_ANY`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angular tolerance, in degrees. This value is applied to the angle constraint. Note that setting [`M_NEIGHBOR_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) to 360.0° is equivalent to setting [`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md) to [`M_ANY`](../../Reference/edge/MedgeControl.md). |

---

### `M_NEIGHBOR_MAXIMUM_NUMBER`

Sets the maximum number of edgels that can be returned (for each point), when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum number of edgels. |

---

### `M_NEIGHBOR_MINIMUM_SPACING`

Sets the minimum distance separating closest edgel candidates within the same edge, when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no minimum distance separating two edgel candidates. |
| `Value >= 1` | Specifies the minimum distance, in edgels. |

---

### `M_SEARCH_ANGLE`

Sets the search angle constraint (applied the Edge Finder result buffer), when finding the closest edgels to a list of points. Only edgels that are located along this angle can be returned.  [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) is relative to the source angle set with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). Note that you can set the angular tolerance and the angular orientation for [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md), using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md) and [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md), respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_SEARCH_ANGLE_SIGN`

Sets the orientation to use for the search angle constraint, when finding the closest edgels to a list of points. The orientation is relative to the source angle set with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md). For example, if the source angle is 45°, then the reverse orientation would be 225°.  Note that you can set the search angle and the angular tolerance for [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md), using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) and [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md), respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies that the orientation of the angle must be the reverse (+ 180°) of the source angle. That is, the angle used will look flipped when compared to the source angle. |
| `M_SAME` *(default)* | Specifies that the orientation of the angle must be the same as the source angle. |
| `M_SAME_OR_REVERSE` | Specifies that the orientation of the angle can be the same as, or the reverse of, the source angle. |

---

### `M_SEARCH_ANGLE_TOLERANCE`

Sets the angular tolerance to use for the search angle constraint, when finding the closest edgels to a list of points. Only edgels that are located within this angular range (in the Edge Finder result buffer) can be returned.  Note that you can set the search angle and the angular orientation for [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md), using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md) and [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md), respectively.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_SEARCH_RADIUS_MAX`

Sets the maximum radius distance in the Edge Finder result buffer that will be searched for the closest edgels. You must use [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) to specify the source point from which the radius is measured.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies a maximum distance radius that spans all edgels in the Edge Finder result buffer. |
| `Value` | Specifies the maximum distance radius, in pixels. |

---

### `M_SEARCH_RADIUS_MIN`

Sets the minimum radius distance in the Edge Finder result buffer that will be searched for the closest edgels. You must use [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md) to specify the source point from which the radius is measured.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum distance radius, in pixels. |

[`M_SEARCH_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/edge/MedgeControl.md), and [`M_SEARCH_ANGLE_SIGN`](../../Reference/edge/MedgeControl.md) limit the search region for potential edgel candidates to an angular sector within the Edge Finder result buffer. Only edgels that are located within this region can be returned.

Used together, [`M_SEARCH_RADIUS_MAX`](../../Reference/edge/MedgeControl.md) and [`M_SEARCH_RADIUS_MIN`](../../Reference/edge/MedgeControl.md) define a ring-like region, in the Edge Finder result buffer, that will be used to find the closest edgels. All edgels that fall outside this ring will be ignored.
