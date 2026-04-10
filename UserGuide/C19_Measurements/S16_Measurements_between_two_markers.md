---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Measurements_between_two_markers
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Measurements between two markers"
---

# Measurements between two markers

Once you have found the position of your markers, you can use [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) to take measurements between two markers. [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) performs measurements according to the line joining the reference position of any two markers. By default, [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) performs all possible measurements (angle, distance, and line equation). This allows you to quickly determine some fundamental information about the relationship between the specified markers, such as how far apart they are from each other.

*[Image: MeasurementCircleReferencePosition2.png]*

By default, a marker's reference position is the same as its actual position ([`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_POSITION`](../../Reference/meas/MmeasGetResult.md)), which depends on the marker's type.

| Marker type | Default reference position |
| --- | --- |
| Point | X- and Y-coordinates specified using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md). |
| Edge | X- and Y-coordinates of the edge's maximum edgevalue. |
| Stripe | X- and Y-coordinates at the center of a theoretical line between the position (maximum edgevalue) of the stripe's two outermost edges. |
| Circle | X- and Y-coordinates at the circle's center. |

To change the marker's reference position, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MARKER_REFERENCE`](../../Reference/meas/MmeasSetMarker.md). Modifying the reference position only affects calculations between two markers ([`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md)); it does not, for example, affect how Aurora Imaging Library finds markers.

## Steps to taking measurements between two markers

The following steps provide a basic methodology for taking measurements between two markers:

1. Allocate a context, using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). This is to allow the user to control the behavior of measurement operations (using [`MmeasControl`](../../Reference/meas/MmeasControl.md)).
2. Allocate a result buffer, using [`MmeasAllocResult`](../../Reference/meas/MmeasAllocResult.md). This is to store the results obtained of the [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation.
3. Allocate and define your markers, and find each marker (for more information, see [Steps to finding and obtaining measurements of markers](S02_Steps_to_finding_and_obtaining_measurements_of_markers.md)). Note that you cannot find point markers; you must set their position with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md).
4. Call [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) and specify the two markers and the measurements to take.
5. Read the results, using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md).
   To draw the resulting line that Aurora Imaging Library used to take measurements between the two markers, call [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_LINE`](../../Reference/meas/MmeasDraw.md), [`M_RESULT`](../../Reference/meas/MmeasDraw.md), and the identifier of the buffer that contains the results of the [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) operation.

## Specifying the measurement context and control settings

While you would normally be required to explicitly specify a measurement context using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md), this is not required to perform basic measurements operations for the [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md), and [`MmeasInquire`](../../Reference/meas/MmeasInquire.md) functions. This is because within those functions, it is possible to specify [`M_DEFAULT`](../../Reference/meas/MmeasInquire.md)as the context's identifier, and in so doing, allocating the default measurement context.

In order to modify a measurement context using [`MmeasControl`](../../Reference/meas/MmeasControl.md), you need to explicitly allocate the measurement context using [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md). This will then allow you to change various context settings, such as the pixel aspect ratio to correct measurement inputs ([`M_PIXEL_ASPECT_RATIO`](../../Reference/meas/MmeasControl.md)). However it is to be noted that these settings are typically reserved for more advanced applications. When you no longer require a measurement context allocated with [`MmeasAllocContext`](../../Reference/meas/MmeasAllocContext.md), free it using [`MmeasFree`](../../Reference/meas/MmeasFree.md).

## Calculating with multiple-occurrence markers

If you specify two multiple-occurance markers, calculations are made using the occurrences of the first marker and the corresponding occurrences of the second marker. The number of calculations is limited to the smallest number of occurrences held in either marker. For example, in the following illustration, _Marker 1_ is a simple edge marker, while _Marker 2_ is a multiple-occurrence marker with four edge occurrences. In this case, Aurora Imaging Library performs the calculations between _Marker 1_ and the first occurrence of _Marker 2_.

*[Image: MeasurementCalculationMultipleMarker.png]*

When retrieving results for a multiple-occurrence marker, you must pass an array to [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) that is large enough to hold the results of all occurrences. If necessary, you can use [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md) to retrieve a single result.

## Angle

To calculate the angle of the line joining the two markers, use [`M_ANGLE`](../../Reference/meas/MmeasCalculate.md).

*[Image: angle.png]*

The angle is measured counter-clockwise, from the positive X-axis, and can be a value from 0 to 360°. The angle is returned in the output units specified using [`MmeasControl`](../../Reference/meas/MmeasControl.md) (for a measurement result buffer) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md).

## Line equation and distance

To calculate the equation of the line joining the two markers, use [`M_LINE_EQUATION`](../../Reference/meas/MmeasCalculate.md). To calculate the distance between the two markers, use [`M_DISTANCE`](../../Reference/meas/MmeasCalculate.md).

The following illustration shows two stripe markers, each of which is a multiple-occurrence marker; the first has two occurrences, and the second has three. In this example, Aurora Imaging Library calculates the line equation and distance between each corresponding occurrence (relative to each marker's reference position). Since each multiple-occurrence marker has a different number of occurrences, Aurora Imaging Library uses the smaller number and ignores the extra occurrence.

*[Image: MeasurementCalculateExample.png]*

Note that you can also retrieve the horizontal or vertical distance between two markers using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_DISTANCE_X`](../../Reference/meas/MmeasGetResult.md) or [`M_DISTANCE_Y`](../../Reference/meas/MmeasGetResult.md).
