---
doctype: Reference
module: meas
function: MmeasSetMarker
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasSetMarker"
---

# MmeasSetMarker

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

> Set an essential characteristic of a marker.

## Syntax

```c
void MmeasSetMarker(
    AIL_ID     MarkerId,             //out
    AIL_INT64  CharacteristicToSet,  //in
    AIL_DOUBLE FirstValue,           //in
    AIL_DOUBLE SecondValue           //in
)
```

## Description

This function sets an essential characteristic of a marker. Essential characteristics are used in determining the location of an edge, stripe, or circle marker in a target image, or specifying the position of point markers, which are generally used as a reference marker. [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) locates the edges, stripes, or circle that best correspond to the specified marker's characteristics. A marker's characteristics should describe the marker as accurately as possible to ensure that it will be successfully located in an image.

## Parameters

### `MarkerId` *(out, AIL_ID)*

Specifies the identifier of the marker.

### `CharacteristicToSet` *(in, AIL_INT64)*

Specifies the type of characteristic to set.

### `FirstValue` *(in, AIL_DOUBLE)*

Specifies the first value to assign to the characteristic.

### `SecondValue` *(in, AIL_DOUBLE)*

Specifies the second value to assign to the characteristic. If only one value is to be returned, it will be returned to [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) should be set to `M_NULL`.

## Parameter Associations

### For

For [`M_POINT`](../../Reference/meas/MmeasAllocMarker.md), [`M_EDGE`](../../Reference/meas/MmeasAllocMarker.md), [`M_STRIPE`](../../Reference/meas/MmeasAllocMarker.md), and [`M_CIRCLE`](../../Reference/meas/MmeasAllocMarker.md) markers, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_MARKER_REFERENCE`

Sets the offset at which to place the marker's reference position, relative to the marker's actual position. The reference position is only used in calculations between two markers ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)).  - For point markers, Aurora Imaging Library measures the specified offset relative to the X- and Y-coordinates set for the point using [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md). - For edge markers, Aurora Imaging Library measures the specified offset relative to the X- and Y-coordinates of the edge's maximum edgevalue. - For stripe markers, Aurora Imaging Library measures the specified offset relative to the X- and Y-coordinates at the center of a theoretical line between the position (maximum edgevalue) of the stripe's two outermost edges. - For circle markers, Aurora Imaging Library measures the specified offset relative to the X- and Y-coordinates at the circle's center.  By default, the marker's reference position is the same as its actual position (the default offset is 0).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-offset value, in the same units as the input coordinate system specified using [`M_MARKER_REFERENCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-offset value, in the same units as the input coordinate system specified using [`M_MARKER_REFERENCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_MARKER_REFERENCE_INPUT_UNITS`

Sets the units with which to interpret [`M_MARKER_REFERENCE`](../../Reference/meas/MmeasSetMarker.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) to draw expected marker characteristics or [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) generates an error if the result was not calculated on a calibrated image. |

### For

For [`M_POINT`](../../Reference/meas/MmeasAllocMarker.md), [`M_EDGE`](../../Reference/meas/MmeasAllocMarker.md), and [`M_STRIPE`](../../Reference/meas/MmeasAllocMarker.md) markers, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_NUMBER`

Sets the number of edges or stripes to locate in the box search region, or the number of points to explicitly define (points are typically used as a reference marker). In the first case, this can be referred to as the number of marker occurrences.  When [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md) is set to a value greater than 1, the marker is considered a multiple-occurrence marker. Note that a circle marker cannot be set as a multiple-occurrence marker.  For edges and stripes, unless a minimum number ([`M_NUMBER_MIN`](../../Reference/meas/MmeasSetMarker.md)) is specified, no results will be returned if the number of marker occurrences found falls below [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies that the search should locate all edges or stripes in the box search region. This setting cannot be used for a point marker.  If using [`M_ALL`](../../Reference/meas/MmeasSetMarker.md) and [`M_NUMBER_MIN`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), [`M_NUMBER_MIN`](../../Reference/meas/MmeasSetMarker.md) is ignored and Aurora Imaging Library will find as many marker occurrences as it can in the image. However, if [`M_NUMBER_MIN`](../../Reference/meas/MmeasSetMarker.md) is set to a value other than [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), this value is respected and results are returned, provided that the specified minimum number of markers are found.  > **Note:** Note that when working with a very large image with many stripes, searching for an unknown number of stripes can significantly increase processing time. |
| `Value` *(default)* | Specifies the number of edges or stripes to locate, or point markers to define. |

### For

For [`M_EDGE`](../../Reference/meas/MmeasAllocMarker.md), [`M_STRIPE`](../../Reference/meas/MmeasAllocMarker.md), and [`M_CIRCLE`](../../Reference/meas/MmeasAllocMarker.md) markers, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_EDGEVALUE_MIN`

Sets the minimum edgevalue required for it to be considered an occurrence of the marker.  *[Image: edgedetectionedgevalueminandvarmin2.png]*  Aurora Imaging Library establishes edgevalues by taking the normalized first derivative of the intensity profile, which generally resembles a peak when an edge is present. When searching for multiple edges, you might need to further constrain the edgevalues that Aurora Imaging Library considers, using [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum edgevalue.  A setting of 0.0 indicates that Aurora Imaging Library considers all edgevalues, while a setting of 100.0 indicates that Aurora Imaging Library only considers the greatest possible edgevalue. The greatest possible edgevalue results from a perfectly straight, perfectly white to perfectly black (or vice versa) grayscale transition over one pixel. If you set the minimum edgevalue to a very low value, [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) can take longer to execute. |

---

### `M_EDGEVALUE_VAR_MIN`

Sets the minimum prominence required for an edge peak, for it to be considered an occurrence of the marker. To determine if the minimum prominence is met, Aurora Imaging Library establishes a local threshold for each peak by subtracting [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) from it.  *[Image: edgedetectionedgevalueminandvarmin4.png]*  The portion of the profile above the local threshold that contains the edge peak can be referred to as the peak's interval, provided that the profile eventually dips below the local threshold on both sides of its edge peak. For Aurora Imaging Library to consider a peak to be an edge, there must be no edgevalues greater than that peak within its interval. If the local threshold calculated with [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) is less than [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md), the local threshold equals [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md).  [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) is typically used by advanced applications where you are trying to find multiple neighboring edges in the search region, which are formed such that the foreground of the first edge becomes the background of the next edge.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum prominence.  A setting of 0.0 indicates no prominence; that is, Aurora Imaging Library only considers the maximum edgevalue of the peak's interval. A setting of 100.0 (default) indicates the greatest possible prominence; that is, Aurora Imaging Library considers all edgevalues of the peak's interval that are above [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md). In this case, [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) has no effect. |

---

### `M_FILTER_SMOOTHNESS`

Sets the degree of smoothness (strength of denoising) applied to the internal projection buffer of the search region during the edge extraction. The recursive edge extraction process involves a denoising operation.  Note that [`M_FILTER_SMOOTHNESS`](../../Reference/meas/MmeasSetMarker.md) is only available if [`M_FILTER_TYPE`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_SHEN`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value.  A value of 100.0 results in a strong noise reduction effect, while a value of 0.0 has almost no noise reduction effect. |

---

### `M_FILTER_TYPE`

Sets the type of the first derivative filter with which to perform the edge extraction. The edgevalue calculated for each projection value of the search region is dependent on this setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EULER` *(default)* | Specifies an Euler FIR filter. This filter performs a non-recursive edge extraction operation with a predefined kernel of size 2: [-1,1].  This filter is faster but more sensitive to noise, compared to [`M_PREWITT`](../../Reference/meas/MmeasSetMarker.md) filter. |
| `M_PREWITT` | Specifies a Prewitt FIR filter. This filter performs a non-recursive edge extraction operation with a predefined kernel of size 3: [-1,0,1].  This filter is slower but less sensitive to noise, compared to [`M_EULER`](../../Reference/meas/MmeasSetMarker.md) filter. |
| `M_SHEN` | Specifies a Shen-Castan Infinite Support Exponential filter. This is an exponential weighting function, of the general form:  *[Image: ShenFunction.png]*  This filter performs a recursive edge extraction operation using an IIR filter. If this filter actually used a kernel, the kernel size would be theoretically infinite.  This IIR filter is slower than FIR filters ([`M_EULER`](../../Reference/meas/MmeasSetMarker.md) or [`M_PREWITT`](../../Reference/meas/MmeasSetMarker.md)). However, this filter is less sensitive to noise and provides more accurate results. You can also control the strength of denoising applied to the projection buffer of the search region during the edge extraction, using the [`M_FILTER_SMOOTHNESS`](../../Reference/meas/MmeasSetMarker.md) setting. |

---

### `M_MAX_ASSOCIATION_DISTANCE`

Sets the maximum distance between a marker's edge (either straight or circular) and its associated subedges. The subedges' positions are points extracted from the marker's subregions that are used during the fit operation. For edge and stripe markers, [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) is measured perpendicular from the fitted edge position, along to the search direction. For circle markers, [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) is measured radially from the fitted circle perimeter.  Subedges falling outside the specified maximum distance are considered outliers and are not used to perform the marker's line or circle fit operation. For edge and stripe markers, this leads to fitted edges more in line with the marker orientation and, for circle markers, this ensures that the fitted circle will be fitted on subedges roughly at the same distance from the fitted circle center.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MAX_POSSIBLE_VALUE` *(default)* | Specifies that there is no maximum distance; the entire search region is searched for subedges. |
| `Value` | Specifies the distance, in the units of the input coordinate system specified using [`M_MAX_ASSOCIATION_DISTANCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_MAX_ASSOCIATION_DISTANCE_INPUT_UNITS`

Sets the units with which to interpret [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) to draw expected marker characteristics or [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_POLARITY`

Specifies the polarity of the marker. Polarity indicates whether an edge must be a rising edge (increase in grayscale value) or a falling edge (decrease in grayscale value).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that polarity is not considered. |
| `M_NEGATIVE` | Specifies that the polarity of the edge is negative (falling edge). For circle markers, this indicates that the inside of the ring-shaped region has a greater grayscale value (falling edge from inside to outside). |
| `M_POSITIVE` | Specifies that the polarity of the edge is positive (rising edge). For circle markers, this indicates that the inside of the ring-shaped region has a lower grayscale value (rising edge from inside to outside). |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies that this parameter does not apply.  This setting must be used for edge and circle markers. |
| `M_ANY` | Specifies that the polarity is not considered. |
| `M_NEGATIVE` | Specifies that the polarity of the second edge is negative (falling edge). |
| `M_OPPOSITE` *(default)* | Specifies that the polarity of the second edge of a stripe marker is the opposite of that of the first edge. |
| `M_POSITIVE` | Specifies that the polarity of the second edge is positive (rising edge). |
| `M_SAME` | Specifies that the polarity of the second edge of a stripe marker is the same as that of the first edge. |

---

### `M_SEARCH_REGION_CLIPPING`

Sets whether Aurora Imaging Library can automatically clip the marker's search region, when it falls outside the image.  > **Note:** This setting is ignored unless the search region's size is explicitly set. That is, the values specified with [`M_BOX_SIZE`](../../Reference/meas/MmeasSetMarker.md) are not set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the search region cannot be automatically clipped. In this case, if the search region falls outside the image, the marker cannot be found (unless you are using subregions). |
| `M_MAXIMIZE_AREA` | Specifies that, if the search region falls outside the image, Aurora Imaging Library can automatically clip the region according to the best valid area in which to search, and the marker can still be found. The modified region is internally created by Aurora Imaging Library; the actual search region that you defined is never altered.  The automatically determined region must be inscribed by the defined search region, must be within the boundaries of the image, and must meet the specified [`M_SEARCH_REGION_CLIPPING_...`](../../Reference/meas/MmeasSetMarker.md) conditions. Aurora Imaging Library will not clip the region if it cannot adhere to all of these requirements. |

---

### `M_SEARCH_REGION_CLIPPING_MIN_AREA`

Sets the minimum area requirement for the clipped region that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum area required for the clipped region, in pixels. |

---

### `M_SEARCH_REGION_CLIPPING_MIN_HEIGHT`

Sets the minimum width requirement for the clipped region that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum height required for the clipped region, in pixels. |

---

### `M_SEARCH_REGION_CLIPPING_MIN_WIDTH`

Sets the minimum width requirement for the clipped region that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the minimum width required for the clipped region, in pixels. |

---

### `M_SEARCH_REGION_CLIPPING_PRESERVE_CENTER`

Sets the center requirement for the clipped region that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md). All the settings are available to edge and stripe markers, but only [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md) and [`M_AUTO`](../../Reference/meas/MmeasSetMarker.md) are available to circle markers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALONG_HEIGHT` | Specifies that the center, along the height of the defined search region, must be preserved in the clipped region. This setting is not available for circle markers. |
| `M_ALONG_WIDTH` | Specifies that the center, along the width of the defined search region, must be preserved in the clipped region. This setting is not available for circle markers. |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library will determine how the center of the defined search region will be preserved in the clipped region. For edge and stripe markers, [`M_AUTO`](../../Reference/meas/MmeasSetMarker.md) is the same as [`M_NONE`](../../Reference/meas/MmeasSetMarker.md). For circle markers, [`M_AUTO`](../../Reference/meas/MmeasSetMarker.md) is the only possible value. |
| `M_NONE` | Specifies that the clipped region need not preserve the center of the defined search region. This setting is not available for circle markers. |

---

### `M_SEARCH_REGION_INPUT_UNITS`

Sets the units with which to interpret [`M_BOX_CENTER`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ORIGIN`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_SIZE`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ANGLE_ACCURACY`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ANGLE_TOLERANCE`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ANGLE_DELTA_POS`](../../Reference/meas/MmeasSetMarker.md), [`M_BOX_ANGLE_DELTA_NEG`](../../Reference/meas/MmeasSetMarker.md), [`M_RING_CENTER`](../../Reference/meas/MmeasSetMarker.md), [`M_RING_RADII`](../../Reference/meas/MmeasSetMarker.md), and [`M_SUB_REGIONS_CHORD_ANGLE`](../../Reference/meas/MmeasSetMarker.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) to draw expected marker characteristics or [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_SUB_REGIONS_NUMBER`

Sets the number of sections in which to divide the search region. Since a subregion of each section is processed, this number also corresponds to the number of subregions to process.  For circle markers, this value will give each subregion the same area; that is, each subregion will occupy `(360/[`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md))<sup>o</sup>` of the circle.  For edge and stripe markers, if you specify one subregion when multiple subregions are required (for example, when using [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) with [`M_DEFAULT`](../../Reference/meas/MmeasFindMarker.md), [`M_ANGLE`](../../Reference/meas/MmeasFindMarker.md), or [`M_LINE_EQUATION`](../../Reference/meas/MmeasFindMarker.md)), Aurora Imaging Library internally uses three subregions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For edge and stripe markers, the default is one. For circle markers, the default is eight. |
| `1 <= Value < 8` | Specifies a number of sections (subregions) that can only be used for edge and stripe markers. |
| `Value >= 8` | Specifies a number of sections (subregions) that can be used for edge, stripe, and circle markers. |

---

### `M_SUBPIXEL_MODE`

Sets how Aurora Imaging Library calculates subpixel positions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL` | Specifies that Aurora Imaging Library calculates subpixel positions based on the entire region within which the edge is found. This mode is recommended when measuring wide edges. |
| `M_LOCAL` *(default)* | Specifies that Aurora Imaging Library calculates subpixel positions based on the immediate neighborhood within which the edge's maximum edgevalue is found. This mode offers more precision, but can be less stable with wide edges. |

### For

For [`M_EDGE`](../../Reference/meas/MmeasAllocMarker.md) and [`M_STRIPE`](../../Reference/meas/MmeasAllocMarker.md) markers, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_BOX_ANGLE`

Sets the angle of the box search region.  The center of rotation of the box search region is set with [`M_BOX_ANGLE_REFERENCE`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies that the content of the box search region is analyzed to automatically determine the angle of the marker. |
| `0 <= Value <= 360` *(default)* | Specifies the angle of the box search region, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md).  An angle interpreted using the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle according to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). |

---

### `M_BOX_ANGLE_ACCURACY`

Sets the accuracy of the angular search.  This determines the size of the step angle to use once the approximate location of the marker is found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies the angle of accuracy to be equal to the angle of tolerance. |
| `0.1 <= Value <= 180.0` | Specifies the accuracy of the angular search, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_ANGLE_DELTA_NEG`

Sets the negative range of angles within which to search for the marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default negative range of angles, in degrees.  For a symmetrical stripe marker (a marker set with [`M_OPPOSITE`](../../Reference/meas/MmeasSetMarker.md)), the default is 180.0.  For an edge marker or a non-symmetrical stripe marker, the default is 360.0. |
| `0.1 <= Value <= 360.0` | Specifies the negative range of angles, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_ANGLE_DELTA_POS`

Sets the positive range of angles within which to search for the marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default positive range of angles, in degrees.  For a symmetrical stripe marker (a marker set with [`M_OPPOSITE`](../../Reference/meas/MmeasSetMarker.md)), the default is 180.0.  For an edge marker or a non-symmetrical stripe marker, the default is 360.0. |
| `0.1 <= Value <= 360.0` | Specifies the positive range of angles, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_ANGLE_MODE`

Enables the use of multiple-angle search, as specified by the settings of other [`M_BOX_ANGLE...`](../../Reference/meas/MmeasSetMarker.md) characteristics. The angle of the marker with the highest match score is returned. For an edge marker, this is the score of the edge, and for a stripe marker, this is the mean score of the edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the search should only be performed at the angle specified by [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md). |
| `M_ENABLE` | Specifies that multiple-angle search is enabled. |

---

### `M_BOX_ANGLE_REFERENCE`

Sets the center of rotation to use when [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) is not zero.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BOX_CENTER` *(default)* | Specifies the center of the box search region as the center of rotation. Use the [`M_BOX_CENTER`](../../Reference/meas/MmeasSetMarker.md) characteristic to set the coordinates of the center of the box search region. |
| `M_BOX_ORIGIN` | Specifies the top-left corner of the box search region as the center of rotation. Use the [`M_BOX_ORIGIN`](../../Reference/meas/MmeasSetMarker.md) characteristic to set the coordinates of the top-left corner of the box search region. |

---

### `M_BOX_ANGLE_TOLERANCE`

Sets the rotation tolerance of the marker.  This is the full range of degrees within which a marker can be rotated from a box search region that is at a specific angle and still be found. This determines the step angle used for a multiple-angle search.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.1 <= Value <= 360.0` *(default)* | Specifies the rotation tolerance, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_CENTER`

Sets the coordinates of the center of the box search region.  When you change [`M_BOX_CENTER`](../../Reference/meas/MmeasSetMarker.md), Aurora Imaging Library recalculates [`M_BOX_ORIGIN`](../../Reference/meas/MmeasSetMarker.md), relative to the new center position.  *[Image: box6.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default X-coordinate of the center of the box search region. This depends on the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). If using pixel units, the default center is the center of the pixel coordinate system. If using world units, the default center is the origin of the relative coordinate system. |
| `Value` | Specifies the X-coordinate of the center of the box search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` *(default)* | Specifies the default Y-coordinate of the center of the box search region. This depends on the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). If using pixel units, the default center is the center of the pixel coordinate system. If using world units, the default center is the origin of the relative coordinate system. |
| `Value` | Specifies the Y-coordinate of the center of the box search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_ORIGIN`

Sets the coordinates of the top-left corner of the box search region.  When you change [`M_BOX_ORIGIN`](../../Reference/meas/MmeasSetMarker.md), Aurora Imaging Library recalculates [`M_BOX_CENTER`](../../Reference/meas/MmeasSetMarker.md), relative to the new box origin.  *[Image: box7.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default X-coordinate of the origin of the box search region. The default origin is the origin of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `Value` | Specifies the X-coordinate of the origin of the box search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` | Specifies the default Y-coordinate of the origin of the box search region. The default origin is the origin of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `Value` | Specifies the Y-coordinate of the origin of the box search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_BOX_SIZE`

Sets the width and height of the box search region.  > **Note:** If [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md), the Measurement module uses the world dimensions specified to estimate a rectangular box in pixel units. The module will then use this approximation for internal calculations.  Note that the search region must fit entirely inside the target image, unless subregions are used or clipping is enabled. If don't use subregions or enable clipping, and you choose a location, size, or search angle that causes the box to extend beyond the edge of the target image, the marker cannot be found ([`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_VALID_FLAG`](../../Reference/meas/MmeasGetResult.md) will return [`M_FALSE`](../../Reference/meas/MmeasGetResult.md)).  When you set the width and height of the search region, the search region expands or shrinks with respect to its origin, set with [`M_BOX_ORIGIN`](../../Reference/meas/MmeasSetMarker.md). When the width or height changes, Aurora Imaging Library recalculates [`M_BOX_CENTER`](../../Reference/meas/MmeasSetMarker.md), relative to the box search region's origin.  *[Image: box5.png]*  Note that if you have enabled clipping, the size of the box search region internally use might be smaller than the specified size. This is only a temporary effect; it does not change the specified size. For more information, see [Search region](../../UserGuide/C19_Measurements/S06_Search_region.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the width of the entire target image. If you specify [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), and [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md), you must also set the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md); otherwise, an error will occur. |
| `Value` | Specifies the width of the box search region within the target image, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). This value must be positive. |
| `M_DEFAULT` *(default)* | Specifies to use the height of the entire target image. If you specify [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), and [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md), you must also set the [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) parameter to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md); otherwise, an error will occur. |
| `Value` | Specifies the height of the box search region within the target image, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). This value must be positive. |

---

### `M_DRAW_PROFILE_SCALE_OFFSET`

Sets the scale and offset to use when drawing the edge profile or edgevalue characteristics using [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_EDGES_PROFILE`](../../Reference/meas/MmeasDraw.md), [`M_DRAW_EDGEVALUE_MIN_IN_PROFILE`](../../Reference/meas/MmeasDraw.md), [`M_DRAW_EDGEVALUE_PEAK_WIDTH_IN_PROFILE`](../../Reference/meas/MmeasDraw.md), or [`M_DRAW_POSITION_IN_PROFILE`](../../Reference/meas/MmeasDraw.md).  *[Image: EdgesProfileScaleOffset.png]*

| Value | Description |
| --- | --- |
| `M_AUTO_SCALE_PROFILE` *(default)* | Specifies to automatically compute the greatest scale factor that will fit the entire drawing within the destination buffer (or within the area that corresponds to the search region, if [`M_DRAW_IN_BOX`](../../Reference/meas/MmeasDraw.md) is combined when drawing) after the offset has been applied. |
| `Value > 0.0` | Specifies the scale multiplier. |
| `M_AUTO_OFFSET_PROFILE` *(default)* | Specifies to automatically compute the offset such that the maximum and minimum of the drawing are touching the ends of the destination buffer (or the area that corresponds to the search region, if [`M_DRAW_IN_BOX`](../../Reference/meas/MmeasDraw.md) is combined when drawing). This value can only be used when [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) is set to[`M_AUTO_SCALE_PROFILE`](../../Reference/meas/MmeasSetMarker.md).  Note that this value is optimized for [`M_DRAW_EDGES_PROFILE`](../../Reference/meas/MmeasDraw.md). Using [`M_AUTO_OFFSET_PROFILE`](../../Reference/meas/MmeasSetMarker.md) with other [`M_DRAW_..._PROFILE`](../../Reference/meas/MmeasDraw.md) values might draw outside of the destination (image buffer or the area corresponding to the search region). |
| `Value` | Specifies the offset from the middle of the height of the destination buffer or the area corresponding to the search region. The ends of the destination buffer or the area corresponding to the search region correspond to offsets -100 and 100. There is no limit to the offset value. However, large values might cause the required drawing to be drawn outside the visible area. |

---

### `M_NUMBER_MIN`

Sets the minimum number of edges or stripes to locate in the box search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  The default number of edges or stripes is the same as the value set in [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md). |
| `Value` | Specifies the minimum number of edges or stripes to locate. |

---

### `M_ORIENTATION`

Sets the orientation of the edge or stripe marker.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` | Specifies that the orientation of the marker is unknown; both horizontal and vertical orientations are searched. |
| `M_HORIZONTAL` | Specifies that the marker has a horizontal orientation. |
| `M_VERTICAL` *(default)* | Specifies that the marker has a vertical orientation. |

---

### `M_SEARCH_REGION_ANGLE_INTERPOLATION_MODE`

Sets the type of interpolation to use when searching for markers. This only has an effect when either [`M_BOX_ANGLE_MODE`](../../Reference/meas/MmeasSetMarker.md) is enabled or [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) is set to a value other than 0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated. |
| `M_BILINEAR` *(default)* | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

---

### `M_SUB_REGIONS_OFFSET`

Sets the offset of the subregions, from the center point of each of their sections.  Specify the offset as a percentage of the distance from the center point of the section to the maximum possible offset in each section. 100 corresponds to the maximum possible offset. The direction of the offset (X or Y) is taken from [`M_ORIENTATION`](../../Reference/meas/MmeasSetMarker.md), with positive values shifting the subregion down or to the right, and negative values shifting up or to the left.  *[Image: subregion_offset.png]*  Note that subregions are offset as a group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-100 <= Value <= 100` *(default)* | Specifies the offset of the subregions. |

---

### `M_SUB_REGIONS_SIZE`

Sets the size of the subregions, as a percentage of their section.  Subregions are always rectangular and extend from one edge of the box search region to the other in the orientation direction. If made smaller than their section, the subregions become thinner and occupy the specified percentage of their section. In this case, subregions will remain centered unless moved with [`M_SUB_REGIONS_OFFSET`](../../Reference/meas/MmeasSetMarker.md).  Note that the size of subregions are set as a group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the size of the subregions, as a percentage. If the size is set to 100, subregions will be evenly distributed within the box search region, with no space between them. |

### For

For [`M_POINT`](../../Reference/meas/MmeasAllocMarker.md) markers only, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_MULTIPLE_POINT_ANGLE`

Sets the angle at which subsequent points are placed, in a theoretical line, for a multiple point marker (at the interval set with [`M_SPACING`](../../Reference/meas/MmeasSetMarker.md) and proceeding from the coordinates defined by [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 360` *(default)* | Specifies the angle for a multiple point marker, in degrees, in the units of the input coordinate system specified using [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md).  An angle interpreted according to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle according to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). |

---

### `M_POINT_INPUT_UNITS`

Sets the units with which to interpret [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md), [`M_SPACING`](../../Reference/meas/MmeasSetMarker.md), and [`M_MULTIPLE_POINT_ANGLE`](../../Reference/meas/MmeasSetMarker.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) to draw expected marker characteristics generates an error if the operation is not performed on a calibrated image. |

---

### `M_POSITION`

Sets the coordinates of a point marker.  For a multiple point marker, this defines the position of the first point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the position is invalid. When a point marker is allocated, [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_ANY`](../../Reference/meas/MmeasSetMarker.md). After allocation, you must explicitly set a position (X-coordinate, Y-coordinate); otherwise, an error will occur. |
| `Value` | Specifies the X-coordinate of the point marker, in the units of the input coordinate system specified using [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the position is invalid. When a point marker is allocated, [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_ANY`](../../Reference/meas/MmeasSetMarker.md). After allocation, you must explicitly set a position (X-coordinate, Y-coordinate); otherwise, an error will occur. |
| `Value` | Specifies the Y-coordinate of the point marker, in the units of the input coordinate system specified using [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_SPACING`

Sets the spacing (distance) between points of a point marker. This setting is only available for multiple point markers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the spacing is invalid. When a point marker is allocated, [`M_SPACING`](../../Reference/meas/MmeasSetMarker.md) is set to [`M_ANY`](../../Reference/meas/MmeasSetMarker.md). After allocation, you must explicitly set the spacing; otherwise, an error will occur. |
| `Value` | Specifies the spacing, in the units of the input coordinate system specified using [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

### For

For [`M_STRIPE`](../../Reference/meas/MmeasAllocMarker.md) markers only, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_INCLUSION_POINT`

Sets a position (inclusion point) in the target image that must either be included or not included in the stripe marker found, depending on [`M_INCLUSION_POINT_INSIDE_STRIPE`](../../Reference/meas/MmeasSetMarker.md). If you specify an inclusion point, you must also set [`M_INCLUSION_POINT_INSIDE_STRIPE`](../../Reference/meas/MmeasSetMarker.md) to [`M_YES`](../../Reference/meas/MmeasSetMarker.md) or [`M_NO`](../../Reference/meas/MmeasSetMarker.md); otherwise, the specified inclusion point is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies no inclusion point. In this case, [`M_INCLUSION_POINT_INSIDE_STRIPE`](../../Reference/meas/MmeasSetMarker.md) is ignored. |
| `Value` | Specifies the inclusion point's X-coordinate, in the units of the input coordinate system specified using [`M_INCLUSION_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies no inclusion point. In this case, [`M_INCLUSION_POINT_INSIDE_STRIPE`](../../Reference/meas/MmeasSetMarker.md) is ignored. |
| `Value` | Specifies the inclusion point's Y-coordinate, in the units of the input coordinate system specified using [`M_INCLUSION_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_INCLUSION_POINT_INPUT_UNITS`

Sets the units with which to interpret [`M_INCLUSION_POINT`](../../Reference/meas/MmeasSetMarker.md). This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, according to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, according to the relative coordinate system. If world units are specified, calling [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) to draw expected marker characteristics or [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_INCLUSION_POINT_INSIDE_STRIPE`

Sets whether the stripe marker found must contain the inclusion point ([`M_INCLUSION_POINT`](../../Reference/meas/MmeasSetMarker.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that [`M_INCLUSION_POINT`](../../Reference/meas/MmeasSetMarker.md) is ignored. |
| `M_NO` | Specifies that only stripes that have the inclusion point falling outside its two outer edges can be found. |
| `M_YES` | Specifies that only stripes that have the inclusion point falling between its two outer edges can be found. |

### For

For [`M_CIRCLE`](../../Reference/meas/MmeasAllocMarker.md) markers only, the [`CharacteristicToSet`](../../Reference/meas/MmeasSetMarker.md) and corresponding [`FirstValue`](../../Reference/meas/MmeasSetMarker.md) and [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameters can be set to the following.  Note that the [`SecondValue`](../../Reference/meas/MmeasSetMarker.md) parameter should be set to [`M_NULL`](../../Reference/meas/MmeasSetMarker.md) if no parameter setting is required.

---

### `M_CIRCLE_ACCURACY`

Sets the accuracy of the circle's fit operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_HIGH` *(default)* | Specifies that the circle's fit operation will be performed with high accuracy. This typically takes longer than low accuracy, but can produce more precise results. |
| `M_LOW` | Specifies that the circle's fit operation will be performed with low accuracy. This is typically faster than high accuracy, but can produce less precise results, particularly with noisy or badly contrasted images. |

---

### `M_CIRCLE_INSIDE_SEARCH_REGION`

Sets whether the circle's fit operation can result in a circle that is outside the ring search region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the circle's fit operation can result in a circle that is outside the ring search region. |
| `M_ENABLE` | Specifies that the circle's fit operation cannot result in a circle that is outside the ring search region. |

---

### `M_RING_CENTER`

Sets the coordinates of the center of the ring search region. The ring's center is considered to be its position.  Note that at least three valid subregions must be within the target image for the circle marker to be found (even if clipping is enabled).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default X-coordinate of the center of the ring search region. This depends on the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). If using pixel units, the default center is the center of the pixel coordinate system. If using world units, the default center is the origin of the relative coordinate system. |
| `Value` | Specifies the X-coordinate of the center of the ring search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` *(default)* | Specifies the default Y-coordinate of the center of the ring search region. This depends on the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). If using pixel units, the default center is the center of the pixel coordinate system. If using world units, the default center is the origin of the relative coordinate system. |
| `Value` | Specifies the Y-coordinate of the center of the ring search region, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_RING_RADII`

Sets the inner and outer radius of the ring search region. The radii values are set according to the region's center ([`M_RING_CENTER`](../../Reference/meas/MmeasSetMarker.md)). The inner and outer radius must be set concurrently. The inner radius must be less than the outer radius; this creates a ring within which the circle must be found.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies an inner radius of 0.0. If the outer radius is set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), the inner radius must also be set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md); otherwise, an error will occur. |
| `Value >= 0` | Specifies the inner radius, relative to the center of the ring search region, in the units specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |
| `M_DEFAULT` *(default)* | Specifies the default outer radius. This value corresponds to either half the target image's width (X-size x 0.5) or half its height (Y-size x 0.5), depending on which is smaller. Note that if the outer radius is set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md), the inner radius must also be set to [`M_DEFAULT`](../../Reference/meas/MmeasSetMarker.md); otherwise, an error will occur. |
| `Value > Inner radius` | Specifies the outer radius, relative to the center of the ring search region, in the units specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md). |

---

### `M_SUB_REGIONS_CHORD_ANGLE`

Sets the angle with which to set the ring search region's outer ring segment (chord). The projection of this chord segment determines the dimension of the rectangular subregion that is perpendicular to radial direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < Value <= 45.0` *(default)* | Specifies the angle, in degrees, in the units of the input coordinate system specified using [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md).  An angle interpreted according to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle according to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). |

The search is performed between the range of angles defined by: ([`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) - [`M_BOX_ANGLE_DELTA_NEG`](../../Reference/meas/MmeasSetMarker.md)) to ([`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) + [`M_BOX_ANGLE_DELTA_POS`](../../Reference/meas/MmeasSetMarker.md)), inclusively, starting with an angle closest to that of [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md).

Note that the search region must fit entirely inside the target image, unless subregions are used or clipping is enabled. If don't use subregions or enable clipping, and you choose a location, size, or search angle that causes the box to extend beyond the edge of the target image, the marker cannot be found ([`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_VALID_FLAG`](../../Reference/meas/MmeasGetResult.md) will return [`M_FALSE`](../../Reference/meas/MmeasGetResult.md)).
