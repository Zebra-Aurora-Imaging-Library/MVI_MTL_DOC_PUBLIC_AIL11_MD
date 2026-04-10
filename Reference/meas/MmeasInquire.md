---
doctype: Reference
module: meas
function: MmeasInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasInquire"
---

# MmeasInquire

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

> Inquire about a measurement context, marker, or result buffer.

## Syntax

```c
AIL_INT MmeasInquire(
    AIL_ID    MeasId,           //in
    AIL_INT64 InquireType,      //in
    void *    FirstUserVarPtr,  //out
    void *    SecondUserVarPtr  //out
)
```

## Description

This function inquires information about an expected marker characteristic, context setting, or measurement result buffer setting. To specify a marker characteristic, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md); to specify settings for a measurement context or measurement result buffer, use [`MmeasControl`](../../Reference/meas/MmeasControl.md).

To retrieve the results of a find marker or calculate measurement operation ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) or [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)), use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/meas/MmeasControl.md) (for example, in [`MmeasControl`](../../Reference/meas/MmeasControl.md)), [`MmeasInquire`](../../Reference/meas/MmeasInquire.md) will return [`M_DEFAULT`](../../Reference/meas/MmeasControl.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/meas/MmeasInquire.md) to the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter.

## Parameters

### `MeasId` *(in, AIL_ID)*

Specifies the identifier of the marker, measurement context, or measurement result buffer about which to inquire information.

*For specifying the measurement object identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default measurement context of the current Aurora Imaging Library application. |
| `Measurement context ID` | Specifies a measurement context. |
| `Measurement marker ID` | Specifies a measurement marker. |
| `Measurement result buffer ID` | Specifies a measurement result buffer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `FirstUserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MmeasInquire`](../../Reference/meas/MmeasInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

### `SecondUserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. If only one value is to be returned, it will be returned to [`FirstUserVarPtr`](../../Reference/meas/MmeasInquire.md) and [`SecondUserVarPtr`](../../Reference/meas/MmeasInquire.md) should be set to [`M_NULL`](../../Reference/meas/MmeasInquire.md).

## Parameter Associations

### For point, edge, stripe, and circle markers, and for a measurement result buffer

For point, edge, and stripe markers, and for measurement result buffers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| *(see [`M_RESULT_OUTPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

### For point, edge, stripe, and circle markers

For point, edge, stripe, and circle markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_MARKER_REFERENCE`

Inquires the offset with which to establish the marker's reference position, relative to the marker's actual position.

---

### `M_MARKER_REFERENCE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_MARKER_REFERENCE`](../../Reference/meas/MmeasInquire.md) inquire type.

| Value | Description |
| --- | --- |
| *(see [`M_MARKER_REFERENCE_INPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_MARKER_TYPE`

Inquires the type of marker.

| Value | Description |
| --- | --- |
| `M_CIRCLE` | Specifies a circle marker buffer. |
| `M_EDGE` | Specifies an edge marker buffer. |
| `M_POINT` | Specifies a point marker buffer. |
| `M_STRIPE` | Specifies a stripe marker buffer. |

### For point, edge, and stripe markers

For point, edge, and stripe markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_NUMBER`

Inquires the number of edges or stripes to locate, or point markers defined.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER`](Reference/meas/MmeasSetMarker.md))* |  |

### For edge, stripe, and circle markers

For edge, stripe, and circle markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_EDGEVALUE_MIN`

Inquires the minimum edgevalue required for it to be considered an occurrence of the marker.

| Value | Description |
| --- | --- |
| *(see [`M_EDGEVALUE_MIN`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_EDGEVALUE_VAR_MIN`

Inquires the minimum prominence required for an edge peak, for it to be considered an occurrence of the marker.

| Value | Description |
| --- | --- |
| *(see [`M_EDGEVALUE_VAR_MIN`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_FILTER_SMOOTHNESS`

Inquires the degree of smoothness (strength of denoising) applied to the internal projection buffer of the search region during the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_SMOOTHNESS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_FILTER_TYPE`

Inquires the type of the first derivative filter with which to perform the edge extraction.

| Value | Description |
| --- | --- |
| *(see [`M_FILTER_TYPE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_MAX_ASSOCIATION_DISTANCE`

Inquires the maximum distance between an edge (either straight or circular) and its associated subedges.

---

### `M_MAX_ASSOCIATION_DISTANCE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasInquire.md) inquire type.

| Value | Description |
| --- | --- |
| *(see [`M_MAX_ASSOCIATION_DISTANCE_INPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_POLARITY`

Inquires the polarity of the marker.

| Value | Description |
| --- | --- |
| *(see [`M_POLARITY`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_POLARITY`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_ANGLE_INTERPOLATION_MODE`

Inquires the type of interpolation to use.

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_ANGLE_INTERPOLATION_MODE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_CLIPPING`

Inquires whether Aurora Imaging Library can automatically clip the search region (box or ring) when it falls outside the image.

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_CLIPPING`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_CLIPPING_MIN_AREA`

Inquires the minimum area requirement for the clipped region (area that Aurora Imaging Library searches for a marker) that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_CLIPPING_MIN_AREA`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_CLIPPING_MIN_HEIGHT`

Inquires the minimum height requirement for the clipped region (area that Aurora Imaging Library searches for a marker) that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_CLIPPING_MIN_HEIGHT`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_CLIPPING_MIN_WIDTH`

Inquires the minimum width requirement for the clipped region (area that Aurora Imaging Library searches for a marker) that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_CLIPPING_MIN_WIDTH`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_CLIPPING_PRESERVE_CENTER`

Inquires the center requirement for the clipped region (area that Aurora Imaging Library searches for a marker) that can be internally created when using [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAXIMIZE_AREA`](../../Reference/meas/MmeasSetMarker.md).

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_CLIPPING_PRESERVE_CENTER`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SEARCH_REGION_INPUT_UNITS`

Inquires the units with which to interpret the [`M_BOX_CENTER`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ORIGIN`](../../Reference/meas/MmeasInquire.md), [`M_BOX_SIZE`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ANGLE`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ANGLE_ACCURACY`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ANGLE_TOLERANCE`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ANGLE_DELTA_POS`](../../Reference/meas/MmeasInquire.md), [`M_BOX_ANGLE_DELTA_NEG`](../../Reference/meas/MmeasInquire.md), [`M_RING_CENTER`](../../Reference/meas/MmeasInquire.md), [`M_RING_RADII`](../../Reference/meas/MmeasInquire.md), and [`M_SUB_REGIONS_CHORD_ANGLE`](../../Reference/meas/MmeasInquire.md) inquire types.

| Value | Description |
| --- | --- |
| *(see [`M_SEARCH_REGION_INPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SUB_REGIONS_NUMBER`

Inquires the number of sections (subregions) in which to divide the search region.

| Value | Description |
| --- | --- |
| *(see [`M_SUB_REGIONS_NUMBER`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SUBPIXEL_MODE`

Inquires how Aurora Imaging Library calculates subpixel positions.

| Value | Description |
| --- | --- |
| *(see [`M_SUBPIXEL_MODE`](Reference/meas/MmeasSetMarker.md))* |  |

### For edge and stripe markers

For edge and stripe markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_BOX_ANGLE`

Inquires the angle of the box search region.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_ACCURACY`

Inquires the accuracy of the angular search.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_ACCURACY`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_DELTA_NEG`

Inquires the negative range of angles within which to search for the marker.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_DELTA_NEG`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_DELTA_POS`

Inquires the positive range of angles within which to search for the marker.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_DELTA_POS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_MODE`

Inquires whether a angular search is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_MODE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_REFERENCE`

Inquires the center of rotation used when performing a search at an angle.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_REFERENCE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ANGLE_TOLERANCE`

Inquires the rotation tolerance of the marker. This is the full range of degrees within which a marker can be rotated from a box search region that is at a specific angle and still be found. This determines the step angle used for an angular search.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_ANGLE_TOLERANCE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_CENTER`

Inquires the coordinates of the center of the box search region.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_CENTER`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_BOX_CENTER`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_BOX_ORIGIN`

Inquires the coordinates of the origin of the box search region.

---

### `M_BOX_SIZE`

Inquires the width and height of the box search region.

| Value | Description |
| --- | --- |
| *(see [`M_BOX_SIZE`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_BOX_SIZE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_DRAW_PROFILE_SCALE_OFFSET`

Inquires the scale and offset used with [`M_DRAW_..._PROFILE`](../../Reference/meas/MmeasDraw.md).

| Value | Description |
| --- | --- |
| *(see [`M_DRAW_PROFILE_SCALE_OFFSET`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_DRAW_PROFILE_SCALE_OFFSET`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_NUMBER_MIN`

Inquires the minimum number of edges or stripes to locate.

| Value | Description |
| --- | --- |
| *(see [`M_NUMBER_MIN`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_ORIENTATION`

Inquires the orientation of the marker.

| Value | Description |
| --- | --- |
| *(see [`M_ORIENTATION`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SUB_REGIONS_OFFSET`

Inquires the offset of the subregions, from the center point of each of their sections.

| Value | Description |
| --- | --- |
| *(see [`M_SUB_REGIONS_OFFSET`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SUB_REGIONS_SIZE`

Inquires the size of the subregions, as a percentage of their section.

| Value | Description |
| --- | --- |
| *(see [`M_SUB_REGIONS_SIZE`](Reference/meas/MmeasSetMarker.md))* |  |

### For point markers

For point markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_MULTIPLE_POINT_ANGLE`

Inquires the angle of the theoretical line along which subsequent points are placed, for a multiple point marker.

| Value | Description |
| --- | --- |
| *(see [`M_MULTIPLE_POINT_ANGLE`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_POINT_INPUT_UNITS`

Inquires the units with which to interpret the [`M_POSITION`](../../Reference/meas/MmeasInquire.md), [`M_SPACING`](../../Reference/meas/MmeasInquire.md), and [`M_MULTIPLE_POINT_ANGLE`](../../Reference/meas/MmeasInquire.md) inquire types.

| Value | Description |
| --- | --- |
| *(see [`M_POINT_INPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_POSITION`

Inquires the coordinates of a point marker.

| Value | Description |
| --- | --- |
| *(see [`M_POSITION`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_POSITION`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SPACING`

Inquires the spacing (distance) between consecutive points used as a multiple-occurrence marker.

| Value | Description |
| --- | --- |
| *(see [`M_SPACING`](Reference/meas/MmeasSetMarker.md))* |  |
| `M_ANY` | Specifies that the spacing between points has not been set. [`M_ANY`](../../Reference/meas/MmeasInquire.md) is only valid if [`M_NUMBER`](../../Reference/meas/MmeasInquire.md) is set to 1. Otherwise, [`M_ANY`](../../Reference/meas/MmeasInquire.md) is an invalid value. |

### For stripe markers

For stripe markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_INCLUSION_POINT`

Inquires the position of the inclusion point.

| Value | Description |
| --- | --- |
| *(see [`M_INCLUSION_POINT`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_INCLUSION_POINT`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_INCLUSION_POINT_INPUT_UNITS`

Inquires the units with which to interpret the [`M_INCLUSION_POINT`](../../Reference/meas/MmeasInquire.md) inquire type.

| Value | Description |
| --- | --- |
| *(see [`M_INCLUSION_POINT_INPUT_UNITS`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_INCLUSION_POINT_INSIDE_STRIPE`

Inquires whether the inclusion point must be inside the stripe.

| Value | Description |
| --- | --- |
| *(see [`M_INCLUSION_POINT_INSIDE_STRIPE`](Reference/meas/MmeasSetMarker.md))* |  |

### For circle markers

For circle markers, the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter can be set to one of the following:

---

### `M_CIRCLE_ACCURACY`

Inquires the accuracy of the circle's fit operation.

| Value | Description |
| --- | --- |
| *(see [`M_CIRCLE_ACCURACY`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_CIRCLE_INSIDE_SEARCH_REGION`

Inquires whether the circle's fit operation can result in a circle that is outside the ring search region.

| Value | Description |
| --- | --- |
| *(see [`M_CIRCLE_INSIDE_SEARCH_REGION`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_RING_CENTER`

Inquires the coordinates of the center of the ring search region.

| Value | Description |
| --- | --- |
| *(see [`M_RING_CENTER`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_RING_CENTER`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_RING_RADII`

Inquires the inner and outer radius of the circle marker's ring-shaped region.

| Value | Description |
| --- | --- |
| *(see [`M_RING_RADII`](Reference/meas/MmeasSetMarker.md))* |  |
| *(see [`M_RING_RADII`](Reference/meas/MmeasSetMarker.md))* |  |

---

### `M_SUB_REGIONS_CHORD_ANGLE`

Inquires the angle with which to set the ring search region's outer ring segment (chord).

| Value | Description |
| --- | --- |
| *(see [`M_SUB_REGIONS_CHORD_ANGLE`](Reference/meas/MmeasSetMarker.md))* |  |

### For a measurement context buffer

When performing an inquiry on a measurement context buffer, [`InquireType`](../../Reference/meas/MmeasInquire.md) can be set to one of the following values.

---

### `M_PIXEL_ASPECT_RATIO`

Inquires the ratio of the width of the pixel to its height.

---

### `M_PIXEL_ASPECT_RATIO_INPUT`

Inquires how the Measurement module interprets specified measurement characteristics relative to the pixel aspect ratio.

| Value | Description |
| --- | --- |
| `M_CORRECTED` *(default)* | Specifies to correct measurement inputs with the specified aspect ratio. |
| `M_NORMAL` | Specifies to not correct measurement inputs (ratio of 1.0). |

---

### `M_PIXEL_ASPECT_RATIO_OUTPUT`

Inquires how the Measurement module returns results relative to the pixel aspect ratio.

| Value | Description |
| --- | --- |
| `M_CORRECTED` *(default)* | Specifies to correct measurement results with the specified aspect ratio. |
| `M_NORMAL` | Specifies to not correct measurement results (ratio of 1.0). |

### For a measurement result buffer

When performing an inquiry on a measurement result buffer, [`InquireType`](../../Reference/meas/MmeasInquire.md) can be set to one of the following values.

---

### `M_RESULT_TYPE`

Inquires the type of result buffer allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CALCULATE`](../../Reference/meas/MmeasInquire.md). |
| `M_CALCULATE` | Specifies a result buffer for an [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation. |

### For inquiring the system on which the measurement context, marker, or result buffer was allocated

To inquire the system on which the measurement context, marker, or result buffer was allocated, set the [`InquireType`](../../Reference/meas/MmeasInquire.md) parameter to the following value.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the measurement context, marker, or result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type, regardless of the current value of the inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, typically returned to [`FirstUserVarPtr`](../../Reference/meas/MmeasInquire.md), cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

An angle interpreted according to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).
