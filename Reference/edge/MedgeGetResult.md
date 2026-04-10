---
doctype: Reference
module: edge
function: MedgeGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeGetResult"
---

# MedgeGetResult

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

> Get results of the included edges from an Edge Finder result buffer.

## Syntax

```c
void MedgeGetResult(
    AIL_ID    EdgeResultId,           //in
    AIL_INT   EdgeIndexOrLabelValue,  //in
    AIL_INT64 ResultType,             //in
    void *    FirstResultArrayPtr,    //out
    void *    SecondResultArrayPtr    //out
)
```

## Description

This function retrieves the results of a specified type from an Edge Finder result buffer, after an [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) call.

The following result types can be retrieved from the Edge Finder result buffer:

- General results related to the edge extraction and edge processing.
- Edge chain results.
- Edge feature results.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel of the image.

> **Note:** If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MedgeControl`](../../Reference/edge/MedgeControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) control type set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) to [`M_WORLD`](../../Reference/edge/MedgeControl.md) without specifying a calibrated image in which to calculate the results, [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) will generate an error.

Note that in the presence of distortion, some results are meaningless when converted from real-world to pixel units (for example, the Feret angles). For example, if an edge appears warped in the source image, but the camera calibration context of the source image compensates for this during the extraction, the resulting Feret angles are meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

Note that, unless otherwise specified, results are only returned to [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md); when this is the case, [`SecondResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) must be set to [`M_NULL`](../../Reference/edge/MedgeGetResult.md).

## Parameters

### `EdgeResultId` *(in, AIL_ID)*

Specifies the identifier of the Edge Finder result buffer from which to get results.

### `EdgeIndexOrLabelValue` *(in, AIL_INT)*

Specifies the edge(s) from which to get results. This parameter can be set to one of the following values.

*For edge results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ALL`](../../Reference/edge/MedgeGetResult.md). |
| `M_ALL` | Specifies all edges. That is, the target array(s) will be filled with the specified type of result for all the included edges. Note that target array values are ordered by edge indices. |
| `Value >= 0` | Specifies either the edge's index or label. Index values must fall within the following range: 0 to the number of included edges in the Edge Finder result buffer - 1. Label values can be returned with the result type [`M_LABEL_VALUE`](../../Reference/edge/MedgeGetResult.md). |

*For specifying an index or label value*

| Value | Description |
| --- | --- |
| `M_TYPE_INDEX` *(default)* | Specifies an edge using its index value. |
| `M_TYPE_LABEL` | Specifies an edge using its label value. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `FirstResultArrayPtr` *(out, *void)*

Specifies the address of the first array in which to write the requested information.

### `SecondResultArrayPtr` *(out, *void)*

Specifies the address of the second array in which to write the requested information. If nothing is to be written, set [`SecondResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) to `M_NULL`.

## Parameter Associations

### For retrieving results related to edge extraction and edge processing

To retrieve general results related to the edge extraction and edge processing, the [`ResultType`](../../Reference/edge/MedgeGetResult.md) parameter must be set to one of the values below.

---

### `M_CONTEXT_TYPE`

Retrieves the type of the Edge Finder context that was used to extract edges.

| Value | Description |
| --- | --- |
| `M_CONTOUR` | Specifies a contour context type, which is used to find object contours in images. |
| `M_CREST` | Specifies a crest context type, which is used to find thin line crests in images. |

---

### `M_NUMBER_OF_CHAINS`

Retrieves the number of included edges.

---

### `M_SIZE_X`

Retrieves the width of the source image and internal processing buffers, in pixels.  > **Note:** Note that if you have used [`MedgePut`](../../Reference/edge/MedgePut.md) to add edge chains to the Edge Finder result buffer, the width returned can be bigger than the width of the source image buffer if the edge chains exceed the boundaries of the source image buffer.

---

### `M_SIZE_Y`

Retrieves the height of the source image and internal processing buffers, in pixels.  > **Note:** Note that if you have used [`MedgePut`](../../Reference/edge/MedgePut.md) to add edge chains to the Edge Finder result buffer, the height returned can be bigger than the height of the source image buffer if the edge chains exceed the boundaries of the source image buffer.

---

### `M_THRESHOLD_HIGH`

Retrieves the upper bound value, used during hysteresis thresholding to extract edges.

---

### `M_THRESHOLD_LOW`

Retrieves the lower bound value, used during hysteresis thresholding to extract edges.

---

### `M_THRESHOLDS`

Retrieves both the lower and upper bound values, used during hysteresis thresholding to extract edges.

---

### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_TIMEOUT`](../../Reference/edge/MedgeControl.md). By default, there is no limit.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout limit has not been reached. |
| `M_TRUE` | Specifies that the timeout limit has been reached. |

### For retrieving information about the internal processing buffers

To retrieve information about the internal processing buffers, you must specify the internal buffer about which to retrieve the information and the type of information to retrieve. To do so, set [`ResultType`](../../Reference/edge/MedgeGetResult.md) to a combination of two values, one from each of the following two tables. The information retrieved can be used to allocate a buffer with the same properties as the internal processing buffer. This newly allocated buffer can then be used, for example, as the destination buffer for [`MedgeDraw`](../../Reference/edge/MedgeDraw.md).  > **Note:** The internal Edge Finder buffers cannot be accessed directly; however, they can be accessed by creating a buffer with the same characteristics as the internal buffer and using [`MedgeDraw`](../../Reference/edge/MedgeDraw.md) to draw the contents of the internal Edge Finder buffer into your new buffer.

---

### `M_ANGLE_ID`

Retrieves information about the internal angle buffer.  Note that information about the internal angle buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_ANGLE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_CROSS_DERIVATIVE_ID`

Retrieves information about the internal cross derivative buffer.  Note that information about the internal cross derivative buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_FIRST_DERIVATIVE_X_ID`

Retrieves information about the internal first derivative buffer in the X-direction.  Note that information about the internal first derivative in the X-direction buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_FIRST_DERIVATIVE_Y_ID`

Retrieves information about the internal first derivative buffer in the Y-direction.  Note that information about the internal first derivative buffer in the Y-direction is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_FIRST_DERIVATIVES_ID`

Retrieves information about the internal first derivative buffers in both the X- and Y-directions.  Note that information about the internal first derivative buffers in both the X- and Y-directions is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_IMAGE_ID`

Retrieves information about the source buffer.  Note that information about the source buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_IMAGE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_MAGNITUDE_ID`

Retrieves information about the internal magnitude buffer.  Note that information about the internal magnitude buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_MAGNITUDE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).  The magnitude buffer is calculated according to [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md). If [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md) is set to [`M_SQR_NORM`](../../Reference/edge/MedgeControl.md), magnitude values are truncated on 15.0 bits. If [`M_MAGNITUDE_TYPE`](../../Reference/edge/MedgeControl.md) is set to [`M_NORM`](../../Reference/edge/MedgeControl.md), magnitude values are truncated on 8.7 bits, with fixed-point precision. 32-bit float buffers use the same conventions, but with floating-point precision.

---

### `M_MASK_ID`

Retrieves information about the internal mask buffer.  Note that information about the internal mask buffer is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_MASK`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_SECOND_DERIVATIVE_X_ID`

Retrieves information about the internal second derivative buffer in the X-direction.  Note that information about the internal second derivative buffer in the X-direction is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_SECOND_DERIVATIVE_Y_ID`

Retrieves information about the internal second derivative buffer in the Y-direction.  Note that information about the internal second derivative buffer in the Y-direction is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_SECOND_DERIVATIVES_ID`

Retrieves information about the internal second derivative buffers in both the X- and Y-directions.  Note that information about the internal second derivative buffers in both the X- and Y-directions is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_DERIVATIVES`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

### Combination Constants — For specifying the type of information to retrieve from an internal processing buffer

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to specify the type of information to retrieve from the specified internal processing buffer.

Note that to inquire the width or height of the internal processing buffers, use [`M_SIZE_X`](../../Reference/edge/MedgeGetResult.md) or [`M_SIZE_Y`](../../Reference/edge/MedgeGetResult.md).

#### `M_SIGN`

Retrieves the buffer range.

| Value | Description |
| --- | --- |
| `M_SIGNED` | Specifies that the buffer range is signed. |
| `M_UNSIGNED` | Specifies that the buffer range is unsigned. |

#### `M_SIZE_BAND`

Retrieves the number of buffer color bands.

#### `M_SIZE_BIT`

Retrieves the depth per band, in bits.

#### `M_TYPE`

Retrieves the buffer data type and depth. Depth is returned in bits.

| Value | Description |
| --- | --- |
| `M_FLOAT + 32` | Specifies 32-bit float data. |
| `M_SIGNED + 8` | Specifies 8-bit signed data. |
| `M_SIGNED + 16` | Specifies 16-bit signed data. |
| `M_SIGNED + 32` | Specifies 32-bit signed data. |
| `M_UNSIGNED + 1` | Specifies 1-bit unsigned data. |
| `M_UNSIGNED + 8` | Specifies 8-bit unsigned data. |
| `M_UNSIGNED + 16` | Specifies 16-bit unsigned data. |
| `M_UNSIGNED + 32` | Specifies 32-bit unsigned data. |

### For retrieving edge chain results

To retrieve chained edgel results, the[`ResultType`](../../Reference/edge/MedgeGetResult.md) parameter can be set to one of the values below. If [`EdgeIndexOrLabelValue`](../../Reference/edge/MedgeGetResult.md) is set to a specific edge, results are returned for that edge. If [`EdgeIndexOrLabelValue`](../../Reference/edge/MedgeGetResult.md) is set to [`M_ALL`](../../Reference/edge/MedgeGetResult.md), results are returned for all the included edges.

---

### `M_CHAIN`

Retrieves the coordinates of the edge(s)'s edgels.

---

### `M_CHAIN_ANGLE`

Retrieves the angle values of the edge(s)'s edgels.  The angle is always returned in uncalibrated pixel units and is measured counter-clockwise in the image, from the positive X-axis towards the negative Y-axis of the image.  Possible values range from 0 to 360 degrees and mapped in the range of 0 to 255 (that is, an 8-bit range). If [`M_FLOAT_MODE`](../../Reference/edge/MedgeControl.md) is set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md), 0° corresponds to 0, and 360° corresponds to 256. If [`M_FLOAT_MODE`](../../Reference/edge/MedgeControl.md) is set to [`M_DISABLE`](../../Reference/edge/MedgeControl.md), 360° corresponds to 0, and 0° corresponds to 0.  Note that the angle value of the edge at each edgel position is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_ANGLE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_CHAIN_CODE`

Retrieves the edge(s)'s chain code. The chain code describes how edgels are connected using the following neighbor code: *[Image: medgefreeman.png]*

---

### `M_CHAIN_INDEX`

Retrieves the index of each edgel's edge.

---

### `M_CHAIN_MAGNITUDE`

Retrieves the magnitude values of the edge(s)'s edgels.  Note that the magnitude value of the edge at each edgel position is only available if it was saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_MAGNITUDE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_CHAIN_MAGNITUDE + M_CHAIN_ANGLE`

Retrieves the magnitude values and the angle values of the edge(s)'s edgels.  Angles are returned, counter-clockwise, from 0 degrees to 360 degrees and mapped in the range of 0 to 255 (that is, an 8-bit range).  Note that the magnitude and angle values of the edge at each edgel position are only available if they were saved using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_SAVE_CHAIN_MAGNITUDE`](../../Reference/edge/MedgeControl.md) and [`M_SAVE_CHAIN_ANGLE`](../../Reference/edge/MedgeControl.md) set to [`M_ENABLE`](../../Reference/edge/MedgeControl.md).

---

### `M_CHAIN_X`

Retrieves the X-coordinates of the edge(s)'s edgels.

---

### `M_CHAIN_Y`

Retrieves the Y-coordinates of the edge(s)'s edgels.

---

### `M_NUMBER_OF_CHAINED_EDGELS`

Retrieves the number of edgels in the edge(s).

---

### `M_NUMBER_OF_VERTICES`

Retrieves the number of vertices in the chain approximation.

---

### `M_VERTICES`

Retrieves the coordinates of the vertices in the chain approximation.

---

### `M_VERTICES_CHAIN_INDEX`

Retrieves the index of the vertices' corresponding edge, in a chain approximation.

---

### `M_VERTICES_INDEX`

Retrieves the index of the vertices' corresponding edgels, in a chain approximation.

---

### `M_VERTICES_X`

Retrieves the X-coordinates of the vertices in the chain approximation.

---

### `M_VERTICES_Y`

Retrieves the Y-coordinates of the vertices in the chain approximation.

### For retrieving edge feature results

To retrieve edge feature results, the [`ResultType`](../../Reference/edge/MedgeGetResult.md) parameter must be set to one of the values below. Note that the specified feature must have already been calculated with [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md).  If [`EdgeIndexOrLabelValue`](../../Reference/edge/MedgeGetResult.md) is set to an index value or a label value, the edge feature for the specified edge is returned. If [`EdgeIndexOrLabelValue`](../../Reference/edge/MedgeGetResult.md) is set to [`M_ALL`](../../Reference/edge/MedgeGetResult.md), the edge feature for all the included edges in the Edge Finder result buffer is returned.

---

### `M_AVERAGE_STRENGTH`

Retrieves the average strength value of each edge.

---

### `M_BOX_X_MAX`

Retrieves the X-coordinate of each edge's extreme right edgel.

---

### `M_BOX_X_MIN`

Retrieves the X-coordinate of each edge's extreme left edgel.

---

### `M_BOX_Y_MAX`

Retrieves the Y-coordinate of each edge's extreme bottom edgel.

---

### `M_BOX_Y_MIN`

Retrieves the Y-coordinate of each edge's extreme top edgel.

---

### `M_CENTER_OF_GRAVITY`

Retrieves the coordinates of each edge's center of gravity.

---

### `M_CENTER_OF_GRAVITY_X`

Retrieves the X-coordinate of each edge's center of gravity.

---

### `M_CENTER_OF_GRAVITY_Y`

Retrieves the Y-coordinate of each edge's center of gravity.

---

### `M_CIRCLE_FIT_CENTER_X`

Retrieves the X-coordinate of the center of the circle that is the best fit for each edge.

---

### `M_CIRCLE_FIT_CENTER_Y`

Retrieves the Y-coordinate of the center of the circle that is the best fit for each edge.

---

### `M_CIRCLE_FIT_COVERAGE`

Retrieves the coverage of the circle that is the best fit for each edge. The coverage describes the portion of the circle covered by the edge.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the amount of coverage of the circle. The lower the value, the lower the coverage. For example, 0 equals no coverage, 0.5 equals 50 percent coverage, and 1.0 equals 100 percent coverage. |

---

### `M_CIRCLE_FIT_ERROR`

Retrieves the fit error of the circle that is the best fit for each edge. This is calculated as the average quadratic error.

---

### `M_CIRCLE_FIT_RADIUS`

Retrieves the radius of the circle that is the best fit for each edge.

---

### `M_CLOSURE`

Retrieves the closure status of each edge.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies an open edge. |
| `M_TRUE` | Specifies a closed edge. |

---

### `M_CONVEX_PERIMETER`

Retrieves the convex elongation of each edge.

---

### `M_ELLIPSE_FIT_ANGLE`

Retrieves the angle, in degrees, of the ellipse that is the best fit for each edge, relative to the output coordinate system specified using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md).

---

### `M_ELLIPSE_FIT_CENTER_X`

Retrieves the X-coordinate of the center of the ellipse that is the best fit for each edge.

---

### `M_ELLIPSE_FIT_CENTER_Y`

Retrieves the Y-coordinate of the center of the ellipse that is the best fit for each edge.

---

### `M_ELLIPSE_FIT_COVERAGE`

Retrieves the coverage of the ellipse that is the best fit for each edge. The coverage describes the portion of the ellipse covered by the edge.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the amount of coverage of the ellipse. The lower the value, the lower the coverage. For example, 0.0 equals no coverage, 0.5 equals 50 percent coverage, and 1.0 equals 100 percent coverage. |

---

### `M_ELLIPSE_FIT_ERROR`

Retrieves the fit error of the ellipse that is the best fit for each edge.

---

### `M_ELLIPSE_FIT_MAJOR_AXIS`

Retrieves the major axis of the ellipse that is the best fit for each edge. The major axis divides the ellipse across its long dimension into two equal halves.  *[Image: ellipse_major_minor_axis.png]*

---

### `M_ELLIPSE_FIT_MINOR_AXIS`

Retrieves the minor axis of the ellipse that is the best fit for each edge. The minor axis divides the ellipse across its short dimension into two equal halves, perpendicular to the major axis.

---

### `M_FAST_LENGTH`

Retrieves the fast length of each edge.

---

### `M_FERET_BOX`

Retrieves the X- and Y-Feret values of each edge. The X-Feret is the dimension of the minimum bounding box of an edge in the horizontal direction. The Y-Feret is the dimension of the minimum bounding box of an edge in the vertical direction.

---

### `M_FERET_ELONGATION`

Retrieves the Feret elongation of each edge. It is accurate for reasonably compact objects, but becomes less accurate for very elongated objects.

---

### `M_FERET_GENERAL`

Retrieves the general Feret of each edge. This is the Feret diameter calculated at [`M_FERET_GENERAL_ANGLE`](../../Reference/edge/MedgeControl.md), which is set in [`MedgeControl`](../../Reference/edge/MedgeControl.md).

---

### `M_FERET_MAX_ANGLE`

Retrieves the angle, in degrees, at which the maximum Feret diameter is found, relative to the output coordinate system specified using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md).

---

### `M_FERET_MAX_DIAMETER`

Retrieves the maximum Feret diameter of each edge.

---

### `M_FERET_MEAN_DIAMETER`

Retrieves the average Feret diameter at all the angles checked (see [`M_NUMBER_OF_FERETS`](../../Reference/edge/MedgeControl.md)).

---

### `M_FERET_MIN_ANGLE`

Retrieves the angle, in degrees, at which the minimum Feret diameter is found, relative to the output coordinate system specified using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with[`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md).

---

### `M_FERET_MIN_DIAMETER`

Retrieves the minimum Feret diameter of each edge.

---

### `M_FERET_X`

Retrieves the X-Feret value of each edge. This is the dimension of the minimum bounding box of an edge in the horizontal direction.

---

### `M_FERET_Y`

Retrieves the Y-Feret value of each edge. This is the dimension of the minimum bounding box of an edge in the vertical direction.

---

### `M_FIRST_POINT`

Retrieves the coordinates of each edge's first point.

---

### `M_FIRST_POINT_X`

Retrieves the X-coordinate of each edge's first point.

---

### `M_FIRST_POINT_Y`

Retrieves the Y-coordinate of each edge's first point.

---

### `M_LABEL_VALUE`

Retrieves the label value of each edge in an image. The label value is a positive integer greater or equal to one; each edge in an image has a unique label value.

---

### `M_LENGTH`

Retrieves the length of each edge.[`M_LENGTH`](../../Reference/edge/MedgeGetResult.md) gives a more accurate but slower approximation of the edge's length than [`M_FAST_LENGTH`](../../Reference/edge/MedgeGetResult.md).

---

### `M_LINE_FIT_A`

Retrieves the _A_ variable of the line that is the best fit for each edge.

---

### `M_LINE_FIT_B`

Retrieves the _B_ variable of the line that is the best fit for each edge.

---

### `M_LINE_FIT_C`

Retrieves the _C_ variable of the line that is the best fit for each edge.

---

### `M_LINE_FIT_ERROR`

Retrieves the fit error of the line that is the best fit for each edge. This is calculated as the average quadratic error.

---

### `M_MOMENT_ELONGATION`

Retrieves the moment elongation of each edge.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the moment elongation of each edge. Values closer to 1.0 are considered to have a low elongation, while values closer to 0.0 are considered to have a high elongation. For example, a straight edge has a null elongation value, while a circular edge has an elongation of 1.0. |

---

### `M_MOMENT_ELONGATION_ANGLE`

Retrieves the angle, in degrees, of the principal axis along each edge's moment elongation, relative to the output coordinate system specified using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with[`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md).

---

### `M_POSITION`

Retrieves the X- and Y-position of each edge. The position of the edge is defined by the middle edgel of the edge.

---

### `M_POSITION_X`

Retrieves the X-position of each edge. The position of the edge is defined by the middle edgel of the edge.

---

### `M_POSITION_Y`

Retrieves the Y-position of each edge. The position of the edge is defined by the middle edgel of the edge.

---

### `M_SIZE`

Retrieves the number of edgels in each edge.

---

### `M_STRENGTH`

Retrieves the strength value of each edge.

---

### `M_TORTUOSITY`

Retrieves the tortuosity measure of each edge.

---

### `M_X_MAX_AT_Y_MAX`

Retrieves the maximum X-coordinate at the maximum Y-coordinate of each edge. Together with [`M_BOX_Y_MAX`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

---

### `M_X_MIN_AT_Y_MIN`

Retrieves the minimum X-coordinate at the minimum Y-coordinate of each edge. Together with [`M_BOX_Y_MIN`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

---

### `M_Y_MAX_AT_X_MIN`

Retrieves the maximum Y-coordinate at the minimum X-coordinate of each edge. Together with [`M_BOX_X_MIN`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

---

### `M_Y_MIN_AT_X_MAX`

Retrieves the minimum Y-coordinate at the maximum X-coordinate of each edge. Together with [`M_BOX_X_MAX`](../../Reference/edge/MedgeControl.md), this is one of four contact points on the convex perimeter of the edge.

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the indices of the edgels from which the specified Feret was calculated.

#### `M_FIRST_FERET_INDEX`

Retrieves the index of the first edgel from which the specified Feret was calculated.

#### `M_FIRST_FERET_INDEX + M_SECOND_FERET_INDEX`

Retrieves the indices of the first and second edgel from which the specified Feret was calculated.

#### `M_SECOND_FERET_INDEX`

Retrieves the index of the second edgel from which the specified Feret was calculated.

### Combination Constants — For packing values

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the results in a packed format.

If you pack values, result types that return values in both [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) and [`SecondResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) will be returned together, interlaced in [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md). For example, if you decide to pack [`MedgeGetResult`](../../Reference/edge/MedgeGetResult.md) with [`M_CENTER_OF_GRAVITY`](../../Reference/edge/MedgeGetResult.md), [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) will contain both the X- and Y-coordinates of the edge's center of gravity (XY XY XY...).

#### `M_PACKED`

Retrieves the specified values in a packed format. Note that only result types that return values in both [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) and [`SecondResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) can be packed.

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For retrieving statistics about results

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get statistical result information.

Note that results are only returned to [`FirstResultArrayPtr`](../../Reference/edge/MedgeGetResult.md); therefore, [`SecondResultArrayPtr`](../../Reference/edge/MedgeGetResult.md) must be set to [`M_NULL`](../../Reference/edge/MedgeGetResult.md).

#### `M_MAX`

Retrieves the maximum value of the requested result type.

#### `M_MAX_ABS`

Retrieves the maximum absolute value of the requested result type.

#### `M_MEAN`

Retrieves the mean value of the requested result type.

#### `M_MIN`

Retrieves the minimum value of the requested result type.

#### `M_MIN_ABS`

Retrieves the minimum absolute value of the requested result type.

#### `M_STANDARD_DEVIATION`

Retrieves the standard deviation of the requested result type.

### Combination Constants — For determining whether edge feature results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine if the edge feature result is available in the result buffer.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

This result is not valid if [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_CHAIN_APPROXIMATION`](../../Reference/edge/MedgeControl.md) is disabled.

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
