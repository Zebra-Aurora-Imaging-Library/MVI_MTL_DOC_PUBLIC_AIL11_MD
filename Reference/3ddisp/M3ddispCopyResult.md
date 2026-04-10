---
doctype: Reference
module: 3ddisp
function: M3ddispCopyResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3ddisp / M3ddispCopyResult"
---

# M3ddispCopyResult

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

> Copy a group of results from a picking area result buffer to an image buffer.

## Syntax

```c
void M3ddispCopyResult(
    AIL_ID    Result3ddispId,  //in
    AIL_INT64 SrcIndex,        //in
    AIL_ID    DstImageBufId,   //out
    AIL_INT64 DstIndex,        //in
    AIL_INT64 CopyType,        //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function copies results from a 3D display picking area result buffer into an image buffer. You can use the resulting image buffer as a mask image.

## Parameters

### `Result3ddispId` *(in, AIL_ID)*

Specifies the identifier of the 3D display picking area result buffer from which to copy. The result buffer must have been allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md), and must contain the results of a call to [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md).

### `SrcIndex` *(in, AIL_INT64)*

Specifies the index of the picking area result to copy. The specified index must correspond to a point cloud graphic.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to copy.

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_MASK_IMAGE` | Specifies to create a mask image whose pixels are non-zero for picked points within the picking region, and zero otherwise. Qualifying points are those of the point cloud graphic that were within the picking region when the graphic was picked. This operation maps the indices of the point cloud graphic's sub-elements ([`M3ddispGetResult`](../../Reference/3ddisp/M3ddispGetResult.md) with [`M_GRAPHIC_SUB_INDICES`](../../Reference/3ddisp/M3ddispGetResult.md)) to corresponding pixel locations in the destination image buffer, which are assigned the maximum buffer value. All other pixels in the buffer are set to 0. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
