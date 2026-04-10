---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Edge_markersfundamental_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Edge markersfundamental characteristics"
---

# Edge markers: basic characteristics that should be specified

This section describes basic edge marker characteristics that you should specify for most applications so that the appropriate edge marker is found. These edge marker characteristics are also characteristics that must be met.

To specify a characteristic of an edge marker that must be met, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). These are known as essential characteristics. Aurora Imaging Library can only find a marker if it meets all the essential characteristics. Note that certain essential characteristics, such as those related to the search region, multiple-occurrence markers, and the search algorithm, are described earlier in this chapter. Other more advanced edge marker characteristics that you can specify and that must be met are described later in this chapter.

## Polarity

The polarity of an edge describes whether it is rising or falling, relative to the direction of the search ([`M_ORIENTATION`](../../Reference/meas/MmeasSetMarker.md)). A rising edge denotes an increase (from dark to light) in grayscale values and a positive polarity, while a falling edge denotes a decrease (from light to dark) in grayscale values and a negative polarity. Use [`M_POLARITY`](../../Reference/meas/MmeasSetMarker.md) to specify that the polarity of the edge marker must be positive ([`M_POSITIVE`](../../Reference/meas/MmeasSetMarker.md)) or negative ([`M_NEGATIVE`](../../Reference/meas/MmeasSetMarker.md)). You can also specify that the edge marker can have any polarity ([`M_ANY`](../../Reference/meas/MmeasSetMarker.md)), in which case Aurora Imaging Library does not consider polarity when trying to find the marker (default).

*[Image: NewMeasDiagram.png]*

For more information about orientation and search direction, see [Marker orientation and search direction](S06_Search_region.md).

## Minimum edgevalue

As previously discussed in this chapter, Aurora Imaging Library establishes edgevalues by taking the normalized first derivative of the intensity profile. When you draw the edgevalues of each column, the graph generally resembles a peak when an edge is present. The minimum edgevalue is a type of threshold characteristic that you can set to represent the edgevalue above which a grayscale variation can be considered an edge. Any grayscale variation in the image that does not meet the specified minimum edgevalue is therefore discarded when Aurora Imaging Library is searching for the marker. Though the default minimum edgevalue is typically sufficient, you can change it using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md).

*[Image: edgedetectionedgevalueminandvarmin2.png]*

When searching for multiple edges, some advanced applications might also need you to specify the minimum prominence of the required edge peaks, using [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md). For more information about [`M_EDGEVALUE_VAR_MIN`](../../Reference/meas/MmeasSetMarker.md), and also about [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md), see [Search algorithm](S05_Search_Algorithm.md).
