---
doctype: Reference
module: buf
function: MbufChildColor2dClip
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufChildColor2dClip"
---

# MbufChildColor2dClip

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

> Allocate a child data buffer within a parent buffer and, if necessary, perform a clipping operation.

## Syntax

```c
AIL_ID MbufChildColor2dClip(
    AIL_ID    ParentBufId,  //in
    AIL_INT   Band,         //in
    AIL_INT   OffX,         //in
    AIL_INT   OffY,         //in
    AIL_INT   SizeX,        //in
    AIL_INT   SizeY,        //in
    AIL_INT * StatusPtr,    //out
    AIL_ID *  BufIdPtr      //out
)
```

## Description

This function allocates a child data buffer within the specified, previously allocated, parent data buffer. It selects a rectangular area in one or all of the bands of the parent data buffer and allocates this area as a child of that buffer. If necessary, the specified rectangular area will be adjusted (clipped) to ensure it fits within the limits of the parent buffer. If the specified rectangular area is completely outside the limits of the parent buffer, the allocation fails and an error is reported.

The child buffer is not allocated in its own memory space; it remains part of the parent buffer. Therefore, any modification to the child buffer affects the parent and vice versa. Note, a parent buffer can have several child buffers.

A child buffer is considered a data buffer in its own right. It can be used in the same circumstances as its parent buffer. A child buffer inherits its type and attributes from the parent buffer.

If the child buffer is larger or positioned outside the parent buffer, the child buffer is clipped and the clipping status is returned using the [`StatusPtr`](../../Reference/buf/MbufChildColor2dClip.md) parameter. In the following example, both the X-offset and the X/Y-size are clipped, so [`M_CLIPPED_OFFSET_X`](../../Reference/buf/MbufChildColor2dClip.md) + [`M_CLIPPED_SIZE_X`](../../Reference/buf/MbufChildColor2dClip.md) + [`M_CLIPPED_SIZE_Y`](../../Reference/buf/MbufChildColor2dClip.md) is returned as the clipping status. If no clipping was necessary, [`M_NULL`](../../Reference/buf/MbufChildColor2dClip.md) is returned as the clipping status.

*[Image: REF-MbufChildColor2dClip-M_CLIPPED.png]*

If a child buffer is allocated using a parent buffer with an ROI, set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), the child buffer inherits the parent buffer's ROI. You can also set an ROI in a child buffer directly. Note that the ROI of the child buffer is only accessible using the child buffer's Aurora Imaging Library identifier.

After allocating the child buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the child buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufChildColor2dClip.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChildColor2dClip.md) was specified).

When the child buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufChildColor2dClip.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufChildColor2dClip.md) was specified, the smart identifier manages the child buffer's lifetime and you must not manually free it.

## Parameters

### `ParentBufId` *(in, AIL_ID)*

Specifies the identifier of the parent buffer.

### `Band` *(in, AIL_INT)*

Specifies the band of the parent data buffer from which to allocate the child data buffer. The specified band should be valid in the parent buffer.

*For specifying the band*

| Value | Description |
| --- | --- |
| `M_ALL_BANDS` | Specifies all bands (for multi-band parent buffers). |
| `M_BLUE` | Specifies the blue color band (for RGB buffers). |
| `M_GREEN` | Specifies the green color band (for RGB buffers). |
| `M_HUE` | Specifies the hue band (for HSL and HSV buffers). |
| `M_LUMINANCE` | Specifies the luminance band (for HSL buffers). |
| `M_RED` | Specifies the red color band (for RGB buffers). |
| `M_SATURATION` | Specifies the saturation band (for HSL and HSV buffers). |
| `M_U` | Specifies the U band (for YUV buffers). |
| `M_V` | Specifies the V band (for YUV and HSV buffers). |
| `M_Y` | Specifies the Y band (for YUV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band to copy.

The relationship between index value and band for RGB, HSL, HSV, and YUV buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers, the hue band for HSL and HSV buffers, or the Y band for YUV buffers. |
| 1 | Corresponds to the green band for RGB buffers, or the saturation band for HSL and HSV buffers, or the U band for YUV buffers. |
| 2 | Corresponds to the blue band for RGB buffers, or the luminance band for HSL buffers, or the V band for YUV and HSV buffers. | |

### `OffX` *(in, AIL_INT)*

Specifies the horizontal offset of the child buffer, relative to the parent buffer's top-left pixel. If necessary, the given offset will be adjusted (clipped) to make sure that the top-left pixel of the child buffer is within the limits of the parent buffer.

### `OffY` *(in, AIL_INT)*

Specifies the vertical offset of the child buffer, relative to the parent buffer's top-left pixel. If necessary, the given offset will be adjusted (clipped) to make sure that the top-left pixel of the child buffer is within the limits of the parent buffer.

### `SizeX` *(in, AIL_INT)*

Specifies the width of the child buffer. If necessary, the given dimension will be adjusted (clipped) to make sure that the bottom-right pixel of the child buffer is within the limits of the parent buffer.

### `SizeY` *(in, AIL_INT)*

Specifies the height of the child buffer. If necessary, the given dimension will be adjusted (clipped) to make sure that the bottom-right pixel of the child buffer is within the limits of the parent buffer.

### `StatusPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which the clipping status is written. If no status is required, set this parameter to [`M_NULL`](../../Reference/buf/MbufChildColor2dClip.md).

*For retrieving clipping results*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no clipping was necessary. |
| `M_CLIPPED_OFFSET_X` | Specifies that the horizontal offset of the child buffer was clipped to fit within the width of the parent buffer. |
| `M_CLIPPED_OFFSET_Y` | Specifies that the vertical offset of the child buffer was clipped to fit within the height of the parent buffer. |
| `M_CLIPPED_SIZE_X` | Specifies that the width of the child buffer was clipped to fit within the width of the parent buffer. |
| `M_CLIPPED_SIZE_Y` | Specifies that the height of the child buffer was clipped to fit within the height of the parent buffer. |

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the child buffer identifier or specifies the data type that the function should use to return the child buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated child buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated child buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the child buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated child buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufChildColor2dClip.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the child buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufChildColor2dClip.md) was specified).
