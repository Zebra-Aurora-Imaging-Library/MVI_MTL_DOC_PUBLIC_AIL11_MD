---
doctype: Reference
module: im
function: MimFlip
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimFlip"
---

# MimFlip

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

> Perform a horizontal or vertical image-flipping operation.

## Syntax

```c
void MimFlip(
    AIL_ID    SrcImageBufId,  //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT64 Operation,      //in
    AIL_INT64 OpFlag          //in
)
```

## Description

This function flips the source image to the destination image according to the specified operation.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform. This parameter can be set to one of the following:

*for specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_FLIP_HORIZONTAL` | Flips the image in a horizontal direction (left to right, along a vertical axis). |
| `M_FLIP_VERTICAL` | Flips the image in a vertical direction (top to bottom, along a horizontal axis). |

### `OpFlag` *(in, AIL_INT64)*

Reserved for future expansion. Must be set to `M_DEFAULT`.
