---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Custom_morphological_operations
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Custom morphological operations"
---

# Custom morphological operations

Morphological operations are neighborhood operations that compute new values according to geometric relationships and matches of known patterns in the input image. The [`MimMorphic`](../../Reference/im/MimMorphic.md) function supports different types of morphological operations:

- Erosion.
- Dilation.
- Thinning.
- Thickening.
- Top hat operations.
- Thickening.
- Bottom hat operations.
- Hit or miss transformation.

You specify the required geometric relationships for each of these operations using a structuring element.

## Defining your own structuring element

To define your own structuring element:

1. Allocate a structuring element buffer ([`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md)), using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). The dimensions of the structuring element determine the size of the neighborhood that is used in the operation. The result of the operation is stored in the destination buffer at the location that corresponds to the structuring element's center pixel. When the structuring element has an even number of rows and/or columns, the center pixel is considered to be the top-left pixel of the central elements in the neighborhood (see [Custom spatial filters](S02_Custom_spatial_filters.md)).
2. Load the structuring element values into this buffer, using [`MbufPut`](../../Reference/buf/MbufPut.md) or [`MbufPut2d`](../../Reference/buf/MbufPut2d.md). Give the structuring element values according to the morphological operation that is to be performed. Any structuring element value can be used, including **M_DONT_CARE** (which means that the corresponding neighbor is not considered in the comparison). For all binary operations, as well as for the grayscale thin and thick operations, any structuring element value other than 0 and **M_DONT_CARE**are interpreted as a 1.
   For custom structuring elements, you can use [`MbufControl`](../../Reference/buf/MbufControl.md) to control how the operation handles the borders (overscan) of the source buffer (see [Custom spatial filters](S02_Custom_spatial_filters.md)) and the position of the neighborhood's center pixel.

## Erosion and dilation

Two fundamental morphological operations are erosion and dilation. These functions allow you to view the possible growth stages of an object in the foreground (non-zero pixels) of an image.

*[Image: eroddil.png]*

There are two versions of erosion and dilation:

- Erosion ([`M_ERODE`](../../Reference/im/MimMorphic.md)).
  - **Binary erosion**. If the structuring element does not match the corresponding neighborhood values exactly, the center pixel is set to zero; otherwise, it remains unchanged. In effect, binary erosion peels off layers of objects.
  - **Grayscale erosion**. Subtracts each structuring element value from the corresponding pixel value in the neighborhood, and then replaces the center pixel of the neighborhood with the minimum value from the resulting neighborhood values.
- Dilation ([`M_DILATE`](../../Reference/im/MimMorphic.md)).
  - **Binary dilation**. If any of the structuring element values match the corresponding neighborhood values, the center pixel is set to the maximum value of the buffer (for example, 0xff for an 8-bit buffer); otherwise, it remains unchanged. In effect, binary dilation adds layers to the objects.
  - **Grayscale dilation**. Adds each structuring element value to the corresponding pixel value in the neighborhood, and then replaces the center pixel of the neighborhood with the maximum value from the resulting neighborhood values.

Note, in binary mode, erosion of the white pixels is the same as dilation of the black pixels.

If the processing mode is set to [`M_BINARY`](../../Reference/im/MimMorphic.md), a binary erosion or dilation is performed and all non-zero pixels are considered as 1's; otherwise, the grayscale version of these operations is performed.

Use [`MblobReconstruct`](../../Reference/blob/MblobReconstruct.md) to perform a conditional dilation.

### Using standard erosion and dilation

Aurora Imaging Library also supports [`MimErode`](../../Reference/im/MimErode.md) and [`MimDilate`](../../Reference/im/MimDilate.md), functions specialized in performing the most standard form of erosion and dilation operation. These operations use the following structuring element when performing in binary mode:

*[Image: StrucElmntBin.png]*

And use the following structuring element in grayscale mode:

*[Image: StrucElmntGray.png]*

In other words, these functions execute a simple 3 by 3 minimum or maximum operation without adding or subtracting anything from the pixel.

For example, to perform the most standard dilation operation on a source image buffer, use [`MimDilate`](../../Reference/im/MimDilate.md) with the processing mode set to [`M_BINARY`](../../Reference/im/MimDilate.md), or use [`MimMorphic`](../../Reference/im/MimMorphic.md) with a 3x3 structuring element of ones and the processing mode set to [`M_BINARY`](../../Reference/im/MimMorphic.md). Note, in general the standard version is faster.

### An example

The _MImMorphic.cpp_ example shows how to define your own structuring element. It demonstrates, on an image with rounded objects, the difference between performing the standard opening operation, [`MimOpen`](../../Reference/im/MimOpen.md), and performing a custom opening with a circular type structuring element. Note, the latter preserves the original shape of the objects better than the square structuring element of the standard erosion. To run/view this and other examples, use Aurora Imaging Example Launcher.

## Thinning and thickening

You can reduce or enlarge objects in the foreground (non-zero pixels) of an image, using operations based on a rigid match of the pixel's neighborhood and the structuring element. Using a thickening operation, you can enlarge the object and perform such operations as a convex hull. Using a thinning operation, you can reduce objects and perform such operations as finding their skeleton.

*[Image: thin.png]*

You can perform a thinning or thickening operation with a specified structuring element, using [`MimMorphic`](../../Reference/im/MimMorphic.md). These operations are typically performed several times, using a different structuring element so that the required pattern is sought in different directions.

You can also perform standard thinning or thickening operations with [`MimThin`](../../Reference/im/MimThin.md) or [`MimThick`](../../Reference/im/MimThick.md), respectively.

There are two versions of thinning and thickening:

- Thinning ([`M_THIN`](../../Reference/im/MimMorphic.md)).
  - **Binary thinning.**
    This operation replaces the center pixel by the value zero if a pixel's neighborhood matches the structuring element exactly. However, if the neighborhood does not match, the pixel value remains unchanged.
  - **Grayscale thinning.**
    If (`MAX(0) &lt; _center pixel_ &lt;= MIN(1)) then (_center pixel_ = MAX(0)) else (_center pixel_ is unchanged`).
    Where MAX(0) is the maximum of all pixels in the neighborhood that correspond to zero in the structuring element, and MIN(1) is the minimum of all pixels in the neighborhood that correspond to one in the structuring element.
- Thickening ([`M_THICK`](../../Reference/im/MimMorphic.md)).
  - **Binary thickening.**
    This operation replaces the center pixel by the maximum possible value of the buffer (for example, 0xff for an 8-bit buffer) if the pixel's neighborhood matches the structuring element exactly. However, if the neighborhood does not match, the pixel value remains unchanged.
  - **Grayscale thickening.**
    If (`MAX(0) &lt;= _center pixel_ &lt; MIN(1)) then (_center pixel_ = MIN(1)) else (_center pixel_ is unchanged`).
    Where MAX(0) is the maximum of all pixels in the neighborhood that correspond to zero in the structuring element, or MIN(1) is the minimum of all pixels in the neighborhood that correspond to one in the structuring element.

> **Note:** Note that all structuring elements values (other than 0 and **M_DONT_CARE**) are interpreted as a 1.

If the processing mode is set to [`M_BINARY`](../../Reference/im/MimMorphic.md), a binary thinning or thickening is performed, otherwise the grayscale version of these operations is performed.

## Top and bottom hat

To isolate thin elements within a given image, you can perform a top hat or bottom hat operation, using [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) or [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md), respectively.

A top hat operation produces an image containing those elements that are smaller than the provided structuring element and brighter than their surroundings. A top hat operation is the same as an open operation ([`M_OPEN`](../../Reference/im/MimMorphic.md)) followed by a subtraction of the results from the source image.

A bottom hat operation produces an image containing those elements that are smaller than the provided structuring element and darker than their surroundings. A bottom hat operation is the same as a close operation ([`M_CLOSE`](../../Reference/im/MimMorphic.md)) followed by a subtraction of the source image from the results.

For best effect, the structuring element should be at least half the width of the largest element to be extracted from the image. Successive open (or close) operations will make thinner objects disappear. Once removed from the image, a subtraction of the results of the operation from the source image will result in an image containing only the thin elements.

*[Image: Denoising-hat.png]*

Use the top hat and bottom hat operations to isolate defects in images, highlight a portion of an object that is narrower or wider than expected, or to make letters or numbers stand out from their background before using the Aurora Imaging Library OCR, String Reader, or SureDotOCR modules. The size of the small bright objects (or dark objects respectively) that are isolated is determined by the size of the structuring element and the number of iterations performed. In the following image, seven iterations of the top hat operation were performed using a 3x3 structuring element. This results in the letter "G" becoming more prominent than its background.

*[Image: morphTopHatBotHat.png]*

## Matching

Matching allows you to determine the degree of similarity between certain areas of the image and a pattern (specified by a structuring element). The operation takes a binary or grayscale source image and produces a corresponding grayscale image, whereby the value of each destination pixel is equal to the total number of matches between the neighborhood of the source image's corresponding pixel and the structuring element values. To perform this matching operation, use [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_MATCH`](../../Reference/im/MimMorphic.md).

## Searching for hits or misses

You can determine which pixels have neighborhoods that match a pattern exactly by performing a 'hit or miss' operation. When the neighborhood of a source image's pixel matches the pattern exactly, the value of the corresponding pixel in the destination image is set to the maximum value of the buffer (for example, 0xff for an 8-bit buffer), except in the case of a floating-point buffer. In the latter case, if an exact match is found, the destination pixel is set to 1. When the neighborhood does not match exactly, the pixel value is set to zero. To search for hits or misses, use [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_HIT_OR_MISS`](../../Reference/im/MimMorphic.md).

## Predefined structuring elements

You can perform various morphological transformations with a structuring element of a predefined shape on binary images, using [`MimMorphicShape`](../../Reference/im/MimMorphicShape.md). For the following operations, the diameter of the circular structuring element that defines the neighborhood is determined by the [`Dimension`](../../Reference/im/MimMorphicShape.md) parameter:

- A circular binary erosion operation ([`M_CIRCULAR_CONTEXT_BINARY_ERODE`](../../Reference/im/MimMorphicShape.md)).
- A circular binary dilation operation ([`M_CIRCULAR_CONTEXT_BINARY_DILATE`](../../Reference/im/MimMorphicShape.md)).
- A circular binary close operation ([`M_CIRCULAR_CONTEXT_BINARY_CLOSE`](../../Reference/im/MimMorphicShape.md)). This operation is equivalent to a circular dilation, followed by a circular erosion.
- A circular binary open operation ([`M_CIRCULAR_CONTEXT_BINARY_OPEN`](../../Reference/im/MimMorphicShape.md)). This operation is equivalent to a circular erosion, followed by a circular dilation.
