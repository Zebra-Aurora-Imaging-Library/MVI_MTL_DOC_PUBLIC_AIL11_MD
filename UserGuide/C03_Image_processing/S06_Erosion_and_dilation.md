---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Erosion_and_dilation
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Erosion and dilation"
---

# Erosion and dilation

Especially during cell analysis, it can be important to know the growth stages of cell particles. Using the image processing erosion and dilation operations, you can view the possible growth stages of these particles.

- Erosion operations peel off layers from objects or particles, removing extraneous pixels and small particles from the image.
- Dilation operations add layers to objects or particles, enlarging any particle. Dilation can return eroded particles to their original size (but not necessarily to their exact original shape).

Erosion and dilation are neighborhood operations that determine each pixel's value according to its geometric relationship with neighborhood pixels, and as such, are part of a group of operations known as morphological operations. This section describes basic erosion and dilation performed using [`MimErode`](../../Reference/im/MimErode.md) and [`MimDilate`](../../Reference/im/MimDilate.md). Both these functions use predefined kernels to perform operations. For a description of advanced erosion and dilation, performed by [`MimMorphic`](../../Reference/im/MimMorphic.md), see [Custom morphological operations](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).

They are also the basic operations used to perform the opening and closing operations discussed in the following section.

Note, zero pixels are considered background, while non-zero pixels are considered foreground and part of objects (more generally referred to as blobs).

## Erosion

You can perform a basic erosion operation on 3 by 3 neighborhoods, using [`MimErode`](../../Reference/im/MimErode.md)with one of the following two options:

- If the erosion mode is set to [`M_BINARY`](../../Reference/im/MimErode.md), any pixel whose neighborhood is not completely white (any non-zero pixel is considered white) is changed to black (0 is considered black).
- If the erosion mode is set to [`M_GRAYSCALE`](../../Reference/im/MimErode.md), each pixel is replaced with the minimum value in its neighborhood.

You can use the iteration parameter of [`MimErode`](../../Reference/im/MimErode.md) to perform an erosion on larger neighborhoods. Iterating the erosion is the equivalent to performing an erosion on a (1 + (2*_i_)) by (1 + (2*_i_)) neighborhood where _i_ is the number of iterations. For example, two iterations of a 3x3 erosion is equivalent to a 5x5 erosion, and three iterations is equivalent to a 7x7 erosion.

If you need to iterate a binary erosion on each blob until it is about to disappear, set the erosion mode to [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) or [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) instead of [`M_BINARY`](../../Reference/im/MimErode.md), and set the iteration parameter to [`M_DEFAULT`](../../Reference/im/MimErode.md). [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) also keeps track of the number of iterations that occurred for each pixel that is not eroded.

The following animation illustrates an example of [`MimErode`](../../Reference/im/MimErode.md) with [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) (on the left) and [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) (on the right).

*[Image: IM_erosion_ultimates_example]*

You can perform a binary erosion operation using a neighborhood of a predefined shape, using [`MimMorphicShape`](../../Reference/im/MimMorphicShape.md). For more information, see [Predefined structuring elements](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).

## Dilation

You can perform a basic dilation operation on 3 by 3 neighborhoods, using [`MimDilate`](../../Reference/im/MimDilate.md)with one of the following two options:

- If the dilation mode is set to [`M_BINARY`](../../Reference/im/MimDilate.md), any pixel that has one or more white pixels (any non-zero pixel is considered white) in its neighborhood is set to white (0xff in an 8-bit image).
- If the dilation mode is set to [`M_GRAYSCALE`](../../Reference/im/MimDilate.md), each pixel is replaced with the maximum value in its neighborhood.

The [`MimDilate`](../../Reference/im/MimDilate.md) function is similar to the [`MimErode`](../../Reference/im/MimErode.md) function in that iterating it will effectively cause a dilation on larger neighborhoods. Iterating the dilation is the equivalent to performing a dilation on a (1 + (2*_i_)) by (1 + (2*_i_)) neighborhood where _i_ is the number of iterations. For example, two iterations of a 3x3 dilation is equivalent to a 5x5 dilation, and three iterations is equivalent to a 7x7 dilation.

The following animation demonstrates the use of [`MimDilate`](../../Reference/im/MimDilate.md) with a fixed number of iterations. The characters in the following animation are composed of dots. After four iterations, the characters are made up of solid lines and can be identified more easily.

*[Image: IM_dilation]*

If you need to iterate a binary dilation on each blob until its holes and background are about to disappear, set the dilation mode to [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) instead of [`M_BINARY`](../../Reference/im/MimErode.md), and set the iteration parameter to [`M_DEFAULT`](../../Reference/im/MimErode.md). If you need to keep track of the number of iterations that occurred for each remaining background or hole pixel during dilation, set the dilation mode to [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) instead. This will take the negative of the image (white pixels will become black and black pixels will become white) and then performs an erosion on the new blobs (on the new white pixels).

The following animation illustrates an example of [`MimDilate`](../../Reference/im/MimDilate.md) with [`M_BINARY_ULTIMATE`](../../Reference/im/MimErode.md) (on the left) and [`M_BINARY_ULTIMATE_ACCUMULATE`](../../Reference/im/MimErode.md) (on the right).

*[Image: IM_dilation_ultimates_example]*

You can perform a binary dilation operation using a neighborhood of a predefined shape, using [`MimMorphicShape`](../../Reference/im/MimMorphicShape.md). For more information, see [Predefined structuring elements](../C04_Advanced_image_processing/S03_Custom_morphological_operations.md).

## An example

You can use erosion or dilation to find the perimeter of objects. Erode or dilate a binary image and `XOR' the result with the original image, using [`MimArith`](../../Reference/im/MimArith.md).

*[Image: cellexo.png]*

The _SimpleDilateErode.cpp_ example shows how to obtain the exoskeletons of objects in an image. To run this and other examples, use Aurora Imaging Example Launcher.
