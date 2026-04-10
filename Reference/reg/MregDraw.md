---
doctype: Reference
module: reg
function: MregDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregDraw"
---

# MregDraw

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

> Draw specific features of the registration results in an image buffer or 2D graphics list.

## Syntax

```c
void MregDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultRegId,             //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT   Index,                   //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of the registration results in the destination image buffer or 2D graphics list.

To draw these results, the required information must be available in the registration result buffer.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values. Note that while this parameter is required for calling [`MregDraw`](../../Reference/reg/MregDraw.md), when using an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, this parameter will not affect the function.

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultRegId` *(in, AIL_ID)*

Specifies the identifier of the registration result buffer from which to extract the results to draw. The registration result buffer must have been previously allocated on the required system using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. This parameter can be set to one of the following:

*For specifying the operation*

| Value | Description |
| --- | --- |
| `M_DRAW_ALBEDO_IMAGE` | Draws the albedo image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_BOX` | Draws the bounding box of the image associated with the specified registration element, at its position, angle, and scale. The position, angle, and scale are determined by the registration.

Note that this operation is only available for an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_DEPTH_CONFIDENCE_MAP` | Draws the confidence map into the target buffer.

Note that this operation is only available for an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable the computation of the confidence map when performing a depth-from-focus registration operation, you must use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_CONFIDENCE_MAP`](../../Reference/reg/MregControl.md) set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_DEPTH_INDEX_MAP` | Draws the index map into the target buffer.

Note that this operation is only available for an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_DEPTH_INTENSITY_MAP` | Draws the intensity map into the target buffer.

Note that this operation is only available for an [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable the computation of the intensity map when performing a depth-from-focus registration operation, you must use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_INTENSITY_MAP`](../../Reference/reg/MregControl.md) set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_EDOF_IMAGE` | Draws the extended depth of field (EDoF) image into the target buffer. In this case, the [`DstImageBufOrListGraId`](../../Reference/reg/MregDraw.md) parameter must be set to an image buffer.

Note that this operation is only available for an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_GAUSSIAN_CURVATURE_IMAGE` | Draws the Gaussian curvature map image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable computation of the Gaussian curvature image when performing a photometric stereo registration operation, [`MregControl`](../../Reference/reg/MregControl.md) with [`M_GAUSSIAN_CURVATURE`](../../Reference/reg/MregControl.md) must be set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_HDR_FULL_IMAGE` | Draws the internal HDR image, without tone mapping, into the target buffer. In this case, the [`DstImageBufOrListGraId`](../../Reference/reg/MregDraw.md) parameter must be set to an image buffer. The internal HDR image is remapped to use the full range of the target buffer, which must have a bit depth that is twice that of the input images. For example, if the input images are 8-bit, then the raw HDR image must be written to a 16-bit buffer.

Note that this operation is only available for an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_HDR_IMAGE` | Draws the tone mapped HDR image into the target buffer, which should have the same bit depth and data type as the input image buffers. The image will be remapped if the provided target buffer is different. For this operation, the [`DstImageBufOrListGraId`](../../Reference/reg/MregDraw.md) parameter must be set to an image buffer.

Note that, if tone mapping was not enabled ([`M_TONE_MAPPING_MODE`](../../Reference/reg/MregControl.md) set to [`M_DISABLE`](../../Reference/reg/MregControl.md)), the image drawn with [`M_DRAW_HDR_IMAGE`](../../Reference/reg/MregDraw.md) is the raw HDR image where the image's lowest pixel value has been subtracted from all pixel values. This ensures that the image has a true black value (minimum pixel value of 0).

Note that this operation is only available for an [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. |
| `M_DRAW_LOCAL_CONTRAST_IMAGE` | Draws the local contrast image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable computation of the local contrast image when performing a photometric stereo registration operation, [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LOCAL_CONTRAST`](../../Reference/reg/MregControl.md) must be set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_LOCAL_SHAPE_IMAGE` | Draws the local shape image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable computation of the local shape image when performing a photometric stereo registration operation, [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LOCAL_SHAPE`](../../Reference/reg/MregControl.md) must be set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_MEAN_CURVATURE_IMAGE` | Draws the mean curvature map image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable computation of the mean curvature image when performing a photometric stereo registration operation, [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MEAN_CURVATURE`](../../Reference/reg/MregControl.md) must be set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |
| `M_DRAW_TEXTURE_IMAGE` | Draws the texture image into the target buffer.

Note that this operation is only available for an [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. To enable computation of the texture image when performing a photometric stereo registration operation, [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TEXTURE_IMAGE`](../../Reference/reg/MregControl.md) must be set to [`M_ENABLE`](../../Reference/reg/MregControl.md). |

### `Index` *(in, AIL_INT)*

Specifies the registration element that is associated with the image whose features you want to draw when drawing results of an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) registration result buffer. This parameter must be set to one of the values below.

*For specifying the registration element*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For an [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md) result buffer, same as [`M_ALL`](../../Reference/reg/MregDraw.md).

For an [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_DEPTH_FROM_FOCUS_RESULT`](../../Reference/reg/MregAllocResult.md), [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md), or [`M_PHOTOMETRIC_STEREO_RESULT`](../../Reference/reg/MregAllocResult.md)result buffer, this is the only possible value. |
| `M_ALL` | Specifies all registration elements. |
| `0 <= Value < M_NUMBER_OF_REGISTRATION_ELEMENTS` | Specifies the registration element's index. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
