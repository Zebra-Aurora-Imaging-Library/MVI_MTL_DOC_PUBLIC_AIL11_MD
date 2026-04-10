---
doctype: Reference
module: edge
function: MedgeInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeInquire"
---

# MedgeInquire

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

> Inquire about an Edge Finder context or an Edge Finder result buffer.

## Syntax

```c
AIL_INT MedgeInquire(
    AIL_ID    ContextOrResultId,  //in
    AIL_INT64 InquireType,        //in
    void *    UserVarPtr          //out
)
```

## Description

This function inquires information about the specified Edge Finder context or Edge Finder result buffer.

Note that for an Edge Finder result buffer, this function only retrieves information about result buffer settings (set with [`MedgeControl`](../../Reference/edge/MedgeControl.md), for example). To retrieve results from the Edge Finder result buffer, use [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/edge/MedgeControl.md) (for example, in [`MedgeControl`](../../Reference/edge/MedgeControl.md)), [`MedgeInquire`](../../Reference/edge/MedgeInquire.md) will return [`M_DEFAULT`](../../Reference/edge/MedgeInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/edge/MedgeInquire.md) to the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter.

## Parameters

### `ContextOrResultId` *(in, AIL_ID)*

Specifies either the Edge Finder context or the Edge Finder result buffer about which to inquire information. Both the Edge Finder context and the Edge Finder result buffer must have been previously allocated on the system using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md) or [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md), respectively.

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MedgeInquire`](../../Reference/edge/MedgeInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For operation settings (both

To inquire about the operation settings for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the following values:

---

### `M_ACCURACY`

Inquires the edgel accuracy of the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that edgel accuracy will be disabled. |
| `M_HIGH` *(default)* | Specifies high accuracy. |
| `M_VERY_HIGH` | Specifies very high accuracy. |

---

### `M_ANGLE_ACCURACY`

Inquires the edgel angle accuracy of the edge extraction.  [`M_ANGLE_ACCURACY`](../../Reference/edge/MedgeInquire.md) can only be used with [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies high precision. |
| `M_LOW` | Specifies low precision. |

---

### `M_CHAIN_ALL_NEIGHBORS`

Inquires whether edge chains are built using all the available neighboring edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that edge chains are built with the least amount of edgel information possible. |
| `M_ENABLE` | Specifies that edge chains are built with as much edgel information as possible. |

---

### `M_EXTRACTION_SCALE`

Inquires the image scale with which to extract the edges.  Returns the scale of the image at which to do the edge extraction. Once the extraction is complete, the results are scaled to the original scale of the image.  A lower extraction scale speeds up the search but can result in a less reliable result, including, the loss of important details and/or a reduction in the accuracy of the search results.  [`M_EXTRACTION_SCALE`](../../Reference/edge/MedgeControl.md) is for advanced users of the Edge Finder module. The default setting usually provides the most accurate search results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the extraction scale. |

---

### `M_FILTER_SMOOTHNESS`

Inquires the degree of smoothness (strength of denoising) of the edge extraction filter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

---

### `M_FILTER_TYPE`

Inquires the type of filter used when performing the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DERICHE` | Specifies a Deriche infinite support filter. |
| `M_FREI_CHEN` | Specifies a Frei Chen filter. |
| `M_PREWITT` | Specifies a Prewitt filter. |
| `M_SHEN` *(default)* | Specifies a Shen-Castan infinite support exponential filter. |
| `M_SOBEL` | Specifies a Sobel filter. |

---

### `M_FLOAT_MODE`

Inquires whether the entire edge extraction process is forced to be performed using floating-point precision calculations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edge extractions are not forced to be performed using floating-point precision calculations. |
| `M_ENABLE` | Specifies that all edge extractions are forced to be performed using floating-point precision calculations. |

---

### `M_KERNEL_SIZE`

Inquires the actual X and Y size (same in X and Y) of the convolution kernel used for kernel filtering mode. Note that the kernel size can change according to the filter type, the smoothness factor, the specified maximum kernel width, the specified kernel depth, and whether you are using floating-point calculations.

| Value | Description |
| --- | --- |
| `Value` | Specifies the actual size, in X and Y, of the convolution kernel. |

---

### `M_MAGNITUDE_TYPE`

Inquires the type of magnitude value used to calculate the magnitude of the edge at each edgel position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value. |
| `M_NORM` | Specifies that the magnitude will be used. |
| `M_SQR_NORM` | Specifies that the square of the magnitude will be used. |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_OVERSCAN`

Inquires the type of overscan used by the convolution filters when extracting edges for kernel filtering mode. Note that [`M_OVERSCAN`](../../Reference/edge/MedgeInquire.md) is ignored if using an IIR filter (e.g. Shen and Deriche filter types).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that no overscan will be used, unless processing the border pixels is faster than ignoring them; in the latter case, Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified operation and the target system. |
| `M_MIRROR` *(default)* | Specifies that the border pixels of a source image are processed using overscan pixel values that mirror the source buffer pixel values. |
| `M_REPLACE` | Specifies that the border pixels of a source image are processed using overscan pixel values set to the overscan replacement value ([`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeControl.md)). |
| `M_TRANSPARENT` | Specifies that the border pixels of a source image are processed using transparent overscan pixel values. |

---

### `M_OVERSCAN_REPLACE_VALUE`

Inquires the replacement value for the overscan pixel values when using replacement type overscan. Note that [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeInquire.md) is ignored unless [`M_OVERSCAN`](../../Reference/edge/MedgeInquire.md) with [`MedgeControl`](../../Reference/edge/MedgeControl.md) is set to [`M_REPLACE`](../../Reference/edge/MedgeInquire.md). In addition, [`M_OVERSCAN_REPLACE_VALUE`](../../Reference/edge/MedgeInquire.md) is ignored if using an IIR filter (e.g. Shen and Deriche filter types).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REPLACE_MAX` | Specifies that the overscan neighborhood pixel values will be set to the maximum value of the source image buffer. |
| `M_REPLACE_MIN` | Specifies that the overscan neighborhood pixel values will be set to the minimum value of the source image buffer. |
| `Value` *(default)* | Specifies the value of the overscan neighborhood pixels. |

---

### `M_THRESHOLD_HIGH`

Inquires the user-defined upper bound of the hysteresis threshold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the upper bound of the hysteresis threshold. |

---

### `M_THRESHOLD_LOW`

Inquires the user-defined lower bound of the hysteresis threshold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the lower bound of the hysteresis threshold. |

---

### `M_THRESHOLD_MODE`

Inquires the threshold mode of the edge extraction. Note that lower threshold values result in a more sensitive edgel detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies no threshold. |
| `M_HIGH` *(default)* | Specifies a high threshold. |
| `M_LOW` | Specifies a low threshold. |
| `M_MEDIUM` | Specifies a medium threshold. |
| `M_USER_DEFINED` | Specifies that the threshold values will be user-defined. |
| `M_VERY_HIGH` | Specifies a very high threshold. |

---

### `M_THRESHOLD_TYPE`

Inquires the type of the hysteresis threshold used when performing the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FULL_HYSTERESIS` | Specifies that the lower bound threshold value is 0. |
| `M_HYSTERESIS` *(default)* | Specifies that both the lower bound threshold value and the upper bound threshold value will be used. |
| `M_NO_HYSTERESIS` | Specifies that the lower bound threshold value is equal to the upper bound threshold value. |

---

### `M_TIMEOUT`

Inquires the maximum edge extraction and calculation time for [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of edge extraction and calculation time. |
| `Value > 0` | Specifies the maximum edge extraction and calculation time, in msec. |

### For operation settings (

To inquire about the operation settings for [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to the value below.

---

### `M_FOREGROUND_VALUE`

Inquires the color used to extract line crests from the image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies that the line crests are both lighter and darker than the image's background color. |
| `M_FOREGROUND_BLACK` *(default)* | Specifies that the line crests are darker than the image's background color. |
| `M_FOREGROUND_WHITE` | Specifies that the line crests are lighter than the image's background color. |

### For internal buffers (both

To inquire whether internal buffers have been saved in the Edge Finder result buffer, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_SAVE_ANGLE`

Inquires whether the internal angle buffer used to extract edges is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle buffer will not be saved. |
| `M_ENABLE` | Specifies that the angle buffer will be saved. |

---

### `M_SAVE_CHAIN_ANGLE`

Inquires whether the angle value at each edgel position is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle values will not be saved. |
| `M_ENABLE` | Specifies that the angle values will be saved. |

---

### `M_SAVE_CHAIN_MAGNITUDE`

Inquires whether the magnitude value at each edgel position is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the magnitude values will not be saved. |
| `M_ENABLE` | Specifies that the magnitude values will be saved. |

---

### `M_SAVE_DERIVATIVES`

Inquires whether the internal derivative buffers used to extract edges are saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the derivative buffers will not be saved. |
| `M_ENABLE` | Specifies that the derivative buffers will be saved. |

---

### `M_SAVE_IMAGE`

Inquires whether the source image is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the image will not be saved. |
| `M_ENABLE` | Specifies that the image will be saved. |

---

### `M_SAVE_MAGNITUDE`

Inquires whether the internal magnitude buffer used to extract edges is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the magnitude buffer will not be saved. |
| `M_ENABLE` | Specifies that the magnitude buffer will be saved. |

---

### `M_SAVE_MASK`

Inquires whether the mask buffer is saved in the Edge Finder result buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the mask will not be saved. |
| `M_ENABLE` | Specifies that the mask will be saved. |

### For edge features (both

To inquire about which edge features will be calculated for each edge, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts. Note that, when an edge feature has been calculated, [`M_ENABLE`](../../Reference/edge/MedgeControl.md) is returned.

---

### `M_AVERAGE_STRENGTH`

Inquires whether the average strength value of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average strength will not be calculated. |
| `M_ENABLE` | Specifies that the average strength will be calculated. |

---

### `M_BOX_X_MAX`

Inquires whether the extreme right edgel coordinate of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme right edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme right edgel coordinate will be calculated. |

---

### `M_BOX_X_MIN`

Inquires whether the extreme left edgel coordinate of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme left edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme left edgel coordinate will be calculated. |

---

### `M_BOX_Y_MAX`

Inquires whether the extreme bottom edgel coordinate of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme bottom edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme bottom edgel coordinate will be calculated. |

---

### `M_BOX_Y_MIN`

Inquires whether the extreme top edgel coordinate of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme top edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme top edgel coordinate will be calculated. |

---

### `M_CENTER_OF_GRAVITY_X`

Inquires whether the X-position of each edge's center of gravity will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the X-position of the center of gravity will be calculated. |

---

### `M_CENTER_OF_GRAVITY_Y`

Inquires whether the Y-position of each edge's center of gravity will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position of the center of gravity will be calculated. |

---

### `M_CIRCLE_FIT_CENTER_X`

Inquires whether the X-coordinate of the center of the circle that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |

---

### `M_CIRCLE_FIT_CENTER_Y`

Inquires whether the Y-coordinate of the center of the circle that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |

---

### `M_CIRCLE_FIT_COVERAGE`

Inquires whether the coverage of the circle that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the circle will be calculated. |

---

### `M_CIRCLE_FIT_ERROR`

Inquires whether the fit error of the circle that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the circle will be calculated. |

---

### `M_CIRCLE_FIT_RADIUS`

Inquires whether the radius of the circle that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the radius will not be calculated. |
| `M_ENABLE` | Specifies that the radius will be calculated. |

---

### `M_CLOSURE`

Inquires whether the closure of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge's closure state will not be calculated. |
| `M_ENABLE` | Specifies that the edge's closure state will be calculated. |

---

### `M_CONVEX_PERIMETER`

Inquires whether the convex elongation of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the convex elongation will not be calculated. |
| `M_ENABLE` | Specifies that the convex elongation will be calculated. |

---

### `M_ELLIPSE_FIT_ANGLE`

Inquires whether the angle of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle will not be calculated. |
| `M_ENABLE` | Specifies that the angle will be calculated. |

---

### `M_ELLIPSE_FIT_CENTER_X`

Inquires whether the X-coordinate of the center of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |

---

### `M_ELLIPSE_FIT_CENTER_Y`

Inquires whether the Y-coordinate of the center of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |

---

### `M_ELLIPSE_FIT_COVERAGE`

Inquires whether the coverage of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the ellipse will be calculated. |

---

### `M_ELLIPSE_FIT_ERROR`

Inquires whether the fit error of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the ellipse will be calculated. |

---

### `M_ELLIPSE_FIT_MAJOR_AXIS`

Inquires whether the major axis of the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the major axis will not be calculated. |
| `M_ENABLE` | Specifies that the major axis will be calculated. |

---

### `M_ELLIPSE_FIT_MINOR_AXIS`

Inquires whether the minor axis the ellipse that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minor axis will not be calculated. |
| `M_ENABLE` | Specifies that the minor axis will be calculated. |

---

### `M_FAST_LENGTH`

Inquires whether the length of each edge will be quickly calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that a fast length will not be calculated. |
| `M_ENABLE` | Specifies that a fast length will be calculated. |

---

### `M_FERET_ELONGATION`

Inquires whether the Feret elongation of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret elongation will not be calculated. |
| `M_ENABLE` | Specifies that the Feret elongation will be calculated. |

---

### `M_FERET_GENERAL`

Inquires whether the general Feret of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the general Feret will not be calculated. |
| `M_ENABLE` | Specifies that the general Feret will be calculated. |

---

### `M_FERET_MAX_ANGLE`

Inquires whether the maximum Feret angle of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret angle will be calculated. |

---

### `M_FERET_MAX_DIAMETER`

Inquires whether the maximum Feret diameter of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret diameter will be calculated. |

---

### `M_FERET_MEAN_DIAMETER`

Inquires whether the average Feret diameter of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the average Feret diameter will be calculated. |

---

### `M_FERET_MIN_ANGLE`

Inquires whether the minimum Feret angle of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret angle will be calculated. |

---

### `M_FERET_MIN_DIAMETER`

Inquires whether the minimum Feret diameter of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret diameter will be calculated. |

---

### `M_FERET_X`

Inquires whether the X-Feret value of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the X-Feret value will be calculated. |

---

### `M_FERET_Y`

Inquires whether the Y-Feret value of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the Y-Feret value will be calculated. |

---

### `M_FIRST_POINT_X`

Inquires whether the X-coordinate of each edge's first point (starting point) will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's X-coordinate will be calculated. |

---

### `M_FIRST_POINT_Y`

Inquires whether the Y-coordinate of each edge's first point (starting point) will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's Y-coordinate will be calculated. |

---

### `M_LABEL_VALUE`

Inquires whether the label value of each edge in an image will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the label value will not be calculated. |
| `M_ENABLE` *(default)* | Specifies that the label value will be calculated. |

---

### `M_LENGTH`

Inquires whether the length of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the length of each edge will not be calculated. |
| `M_ENABLE` | Specifies that the length of each edge will be calculated. |

---

### `M_LINE_FIT_A`

Inquires whether the coefficient _A_ of the line that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _A_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _A_ will be calculated. |

---

### `M_LINE_FIT_B`

Inquires whether the coefficient _B_ of the line that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _B_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _B_ will be calculated. |

---

### `M_LINE_FIT_C`

Inquires whether the coefficient _C_ of the line that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _C_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _C_ will be calculated. |

---

### `M_LINE_FIT_ERROR`

Inquires whether the fit error of the line that is the best fit for each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error will not be calculated. |
| `M_ENABLE` | Specifies that the fit error will be calculated. |

---

### `M_MOMENT_ELONGATION`

Inquires whether the moment elongation of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the moment elongation will be calculated. |

---

### `M_MOMENT_ELONGATION_ANGLE`

Inquires whether the angle of the principal axis along each edge's moment elongation will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle of the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the angle of the moment elongation will be calculated. |

---

### `M_POSITION_X`

Inquires whether the X-position of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position will not be calculated. |
| `M_ENABLE` | Specifies that the X-position will be calculated. |

---

### `M_POSITION_Y`

Inquires whether the Y-position of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position will be calculated. |

---

### `M_SIZE`

Inquires whether the number of edgels in each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the number of edgels will not be calculated. |
| `M_ENABLE` | Specifies that the number of edgels will be calculated. |

---

### `M_STRENGTH`

Inquires whether the strength value of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the strength will not be calculated. |
| `M_ENABLE` | Specifies that the strength will be calculated. |

---

### `M_TORTUOSITY`

Inquires whether the tortuosity measure of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the tortuosity measure will not be calculated. |
| `M_ENABLE` | Specifies that the tortuosity measure will be calculated. |

---

### `M_X_MAX_AT_Y_MAX`

Inquires whether the X-maximum at Y-maximum contact point of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-maximum at Y-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the X-maximum at Y-maximum will be calculated. |

---

### `M_X_MIN_AT_Y_MIN`

Inquires whether the X-minimum at Y-minimum contact point of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-minimum at Y-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the X-minimum at Y-minimum will be calculated. |

---

### `M_Y_MAX_AT_X_MIN`

Inquires whether the Y-maximum at X-minimum contact point of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-maximum at X-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-maximum at X-minimum will be calculated. |

---

### `M_Y_MIN_AT_X_MAX`

Inquires whether the Y-minimum at X-maximum contact point of each edge will be calculated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-minimum at X-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-minimum at X-maximum will be calculated. |

### For sorting keys (both

To inquire about the corresponding edge feature associated with the specified sorting key, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_SORTn_DOWN`

Inquires the feature associated with the _n_ <sup>th</sup> sorting key (in descending order), where _n_ stands for an integer between 1 and 3.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average strength will not be calculated. |
| `M_ENABLE` | Specifies that the average strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme right edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme right edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme left edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme left edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme bottom edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme bottom edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme top edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme top edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the X-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the radius will not be calculated. |
| `M_ENABLE` | Specifies that the radius will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge's closure state will not be calculated. |
| `M_ENABLE` | Specifies that the edge's closure state will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the convex elongation will not be calculated. |
| `M_ENABLE` | Specifies that the convex elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle will not be calculated. |
| `M_ENABLE` | Specifies that the angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the major axis will not be calculated. |
| `M_ENABLE` | Specifies that the major axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minor axis will not be calculated. |
| `M_ENABLE` | Specifies that the minor axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that a fast length will not be calculated. |
| `M_ENABLE` | Specifies that a fast length will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret elongation will not be calculated. |
| `M_ENABLE` | Specifies that the Feret elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the general Feret will not be calculated. |
| `M_ENABLE` | Specifies that the general Feret will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the average Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the X-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the Y-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the label value will not be calculated. |
| `M_ENABLE` *(default)* | Specifies that the label value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the length of each edge will not be calculated. |
| `M_ENABLE` | Specifies that the length of each edge will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _A_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _A_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _B_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _B_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _C_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _C_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error will not be calculated. |
| `M_ENABLE` | Specifies that the fit error will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle of the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the angle of the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position will not be calculated. |
| `M_ENABLE` | Specifies that the X-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the number of edgels will not be calculated. |
| `M_ENABLE` | Specifies that the number of edgels will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the strength will not be calculated. |
| `M_ENABLE` | Specifies that the strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the tortuosity measure will not be calculated. |
| `M_ENABLE` | Specifies that the tortuosity measure will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-maximum at Y-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the X-maximum at Y-maximum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-minimum at Y-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the X-minimum at Y-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-maximum at X-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-maximum at X-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-minimum at X-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-minimum at X-maximum will be calculated. |
| `M_NULL` | Specifies that no feature has been associated with the specified sorting key. |

---

### `M_SORTn_UP`

Inquires the feature associated with the _n_ <sup>th</sup> sorting key (in ascending order), where _n_ stands for an integer between 1 and 3.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average strength will not be calculated. |
| `M_ENABLE` | Specifies that the average strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme right edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme right edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme left edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme left edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme bottom edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme bottom edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the extreme top edgel coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the extreme top edgel coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the X-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position of the center of gravity will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position of the center of gravity will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the circle will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the circle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the radius will not be calculated. |
| `M_ENABLE` | Specifies that the radius will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge's closure state will not be calculated. |
| `M_ENABLE` | Specifies that the edge's closure state will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the convex elongation will not be calculated. |
| `M_ENABLE` | Specifies that the convex elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle will not be calculated. |
| `M_ENABLE` | Specifies that the angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coverage of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the coverage of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error of the ellipse will not be calculated. |
| `M_ENABLE` | Specifies that the fit error of the ellipse will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the major axis will not be calculated. |
| `M_ENABLE` | Specifies that the major axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minor axis will not be calculated. |
| `M_ENABLE` | Specifies that the minor axis will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that a fast length will not be calculated. |
| `M_ENABLE` | Specifies that a fast length will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Feret elongation will not be calculated. |
| `M_ENABLE` | Specifies that the Feret elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the general Feret will not be calculated. |
| `M_ENABLE` | Specifies that the general Feret will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the maximum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the maximum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the average Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the average Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret angle will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret angle will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the minimum Feret diameter will not be calculated. |
| `M_ENABLE` | Specifies that the minimum Feret diameter will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the X-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-Feret value will not be calculated. |
| `M_ENABLE` | Specifies that the Y-Feret value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's X-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's X-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the first point's Y-coordinate will not be calculated. |
| `M_ENABLE` | Specifies that the first point's Y-coordinate will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the label value will not be calculated. |
| `M_ENABLE` *(default)* | Specifies that the label value will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the length of each edge will not be calculated. |
| `M_ENABLE` | Specifies that the length of each edge will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _A_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _A_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _B_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _B_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the coefficient _C_ will not be calculated. |
| `M_ENABLE` | Specifies that the coefficient _C_ will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the fit error will not be calculated. |
| `M_ENABLE` | Specifies that the fit error will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the angle of the moment elongation will not be calculated. |
| `M_ENABLE` | Specifies that the angle of the moment elongation will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-position will not be calculated. |
| `M_ENABLE` | Specifies that the X-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-position will not be calculated. |
| `M_ENABLE` | Specifies that the Y-position will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the number of edgels will not be calculated. |
| `M_ENABLE` | Specifies that the number of edgels will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the strength will not be calculated. |
| `M_ENABLE` | Specifies that the strength will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the tortuosity measure will not be calculated. |
| `M_ENABLE` | Specifies that the tortuosity measure will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-maximum at Y-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the X-maximum at Y-maximum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the X-minimum at Y-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the X-minimum at Y-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-maximum at X-minimum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-maximum at X-minimum will be calculated. |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Y-minimum at X-maximum will not be calculated. |
| `M_ENABLE` | Specifies that the Y-minimum at X-maximum will be calculated. |
| `M_NULL` | Specifies that no feature has been associated with the specified sorting key. |

### For general settings (both

To inquire about general Edge Finder context settings, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_CONTEXT_TYPE`

Inquires the type of the Edge Finder context.

| Value | Description |
| --- | --- |
| `M_CONTOUR` | Specifies a contour context type, which is used to find object contours in images. |
| `M_CREST` | Specifies a crest context type, which is used to find thin line crests in images. |

---

### `M_FILTER_POWER`

Inquires the power of the filter used to extract edges. This indicates the decreasing factor of the noise variance. In the following formula, _h_ represents the filter values. The sum is computed over the filter support region. This region can be finite or infinite; it includes the neighboring image elements that are taken into account when computing the output image element.  *[Image: medgePowerDefinition.png]*

| Value | Description |
| --- | --- |
| `Value` | Specifies the power of the filter used to extract edges. |

---

### `M_MASK_SIZE_X`

Inquires the X-size of the mask.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no mask has been set using [`MedgeMask`](../../Reference/edge/MedgeMask.md). |
| `Value` | Specifies the X-size of the mask, in pixels. |

---

### `M_MASK_SIZE_Y`

Inquires the Y-size of the mask.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no mask has been set using [`MedgeMask`](../../Reference/edge/MedgeMask.md). |
| `Value` | Specifies the Y-size of the mask, in pixels. |

### For Feret settings (both

To inquire about the values used to calculate Feret settings, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_FERET_ANGLE_SEARCH_END`

Inquires the end of the angular range at which to search for Feret diameters. The angular range is used to calculate the following Feret features: [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), and [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the end of the angular region. |

---

### `M_FERET_ANGLE_SEARCH_START`

Inquires the start of the angular range at which to search for Feret diameters. The angular range is used to calculate the following Feret features: [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), and [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the start of the angular region. |

---

### `M_FERET_GENERAL_ANGLE`

Inquires the angle at which to calculate [`M_FERET_GENERAL`](../../Reference/edge/MedgeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MOMENT_ELONGATION_ANGLE + n` | Specifies to use the moment elongation angle plus the offset _n_. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_NUMBER_OF_FERETS`

Inquires the number of Ferets used to calculate [`M_FERET_MAX_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_DIAMETER`](../../Reference/edge/MedgeControl.md), [`M_FERET_MAX_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_FERET_MIN_ANGLE`](../../Reference/edge/MedgeControl.md), [`M_FERET_MEAN_DIAMETER`](../../Reference/edge/MedgeInquire.md), [`M_FERET_ELONGATION`](../../Reference/edge/MedgeControl.md), and [`M_CONVEX_PERIMETER`](../../Reference/edge/MedgeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the number of Ferets. |

### For post-calculations (both

To inquire about the settings used when performing post-calculations on extracted edges, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below. These values can be specified for both [`M_CONTOUR`](../../Reference/edge/MedgeAlloc.md) and [`M_CREST`](../../Reference/edge/MedgeAlloc.md) Edge Finder contexts.

---

### `M_APPROXIMATION_TOLERANCE`

Inquires the resolution of the edge approximation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the resolution. |

---

### `M_CHAIN_APPROXIMATION`

Inquires the simple geometric features used to approximate edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the edge approximation will not be performed. |
| `M_LINE` | Specifies that the edge approximation will be performed using a polygonal segmentation of each edge, in pixel units. |
| `M_WORLD_LINE` | Specifies that the edge approximation will be performed using a polygonal segmentation of each edge, in world units. |

---

### `M_FILL_GAP_ANGLE`

Inquires the aperture angle where Edge Finder searches for edge extremity candidates when filling edge gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the aperture angle, in degrees. |

---

### `M_FILL_GAP_CANDIDATE`

Inquires whether an edge extremity is filled with the other extremity of the same edge, or with an extremity of any edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the extremity of an edge can be connected with the extremity of any edge. |
| `M_SAME` | Specifies that the extremity of an edge can only be connected with the other extremity of the same edge. |

---

### `M_FILL_GAP_CONTINUITY`

Inquires the continuity constraint used when performing edge gap filling, when more than one edge extremity candidate is present.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the continuity constraint. |

---

### `M_FILL_GAP_DISTANCE`

Inquires the maximum distance radius where Edge Finder searches for edge extremity candidates when filling edge gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` | Specifies an infinite maximum distance radius. |
| `Value` *(default)* | Specifies the maximum distance radius, in pixels. |

---

### `M_FILL_GAP_POLARITY`

Inquires the polarity constraint used when performing the filling of edge gaps.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that gaps between edges will be filled, regardless of their polarity. |
| `M_REVERSE` | Specifies that gaps between edges that have reverse polarity will be filled. |
| `M_SAME` | Specifies that gaps between edges that have the same polarity will be filled. |

### For result buffer settings (general)

To inquire about general Edge Finder result buffer settings, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the following values:

---

### `M_AGM_COMPATIBLE`

Inquires whether the Edge Finder result buffer is ready to be used with an AGM context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Edge Finder result buffer cannot be used with an AGM context. |
| `M_ENABLE` | Specifies that the Edge Finder result buffer can be used with an AGM context. |

---

### `M_DRAW_CROSS_SIZE`

Inquires the size of the cross used for certain drawing operations. For example, [`M_DRAW_EDGELS`](../../Reference/edge/MedgeDraw.md), [`M_DRAW_POSITION`](../../Reference/edge/MedgeDraw.md), and [`M_DRAW_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeDraw.md) are drawn using this cross.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the size of the cross, in pixels. |

---

### `M_MODEL_FINDER_COMPATIBLE`

Inquires whether the Edge Finder result buffer is ready to be used with a Model Finder context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Edge Finder result buffer cannot be used with a Model Finder context. |
| `M_ENABLE` | Specifies that the Edge Finder result buffer can be used with a Model Finder context. |

---

### `M_NEAREST_NEIGHBOR_RADIUS`

Inquires the radius distance used to select ([`MedgeSelect`](../../Reference/edge/MedgeSelect.md)) the closest edge or edges from a point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the radius distance, in pixels. |

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### For result buffer settings (closest-edgel operations)

To inquire about Edge Finder result buffer settings used when performing closest-edgel operations with [`MedgeGetNeighbors`](../../Reference/edge/MedgeGetNeighbors.md), set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to one of the values below.

---

### `M_NEIGHBOR_ANGLE`

Inquires the gradient angle that an edgel must have, before being considered a candidate, when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the edgel candidate can have any gradient angle. |
| `M_REVERSE` | Specifies that the gradient angle of the edgel candidate must have the reverse angle (+ 180°) of the source point. |
| `M_SAME` | Specifies that the gradient angle of the edgel candidate must have the same angle as the source point. |
| `M_SAME_OR_REVERSE` | Specifies that the gradient angle of the edgel candidate must either have the same, or the reverse angle of the source point. |

---

### `M_NEIGHBOR_ANGLE_TOLERANCE`

Inquires the angular tolerance used for the angle constraint ([`M_NEIGHBOR_ANGLE`](../../Reference/edge/MedgeControl.md)), when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angular tolerance, in degrees. |

---

### `M_NEIGHBOR_MAXIMUM_NUMBER`

Inquires the maximum number of closest edgel candidates that can be returned (for each source point), when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum number of edgels. |

---

### `M_NEIGHBOR_MINIMUM_SPACING`

Inquires the minimum distance separating edgels within the same edge, in order for each to be considered potential candidates, when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no minimum distance separating two edgel candidates. |
| `Value >= 1` | Specifies the minimum distance, in edgels. |

---

### `M_SEARCH_ANGLE`

Inquires the search angle constraint (applied the Edge Finder result buffer), when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_SEARCH_ANGLE_SIGN`

Inquires the orientation to use for the search angle constraint, when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies that the orientation of the angle must be the reverse (+ 180°) of the source angle. |
| `M_SAME` *(default)* | Specifies that the orientation of the angle must be the same as the source angle. |
| `M_SAME_OR_REVERSE` | Specifies that the orientation of the angle can be the same as, or the reverse of, the source angle. |

---

### `M_SEARCH_ANGLE_TOLERANCE`

Inquires the angular tolerance used for the search angle constraint, when finding the closest edgels to a list of points.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_SEARCH_RADIUS_MAX`

Inquires the maximum radius distance used to search for the closest edgel candidates that match source edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies a maximum distance radius that spans all edgels in the Edge Finder result buffer. |
| `Value` | Specifies the maximum distance radius, in pixels. |

---

### `M_SEARCH_RADIUS_MIN`

Inquires the minimum radius distance used to search for the closest edgel candidates that match source edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum distance radius, in pixels. |

### For inquiring about the system

To inquire about the system on which either the Edge Finder context or Edge Finder result buffer has been allocated, set the [`InquireType`](../../Reference/edge/MedgeInquire.md) parameter to the value below.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which either the Edge Finder context or Edge Finder result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring if an inquire type is sortable or is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is sortable or supported for the Edge Finder context currently being inquired.

Note that to inquire if an inquire type is sortable, you must add [`M_SUPPORTED`](../../Reference/edge/MedgeInquire.md) to the appropriate sort value ([`M_SORTn_UP`](../../Reference/edge/MedgeInquire.md) or [`M_SORTn_DOWN`](../../Reference/edge/MedgeInquire.md)) and the specified inquire type (for example, [`M_SIZE`](../../Reference/edge/MedgeInquire.md) + [`M_SORTn_UP`](../../Reference/edge/MedgeInquire.md) + [`M_SUPPORTED`](../../Reference/edge/MedgeInquire.md)). If the inquire type is not supported for the Edge Finder context, or sortable, [`M_FALSE`](../../Reference/edge/MedgeInquire.md) is returned.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is either sortable, or supported for the Edge Finder context.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/edge/MedgeInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/edge/MedgeInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
