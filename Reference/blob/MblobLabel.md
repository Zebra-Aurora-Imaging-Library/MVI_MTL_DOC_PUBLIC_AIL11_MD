---
doctype: Reference
module: blob
function: MblobLabel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / blob / MblobLabel"
---

# MblobLabel

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

> Draw a labeled image.

## Syntax

```c
void MblobLabel(
    AIL_ID    ResultBlobId,   //in
    AIL_ID    DstImageBufId,  //out
    AIL_INT64 ControlFlag     //in
)
```

## Description

This function draws a labeled image in which each blob existing in the specified result buffer is represented with its own unique label value.

The label values are taken from the blob analysis result buffer. A call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) must have been made to generate label values for an image. Blobs that have been deleted from the result buffer are not drawn.

## Parameters

### `ResultBlobId` *(in, AIL_ID)*

Specifies the identifier of the blob analysis result buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination (labeled) image buffer. This must be a single band, 8-, 16-, or 32-bit unsigned buffer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether or not to clear the destination image buffer before drawing the labeled image into it. This parameter can be set to one of the following:

*For the destination image buffer*

| Value | Description |
| --- | --- |
| `M_CLEAR` | Clears the destination image buffer before placing the labeled image into it. |
| `M_NO_CLEAR` | Does not clear the destination image buffer before placing the labeled image into it (background pixels will be unchanged). |
