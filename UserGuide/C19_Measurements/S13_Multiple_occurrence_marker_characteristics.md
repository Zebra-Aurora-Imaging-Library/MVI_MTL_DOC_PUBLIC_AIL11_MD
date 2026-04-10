---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Multiple_occurrence_marker_characteristics
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Multiple occurrence marker characteristics"
---

# Multiple-occurrence marker characteristics

To define a multiple edge or stripe marker, specify the number of edges or stripes to find using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md); the default is 1. If you expect a regular spacing pattern between the edges or stripes, you can have Aurora Imaging Library calculate a score according to how well the actual found spacing adheres to the expected spacing, using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with [`M_SPACING_SCORE`](../../Reference/meas/MmeasSetScore.md). In this case, the spacing score can affect which markers Aurora Imaging Library finds. To view the found spacing between the multiple edges or stripes found, you can draw an H-type line (|-|) between them using [`MmeasDraw`](../../Reference/meas/MmeasDraw.md) with [`M_DRAW_SPACING`](../../Reference/meas/MmeasDraw.md). For more information, see [Score characteristics](S14_Score_characteristics.md).

*[Image: Spacing.png]*

Unless you specify a minimum number of occurrences ([`M_NUMBER_MIN`](../../Reference/meas/MmeasSetMarker.md)), you will receive no results if the number of edges or stripes found falls below [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md). If the exact number of edges or stripes is unknown, set [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md) to [`M_ALL`](../../Reference/meas/MmeasSetMarker.md).

You can also define a multiple-occurrence point marker. To do so, use [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md) (the first point's position), [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md) (the number of points), and [`M_SPACING`](../../Reference/meas/MmeasSetMarker.md) (the distance between points). You can only define a multiple-occurrence point marker that uses the same spacing between its points. If you require irregularly spaced points, do not use a multiple-occurrence point marker; specify individual point markers instead.

## Dealing with a very large image with many stripes

When working with a very large image with many stripes, searching for an unknown number of stripes can significantly increase processing time. To reduce processing time, you can try:

- Avoiding using [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md) with [`M_ALL`](../../Reference/meas/MmeasSetMarker.md) because this can increase the number of possible edges that are considered when finding stripes.
- Setting [`M_NUMBER`](../../Reference/meas/MmeasSetMarker.md) to an expected value to decrease search time.
- If [`M_ALL`](../../Reference/meas/MmeasSetMarker.md) must be used, divide the search region into smaller tiles (see [Search region](S06_Search_region.md)). To prevent splitting a stripe between two consecutive regions, ensure that the tiles overlap by at least the width of the largest expected stripe.
