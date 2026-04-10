---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Markers
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Markers"
---

# Markers

To take any type of measurement with the Aurora Imaging Library Measurement module, you must first allocate a marker buffer and initialize it with the definition (image characteristics) of the marker for which to search. Allocate the marker buffer using [`MmeasAllocMarker`](../../Reference/meas/MmeasAllocMarker.md) and initialize it using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md). Each marker buffer can define the characteristics for only one specified type of marker in the target image.

## Marker types

There are four types of markers:

- **Point marker**. A marker consisting of a single point or multiple points. You cannot search for point markers, but you can explicitly place them at required locations as reference positions and use them in calculations involving two markers. You can, for example, use point markers to represent positional results from a previous pattern matching or blob analysis operation.
- **Edge marker**. A marker consisting of an edge or multiple edges. In an image, edges are curves that delineate a boundary, which can be established from intensity transitions.
  *[Image: edgemarker.png]*
- **Stripe marker**. A marker consisting of a stripe (two edges) or multiple stripes. The edges of a stripe marker do not have to be parallel.
  *[Image: stripemarker.png]*
- **Circle marker**. A marker consisting of a single circle (a circular edge).
  *[Image: MeasurementCircleMarker.png]*

## A multiple-occurrence marker

The Measurement module allows you to define a multiple-occurrence edge, stripe, or point marker. For edges and stripes, this allows you to search for multiple instances of the same image characteristics. You can specify the number of edges or stripes to locate, or the number of points to define, using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md); you cannot define a multiple-occurrence circle marker. The following example illustrates four occurrences of a stripe marker.

*[Image: MeasurementMultipleStripeMarkerIntro.png]*

By searching for a multiple-occurrence marker, you can take global measurements, and then compare them for conformity. For example, you can search for a multiple-occurrence marker to verify that a series of presumed-identical pins are actually of equal width or that the spacing between the pins is within established limits. Note that to define a multiple-occurrence marker, only one set of marker characteristics is specified, but Aurora Imaging Library searches for a specified number of instances of the same characteristics.
