---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Color_separation
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Color separation"
---

# Color separation

You can use [`McolProject`](../../Reference/col/McolProject.md) with [`M_COLOR_SEPARATION`](../../Reference/col/McolProject.md) to remove a color from an image. Eliminating a color can be useful since it allows you to isolate colors which should be considered part of the background. To perform color separation, you must identify the background, selected, and rejected colors; these are referred to as the separation colors.

*[Image: Color_ProjectionSeparationExample.png]*

By properly specifying the colors, the projection operation is able to identify the unwanted color information (the stamp), and create a new version of the image that only contains the wanted color information (the background and the signature). The following two-dimensional graphs illustrate the color distribution of the signature/stamp image, and how that distribution is modified to remove the stamp and keep the signature.

*[Image: Color_SeparationGraph.png]*

Various conditions, such as different cameras and illuminants, can cause color from identical images to appear dissimilar. If this occurs, you can call [`McolTransform`](../../Reference/col/McolTransform.md) to perform a relative color calibration before separating colors. Relative color calibration ensures all colors are consistent according to a specified reference color. For more information, see [Relative color calibration](S04_Relative_color_calibration.md).

## Separation operation

The color separation operation can return either an image with the color separation result, or the transformation matrix with which to perform the color separation. This depends on whether you store the result in an image buffer or an Aurora Imaging Library array buffer. If you retrieve the matrix, you can then use it with [`MimConvert`](../../Reference/im/MimConvert.md) to separate the colors. When doing this, make sure that the color distribution of all the images to convert is similar, otherwise unpredictable results can occur.

You can pass the separation colors to [`McolProject`](../../Reference/col/McolProject.md) as either three explicit color values, or as colors taken from the source image. To specify explicit color values, you must use a 3x3 Aurora Imaging Library array buffer. To specify colors from the source image, you must use a data identification image that identifies the corresponding source image pixels to use as the background (**M_BACKGROUND_LABEL**), selected (**M_SELECTED_LABEL**), and rejected (**M_REJECTED_LABEL**) colors.

*[Image: Color_ProjectionSeparationMaskExample.png]*

Aurora Imaging Library ignores pixels in the source image that you do not identify as background, selected, or rejected. You must identify at least one pixel for each. If you identify multiple pixels as background, selected, or rejected, Aurora Imaging Library averages the colors of those pixels. If the data identification image is smaller than the source image, Aurora Imaging Library ignores the outlying source image pixels.
