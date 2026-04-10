---
doctype: Reference
module: ocr
function: MocrModifyFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrModifyFont"
---

# MocrModifyFont

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

> Invert or resize the font of an OCR font context to match the target image characters.

## Syntax

```c
void MocrModifyFont(
    AIL_ID    FontContextOcrId,  //out
    AIL_INT64 Operation,         //in
    AIL_INT64 OperationFlag      //in
)
```

## Description

This function physically modifies the polarity and sizing of the font of an OCR font context.

## Parameters

### `FontContextOcrId` *(out, AIL_ID)*

Specifies the identifier of the OCR font context to modify.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform. This parameter can be set to one or more of the following values:

*For specifying the type of operation*

| Value | Description |
| --- | --- |
| `M_INVERT` | Physically inverts the character representation of the font (for example, if the foreground is white, this will change it to black). |
| `M_RESIZE` | Scales the font.

This operation physically changes the size of the characters of the font to match the target character size ([`M_TARGET_CHAR_SIZE_X`](../../Reference/ocr/MocrControl.md) and [`M_TARGET_CHAR_SIZE_Y`](../../Reference/ocr/MocrControl.md)). Once performed, all of the font's specifications, which were set at allocation ([`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md)), are modified to this new size.

Note that changing the size of the font permanently in the OCR font context can save time when compared to resizing the font before each read/verify operation. Increasing the size of the font might be slower because the resize time might be less than the additional processing time required during a read or verify operation to find characters of a larger font. |

### `OperationFlag` *(in, AIL_INT64)*

Specifies the interpolation mode for the function. This parameter can be set to one of the following:

*For specifying the interpolation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_BILINEAR`](../../Reference/ocr/MocrModifyFont.md).

Note that if the[`Operation`](../../Reference/ocr/MocrModifyFont.md) parameter is set to [`M_INVERT`](../../Reference/ocr/MocrModifyFont.md), this parameter should be set to [`M_DEFAULT`](../../Reference/ocr/MocrModifyFont.md). No interpolation is done with [`M_INVERT`](../../Reference/ocr/MocrModifyFont.md). |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |
