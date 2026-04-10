---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Measurements
section: Measurement_module
module_tag: meas
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / meas / Measurement module"
---

# Aurora Imaging Library Measurement module

The Aurora Imaging Library Measurement module allows you to find sets of markers in an image based on differences in pixel intensities. Upon finding a marker, such as an edge, stripe, or circle, the module returns its spatial reference position, as well as analytical measurements such as its width and angle. The module also allows you to create point markers at explicit locations, and to take a variety of measurements between any two markers.

You can use the Measurement module to perform numerous types of imaging tasks. For example, you can find stripes that represent the conductive traces on printed circuit boards (PCBs) or pins protruding from chips and measure their angle and width, as well as the distance between them.

*[Image: MeasurementIntroStripeExample.png]*

The Measurement module relies on a one-dimensional analysis. As such, it has several advantages over other modules, such as Pattern Matching, when locating relatively simple image characteristics; it is independent of lighting, more tolerant of slight differences, and much faster.

To help locate the marker in an image, you can define its essential characteristics, such as the required polarity and location. The more precisely you define the characteristics of a marker, the more likely Aurora Imaging Library can distinguish it from other similar (though unwanted) aspects of the image. If necessary, Aurora Imaging Library can also further distinguish a marker by calculating scores according to how well some of its characteristics are represented in the image; the higher the score, the more likely the marker found is the one you want.

The Measurement module can operate on 8-bit or 16-bit unsigned grayscale image buffers. Measurements are made with subpixel accuracy and results can be returned in pixels or real-world units. For more information, see [Calibrating your camera setup](../C28_Calibration/ChapterInformation.md).
