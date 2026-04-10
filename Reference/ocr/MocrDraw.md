---
doctype: Reference
module: ocr
function: MocrDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrDraw"
---

# MocrDraw

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

> Draw specific features of OCR fonts or OCR result occurrences in an image buffer or 2D graphics list.

## Syntax

```c
void MocrDraw(
    AIL_ID             ContextGraId,              //in
    AIL_ID             FontContextOrResultOcrId,  //in
    AIL_ID             DstImageBufOrListGraId,    //out
    AIL_INT64          Operation,                 //in
    AIL_INT            Index,                     //in
    AIL_CONST_TEXT_PTR CharList,                  //in
    AIL_INT64          ControlFlag                //in
)
```

## Description

This function draws specific features of the OCR fonts or OCR results in the destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `FontContextOrResultOcrId` *(in, AIL_ID)*

Specifies the OCR Font context or result buffer from which to extract the features to draw. The OCR Font context or result buffer must have been previously allocated using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md) or [`MocrAllocResult`](../../Reference/ocr/MocrAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For specifying the type of operation to perform*

| Value | Description |
| --- | --- |
| `M_DRAW_CHAR` | Draws a character representation of the font in the destination image buffer. Characters are drawn from left to right, and top to bottom. On each line, as many characters as possible will be drawn where the size of each character is the Cell size X. For more information see [`MocrInquire`](../../Reference/ocr/MocrInquire.md) with [`M_CHAR_CELL_SIZE_X`](../../Reference/ocr/MocrInquire.md). The buffer height must be at least the character Cell size Y multiplied by the number of lines necessary to hold all the characters. For more details see [`MocrInquire`](../../Reference/ocr/MocrInquire.md) with [`M_CHAR_CELL_SIZE_Y`](../../Reference/ocr/MocrInquire.md).

> **Note:** This operation cannot draw in a 2D graphics list. |

*For an OCR Font context*

| Value | Description |
| --- | --- |
| `M_DRAW_STRING_BOX` | Draws a box around the string that is read in the target. Note that the bounding box of the string is the bounding box of all the string's characters bounding box, including the draw margin. |
| `M_DRAW_STRING_CHAR_BOX` | Draws all the character boxes of the result(s) of the string(s) read. The character boxes of the strings are drawn at the location read in the target with the correct angle, scale and aspect ratio. The character box drawn is the bounding box of the character plus the draw margin. |
| `M_DRAW_STRING_CHAR_POSITION` | Draws a cross at the center of the bounding box of each string's character. Note that the angle, scale, and aspect ratio of the character are taken into account. |

### `Index` *(in, AIL_INT)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CharList` *(in, AIL_CONST_TEXT_PTR)*

Specifies a string containing a list of valid characters to draw when drawing an OCR font. To draw all the characters in the font or to draw results, this parameter should be set to `M_NULL`.

*For specifying the string*

| Value | Description |
| --- | --- |
| `"CharList"` | Specifies that, when drawing an OCR font, this parameter can be a null-terminated string specifying a list of valid characters to draw. Characters will be drawn in the specified order. This string must be null-terminated. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
