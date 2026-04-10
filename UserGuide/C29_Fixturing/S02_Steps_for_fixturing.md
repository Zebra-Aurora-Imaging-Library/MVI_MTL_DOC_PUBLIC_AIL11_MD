---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Steps_for_fixturing
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Steps for fixturing"
---

# Steps for fixturing

The following steps provide a basic methodology for fixturing in Aurora Imaging Library:

1. Set up the analysis operation that will return a reference location that you can use to displace the relative coordinate system. The reference location must be at a fixed positional and angular offset from the object to analyze or process. For example, set up a Model Finder or Pattern Matching model that appears at a fixed offset from the target object.
2. Optionally, allocate and set up a fixturing offset object, using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_FIXTURING_OFFSET`](../../Reference/cal/McalAlloc.md) and [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md). This allows you to place the relative coordinate system at an offset from the calculated reference location.
3. Optionally, define a region of interest in the target image buffer with respect to the relative coordinate system, using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). This allows you to restrict processing and analysis to a region of the object.
4. Set up the processing and analysis operations that you need to perform once the relative coordinate system is fixtured to the object. You must set positions and angles in world units, since you want these to be with respect to the relative coordinate system. Most position and angle control types can be specified in world units if you set their corresponding control type (for example, [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md)) to [`M_WORLD`](../../Reference/meas/MmeasSetMarker.md).
5. Perform the analysis operation that will determine the reference location for each instance of the object to analyze/process in the image. Then, for each established reference location:
   1. Call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md), the reference location, and if set up, the fixturing offset object. This places the relative coordinate system at the specified offset from the reference location.
   2. Analyze or process the object, using the analysis/processing operations set up in step 4.
6. Repeat step 5 for each grabbed image.
7. Free the fixturing offset object using [`McalFree`](../../Reference/cal/McalFree.md), unless [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified during allocation.

If you know the position and angle for the new relative coordinate system with respect to the absolute coordinate system, you can skip step 2 and set the new relative coordinate system directly, using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) or [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) instead of using [`McalFixture`](../../Reference/cal/McalFixture.md).
