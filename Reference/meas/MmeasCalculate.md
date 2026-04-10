---
doctype: Reference
module: meas
function: MmeasCalculate
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasCalculate"
---

# MmeasCalculate

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

> Calculate measurements between two markers.

## Syntax

```c
void MmeasCalculate(
    AIL_ID    ContextId,       //in
    AIL_ID    Marker1Id,       //in
    AIL_ID    Marker2Id,       //in
    AIL_ID    MeasResultId,    //out
    AIL_INT64 MeasurementList  //in
)
```

## Description

This function calculates the specified measurements between the two specified markers. Before using an edge, stripe, or circle marker with this function, it must have been previously found in a target image ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)). Before using a point marker, its position must have been previously set ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md)).

The settings of the specified measurement context control the behavior of this function; to adjust these settings, use [`MmeasControl`](../../Reference/meas/MmeasControl.md). Aurora Imaging Library stores the results of an [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation in the specified measurement result buffer; to obtain the results, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md). If the markers were found in a calibrated image, you can specify the output units using [`MmeasControl`](../../Reference/meas/MmeasControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasControl.md). Aurora Imaging Library calculates the specified measurements according to the line joining the reference position of each marker ([`Marker1Id`](../../Reference/meas/MmeasCalculate.md) and [`Marker2Id`](../../Reference/meas/MmeasCalculate.md)).

For edge, stripe, and circle markers, the default reference position corresponds to the marker's found position. For an edge marker, the default reference position is the X- and Y-coordinates of the edge's maximum edgevalue. For a stripe marker, the default reference position is the center of the center line that is fit between the stripe's two outer edges. For a circle marker, the default reference position is the X- and Y-coordinates of the circle's center. For a point marker, the default reference position is the same as the position specified using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md).

To change the reference position of any marker, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MARKER_REFERENCE`](../../Reference/meas/MmeasSetMarker.md). Modifying the reference position only affects calculations between two markers ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)); it does not, for example, affect how Aurora Imaging Library locates or returns the position of markers ([`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md)).

If you specify multiple-occurrence markers, Aurora Imaging Library performs its calculations using the occurrences of the first marker and the corresponding occurrences of the second marker. Aurora Imaging Library limits the number of calculations to the smallest number of occurrences held in either marker. For example, if a marker contains one edge occurrence, Aurora Imaging Library only performs one calculation, regardless of the number of edge occurrences in the other marker.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the measurement context to use.

*For specifying the measurement context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default measurement context of the current Aurora Imaging Library application. |
| `Measurement context ID` | Specifies a valid measurement context, allocated using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). |

### `Marker1Id` *(in, AIL_ID)*

Specifies the identifier of the first marker buffer to use for calculating measurements.

### `Marker2Id` *(in, AIL_ID)*

Specifies the identifier of the second marker buffer to use for calculating measurements.

### `MeasResultId` *(out, AIL_ID)*

Specifies the identifier of the result buffer in which to place results.

### `MeasurementList` *(in, AIL_INT64)*

Specifies which measurement(s) to calculate. This parameter must be set to one of the following values. To calculate more than one measurement, add the values together (for example, [`M_DISTANCE`](../../Reference/meas/MmeasCalculate.md)+[`M_ANGLE`](../../Reference/meas/MmeasCalculate.md)).

*For specifying which measurement(s) to calculate*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_ANGLE`](../../Reference/meas/MmeasCalculate.md) + [`M_DISTANCE`](../../Reference/meas/MmeasCalculate.md) + [`M_LINE_EQUATION`](../../Reference/meas/MmeasCalculate.md). |
| `M_ANGLE` | Calculates the angle of the line joining both markers, relative to the positive X-axis. The resulting angle can be any value between 0° and 360°. |
| `M_DISTANCE` | Calculates the distance of the line joining both markers. |
| `M_LINE_EQUATION` | Calculates the equation of the line joining both markers. |
