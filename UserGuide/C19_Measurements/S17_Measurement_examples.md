---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Measurement_examples
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Measurement examples"
---

# Measurement examples

The Measurement example _MMeas.cpp_ demonstrates how to find a stripe in an image and measure its position, width, and angle.

*[Image: MeasExample2.png]*

This example also demonstrates how to find the average position, average width, and average angle of a row of pins on a chip.

*[Image: Example1.png]*

In addition, the example shows you how to perform drawing operations and return measurement statistics.

> **Code example:** [mmeas.cpp](mmeas.cpp)

## Circle markers and advanced settings

There are also examples regarding circle markers and advanced measurement settings. These examples illustrate how to handle:

- A simple circle.
  *[Image: MeasurementCircleProcessingExample.png]*
  In this case, a circle is found with default settings.
- A circle with two possible outer edges (circles).
  *[Image: MeasurementCircleProcessingExampleDoubleEdge.png]*
  In this case, only the outermost circle must be measured. To achieve proper results, [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) (restricts edges) was used.
- A circle that is both incomplete and textured (can detect many edges).
  *[Image: MeasurementCircleProcessingExampleOcclusion.png]*
  In this case, only the main circle must be measured. To achieve proper results, [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_EDGEVALUE_MIN`](../../Reference/meas/MmeasSetMarker.md) was set above the default (reduces noise) and [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) was given a value (restricts edges).
- Concentric circles.
  *[Image: MeasurementCircleProcessingExampleNumerousCircles.png]*
  In this case, only the inner-most circle must be measured. To achieve proper results, [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MAX_ASSOCIATION_DISTANCE`](../../Reference/meas/MmeasSetMarker.md) (restricts edges) was used. Also, the strength score characteristic was removed and a radius score characteristic was set, using [`MmeasSetScore`](../../Reference/meas/MmeasSetScore.md) with [`M_STRENGTH_SCORE`](../../Reference/meas/MmeasSetScore.md) and [`M_RADIUS_SCORE`](../../Reference/meas/MmeasSetScore.md) (locates smallest circle).

To run these examples, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.
