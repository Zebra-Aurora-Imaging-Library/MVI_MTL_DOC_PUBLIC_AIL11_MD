---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Edge_markersadvanced_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Edge markersadvanced characteristics"
---

# Edge markers: advanced characteristics that can be specified

This section describes the characteristics of an edge marker that you would usually set for advanced applications, when the correct marker is difficult to find. These characteristics fall into two categories: those that must be met (essential characteristics) and those for which you can set a range of acceptable scores (score characteristics).

## Essential characteristics

To specify an advanced essential characteristic of a marker, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library only finds a marker occurrence if it meets all the essential characteristics set in the marker buffer.

### Maximum association distance of subedges

The maximum association distance ([`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md)) determines the farthest a subedge can be from the fitted edge position, for Aurora Imaging Library to consider it part of the edge marker. By default, the maximum association distance is unlimited; that is, Aurora Imaging Library uses the entire box search region to establish markers. Typically, you might want to restrict the maximum association distance in certain types of noisy images, where Aurora Imaging Library can interpret the noise as subedges, which can then be incorrectly used to fit the marker.

To manage such situations, you should usually try restricting the size of the box search region; however, this might not always be convenient. For instance, if noisy images are rare but the edge location often varies, you might not want to reduce the box search region, although you have to account for an occasional noisy image that skews your results. In these cases, you can use [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) to restrict the image data that Aurora Imaging Library uses to establish the marker.

Aurora Imaging Library measures the specified maximum distance from the center of the fitted edge along a straight line that is parallel to the search direction. Subedges that fall outside the maximum association distance do not affect the marker's fit operation even if they are in the box search region. Therefore, you can consider [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) as a way of restricting the image data that Aurora Imaging Library uses to establish the edge, when it is not practical to decrease the size of box search region (or subregions).

Although the maximum association distance applies to subedges, you might still need to specify it when dealing with noisy images, even if you have not explicitly subdivided the box search region into subregions (subedges). For example, to calculate an edge marker's line equation (mean line), Aurora Imaging Library internally uses three subedges, even if you did not specify them (three fit points are a requirement to calculate a line). If an image has substantial specks (noise) in the subregions, the line that Aurora Imaging Library calculates might not be what you expect; for example, it can be at an incorrect angle. By reducing the maximum association distance, the line can be much closer to what is expected.

*[Image: MeasurementMaximumAssociationDistance.png]*

To retrieve the distance between the calculated marker and its farthest subedge, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_FIT_ERROR_MAX`](../../Reference/meas/MmeasGetResult.md). To retrieve the number of outliers (ignored subedges), use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_NUMBER_OF_OUTLIERS`](../../Reference/meas/MmeasGetResult.md).

### Minimum variation

The minimum variation refers to a type of threshold characteristic that you can set to represent the minimum prominence of the required edge peaks for the peak to be considered an occurrence of a marker. Advanced applications might need to specify this characteristic when trying to find multiple neighboring edges in the search region, which are formed such that the foreground of the first edge becomes the background of the next edge. To set the minimum variation characteristic, use [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md). Note that, regardless of the [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md) setting, edgevalues must always be greater than the minimum edgevalue specified ([`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md)) to be considered an occurrence of a marker. For more information, see [Minimum variation](S05_Search_Algorithm.md).

## Score characteristics

To specify a score characteristic of a marker, use [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). As previously discussed, the score characteristics only validate markers that have all the essential characteristics. Of these markers, Aurora Imaging Library chooses the one with the highest edge strength score, by default. This behavior is typically sufficient and you usually don't have to call [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) unless you are working on advanced applications.

Note that although you can specify multiple score characteristics, this can cause results that are difficult to interpret. It is often sufficient, and simpler, to use one score characteristic with which to find the best marker. Since the marker with the highest (best) strength score is found by default, you should therefore typically disable the strength score characteristic before setting another score characteristic (such as, for example, the contrast). For more information about how Aurora Imaging Library calculates scores (as well as how to set a minimum acceptable score), see [Score characteristics](S14_Score_characteristics.md).

### Strength score

As discussed earlier in this chapter, an edge's strength refers to its maximum edgevalue. The edge with the greatest strength can be referred to as the strongest edge and is, by default, the edge with the highest score.

> **Note:** The edge's strength can actually refer to what is theoretically the lowest edgevalue (that is, the largest negative edgevalue) since edges can be found with positive or negative polarity. However, for explanatory purposes, the documentation generally refers to edges in the absolute, and therefore only to positive edgevalues and edge strengths.

In the diagram below, the edge's strength is 100, which is the highest strength (and edgevalue) possible. This results from the theoretical case where the edge has a width of one pixel, is perfectly vertical, and has the greatest possible pixel value variation.

*[Image: EdgeValue.png]*

Unlike the edge in the above example, the edges _A_ and _B_ in the diagram below do not come from a very high pixel variation, which means a lower strength. Also, since edge _B_ is at an angle (its edge profile is distributed over ten edgevalues), while edge _A_ has an edge width of one pixel, the strength of edge _B_ (7) is less than edge _A_ (31). Therefore, if you are finding an edge marker based on strength, as is the default case, edge _A_ will be chosen.

*[Image: MeasurementStrength.png]*

If you want to find edge _B_ instead of edge _A_, you can use [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) to disable the strength score characteristic, and use the contrast score characteristic to select the best edge. Since the contrast is typically based on the difference in grayscale values between the start and the end of the intensity transition from which an edge is established, and edge _B_ has a higher contrast `(255 - 80 = 175)` than edge _A_ `(80 - 0 = 80),` edge _B_ would have a higher contrast score and be chosen as the edge found. For more information about specifying the contrast score characteristic, see [Contrast score](S09_Edge_markersadvanced_characteristics.md).

To retrieve the calculated edge strength, use [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_EDGE_STRENGTH`](../../Reference/meas/MmeasGetResult.md). To specify the expected edge strength and tolerable variances, or to disable its effect, use [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with [`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md).

You can also retrieve all the edgevalues of the marker buffer's search region, using [`MmeasGetResult`](../../Reference/meas/MmeasGetResult.md) with [`M_BOX_EDGEVALUES`](../../Reference/meas/MmeasGetResult.md). Specifically, this returns the calculated edgevalue for every projection value within the search region. This allows you to determine what Aurora Imaging Library will likely consider an edge. You can then use this information to, for example, specify an appropriate value for [`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md).

### Contrast score

Contrast generally refers to the distinctiveness of an edge. Setting the contrast score characteristic can be useful when, for example, you must distinguish an edge from several other edges, particularly when the required edge does not have the highest edge strength (usually due to poor illumination, which strength is very dependent on), or when the edge is at an angle.

To specify the contrast score characteristic, use either [`M_EDGE_CONTRAST_SCORE`](../../Reference/meas/MmeasSetScore.md) or [`M_EDGEVALUE_PEAK_CONTRAST_SCORE`](../../Reference/meas/MmeasSetScore.md), depending on whether you want to determine the contrast based on the positions of the start and end of the edge, or based on positions relative to the minimum and maximum of the edge peak, respectively. For more information, see [Search algorithm](S05_Search_Algorithm.md).

### Position score

As previously discussed, an edge marker's position refers to the position of its maximum edgevalue. With [`M_DISTANCE_FROM_BOX_ORIGIN_SCORE`](../../Reference/meas/MmeasSetScore.md), you can have Aurora Imaging Library calculate a score based on the proximity of the marker's position to the origin of its box search region.

Setting the position score characteristic can be useful when you want to find one particular edge marker that is among several edges that have similar characteristics and are all within the same box search region. For example, in the following illustration, there are three possible edge markers in the box search region, which are virtually identical; therefore, you cannot distinguish between them with score characteristics such as strength and contrast. However, with [`M_DISTANCE_FROM_BOX_ORIGIN_SCORE`](../../Reference/meas/MmeasSetScore.md), you can find the first marker since it has the highest position score, given that it is the closest to the box origin.

*[Image: MeasurementEdgeMarkerPosition.png]*
