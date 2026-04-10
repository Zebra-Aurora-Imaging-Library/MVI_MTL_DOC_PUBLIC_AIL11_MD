---
doctype: Reference
module: im
function: MimMorphicShape
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimMorphicShape"
---

# MimMorphicShape

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

> Perform a morphological transformation using a predefined shape.

## Syntax

```c
void MimMorphicShape(
    AIL_ID     ContextId,      //in
    AIL_ID     SrcImageBufId,  //in
    AIL_ID     DstImageBufId,  //out
    AIL_DOUBLE Dimension,      //in
    AIL_INT64  ControlFlag     //in
)
```

## Description

This function performs morphological transformations on the specified source image, using a circular shape.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies a predefined context used to perform the morphological transformation.

*For specifying the predefined context*

| Value | Description |
| --- | --- |
| `M_CIRCULAR_CONTEXT_BINARY_CLOSE` | Specifies a binary close operation using a circle. This operation is equivalent to a circular dilation followed by a circular erosion. |
| `M_CIRCULAR_CONTEXT_BINARY_DILATE` | Specifies a binary dilate operation using a circle. |
| `M_CIRCULAR_CONTEXT_BINARY_ERODE` | Specifies a binary erode operation using a circle. |
| `M_CIRCULAR_CONTEXT_BINARY_OPEN` | Specifies a binary open operation using a circle. This operation is equivalent to a circular erosion followed by a circular dilation. |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer in which to put the result of the operation.

### `Dimension` *(in, AIL_DOUBLE)*

Specifies the diameter of the circle element, in pixels.

*For specifying the diameter*

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the diameter of the circle element used for the morphological transformation. The value must be an odd integer. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
