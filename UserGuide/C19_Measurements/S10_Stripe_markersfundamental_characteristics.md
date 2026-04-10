---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Stripe_markersfundamental_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Stripe markersfundamental characteristics"
---

# Stripe markers: basic characteristics that should be specified

A stripe marker is simply a marker with two edges. Therefore edge marker characteristics, as discussed previously in this chapter, generally apply to stripe markers. The following example illustrates two stripe markers and some of their associated characteristics.

*[Image: StripeDiagram.png]*

This section describes basic stripe marker characteristics that you should specify for most applications so that the appropriate stripe marker is found. These stripe marker characteristics are also characteristics that must be met.

To specify a characteristic of a stripe marker that must be met, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). These are known as essential characteristics. Aurora Imaging Library can only find a marker if it meets all the essential characteristics. Note that certain essential characteristics, such as those related to the search region, multiple-occurrence markers, and the search algorithm, are described earlier in this chapter. Other more advanced stripe marker characteristics that you can specify and that must be met are described later in this chapter. Note that this section generally focuses on aspects of stripe markers that differ from edge markers. For stripes, characteristics usually apply to its mean line, to one of its two outermost edges, or to the area within its outermost edges. A stripe's first edge is the edge located closest to the search region origin.

## Polarity

As explained for edge markers, polarity describes whether an edge is rising (increase in grayscale value) or falling (decrease in grayscale value). You can set the polarity for the first or second edge of a stripe by setting [`M_POLARITY`](../../Reference/meas/MmeasSetMarker.md) to [`M_POSITIVE`](../../Reference/meas/MmeasSetMarker.md) (rising edge), [`M_NEGATIVE`](../../Reference/meas/MmeasSetMarker.md) (falling edge), or [`M_ANY`](../../Reference/meas/MmeasSetMarker.md) (rising or falling edge). You can also set the second edge of a stripe to the same ([`M_SAME`](../../Reference/meas/MmeasSetMarker.md)) or the opposite ([`M_OPPOSITE`](../../Reference/meas/MmeasSetMarker.md)) polarity, relative to the first edge.

*[Image: meas_polarity.png]*

When setting the polarity of a marker, it is important to keep in mind the direction of the search. For more information, see [Marker orientation and search direction](S06_Search_region.md).
