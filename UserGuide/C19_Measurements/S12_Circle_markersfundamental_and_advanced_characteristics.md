---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Circle_markersfundamental_and_advanced_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Circle markersfundamental and advanced characteristics"
---

# Circle markers: basic and advanced characteristics that should or can be specified

A circle marker is simply a marker with a closed circular edge. Therefore, edge marker characteristics, as discussed previously in this chapter, generally apply to circle marker characteristics. To establish a circle marker, Aurora Imaging Library finds and extracts subedges using rectangular subregions of a ring search region and applies a circular fit to the subedges. The illustration below shows different possible circles, and how Aurora Imaging Library extracts and fits the subedges according to the ring search region. Note that, although circle markers are closed, the edges that Aurora Imaging Library uses to create them might not be closed; that is, Aurora Imaging Library can establish a circle marker even if part of it is missing. This is due to the fact that only three valid subedges are required to identify a circle marker. For more information, see [Ring search region](S06_Search_region.md).

*[Image: MeasurementHappyCircle.png]*

This section describes basic circle marker characteristics that you should specify for most applications so that the appropriate circle marker is found, as well as some advanced characteristics of a circle marker that you would usually set for advanced applications, when the correct marker is difficult to find. The latter characteristics fall into two categories: those that must be met (essential characteristics) and those for which you can set a range of acceptable scores (score characteristics).

[`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) sets essential characteristic settings that you should consider both basic (you typically change them) and advanced (you rarely change them), while [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) should be considered entirely advanced.

## Essential characteristics

To specify an essential characteristic of a circle marker, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library only finds a marker occurrence if it meets all the essential characteristics set in the marker buffer.

### Polarity

The polarity of an edge describes whether it is rising (increase in grayscale value) or falling (decrease in grayscale value). For circle markers, the polarity is measured outward, from the circle's center (radially). This is the same direction that [`MmeasFindMarker`](../../Reference/meas/MmeasFindMarker.md) uses to find circles.

*[Image: MeasurementCirclePolarity.png]*

To specify the polarity, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POLARITY`](../../Reference/meas/MmeasSetMarker.md) set to [`M_ANY`](../../Reference/meas/MmeasSetMarker.md) (default), [`M_NEGATIVE`](../../Reference/meas/MmeasSetMarker.md) (falling edge), or [`M_POSITIVE`](../../Reference/meas/MmeasSetMarker.md) (rising edge). Polarity is considered a basic characteristic since you must typically consider it for most applications.

### Maximum association distance of subedges

As previously discussed with edge markers, you can ignore unwanted subedges using [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md). Aurora Imaging Library measures the maximum distance radially, along the perimeter of the fitted circle within the rectangular subregions. Subedges that fall outside this range do not affect the marker's fit operation, even if they are in the search region. Therefore, [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) gives you greater control over restricting the area in which to perform the fit operation, allowing you to better ignore unwanted image elements, such as specks and noise, which could skew your results.

*[Image: MeasurementMaxAssocCircle.png]*

## Score characteristics

To specify a score characteristic of a marker, use [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md). As previously discussed, the score characteristics only validate markers that have all the essential characteristics. Of these markers, Aurora Imaging Library chooses the one with the highest edge strength score, by default. This behavior is typically sufficient and you usually don't have to call [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) unless you are working on advanced applications.

Note that although you can specify multiple score characteristics, this can cause results that are difficult to interpret. It is often sufficient, and simpler, to use one score characteristic with which to find the best marker. Since the marker with the highest (best) strength score is found by default, you should therefore typically disable the strength score characteristic before setting another score characteristic (such as, for example, the contrast). For more information about how Aurora Imaging Library calculates scores (as well as how to set a minimum acceptable score), see [Score characteristics](S14_Score_characteristics.md).

### Radius

Radius refers to the distance between a circle's circumference and its center. If the target image has multiple circles with similar radii that share a common position (center), you might have difficulty isolating the circle you want. In such cases, you can find the marker based on a score Aurora Imaging Library establishes according to how well the circle's radius corresponds to the radius specifications set with [`M_RADIUS_SCORE`](../../Reference/meas/MmeasSetScore.md).

*[Image: MeasurementCircleRadius.png]*

In this example, there are three circles that share the same position (their centers coincide). If you want to find the biggest of these circles, you would typically narrow the ring search region. However, in certain cases, this might cause you to miss smaller circles that you would otherwise want to find if bigger circles were not there. Without having to adjust other characteristics (such as the ring search region), you can use [`M_RADIUS_SCORE`](../../Reference/meas/MmeasSetScore.md) to calculate a score for the circles within the ring search region, such that the circle with the biggest radius receives that highest score and is therefore found by Aurora Imaging Library.
