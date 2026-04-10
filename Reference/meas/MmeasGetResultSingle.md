---
doctype: Reference
module: meas
function: MmeasGetResultSingle
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasGetResultSingle"
---

# MmeasGetResultSingle

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

> Get the specified type of result for a specific point, edge, stripe, or circle from a measurement marker buffer or a measurement result buffer.

## Syntax

```c
void MmeasGetResultSingle(
    AIL_ID    MarkerOrMeasResultId,  //in
    AIL_INT64 ResultType,            //in
    void *    FirstResultArrayPtr,   //out
    void *    SecondResultArrayPtr,  //out
    AIL_INT   ResultIndex            //in
)
```

## Description

This function retrieves the specified result of the specified type for a point, edge, stripe, or circle from a measurement marker buffer or a measurement result buffer. Measurement marker buffers hold the results of an [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) operation, while measurement result buffers hold the results of an [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation. Depending on the buffer you are using, you must call either [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) or [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) prior to calling [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md); otherwise, you will get an incorrect result.

If you are using a multiple-occurrence marker, [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) returns the specified result for a single occurrence, which you must specify with the [`ResultIndex`](../../Reference/meas/MmeasGetResultSingle.md) parameter. To return results for all occurrences, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md).

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

If your target image was associated with a camera calibration context and you want to retrieve positional and dimensional results in pixel units, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) for a measurement marker buffer, or [`MmeasControl`](../../Reference/meas/MmeasControl.md) for a measurement result buffer, with [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) set to [`M_PIXEL`](../../Reference/meas/MmeasSetMarker.md). If you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md) without specifying a calibrated image in which to calculate the results, [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) will generate an error.

For certain result types, Aurora Imaging Library returns one value for an edge marker, and two values for a stripe marker. In these cases, for an edge marker, Aurora Imaging Library returns the result to [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md); [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) must be set to [`M_NULL`](../../Reference/meas/MmeasGetResultSingle.md).

When retrieving a result for a stripe marker, result types can return one general result for the stripe, a result for both edges of the stripe, or a result for one edge of the stripe. To return the result for one edge of the stripe, you must specify the edge using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md).

When retrieving non-positional results for both edges of a stripe marker, Aurora Imaging Library returns the results for the first edge to [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) and for the second edge to [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md). When retrieving non-positional results for one edge of the stripe, Aurora Imaging Library always returns the result to [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md).

When retrieving positional results for a stripe, Aurora Imaging Library returns the results for the X-coordinate of the stripe to [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) and the Y-coordinate of the stripe to [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md). When retrieving positional results for one edge of the stripe, Aurora Imaging Library returns the X-coordinate to [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) and the Y-coordinate to [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md). You can set [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md)or [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md)to[`M_NULL`](../../Reference/meas/MmeasGetResultSingle.md) to retrieve only one coordinate.

## Parameters

### `MarkerOrMeasResultId` *(in, AIL_ID)*

Specifies the identifier of the measurement marker (allocated with [`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md)) or measurement result buffer (allocated with [`MmeasAllocResult`](../../Reference/meas/MmeasAllocResult.md)) from which to retrieve results.

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `FirstResultArrayPtr` *(out, *void)*

Specifies the address of the first variable or array in which to write the requested information.

### `SecondResultArrayPtr` *(out, *void)*

Specifies the address of the second variable or array in which to write the requested information.

### `ResultIndex` *(in, AIL_INT)*

Specifies the index of the occurrence for which to retrieve information.

*For specifying the index*

| Value | Description |
| --- | --- |
| `0 <= Value < NumberOfOccurrences` | Specifies the index of the occurrence. |

## Parameter Associations

### For any type of marker (measurement marker buffer) or a measurement result buffer

To retrieve a result from any type of marker (measurement marker buffer), or from a measurement result buffer, [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to one of the values specified in the table below. If you are getting results from a measurement result buffer, and you are using two multiple-occurrence markers that have a different number of occurrences, Aurora Imaging Library uses the fewest number of occurrences to calculate results ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)).

---

### `M_NUMBER`

Retrieves the number of points, edges, stripes, or circles measured. Note that edges, stripes, and circles are searched for, but points are placed manually. For circle markers, [`M_NUMBER`](../../Reference/meas/MmeasGetResultSingle.md) will never return a value greater than one, since circles cannot be defined as a multiple-occurrence marker.  After a call to the [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) function, this number is equal to the number of occurrences of a marker found in the search region.  After a call to the [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) function, this number is equal to the smallest number of occurrences held in either marker involved in the calculation.

### For an edge or stripe marker (measurement marker buffer) or a measurement result buffer

To retrieve a result from an edge or stripe marker (measurement marker buffer), or from a measurement result buffer, [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to one of the values specified in the table below. If you are getting results from a measurement result buffer, and you are using two multiple-occurrence markers that have a different number of occurrences, Aurora Imaging Library uses the fewest number of occurrences to calculate results ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)).

---

### `M_ANGLE`

Retrieves the angle of the edge for the marker occurrence, in degrees. The angle is relative to the output coordinate system specified using [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) with either [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) (for a measurement marker buffer) or [`MmeasControl`](../../Reference/meas/MmeasControl.md) (for a measurement result buffer).  If retrieved from a measurement marker buffer, [`M_ANGLE`](../../Reference/meas/MmeasGetResult.md) returns the angle of the line measured for the edge. For a stripe marker, this line follows the center of the stripe and is calculated as the mean of the lines following its edges. If the result is requested for a specific edge of the stripe, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), the angle of the first or second edge is returned, respectively.  If retrieved from a measurement result buffer, [`M_ANGLE`](../../Reference/meas/MmeasGetResult.md) returns the angle of the line joining the two marker occurrences, relative to the positive X-axis.  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

---

### `M_LINE_A`

Retrieves the coefficient _A_ of the line equation, for the marker occurrence. The line equation is of the general form, `_Ax_ + _By_ + _C_ = 0.`  If retrieved from an edge marker (measurement marker buffer), [`M_LINE_A`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _A_ of the line measured for the edge. For a stripe marker, this line follows the center of the stripe, and is calculated as the mean of the lines following its edges. If the result is requested for a specific edge of the stripe, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), the coefficient _A_ of the line equation of the first or second edge, respectively, is returned.  If retrieved from a measurement result buffer, [`M_LINE_A`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _A_ of the line joining the two markers.

---

### `M_LINE_B`

Retrieves the coefficient _B_ of the line equation, for the marker occurrence. The line equation is of the general form, `_Ax_ + _By_ + _C_ = 0.`  If retrieved from an edge marker (measurement marker buffer), [`M_LINE_B`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _B_ of the line measured for the edge. For a stripe marker, this line follows the center of the stripe, and is calculated as the mean of the lines following its edges. If the result is requested for a specific edge of the stripe, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), the coefficient _B_ of the line equation of the first or second edge, respectively, is returned.  If retrieved from a measurement result buffer, [`M_LINE_B`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _B_ of the line joining the two markers.

---

### `M_LINE_C`

Retrieves the coefficient _C_ of the line equation, for the marker occurrence. The line equation is of the general form, `_Ax_ + _By_ + _C_ = 0.`  If retrieved from an edge marker (measurement marker buffer), [`M_LINE_C`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _C_ of the line measured for the edge. For a stripe marker, this line follows the center of the stripe, and is calculated as the mean of the lines following its edges. If the result is requested for a specific edge of the stripe, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), the coefficient _C_ of the line equation of the first or second edge, respectively, is returned.  If retrieved from a measurement result buffer, [`M_LINE_C`](../../Reference/meas/MmeasGetResult.md) returns the coefficient _C_ of the line joining the two markers.

### For any type marker (measurement marker buffer)

To retrieve a result from any type of marker (measurement marker buffer), [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to one of the values specified in the table below.

---

### `M_POSITION`

Retrieves the X- and Y-coordinates of the position, for the marker occurrence.  For point markers, the position refers to the X- and Y-coordinates specified using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md).  For edge markers, the position refers to the X- and Y-coordinates of the edge's maximum edgevalue (highest edge peak) along the center of the search region.  For stripe markers, the position refers to the X- and Y-coordinates at the center of a theoretical line between the position (maximum edgevalue) of the stripe's two outermost edges. If you use [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), you will get the X- and Y-coordinates of that edge's maximum edgevalue (highest edge peak).  For circle markers, the position refers to the X- and Y-coordinates at the circle's center.

---

### `M_VALID_FLAG`

Retrieves whether the marker was found, or that the coordinates of a point marker ([`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md) with [`M_POINT`](../../Reference/meas/MmeasAllocMarker.md)) are set to a valid position.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the marker was not found. |
| `M_TRUE` | Specifies that the marker was found. |

### For an edge, stripe, or circle marker (measurement marker buffer)

To retrieve a result from an edge, stripe, or circle marker (measurement marker buffer), [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to one of the values specified in the table below.

---

### `M_FIT_ERROR_MAX`

Retrieves the maximum distance from a subedge to the fitted line equation (for edge or stripe markers) or to the fitted circle (for circle markers), for the marker occurrence. This distance is called the maximum fit error. If you did not specify subregions (for edges or stripes), [`M_FIT_ERROR_MAX`](../../Reference/meas/MmeasGetResultSingle.md) returns 0. Note that circles always have subregions.

---

### `M_NUMBER_OF_OUTLIERS`

Retrieves the number of subedges considered outliers, for the marker occurrence. Outliers are subedges within the search region that do not contribute to marker's fit operation since they are considered to be too far away from the general distribution of subedges. The number of outliers corresponds to the number of fitted subedges subtracted from the number of subregions.

---

### `M_SEARCH_REGION_WAS_CLIPPED`

Retrieves whether the search region was clipped. This value is only useful if you enabled clipping ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SEARCH_REGION_CLIPPING`](../../Reference/meas/MmeasSetMarker.md)).

| Value | Description |
| --- | --- |
| `0` | Specifies that the search region was not clipped. |
| `1` | Specifies that the search region was clipped. |

---

### `M_SUB_EDGES_MARKER_INDEX`

Retrieves the index on which each subedge is located, for the marker occurrence. If a subedge is not found in a subregion, an arbitrary number is returned. Use [`M_SUB_EDGES_WEIGHT`](../../Reference/meas/MmeasGetResultSingle.md) to determine if a subedge was actually found.  This result is useful when retrieving subedge-type of results for a multiple-occurrence marker. For example, you can use the index to separate, by marker, the values obtained with [`M_SUB_EDGES_POSITION`](../../Reference/meas/MmeasGetResultSingle.md) and [`M_SUB_EDGES_WEIGHT`](../../Reference/meas/MmeasGetResultSingle.md).

| Value | Description |
| --- | --- |
| `M_EDGE_FIRST` | Specifies that the edge is the first edge in the stripe. |
| `M_EDGE_SECOND` | Specifies that the edge is the second edge in the stripe. |

---

### `M_SUB_EDGES_POSITION`

Retrieves the X- and Y-coordinates of the subedges, for the marker occurrence.  If a subedge is not found in a subregion, the X- and Y- positions will be arbitrary numbers. Use [`M_SUB_EDGES_WEIGHT`](../../Reference/meas/MmeasGetResultSingle.md) to determine whether a subedge was found.

---

### `M_SUB_EDGES_WEIGHT`

Retrieves the weight of the subedges, for the marker occurrence. The weight indicates whether a subedge was found in the subregion. Found subedges are used to calculate the fit, angle, and line equation values.

| Value | Description |
| --- | --- |
| `0` | Specifies that the subedge was not found. |
| `1` | Specifies that the subedge was found. |

---

### `M_SUB_REGIONS_NUMBER_USED`

Retrieves the number of subregions used.  Note that if you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library will internally use three to perform certain measurements.

### For an edge or stripe marker (measurement marker buffer)

To retrieve a result from an edge or stripe marker (measurement marker buffer), [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to one of the values specified in the table below, unless otherwise specified.

---

### `M_BOX_ANGLE_FOUND`

Retrieves the angle found for the box search region when the angle is internally determined by Aurora Imaging Library. In this case, you must have specified [`M_ANY`](../../Reference/meas/MmeasSetMarker.md) as the angle of the box search region ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md)) or enabled an angular search ([`M_BOX_ANGLE_MODE`](../../Reference/meas/MmeasSetMarker.md) set to [`M_ENABLE`](../../Reference/meas/MmeasSetMarker.md)).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

---

### `M_BOX_CORNER_BOTTOM_LEFT`

Retrieves the coordinates of the bottom-left corner of the marker's box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the result will take that rotation into account.

---

### `M_BOX_CORNER_BOTTOM_RIGHT`

Retrieves the coordinates of the bottom-right corner of the marker's box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the result will take that rotation into account.

---

### `M_BOX_CORNER_TOP_LEFT`

Retrieves the coordinates of the top-left corner of the marker's box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the result will take that rotation into account.

---

### `M_BOX_CORNER_TOP_RIGHT`

Retrieves the coordinates of the top-right corner of the marker's box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the result will take that rotation into account.

---

### `M_BOX_EDGEVALUES`

Retrieves the edgevalues of the marker's box search region.  Aurora Imaging Library establishes edgevalues by applying a first derivative filter to the search region's intensity profile and normalizing the output according to the number of pixels and the maximum pixel value possible. To establish the intensity profile, Aurora Imaging Library projects the pixels bounded by the two-dimensional search region (or each subregion) into a one-dimensional pixel intensity summation, which is performed vertically or horizontally, depending on the search region's origin and search direction.

---

### `M_BOX_EDGEVALUES_NUMBER`

Retrieves the number of edgevalues that [`M_BOX_EDGEVALUES`](../../Reference/meas/MmeasGetResultSingle.md) returns. This value corresponds to the number of pixels along the width or height of the box search region, depending on the search region's origin and search direction.

---

### `M_DISTANCE_FROM_BOX_ORIGIN`

Retrieves the position, as a distance value relative to the origin of the box search region, for the marker occurrence.

---

### `M_EDGE_CONTRAST`

Retrieves the grayscale difference between the start ([`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md)) and end ([`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md)) of the intensity transition from which the edge is established, for the marker occurrence.

---

### `M_EDGE_END`

Retrieves the X- and Y-coordinates of the end of the edge (that is, the end of the intensity transition from which the edge is established), for the marker occurrence. For a stripe marker, the coordinates representing the average value of end positions is returned unless otherwise specified by the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md).  In the intensity profile calculated for a search region, [`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md) represents the first significant slope used to establish the edge, while [`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md) represents the first significant slope in the opposite direction used to establish the edge. Depending on the search region's origin and search direction, the edge's start and end can be on either side of the edge.  *[Image: EdgedetectionReference.png]*  To retrieve the minimum and maximum position of the edge peak of the marker, use [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResultSingle.md) and [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_EDGE_INSIDE`

Retrieves the number of edges located between the two exterior edges of a stripe, for the marker occurrence. This result only applies to stripes.

---

### `M_EDGE_START`

Retrieves the X- and Y-coordinates of the start of the edge (that is, the start of the intensity transition from which the edge is established), for the marker occurrence. For a stripe marker, the coordinates representing the average value of start positions is returned unless otherwise specified by the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md).  In the intensity profile calculated for a search region, [`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md) represents the first significant slope used to establish the edge, while [`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md) represents the first significant slope in the opposite direction used to establish the edge. Depending on the search region's origin and search direction, the edge's start and end can be on either side of the edge.  *[Image: EdgedetectionReference.png]*  To retrieve the minimum and maximum position of the edge peak of the marker, use [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResultSingle.md) and [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_EDGE_STRENGTH`

Retrieves the greatest edgevalue of the edge, for the marker occurrence. The greatest edgevalue of an edge is considered to be its edge strength.

---

### `M_EDGE_WIDTH`

Retrieves the distance between the start ([`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md)) and end ([`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md)) of the intensity transition from which the edge is established, for the marker occurrence.  To retrieve the width of the edge established by the edge peak of the marker, use [`M_EDGEVALUE_PEAK_WIDTH`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_EDGEVALUE_PEAK_CONTRAST`

Retrieves the grayscale difference of the intensity transition between the first zero edgevalues on both sides of the established edge peak (before [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResultSingle.md), and after [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResultSingle.md)), for the marker occurrence.  Note that some images might never have a zero edgevalue, in which case you should use [`M_EDGE_CONTRAST`](../../Reference/meas/MmeasGetResultSingle.md) for a more meaningful result.

---

### `M_EDGEVALUE_PEAK_POS_MAX`

Retrieves the X- and Y-coordinates of the maximum position of the edge peak (that is, the maximum edgevalue along the first derivative representation of the intensity profile), for the marker occurrence. For a stripe marker, the coordinates representing the average value of maximum positions is returned unless otherwise specified by the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md).  To establish the maximum and minimum positions of the edge peak, Aurora Imaging Library considers the portion of the peak that contains the maximum edgevalue, and that satisfies the [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md) and [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) constraints set using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). That is, the peak must dip, on both sides, below either the minimum edgevalue threshold ([`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md)) or the local edgevalue threshold established by subtracting [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) from the peak's summit, whichever is highest.  *[Image: EdgedetectionEdgeValueMinAndVarMINReference.png]*  Depending on the search region's origin and search direction, the maximum and minimum position can be on either side of the edge peak's summit. Note that you can also retrieve different start and end positions of the edge using [`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md) and [`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_EDGEVALUE_PEAK_POS_MIN`

Retrieves the X- and Y-coordinates of the minimum position of the edge peak (that is, the minimum edgevalue along the first derivative representation of the intensity profile), for the marker occurrence. For a stripe marker, the coordinates representing the average value of minimum positions is returned unless otherwise specified by the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md).  To establish the minimum and maximum positions of the edge peak, Aurora Imaging Library considers the portion of the peak that contains the maximum edgevalue, and that satisfies the [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md) and [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) constraints set using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). That is, the peak must dip, on both sides, below either the minimum edgevalue threshold ([`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md))or the local edgevalue threshold established by subtracting [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) from the peak's summit, whichever is highest.  *[Image: EdgedetectionEdgeValueMinAndVarMINReference.png]*  Depending on the search region's origin and search direction, the maximum and minimum position can be on either side of the edge peak's summit. Note that you can also retrieve different start and end positions of the edge using [`M_EDGE_START`](../../Reference/meas/MmeasGetResultSingle.md) and [`M_EDGE_END`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_EDGEVALUE_PEAK_WIDTH`

Retrieves the distance between the minimum ([`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResultSingle.md)) and maximum ([`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResultSingle.md)) position of the edge peak (that is, the minimum and maximum positions of the edgevalue along the first derivative representation of the intensity profile), for the marker occurrence.  To retrieve the width of the edge instead of the width of the edge peak, use [`M_EDGE_WIDTH`](../../Reference/meas/MmeasGetResultSingle.md).

---

### `M_LENGTH`

Retrieves the length of the side of the search region perpendicular to the search direction. This is useful to retrieve the number of pixels that were projected into a single value of the intensity profile without having to find [`M_ORIENTATION`](../../Reference/meas/MmeasGetResult.md) first.

---

### `M_LINE_END_POINT_FIRST`

Retrieves the X- and Y-coordinates of the first intersection point, for the marker occurrence, between the edge's mean line and the box search region. For a stripe marker, the mean line is the mean of the lines following its two edges.  If you request this result for a specific edge of a stripe, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), [`M_LINE_END_POINT_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) will return the first intersection point of the line following the specified edge and the box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the intersection will take that rotation into account.  For vertical edges, the first intersection point is typically at the top of the box search region, while for horizontal edges the first intersection point is typically on the right of the box search region. Note that the intersection point that is considered to be the first or second ([`M_LINE_END_POINT_SECOND`](../../Reference/meas/MmeasGetResult.md)) can change depending on the edge's angle and whether the box search region is rotated.

---

### `M_LINE_END_POINT_SECOND`

Retrieves the X- and Y-coordinates of the second intersection point, for the marker occurrence, between the edge's mean line and the box search region. For a stripe marker, the mean line is the mean of the lines following its two edges.  If you request a result for a specific edge of a stripe marker, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), [`M_LINE_END_POINT_SECOND`](../../Reference/meas/MmeasGetResultSingle.md) returns the second intersection point of the line following the specified edge and the box search region.  If the box search region was rotated using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_BOX_ANGLE`](../../Reference/meas/MmeasSetMarker.md) or from an angular search, the intersection will take that rotation into account.  For vertical edges, the second intersection point is typically at the bottom of the box search region, while for horizontal edges the second intersection point is typically on the left of the box search region. Note that the intersection point that is considered to be the first ([`M_LINE_END_POINT_FIRST`](../../Reference/meas/MmeasGetResult.md)) or second can change depending on the edge's angle and whether the box search region is rotated.

---

### `M_ORIENTATION`

Retrieves the orientation of the marker.

| Value | Description |
| --- | --- |
| `M_HORIZONTAL` | Specifies a horizontal orientation. |
| `M_VERTICAL` | Specifies a vertical orientation. |

---

### `M_POLARITY`

Retrieves the polarity, for the marker occurrence.

| Value | Description |
| --- | --- |
| `M_NEGATIVE` | Specifies that the polarity of the edge is negative. |
| `M_POSITIVE` | Specifies that the polarity of the edge is positive. |
| `M_NEGATIVE` | Specifies that the polarity of the second edge is negative. |
| `M_POSITIVE` | Specifies that the polarity of the second edge is positive. |

---

### `M_SCORE`

Retrieves the score, for the marker occurrence. This score represents how confident you should be that Aurora Imaging Library found what you expected. For a stripe marker, the score is the average of the score of both edges.

---

### `M_SCORE_TOTAL`

Retrieves the marker's final score. This score represents how confident you should be that Aurora Imaging Library found what you expected. Aurora Imaging Library establishes the marker's final score using all marker occurrences and score characteristics. For a stripe marker, the score is the average of the score of both edges.

---

### `M_SPACING`

Retrieves the distance between the marker occurrence and the next.

---

### `M_STRIPE_WIDTH`

Retrieves the width of the stripe, for the marker occurrence. The stripe's width refers to the distance between the position of the maximum edgevalue of the stripe's two outermost edges. This result is only available for stripes.

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the edge for which to retrieve the result (stripe markers only)

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the edge for which to retrieve the result (stripe markers only).

Note that when retrieving non-positional results, if you explicitly request the result for a specific edge of a stripe, using [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResultSingle.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResultSingle.md), you must retrieve the result for that edge with [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md). In this case, you must set [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) to [`M_NULL`](../../Reference/meas/MmeasGetResultSingle.md). For positional results, you can set [`FirstResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md)or [`SecondResultArrayPtr`](../../Reference/meas/MmeasGetResultSingle.md) to [`M_NULL`](../../Reference/meas/MmeasGetResultSingle.md)if you only want to retrieve the X- or Y-coordinates, respectively. You must always pass at least one pointer.

| Value | Description |
| --- | --- |
| `M_EDGE_FIRST` | Retrieves the specified result for the first outermost edge of a stripe. |
| `M_EDGE_SECOND` | Retrieves specified result for the second outermost edge of a stripe. |

### For a circle marker (measurement marker buffer)

To retrieve a result from a circle marker (measurement marker buffer), [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) can be set to the value specified in the table below.

---

### `M_RADIUS`

Retrieves the circle's radius.

### For a measurement result buffer

To retrieve results from a measurement result buffer, the [`ResultType`](../../Reference/meas/MmeasGetResultSingle.md) parameter can be set to one of the following values. If you are using two multiple-occurrence markers that have a different number of occurrences, Aurora Imaging Library uses the fewest number of occurrences to calculate results ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)).

---

### `M_DISTANCE`

Retrieves the distance between the occurrence of the two markers.

---

### `M_DISTANCE_X`

Retrieves the distance on the X-axis between the occurrence of the two markers.

---

### `M_DISTANCE_Y`

Retrieves the distance on the Y-axis between the occurrence of the two markers.

### Combination Constants — For specifying the required data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

Use [`M_SUB_EDGES_MARKER_INDEX`](../../Reference/meas/MmeasGetResultSingle.md) to establish the occurrence to which the subedge belongs.
