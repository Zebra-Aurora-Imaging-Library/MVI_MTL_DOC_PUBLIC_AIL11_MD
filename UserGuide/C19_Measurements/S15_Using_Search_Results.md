---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Using_Search_Results
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Using Search Results"
---

# Search results

You can obtain results using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) (or [`MmeasGetResultSingle`](../../Reference/meas/MmeasGetResultSingle.md)) or [`MmeasGetScore`](../../Reference/meas/MmeasGetScore.md). Aurora Imaging Library stores results from [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) directly within the marker buffer rather than in a result buffer. Results do not overwrite specified marker characteristics. If you have specified a score characteristic with [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md), you can obtain the results using [`MmeasGetScore`](../../Reference/meas/MmeasGetScore.md). All positional results are relative to the center of the top-left pixel in the target image or the origin of the relative coordinate system of an calibrated image.

For a multiple-occurrence marker, [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) takes the required measurements for all the found edges or stripes (all marker occurrences). To obtain the number of edges or stripes found, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_NUMBER`](../../Reference/meas/MmeasGetResult.md). Global results, such as the maximum, minimum, mean, or standard deviation of any characteristic of the calculated results can be returned.

## Finding the marker and taking measurements

Use [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) to find a marker and take its measurements, such as its strength, position, and contrast. By default, Aurora Imaging Library performs all measurements that apply to the marker type, which is typically what you want. Measurements are taken with subpixel accuracy. When you call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), however, you can limit the number of measurements performed using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Advanced users can modify how Aurora Imaging Library calculates the positions of the subpixels using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUBPIXEL_MODE`](../../Reference/meas/MmeasSetMarker.md).

## Edge measurements taken by the find operation

When you call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), Aurora Imaging Library not only locates the best possible marker, but also takes its measurements, such as the marker's angle and width. If you call this function with [`M_DEFAULT`](../../Reference/meas/MmeasFindMarker.md), Aurora Imaging Library performs all possible measurements on the marker, which is typically appropriate for the vast majority of applications. However, you can also call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) with a specific set of measurements to calculate. In this case, be aware that certain results might not be available for retrieval. For example, if you use [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) to calculate just the line equation of an edge marker ([`M_LINE_EQUATION`](../../Reference/meas/MmeasFindMarker.md)), you cannot use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md) to retrieve the X- and Y-coordinates of the end of the edge. Except for some very rare cases, there is no reason to restrict the default measurements [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) performs.

Some of the less evident measurements that Aurora Imaging Library takes are discussed below; for a complete list, refer to [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) in the Aurora Imaging Library Reference.

### Line equation (mean line)

When you call the find operation, Aurora Imaging Library calculates the equation of the line that best follows the edge marker. Aurora Imaging Library bases the line equation on the general form, `_Ax_ + _By_ + _C_ = 0`. Use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_LINE_A`](../../Reference/meas/MmeasGetResult.md), [`M_LINE_B`](../../Reference/meas/MmeasGetResult.md), and [`M_LINE_C`](../../Reference/meas/MmeasGetResult.md) to retrieve the coefficients _A_, _B_, and _C_, respectively.

If you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library internally uses three, since it is the minimum number it requires to calculate the line equation (mean line).

### Angle

When you call the find operation, Aurora Imaging Library calculates the angle of an edge marker, which you can then retrieve using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_ANGLE`](../../Reference/meas/MmeasGetResult.md). Aurora Imaging Library measures the angle, as a value from 0° to 360°, between the positive X-axis and the edge's mean line.

*[Image: angle_edge.png]*

### Length

When you call the find operation, Aurora Imaging Library also calculates the length of the side of the search region perpendicular to the search direction, in pixels, which you can then retrieve using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_LENGTH`](../../Reference/meas/MmeasFindMarker.md). This is useful to retrieve the number of pixels that were projected into a single value of the intensity profile without having to find [`M_ORIENTATION`](../../Reference/meas/MmeasGetResult.md) first.

### Position and reference position

The edge marker's position refers to the X- and Y-coordinates of its maximum edgevalue (highest edge peak) and the center of the search region, as found with [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). To retrieve the marker's found position, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_POSITION`](../../Reference/meas/MmeasGetResult.md). To draw a cross (+) at this position, use [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_POSITION`](../../Reference/meas/MmeasDraw.md); if you are using subregions, you can specify [`M_DRAW_SUB_POSITIONS`](../../Reference/meas/MmeasDraw.md) to draw a cross at the found position of each subedge.

*[Image: MeasurementEdgeReferencePosition.png]*

Aurora Imaging Library uses the found position of the edge marker ([`M_DRAW_POSITION`](../../Reference/meas/MmeasDraw.md)) as its default reference position, when calculating measurements between two markers using [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md). To change the default, call [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MARKER_REFERENCE`](../../Reference/meas/MmeasSetMarker.md) and specify X- and Y-offset values relative to the found position. Only [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) makes use of this reference position. For more information, see [Measurements between two markers](S16_Measurements_between_two_markers.md).

### Intersection point

When you call the find operation, Aurora Imaging Library calculates the X- and Y-coordinates of the two intersection points between the marker's mean line and the box search region. To retrieve the intersection points, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_LINE_END_POINT_FIRST`](../../Reference/meas/MmeasGetResult.md) and [`M_LINE_END_POINT_SECOND`](../../Reference/meas/MmeasGetResult.md).

*[Image: MeasurementLineEquationIntersection.png]*

For vertical edges, the first intersection point is typically at the top of the box search region, while the second intersection point is typically at the bottom. For horizontal edges, the first intersection point is typically on the right of the box search region, while the second intersection point is typically on the left. However, the intersection point that Aurora Imaging Library considers first and second can change depending on certain factors, such as the orientation, the edge's angle, and the rotation of the box search region.

*[Image: MeasurementLineEquationIntersectionAtypical.png]*

### An edge's start, end, and width (based on the intensity profile)

When you call the find operation, Aurora Imaging Library calculates the start and end position of the edge, based on the intensity profile of the search region. To retrieve these results, use [`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md). You can also retrieve the distance between [`M_EDGE_START`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGE_END`](../../Reference/meas/MmeasGetResult.md), using [`M_EDGE_WIDTH`](../../Reference/meas/MmeasGetResult.md). For more information, see [Search algorithm](S05_Search_Algorithm.md).

### An edge peak's maximum/minimum position and width (based on the first derivative)

When you call the find operation, Aurora Imaging Library calculates the minimum and maximum positions of the edge peak, based on the edge profile (the normalized first derivative of the intensity profile). To retrieve these results, use [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md). You can also retrieve the distance between [`M_EDGEVALUE_PEAK_POS_MIN`](../../Reference/meas/MmeasGetResult.md) and [`M_EDGEVALUE_PEAK_POS_MAX`](../../Reference/meas/MmeasGetResult.md), using [`M_EDGEVALUE_PEAK_WIDTH`](../../Reference/meas/MmeasGetResult.md). For more information, see [Search algorithm](S05_Search_Algorithm.md).

## Stripe measurements taken by the find operation

When you call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), Aurora Imaging Library not only locates the best possible marker, but also takes its measurements, such as the stripe marker's length and width. As with edge markers, you would typically perform all possible measurements on the stripe ([`M_DEFAULT`](../../Reference/meas/MmeasFindMarker.md)).

### Line equation (mean line)

When you call the find operation to find stripe markers, Aurora Imaging Library calculates the equation of three lines: the stripe's mean line, the line that best represents the stripe's first edge, and the line that best represents the stripe's second edge.

Much like for edge markers, each line equation is based on the general form, `_Ax_ + _By_ + _C_ = 0`. Use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_LINE_A`](../../Reference/meas/MmeasGetResult.md), [`M_LINE_B`](../../Reference/meas/MmeasGetResult.md), and [`M_LINE_C`](../../Reference/meas/MmeasGetResult.md) to retrieve the coefficients _A_, _B_, and _C_, respectively. You can retrieve these coefficients for a specific edge's line equation, using the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResult.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResult.md). If you do not specify a specific edge, the function returns the coefficients of the line equation for the stripe's mean line. The mean line follows the center of the marker occurrence, between the lines Aurora Imaging Library calculates for the stripe's two outermost edges.

*[Image: linestr.png]*

If you did not specify multiple subregions ([`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SUB_REGIONS_NUMBER`](../../Reference/meas/MmeasSetMarker.md)), Aurora Imaging Library internally uses three subregions, since it is the minimum number it requires to calculate a line equation.

### Angle

When you call the find operation, Aurora Imaging Library calculates the angle of a stripe marker, in much the same manner as it does for an edge based marker (discussed in [](S15_Using_Search_Results.md)), that is to say by measuring the angle between the positive X-axis and the stripe's mean line. The angle of a stripe marker is independent of the angle of the search region. The angle is measured in the coordinate system specified, and can be changed using [`M_RESULT_OUTPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md).

*[Image: angle_singlestripe.png]*

You can also retrieve the angle of the stripe's first or second edge, using [`M_ANGLE`](../../Reference/meas/MmeasGetResult.md) and the combination value [`M_EDGE_FIRST`](../../Reference/meas/MmeasGetResult.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasGetResult.md), respectively.

### Length

As with an edge marker, the length of a stripe marker is the length of the side of the search region perpendicular to the search region. The retrieval of the length of a marker is discussed in [](S15_Using_Search_Results.md).

### Position and reference position

The stripe marker's position refers to the X- and Y-coordinates of the center of a theoretical line between the position of the stripe's two outermost edges, as found with [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). Note that since the position of an edge refers to the X- and Y-coordinates of its maximum edgevalue and the center of the search region, the stripe marker's found position will always be along the center of the search region. The process of retrieving the found position for a stripe marker is much the same as for an edge marker, as discussed in [](S15_Using_Search_Results.md). Furthermore, all operations in the previously mentioned corresponding section for an edge marker can also be performed for a stripe marker.

*[Image: ReferenceStripe.png]*

### Stripe width

To retrieve the actual stripe width, call [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_STRIPE_WIDTH`](../../Reference/meas/MmeasGetResult.md). This result is available even if you do not use [`M_STRIPE_WIDTH_SCORE`](../../Reference/meas/MmeasSetScore.md). To draw an H-type line (|-|) along the found width of the stripe, use [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_WIDTH`](../../Reference/meas/MmeasDraw.md).

### Maximum association distance of subedges

To retrieve the distance between the calculated stripe marker and its farthest subedge, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_FIT_ERROR_MAX`](../../Reference/meas/MmeasGetResult.md). To retrieve the number of outliers (ignored subedges), use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_NUMBER_OF_OUTLIERS`](../../Reference/meas/MmeasGetResult.md).

## Circle measurements taken by the find operation

When you call [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md), Aurora Imaging Library not only locates the best possible marker, but also takes its measurements, such as the circle marker's position and radius. As with edge markers, you would typically perform all possible measurements on the circle ([`M_DEFAULT`](../../Reference/meas/MmeasFindMarker.md)).

### Position and reference position

The circle marker's position refers to the X- and Y-coordinates of its center, as found with [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md). The process of retrieving the found position for a circle marker is much the same as for an edge or stripe marker, as discussed in [](S15_Using_Search_Results.md). Furthermore, all operations in the previously mentioned corresponding section for an edge marker can also be performed for a circle marker.

*[Image: MeasurementCircleReferencePosition.png]*

### Maximum association distance of subedges

The retrieval of the distance between a circle marker and its farthest subedge is done in much the same way as it is done for a stripe marker, as discussed in [](S15_Using_Search_Results.md).

### Radius

To retrieve a circle marker's radius, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_RADIUS`](../../Reference/meas/MmeasGetResult.md). This result is available even if you do not use [`M_RADIUS_SCORE`](../../Reference/meas/MmeasSetScore.md).

## Drawing

To verify marker specifications or results, you can call [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_MARKER`](../../Reference/meas/MmeasDraw.md) or [`M_RESULT`](../../Reference/meas/MmeasDraw.md), respectively; that is, you can perform the drawing operation ([`M_DRAW_...`](../../Reference/meas/MmeasDraw.md)) using the expected marker characteristics ([`M_MARKER`](../../Reference/meas/MmeasDraw.md)), or using the marker characteristics found in the target image ([`M_RESULT`](../../Reference/meas/MmeasDraw.md)). For example, you can draw the marker's search region defined using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) ([`M_DRAW_SEARCH_REGION`](../../Reference/meas/MmeasDraw.md)), as well as the found position of a marker ([`M_DRAW_POSITION`](../../Reference/meas/MmeasDraw.md)). When drawing results, you can use the [`M_RESULT_PER_SUBREGION()`](../../Reference/meas/MmeasDraw.md) macro to indicate that Aurora Imaging Library should perform the drawing operation according to the specified subregions (one or all) of the specified occurrences (one or all). For stripe markers, you can perform certain drawing operations based on the marker's first or second outermost edge, by adding either [`M_EDGE_FIRST`](../../Reference/meas/MmeasDraw.md) or [`M_EDGE_SECOND`](../../Reference/meas/MmeasDraw.md) to the operation (for example, [`M_DRAW_POSITION`](../../Reference/meas/MmeasDraw.md) + [`M_EDGE_SECOND`](../../Reference/meas/MmeasDraw.md)). A stripe's first edge is the edge located closest to the search region origin.

Aurora Imaging Library stores both the specified (expected) marker characteristics, and the characteristics of marker results, in the same measurement marker buffer. You must therefore pass the identifier of this one marker to [`MmeasDraw`](../../Reference/meas/MmeasDraw.md), whether you are using [`M_MARKER`](../../Reference/meas/MmeasDraw.md) or [`M_RESULT`](../../Reference/meas/MmeasDraw.md). [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) also accepts the identifier of a measurement result buffer, but only for drawing the resulting line between two markers when using [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md). For more information, see [Measurements between two markers](S16_Measurements_between_two_markers.md).

To control the drawing color of [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) operations, you can use a previously allocated 2D graphics context or the default 2D graphics context (see [2D graphics context](../C26_Generating_graphics/S03_Graphics_context.md)). You can draw directly into an image buffer or a 2D graphics list. By drawing into the display's overlay buffer or associating the 2D graphics list with the display, you can also annotate non-destructively. For more information, see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md).
