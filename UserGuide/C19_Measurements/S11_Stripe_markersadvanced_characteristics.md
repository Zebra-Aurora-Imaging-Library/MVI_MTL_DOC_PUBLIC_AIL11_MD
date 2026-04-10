---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Stripe_markersadvanced_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Stripe markersadvanced characteristics"
---

# Stripe markers: advanced characteristics that can be specified

This section describes the characteristics of a stripe marker that you would usually set for advanced applications, when the correct marker is difficult to find. These characteristics fall into two categories: those that must be met (essential characteristics) and those for which you can set a range of acceptable scores (score characteristics).

Note that this section focuses on stripe marker characteristics that differ from edge marker characteristics, which have already been discussed earlier in this chapter. In general, characteristics applicable to edge markers are also applicable to stripe markers.

## Essential characteristics

To specify an advanced essential characteristic of a stripe marker, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library only finds a marker occurrence if it meets all the essential characteristics.

### Inclusion point

An inclusion point is an X- and Y-position that you can specify in the target image, using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_INCLUSION_POINT`](../../Reference/meas/MmeasSetMarker.md). A stripe can only be found if the inclusion point falls on the inside, or on the outside, of the stripe's two outer edges, depending on the [`M_INCLUSION_POINT_INSIDE_STRIPE`](../../Reference/meas/MmeasSetMarker.md) setting. By default, there is no inclusion point.

Using an inclusion point can be useful when you know the general location at which Aurora Imaging Library should find the stripe, and you are having difficulty finding it (for example, when other aspects of the image have similar features or stronger edges, or when you cannot set a proper box search region without clipping other similar image characteristics). If necessary, you can set the inclusion point outside the search region (to determine its position, Aurora Imaging Library projects it along the one-dimensional pixel summation). To draw the inclusion point, use [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_INCLUSION_POINT`](../../Reference/meas/MmeasDraw.md).

### Maximum association distance of subedges

As previously discussed with edge markers, you can ignore unwanted subedges for stripe markers using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library measures the subedge's distance from the stripe's center (that is, the center of the fitted center line between the positions of the stripe's two outermost edges). Subedges that fall beyond this range do not affect the marker's fit operation, even if they are in the search region. Therefore, [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) gives you greater control over restricting the area in which to perform the fit operation when decreasing the box search region is not possible, allowing you to better ignore unwanted image elements such as specks and noise, which could be skewing your results.

Although the maximum association distance applies to subedges, you might still need to specify it when dealing with noisy images, even if you have not explicitly subdivided the box search region into subregions (subedges). For example, to calculate the line equations for a stripe marker, Aurora Imaging Library internally uses three subedges, even if you did not specify them (three fit points are a requirement to calculate a line). If an image has substantial specks (noise) in the subregions, the lines that Aurora Imaging Library calculates for the stripe might not be what you expect; for example, they can be at an incorrect angle. By reducing the maximum association distance, the lines can be much closer to what is expected.

*[Image: MeasurementMaximumAssocStripe.png]*

## Score characteristics

To specify a score characteristic of a marker, use [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). As previously discussed, the score characteristics only validate markers that have all the essential characteristics. Of these markers, Aurora Imaging Library chooses the one with the highest edge strength score, by default. This behavior is typically sufficient and you usually don't have to call [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) unless you are working on advanced applications.

Note that although you can specify multiple score characteristics, this can cause results that are difficult to interpret. It is often sufficient, and simpler, to use one score characteristic with which to find the best marker. Since the marker with the highest (best) strength score is found by default, you should therefore typically disable the strength score characteristic before setting another score characteristic (such as, for example, the contrast). For more information about how Aurora Imaging Library calculates scores (as well as how to set a minimum acceptable score), see [Score characteristics](S14_Score_characteristics.md).

### Edges inside a stripe marker

If there are multiple edges within a stripe, you might have difficulty isolating the edges of the target stripe (for example, the stripe consisting of the two outer edges). In such cases, you can specify [`M_EDGE_INSIDE_SCORE`](../../Reference/meas/MmeasSetScore.md), which is a score characteristic related to the number of edges located between the outermost edges of a stripe marker.

*[Image: stripein.png]*

In this example, two stripes share the same position since their centers coincide. Either stripe marker can be found, depending on whether [`M_EDGE_INSIDE_SCORE`](../../Reference/meas/MmeasSetScore.md) specifies that the stripe must contain two or zero edges. This allows you to avoid setting other characteristics to find the stripe (such as its width). When specifying the number of edges that should be inside a stripe, you should typically set the value in increments of two, since stripes always require two edges.

### Stripe width

As previously discussed, Aurora Imaging Library measures the width of a stripe marker as the average distance in pixels between its outermost edges, perpendicular to the orientation of its box search region.

*[Image: stripwid.png]*

To find a stripe marker according to an expected width value, you can use [`M_STRIPE_WIDTH_SCORE`](../../Reference/meas/MmeasSetScore.md).
