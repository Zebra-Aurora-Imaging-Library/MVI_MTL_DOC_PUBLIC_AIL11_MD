---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Color_processing_and_analysis_example
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Color processing and analysis example"
---

# Color processing and analysis examples

The examples _ColorRelativeCalibration.cpp_ and _Mcol.cpp_ demonstrate the various operations available with the Aurora Imaging Library Color Analysis module. To run these and other examples, use Aurora Imaging Example Launcher. You can also view the general example _Mcol.cpp_here:

> **Code example:** [Mcol.cpp](Mcol.cpp)

## Example of relative color calibration operations

The example _ColorRelativeCalibration.cpp_ shows you how to use relative color calibration to perform:

- Food inspection, with [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).
  *[Image: ColorRelativeCalibration_ExampleFoodHistogramBased.png]*
- Print inspection, with [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).
  *[Image: ColorRelativeCalibration_ExamplePrintGlobalMeanVariance.png]*
- Electronic board inspection, with [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) ([`McolSetMethod`](../../Reference/col/McolSetMethod.md)).
  *[Image: ColorRelativeCalibration_ExampleBoardColorToColor.png]*

## Example of color processing operations

The example _Mcol.cpp_ shows you how to use color processing operations to perform:

- Color segmentation of an image by classifying each pixel with 1 out of 6 color-samples. The ratio of each color in the image is then calculated.
  *[Image: Color_ExampleSegmentation.png]*
- Color identification of circular regions in objects located with the Aurora Imaging Library Model Finder module.
  *[Image: Color_ExampleIdentification.png]*
- Color separation of a signature and a stamp.
  *[Image: Color_ExampleSeparation.png]*
