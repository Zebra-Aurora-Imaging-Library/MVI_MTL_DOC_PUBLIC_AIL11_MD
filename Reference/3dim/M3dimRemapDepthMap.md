---
doctype: Reference
module: 3dim
function: M3dimRemapDepthMap
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimRemapDepthMap"
---

# M3dimRemapDepthMap

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

> Remap a depth map to fit within a specified data range, or convert the depth map to a different bit depth.

## Syntax

```c
void M3dimRemapDepthMap(
    AIL_ID    RemapContext3dimId,  //in
    AIL_ID    SrcImageBufId,       //in
    AIL_ID    DstImageBufId,       //out
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function linearly remaps source pixels to destination pixels according to the specified mode, which determines the range of Z-values (Z-range) for the destination depth map. Use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to set the remap mode ([`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md)) and other remap options. Note that the remap operation maintains the same calibration in X and Y as the source buffer.

You can use [`M3dimRemapDepthMap`](../../Reference/3dim/M3dimRemapDepthMap.md) to remap a depth map to a different data type. This allows you to pass the remapped depth map to functions that only accept image buffers of certain bit depths (for example, [`MmodFind`](../../Reference/mod/MmodFind.md)).

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If both the source and the destination image buffers have an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `RemapContext3dimId` *(in, AIL_ID)*

Specifies a remap 3D image processing context.

*For specifying the remap 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_REMAP_CONTEXT_BUFFER_LIMITS` | Specifies a predefined remap 3D image processing context with all remap context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, including [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md), which is set to [`M_BUFFER_LIMITS`](../../Reference/3dim/M3dimControl.md). Use this predefined context to remap the source depth map using its [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) as the values to define the destination's Z-range. |
| `M_REMAP_CONTEXT_DATA_EXTREMES` | Specifies a predefined remap 3D image processing context with all remap context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except for [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md), which is set to [`M_DATA_EXTREMES`](../../Reference/3dim/M3dimControl.md). Use this predefined context to remap the source depth map using its minimum and maximum pixel values to define the destination's Z-range. |
| `M_REMAP_CONTEXT_USE_DESTINATION` | Specifies a predefined remap 3D image processing context with all remap context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)) set to their default, except for [`M_REMAP_MODE`](../../Reference/3dim/M3dimControl.md), which is set to [`M_USE_DESTINATION_CALIBRATION`](../../Reference/3dim/M3dimControl.md). Use this predefined context to remap the source depth map using the destination image buffer's calibration information or destination container's scale, offset, and matrix information to define the destination's Z-range. |
| `Remap 3D image processing context identifier` | Specifies the identifier of a remap 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_REMAP_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** In this case, the function applies the remap control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source depth map image buffer or depth map container.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination depth map image buffer or container.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
