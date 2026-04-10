---
doctype: Reference
module: im
function: MimUnwarpAlongPath
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimUnwarpAlongPath"
---

# MimUnwarpAlongPath

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

> Unwarp an image along a path with a specified width.

## Syntax

```c
void MimUnwarpAlongPath(
    AIL_ID    UnwarpAlongPathContextImId,  //in
    AIL_ID    SrcImageBufId,               //in
    AIL_ID    DstImageBufId,               //out
    AIL_INT64 ControlFlag                  //in
)
```

## Description

This function unwarps an image along a path with a specified width.

Before calling this function, you must add a path to the context using [`MimPut`](../../Reference/im/MimPut.md) with [`M_XY_PATH`](../../Reference/im/MimPut.md)and two arrays (one for the X-coordinates and one for the Y-coordinates). In addition, you must specify the width of the path using with using [`MimControl`](../../Reference/im/MimControl.md) with [`M_SINGLE_WIDTH`](../../Reference/im/MimControl.md).

This function generates quadrilateral regions from the intersections of the top and bottom lines, defined one half-width above and below the center of the path. When using the [`M_ADAPTIVE_ALONG_PATH`](../../Reference/im/MimControl.md) sampling mode, the image is unwarped along the entire quadrilateral. If the [`M_ADAPTIVE_AT_JUNCTION`](../../Reference/im/MimControl.md) sampling mode is used, this function will generate rectangles from the shortest side (top or bottom) of the quadrilateral, then generate triangles to fill in the rest of the original quadrilateral. In this case, the image is only distorted in the triangular region at the junction of each path segment.

## Parameters

### `UnwarpAlongPathContextImId` *(in, AIL_ID)*

Specifies the identifier of the unwarp along path image processing context. This context must have been previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with[`M_UNWARP_ALONG_PATH_CONTEXT`](../../Reference/im/MimAlloc.md).

*For specifying the unwarp along path image processing context identifier*

| Value | Description |
| --- | --- |
| `Unwarp along path image processing context ID` | Specifies the identifier of the unwarp along path image processing context. |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. The buffer must have been previously allocated with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). If this parameter is not used, it must be set to `M_NULL`.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer in which to put the result of the operation. The buffer must have been previously allocated with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). If this parameter is not used, it must be set to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies which unwarp operation to perform.

*For specifying which unwarp operation to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Performs the unwarp along path operation. |
| `M_PREPROCESS` | Preprocesses the unwarp along path image processing context specified by [`UnwarpAlongPathContextImId`](../../Reference/im/MimUnwarpAlongPath.md). Note that, if not called explicitly, this operation will be performed automatically upon the first call to [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md).

If [`DstImageBufId`](../../Reference/im/MimUnwarpAlongPath.md) is not specified, the optimal size of the destination buffer is determined according to the path length (X-size) and the path width (Y-size), rounded up to the nearest integer. You can use [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_DESTINATION_SIZE_X`](../../Reference/im/MimInquire.md) and [`M_DESTINATION_SIZE_Y`](../../Reference/im/MimInquire.md) to inquire the optimal destination image buffer size to allocate. |
