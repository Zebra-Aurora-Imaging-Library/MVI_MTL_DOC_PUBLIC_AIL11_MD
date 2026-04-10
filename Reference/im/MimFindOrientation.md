---
doctype: Reference
module: im
function: MimFindOrientation
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimFindOrientation"
---

# MimFindOrientation

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

> Find the dominant orientations of an image buffer.

## Syntax

```c
void MimFindOrientation(
    AIL_ID    OrientationContextImId,  //in
    AIL_ID    SrcImageBufId,           //in
    AIL_ID    OrientationResultImId,   //out
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function finds the dominant orientations of the source image using the consistent spatial patterns in the image. Aurora Imaging Library writes the list of dominant orientations (best viewing angles) and their associated score in the specified result buffer. The number of orientations found depends on the number of entries allocated in the find orientation result buffer using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md).

You can read the calculated orientations (angle values) and their associated scores using [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) with [`M_ANGLE`](../../Reference/im/MimGetResult1d.md) and [`M_SCORE`](../../Reference/im/MimGetResult1d.md), respectively.

This function requires the source image buffer to have dimensions that are a power of 2. If the image buffer's dimensions do not meet this criteria, you can use [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md) and [`MimControl`](../../Reference/im/MimControl.md) with [`M_INTERPOLATION_MODE`](../../Reference/im/MimControl.md) to specify how Aurora Imaging Library should clip or resize the image for calculations. This will not alter the original image.

## Parameters

### `OrientationContextImId` *(in, AIL_ID)*

Specifies the find orientation image processing context.

*For specifying the find orientation image processing context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default [`M_FIND_ORIENTATION_CONTEXT`](../../Reference/im/MimAlloc.md) context with all the controls in the context set to their default value. |
| `Find orientation image processing context ID` | Specifies a valid find orientation image processing context identifier, previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FIND_ORIENTATION_CONTEXT`](../../Reference/im/MimAlloc.md). |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image. The buffer used must be a 1-band image buffer.

### `OrientationResultImId` *(out, AIL_ID)*

Specifies the identifier of the find orientation result buffer. The buffer must have been allocated using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
