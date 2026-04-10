---
doctype: Reference
module: dlocr
function: MdlocrDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrDraw"
---

# MdlocrDraw

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

> Draw specific features of a Deep Learning OCR result.

## Syntax

```c
void MdlocrDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultDlocrId,           //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT64 StringIndex,             //in
    AIL_INT64 CharIndex,               //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of a Deep Learning OCR read result in the destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. Set this parameter to one of the values below.

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR read result buffer from which to extract the features to draw. The result buffer must have been previously allocated on the system using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the drawing operation to perform. When applicable, [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md)draws the required information at the location that the string was read in the target, at the correct angle, scale, and aspect ratio.

*For a drawing operation related to strings*

| Value | Description |
| --- | --- |
| `M_DRAW_STRING` | Draws the string that was read. |
| `M_DRAW_STRING_BOX` | Draws a bounding box around the string that was read. This bounding box encloses all of the string's characters. |
| `M_DRAW_STRING_INDEX` | Draws the index associated with the string read. This drawing operation is useful when defining a model using [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md). |

*For a drawing operation related to characters*

| Value | Description |
| --- | --- |
| `M_DRAW_CHAR` | Draws the character that was read. |
| `M_DRAW_STRING_CHAR_BOX` | Draws the character's bounding box at the location at which the character was read. |
| `M_DRAW_STRING_CHAR_POSITION` | Draws a circled dot at the center of each bounding box at the location at which the character was read. |

### `StringIndex` *(in, AIL_INT64)*

Specifies the string result (one or all) for which to perform the drawing operation. Set this parameter to one of the values below.

*For specifying the string index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to perform the drawing operation for all string results. |
| `M_GENERAL` | Specifies to perform a global drawing operation for a single bounding box that encompasses all strings. The [`Operation`](../../Reference/dlocr/MdlocrDraw.md) parameter must be set to [`M_DRAW_STRING_BOX`](../../Reference/dlocr/MdlocrDraw.md); otherwise, an error is generated. |
| `Value >= 0` | Specifies the index of the string result for which to perform the drawing operation. |

### `CharIndex` *(in, AIL_INT64)*

Specifies the character (one or all) in the string result for which to perform the drawing operation. Set this parameter to one of the values below.

*For specifying the character index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior.

For drawing operations related to strings, the default indicates that this parameter is not required. For drawing operations related to characters, the default is the same as setting the **M_INDEX_IN_STRING()** macro to **M_INDEX_IN_STRING()**. |
| `M_INDEX_IN_STRING` | Specifies the character(s) for which to perform the drawing operation. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
