---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Unwarping_along_a_path
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Unwarping along a path"
---

# Unwarping an image along a path

In some cases, distortions might not be uniform across a whole image. This can make it difficult to achieve desirable results using other geometric transforms ([`MimFlip`](../../Reference/im/MimFlip.md), [`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), [`MimTranslate`](../../Reference/im/MimTranslate.md), and [`MimPolarTransform`](../../Reference/im/MimPolarTransform.md)) to correct images for processing. In these cases, Aurora Imaging Library includes the [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) function to unwarp an image along a user defined path.

For example, in the following image, the text has been distorted in a way that is not trivial to correct. This can make it difficult for text reading modules to read this text.

*[Image: MimUnwarp_Wavy.png]*

A path can be defined along the midpoint of the distorted text, shown in blue. Quadrilateral regions are defined at a set width, shown in yellow. The result of the [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) operation is overlayed in the image, shown in green.

*[Image: MimUnwarpAlongPath.png]*

## Steps to unwarp an image along a path

The following steps provide a basic methodology for unwarping along a path.

1. Allocate an image processing context for unwarping along a path, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md).
2. Set the path along which to unwarp, using [`MimPut`](../../Reference/im/MimPut.md).
3. Set the width of the path, in pixels, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md).
4. Optionally, set the interpolation mode, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_INTERPOLATION_MODE`](../../Reference/im/MimControl.md).
5. Optionally, set the overscan behavior, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_OVERSCAN`](../../Reference/im/MimControl.md).
6. Optionally, set the sampling mode, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_UNWARP_SAMPLING_MODE`](../../Reference/im/MimControl.md).
7. Optionally, preprocess the unwarp along path context, using [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) with [`M_PREPROCESS`](../../Reference/im/MimUnwarpAlongPath.md). This is recommended when using the context with multiple images. If not called explicitly, the context will be preprocessed automatically upon the first call to [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md).
8. Use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_DESTINATION_SIZE_X`](../../Reference/im/MimInquire.md) and [`M_DESTINATION_SIZE_Y`](../../Reference/im/MimInquire.md) to retrieve the optimal destination buffer size. This is the size of the destination buffer if one was specified while preprocessing. If no destination buffer was specified during preprocessing, then the optimal width and height of the buffer are determined according to the path length (X-size) and width (Y-size).
9. Optionally, draw the path, quadrilaterals, and vertical grid lines, using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_WORK_PATH`](../../Reference/im/MimDraw.md), [`M_DRAW_WORK_QUADRILATERALS`](../../Reference/im/MimDraw.md), and [`M_DRAW_WORK_GRID_VERTICAL_LINES`](../../Reference/im/MimDraw.md), respectively.
10. Perform the unwarp along path operation, using [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md). This function performs the unwarp along path operation on a source image and places the result in the destination image buffer.
11. If necessary, save your unwarp along path context, using [`MimSave`](../../Reference/im/MimSave.md) or [`MimStream`](../../Reference/im/MimStream.md).
12. Free all of your allocated objects, using [`MimFree`](../../Reference/im/MimFree.md), unless [`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified during allocation.

## Setting the path

To unwarp an image along a path, you must add a path to the unwarp along path image processing context, using [`MimPut`](../../Reference/im/MimPut.md). The center path is defined by a polyline. The vertices of the polyline are defined by two arrays (one for the X-coordinates and one for the Y-coordinates). Each point along the path is specified by pairing the value at a given index in the X-coordinate array with the value at the same index in the Y-coordinate array. The direction of the path is determined by the order of the entries in the arrays: the first entry in each array specifies the start of the path, and the last entry specifies the end. The coordinate values themselves do not have to be in increasing or decreasing order.

You must also set the width of the path, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md). This specifies the perpendicular distance between the top and bottom of the path, in pixels. The top and bottom of the path are defined one-half-width above and below the working path relative to the direction of the path.

Generally, the more path segments, the better the unwarping result. This, however, comes at the cost of computation time. To effectively unwarp the path, ensure a sufficient number of line segments to smoothly track the path. The following image shows how a different number of segments affects the results of the unwarping along path operation:

*[Image: Mim_Unwarp_paths.png]*

## Interpolation mode

When you perform an unwarping, each pixel position in the destination buffer `(X<sub>d</sub>, Y<sub>d</sub>)`, gets associated with a specific point in the source buffer `(X<sub>s</sub>, Y<sub>s</sub>)`, and a computed intensity value for `(X<sub>s</sub>, Y<sub>s</sub>)` is then copied to `(X<sub>d</sub>, Y<sub>d</sub>)`. The destination coordinates have integer values but the source coordinates, in general, do not. Therefore, the pixel value at `(X<sub>d</sub>, Y<sub>d</sub>)` is determined from several source pixels that are near `(X<sub>s</sub>, Y<sub>s</sub>)`, according to a specified interpolation mode.

The various interpolation modes available when using [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) are:

- A standard nearest neighbor interpolation ([`M_NEAREST_NEIGHBOR`](../../Reference/im/MimControl.md)).
- A standard bilinear interpolation ([`M_BILINEAR`](../../Reference/im/MimControl.md)).
- A standard bicubic interpolation ([`M_BICUBIC`](../../Reference/im/MimControl.md)).

In general, nearest-neighbor interpolation is the fastest to perform, and bicubic interpolation is the slowest. However, nearest-neighbor interpolation produces the least accurate results, and bicubic interpolation produces the most accurate. Bilinear interpolation is often the best compromise between speed and accuracy.

For more information on interpolation modes, see [Interpolation modes](../C03_Image_processing/S15_Basic_geometrical_transform.md).

## Points outside the source buffer

Sometimes, the point associated with a destination pixel will fall outside the source buffer. In such cases, the new value for the destination pixel can be determined in one of the following ways:

- You can use pixels from the source buffer's ancestor buffer ([`M_OVERSCAN_ENABLE`](../../Reference/im/MimControl.md)). If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, the destination pixel will be left as is.
- You can just leave the destination pixel as is ([`M_OVERSCAN_DISABLE`](../../Reference/im/MimControl.md)).
- You can set the destination pixel to 0 ([`M_OVERSCAN_CLEAR`](../../Reference/im/MimControl.md)).

In general, you should use pixels from the source buffer's ancestor buffer when the source buffer is a child buffer. This will ensure that the pixels you use are related to the source buffer. If the source buffer is not a child buffer, use one of the other options.

Note that you can set the destination pixels that correspond to values outside the source buffer to a fixed value other than 0 by first clearing the destination buffer to that value, using [`MbufClear`](../../Reference/buf/MbufClear.md).

## Sampling mode

The sampling mode defines the region where deformations occur during the unwarp along path operation. The regions can be deformed in one of the following ways:

- You can deform along the entire path ([`M_ADAPTIVE_ALONG_PATH`](../../Reference/im/MimControl.md)). With this sampling mode, the entire quadrilateral region is deformed. For example, in the following path, the shaded region is deformed:
  *[Image: Mim_Unwarp_adaptive_along_path.png]*
- You can deform only near the junction of the path segments ([`M_ADAPTIVE_AT_JUNCTION`](../../Reference/im/MimControl.md)). With this sampling mode, a rectangle is defined along the shortest edge (top or bottom) of the quadrilateral region. No deformation is applied in this rectangular region. Only the remaining triangles near the junction are deformed. For example, in the following path, the shaded region is deformed:
  *[Image: Mim_Unwarp_adaptive_at_junction.png]*

## Unwarping along path example

The example _MimUnwarpAlongPath.cpp_ demonstrates the use of the [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md) function to unwarp sections of an image along a designated path with a specified width.

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.
