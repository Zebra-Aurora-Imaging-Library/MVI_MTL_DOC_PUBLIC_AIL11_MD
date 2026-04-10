---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Color_processing_and_analysis_overview
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Color processing and analysis overview"
---

# Color processing and analysis overview

Although most cameras can acquire color images, most analysis modules only operate on monochrome images. In these cases, you must do one of the following: convert the color image to grayscale, create a child buffer from one of the three color bands, or copy one of the three color bands to a 1-band buffer. Using Aurora Imaging Library Lite, you can perform these operations and store the transformed data in a monochrome buffer. This data can then be passed to an analysis module, which typically requires the full version of Aurora Imaging Library.

You can, however, implement advanced color processing and analysis on color images directly, with the Aurora Imaging Library Color Analysis module. You can use this module to perform the following color-based procedures:

- **Transformation (relative color calibration)**. The transformation operation allows you to adjust color data according to a specific color reference, using relative color calibration. This can be useful to, for example, homogenize colors that can appear unalike when taken with different cameras or illuminants.
  *[Image: ColorRelativeCalibration_IntroExampleFood.png]*
- **Distance**. The distance operation allows you to calculate the difference in color between two images or to calculate the distance between a color image and a color constant. This can be useful to, for example, visualize the difference in color between images, or detect color defects.
  *[Image: Color_DistanceIntroExample.png]*
- **Matching**. The matching operation allows you to define color-samples composed of individual color elements, and use them for color identification or supervised color segmentation. This can be useful to, for example, identify the color of an object found with a pattern recognition module (such as Aurora Imaging Library Model Finder), or segment colors to determine the percentage of color in an image.
  *[Image: Color_MatchingIdentificationIntroExample.png]*
- **Projection**. The projection operation allows you to separate a selected color from a rejected one, convert color images to grayscale, or calculate an image's covariance matrix or its principal color components. This can be useful to, for example, discard an unwanted color from an image, or perform a mathematically optimal color-to-grayscale transformation.
  *[Image: Color_ProjectionSeparationIntroExample.png]*

> **Note:** The full version of Aurora Imaging Library is required to use the Aurora Imaging Library Color Analysis module.
