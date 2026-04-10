---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Steps_to_finding_and_obtaining_measurements_of_markers
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Steps to finding and obtaining measurements of markers"
---

# Steps to finding and obtaining measurements of markers

The following steps provide a basic methodology for using the Aurora Imaging Library Measurement module:

1. Allocate an edge, stripe, or circle marker buffer. For a multiple-occurrence marker, set the number of occurrences of the edge or stripe.
   You can also allocate a point marker buffer or a multiple-point marker buffer. Points are not found, they are explicitly set and typically used as a reference marker.
2. In the marker buffer, specify the search region for the marker and set the marker's essential characteristics.
   Optionally, set a score characteristic for an edge, stripe, or circle marker. Note that preprocessing operations (such as filtering) can cause the location of edges to shift slightly. If precise edge location and measurement are crucial to your application, keep preprocessing operations to a minimum.
3. Grab or load a target image.
   Optionally, preprocess it to improve its quality.
4. Find the marker (defined in the marker buffer) in the target image and calculate measurements. Note that marker results are stored in the marker buffer, but they are stored separately.
5. Read the results and, if necessary, annotate an image (for example, the original target image) with them. You can also draw the specified chacteristics of the requested marker (rather than the marker characteristics found in the target image).
6. Free all your allocated objects, using [`MmeasFree`](../../Reference/meas/MmeasFree.md), unless [`M_UNIQUE_ID`](../../Reference/meas/MmeasAllocResult.md) was specified during allocation.

In general, the first two steps are performed once, while steps 3 to 5 are repeated as required. Note that you can avoid the first two steps by saving an initialized marker buffer on disk and restoring it when needed, using [`MmeasSaveMarker`](../../Reference/meas/MmeasSaveMarker.md), [`MmeasRestoreMarker`](../../Reference/meas/MmeasRestoreMarker.md), or [`MmeasStream`](../../Reference/meas/MmeasStream.md). Saving a marker buffer stores all of the marker settings, but does not save any found marker results.

You can also use [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md) to take measurements between two markers. For more information, see [Measurements between two markers](S16_Measurements_between_two_markers.md).
