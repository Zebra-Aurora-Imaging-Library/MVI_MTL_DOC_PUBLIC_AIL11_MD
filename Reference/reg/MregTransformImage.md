---
doctype: Reference
module: reg
function: MregTransformImage
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / reg / MregTransformImage"
---

# MregTransformImage

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Composes a mosaic from the specified source images and stores it in the destination image buffer.

## Syntax

```c
void MregTransformImage(
    AIL_ID         RegResultId,               //in
    const AIL_ID * ImageOrContainerArrayPtr,  //in
    AIL_ID         DestImageBufId,            //out
    AIL_INT        NumImages,                 //in
    AIL_INT64      InterpolationMode,         //in
    AIL_INT64      ControlFlag                //in
)
```

## Description

This function composes a mosaic from the specified source images and stores it in the destination image buffer. The source images are transformed according to the results, stored in the registration result buffer, that were calculated during the [`MregCalculate`](../../Reference/reg/MregCalculate.md) operation.

Each source image is associated with the registration result element with the same index. For example, the third image in the array (or the third image buffer component in the container) is associated with the third registration result element. Note that for the image to appear at the correct location in the mosaic, its index in the array or its component index must match the index of the registration result element containing the image's transformation.

When using an image buffer array, it is possible to use some images and omit others when composing the mosaic. In this case, replace the images that you want to omit by [`M_NULL`](../../Reference/reg/MregTransformImage.md) in the image buffer array. It can be useful to omit images from a mosaic if you want to add images to a preexisting mosaic, for example. In this case, replace all the images in the array by [`M_NULL`](../../Reference/reg/MregTransformImage.md), except the ones to add, and call [`MregTransformImage`](../../Reference/reg/MregTransformImage.md) with the preexisting mosaic passed in [`DestImageBufId`](../../Reference/reg/MregTransformImage.md).

When composing the mosaic, you can control how it will appear in the destination image buffer. You can specify which image to place first, using [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md). This image will be placed upright and will not be scaled. The other images in the mosaic will be positioned relative to this image's pixel coordinate system. In addition, you can also adjust the vertical and horizontal offsets between the origin of the mosaic's coordinate system and the origin of the destination image buffer, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_OFFSET_X`](../../Reference/reg/MregControl.md) and [`M_MOSAIC_OFFSET_Y`](../../Reference/reg/MregControl.md).

## Parameters

### `RegResultId` *(in, AIL_ID)*

Specifies the identifier of the registration result buffer with which to perform the mosaicing operation. The registration result buffer must have been previously allocated or restored on the required system using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) or [`MregRestore`](../../Reference/reg/MregRestore.md), respectively.

### `ImageOrContainerArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array containing the buffer identifiers of the input images or specifies the address of the variable containing the identifier of the image buffer container. When using an array of individual image buffers, to omit images from the mosaic, replace their identifiers with `M_NULL` in the array. When using a container, you cannot specify a null image.

### `DestImageBufId` *(out, AIL_ID)*

Specifies the destination image buffer that will contain the mosaic after the [`MregTransformImage`](../../Reference/reg/MregTransformImage.md) operation is performed.

### `NumImages` *(in, AIL_INT)*

Specifies the size of the specified image buffer array, or specifies the number of image buffer components in the specified image buffer container.

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode. This parameter must be set to one of the values below.

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimWarp.md) + [`M_OVERSCAN_ENABLE`](../../Reference/im/MimWarp.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

*For overscan*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination pixel to 0. |
| `M_OVERSCAN_DISABLE` | Leaves the destination pixel as is. |
| `M_OVERSCAN_ENABLE` *(default)* | Uses pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer or if the point falls outside all of the ancestor buffers, it leaves the destination pixel as is. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
