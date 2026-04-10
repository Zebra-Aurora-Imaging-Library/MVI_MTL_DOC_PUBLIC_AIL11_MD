---
doctype: Reference
module: meas
function: MmeasFindMarker
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasFindMarker"
---

# MmeasFindMarker

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

> Find a marker in a target image and take the specified measurements.

## Syntax

```c
void MmeasFindMarker(
    AIL_ID    ContextId,       //in
    AIL_ID    ImageBufId,      //in
    AIL_ID    MarkerId,        //in-out
    AIL_INT64 MeasurementList  //in
)
```

## Description

This function finds an edge, stripe, or circle marker in the specified target image and takes the specified measurements. To locate the marker, Aurora Imaging Library uses its essential characteristics ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md)), its score characteristics ([`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md)), and the measurement context ([`MmeasControl`](../../Reference/meas/MmeasControl.md)).

For edge and stripe markers, it is recommended that the box search region be set to an angle close to that of the required marker (that is, within the marker's particular rotation tolerance) to ensure that the marker is found and that results are accurate. The greater the angle of the marker relative to the search region, the greater the distortion of the marker's actual characteristics, which increases the chance that [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) might not successfully locate the marker.

[`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) stores its results in the specified measurement marker (not in a result buffer). To obtain results, use [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) or [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md). All positional results are relative to the top-left pixel in the target image, or the origin of the relative coordinate system if the target is a calibrated image.

If you have associated the target image with a camera calibration context, you can specify that certain input settings be interpreted in world units. To do so, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MARKER_REFERENCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), [`M_INCLUSION_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md), and/or [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) set to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md). By using a calibrated image, you can specify the search region according to the relative coordinate system using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md), and then fixture the relative coordinate system to each object. This way, you will only need to place the search region once, and all of the search results will be made from the same reference location. Note that if you set any of these constants to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md) but you don't pass [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) a calibrated target image, the function will generate an error.

For edge and stripe markers, you can limit the find operation to a region of the target image buffer using a rectangular region of interest (ROI) set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). For circle markers, you can limit the find operation to a region of the target image buffer using a circular region of interest (ROI) set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The find marker operation will then search inside the ROI instead of inside any previously set search regions. Note that while the region of interest (ROI) affects the marker occurrences, it will not change the settings of any previously set search regions. In these cases, the region must be defined in vector format from a 2D graphics list ([`M_VECTOR`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error will be generated if the ROI is only in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md)).

When finding an edge or stripe marker, the target image buffer can have a rectangular region of interest (ROI), defined using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) (with or without rotation). When finding a circle marker, the target image buffer can have a circular region of interest (ROI), defined using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the measurement context.

*For specifying the measurement context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default measurement context of the current Aurora Imaging Library application. |
| `Measurement context ID` | Specifies a valid measurement context, previously allocated using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). |

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the target image buffer in which to locate the marker. This buffer must be a 1-band, 8-bit or 16-bit unsigned image buffer.

### `MarkerId` *(in-out, AIL_ID)*

Specifies the identifier of the marker to be located in the image.

### `MeasurementList` *(in, AIL_INT64)*

Specifies the measurements to perform.

*For specifying the measurements to perform when using edge, stripe, or circle markers*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs all measurements that apply to the marker type. For example, if you are using a circle, Aurora Imaging Library takes all measurements possible for circle markers. |
| `M_POSITION` | Determines the position of the marker.

By specifying [`M_POSITION`](../../Reference/meas/MmeasFindMarker.md), all of the following will also be accessible with [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) (for edge and stripe markers):[`M_BOX_EDGEVALUES`](../../Reference/meas/MmeasGetResult.md), [`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md), [`M_EDGE_STRENGTH`](../../Reference/meas/MmeasGetResult.md), [`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md), [`M_ORIENTATION`](../../Reference/meas/MmeasGetResult.md), [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md), [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md), and [`M_SPACING`](../../Reference/meas/MmeasGetResult.md). |

*For specifying the measurements to perform when using edge or stripe markers*

| Value | Description |
| --- | --- |
| `M_ANGLE` | Determines the angle of the mean line of a marker, relative to the output coordinate system specified using [`MmeasControl`](../../Reference/meas/MmeasControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasControl.md). The angle can result in a value between 0° and 360°. For a stripe marker, Aurora Imaging Library establishes the angle of three lines: the stripe's mean line, the stripe's first outermost edge, and the stripe's second outermost edge.

An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

If you did not explicitly specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library will internally use three to establish the lines it requires to determine [`M_ANGLE`](../../Reference/meas/MmeasFindMarker.md). |
| `M_EDGE_CONTRAST` | Determines the grayscale difference between the start ([`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md)) and end ([`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md)) of the intensity transition from which the edge is established. For a stripe marker, Aurora Imaging Library establishes this value from the stripe's first and second outermost edges. |
| `M_EDGE_INSIDE` | Determines the number of edges located between the two exterior edges of a stripe. This only applies to stripe markers. |
| `M_EDGE_WIDTH` | Determines the distance between the start ([`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md)) and end ([`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md)) of the intensity transition from which the edge is established. For a stripe marker, Aurora Imaging Library establishes this value for the stripe's first and second outermost edges. |
| `M_EDGEVALUE_PEAK_CONTRAST` | Determines the grayscale difference of the intensity transition between the first zero edgevalues on both sides of the edge peak (before [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md), and after[`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md)). For a stripe marker, Aurora Imaging Library establishes this value for the stripe's first and second outermost edges.

Note that some images might never have a zero edgevalue, in which case you should use [`M_EDGE_CONTRAST`](../../Reference/meas/MmeasFindMarker.md) for a more meaningful result. |
| `M_EDGEVALUE_PEAK_WIDTH` | Determines the distance between the minimum ([`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md)) and maximum ([`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md)) positions of the edge peak (that is, the minimum and maximum position of the edgevalue along the first derivative representation of the intensity). For a stripe marker, Aurora Imaging Library establishes this value for the stripe's first and second outermost edges. |
| `M_LENGTH` | Determines the length of the side of the marker perpendicular to the search direction. This is useful to retrieve the number of pixels that were projected into a single value of the intensity profile without having to find [`M_ORIENTATION`](../../Reference/meas/MmeasGetResult.md) first. |
| `M_LINE_END_POINT_FIRST` | Determines the X- and Y-coordinates of the first intersection point between the marker's mean line and the box search region. For a stripe marker, the mean line is the mean of the lines following its two edges.

If you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library will internally use three to establish the line it requires to determine [`M_LINE_END_POINT_FIRST`](../../Reference/meas/MmeasFindMarker.md). |
| `M_LINE_END_POINT_SECOND` | Determines the X- and Y-coordinates of the second intersection point between the marker's mean line and the box search region. For a stripe marker, the mean line is the mean of the lines following its two edges.

If you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library will internally use three to establish the line it requires to determine [`M_LINE_END_POINT_SECOND`](../../Reference/meas/MmeasFindMarker.md). |
| `M_LINE_EQUATION` | Determines the equation of the line following an edge. The line equation is of the general form, `_Ax_ + _By_ + _C_ = 0.` For a stripe marker, Aurora Imaging Library establishes the equation of three lines: the stripe's mean line, the stripe's first edge, and the stripe's second edges.

If you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library will internally use three to establish the lines it requires to determine [`M_LINE_EQUATION`](../../Reference/meas/MmeasFindMarker.md). |
| `M_POLARITY` | Determines whether the edge(s) are rising (positive polarity) or falling (negative polarity). |
| `M_STRIPE_WIDTH` | Determines the width of the stripe marker. The stripe's width refers to the distance between the position of the maximum edgevalue of the stripe's two outermost edges. This only applies to stripe markers. |

*For specifying the measurement to perform when using circle markers*

| Value | Description |
| --- | --- |
| `M_RADIUS` | Determines the distance between the circle's circumference and the circle's center. |
