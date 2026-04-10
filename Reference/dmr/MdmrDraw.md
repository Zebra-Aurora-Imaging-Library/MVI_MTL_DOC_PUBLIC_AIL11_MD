---
doctype: Reference
module: dmr
function: MdmrDraw
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrDraw"
---

# MdmrDraw

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

> Draw specific features of a SureDotOCR result buffer.

## Syntax

```c
void MdmrDraw(
    AIL_ID    ContextGraId,            //in
    AIL_ID    ResultDmrId,             //in
    AIL_ID    DstImageBufOrListGraId,  //out
    AIL_INT64 Operation,               //in
    AIL_INT64 Index,                   //in
    AIL_INT64 CharIndex,               //in
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function draws specific features of a SureDotOCR result buffer in the destination image buffer or 2D graphics list.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. Set this parameter to one of the values below.

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ResultDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR result buffer from which to extract the features to draw. The result buffer must have been previously allocated on the system using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md).

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the drawing operation to perform. When applicable, [`MdmrDraw`](../../Reference/dmr/MdmrDraw.md)draws the required information at the location that the string was read in the target, at the correct angle, scale, and aspect ratio.

*For a drawing operation related to strings*

| Value | Description |
| --- | --- |
| `M_DRAW_AIL_FONT_FORMATTED_STRING` | Draws the names of the characters that were read, and they appear according to how the target string is formatted in the image (including non-constrained spaces). Character names are drawn under the bottom-left corner of the bounding box of the string ([`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md)). Character names come from the SureDotOCR font that defines them. To draw the names, SureDotOCR uses the font associated with the 2D graphics context. |
| `M_DRAW_AIL_FONT_STRING` | Draws the names of the characters that were read, and they appear according to the string model definition. Character names are drawn under the bottom-left corner of the bounding box of the string ([`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md)). Character names come from the SureDotOCR font that defines them. To draw the names, SureDotOCR uses the font associated with the 2D graphics context. |
| `M_DRAW_STRING_BOX` | Draws a bounding box around the string that was read. This bounding box encloses all of the string's characters. |

*For a drawing operation related to characters*

| Value | Description |
| --- | --- |
| `M_DRAW_STRING_CHAR_BOX` | Draws the character's bounding box at the location at which the character was read. |
| `M_DRAW_STRING_CHAR_POSITION` | Draws a circled dot at the center of each bounding box that surrounds the individual characters of a string. |

### `Index` *(in, AIL_INT64)*

Specifies the string result (one or all) for which to perform the drawing operation. Set this parameter to one of the values below.

*For specifying the string index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to perform the drawing operation for all string results. |
| `M_GENERAL` | Specifies to perform a global drawing operation for a single bounding box that encompasses all strings. This parameter must be used with [`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md) or it will generate an error. |
| `Value >= 0` | Specifies the index of the string result for which to perform the drawing operation. |

### `CharIndex` *(in, AIL_INT64)*

Specifies the character (one or all) in the string result for which to perform the drawing operation. Set this parameter to one of the values below.

*For specifying the character index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior.

For drawing operations related to strings, the default indicates that this parameter is not required. For drawing operations related to characters, the default is the same as setting the **M_INDEX_IN_STRING()** macro to **M_INDEX_IN_STRING()**. |
| `M_INDEX_IN_FORMATTED_STRING` | Specifies to perform the drawing operation by indicating the character's index, relative to its position in the formatted string. The formatted string includes spaces, if any were read in the target string, even if you did not add spaces to the string model as permitted character constraints ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). |
| `M_INDEX_IN_STRING` | Specifies to perform the drawing operation by indicating the character's index, relative to its position in the string. The string can include spaces, but only if they correspond to a space permitted character constraint in the string model ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_SPACE`](../../Reference/dmr/MdmrControlStringModel.md)). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
