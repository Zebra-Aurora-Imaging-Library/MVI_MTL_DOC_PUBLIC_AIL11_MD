---
doctype: Reference
module: str
function: MstrDraw
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrDraw"
---

# MstrDraw

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

> Draw specific features of the String Reader context or String Reader results.

## Syntax

```c
void MstrDraw(
    AIL_ID       ContextGraId,            //in
    AIL_ID       ContextOrResultStrId,    //in
    AIL_ID       DstImageBufOrListGraId,  //out
    AIL_INT64    Operation,               //in
    AIL_INT      Index,                   //in
    const void * CharListPtr,             //in
    AIL_INT64    ControlFlag              //in
)
```

## Description

This function draws specific features of the String Reader context or String Reader results in the destination image buffer or specified 2D graphics list.

After each drawing operation, the optimal image size that was needed for that operation is recorded in the String Reader context or result buffer (depending on what is passed) and can be inquired using MstrInquire with [`M_DRAW_LAST_SIZE_X`](../../Reference/str/MstrInquire.md) and [`M_DRAW_LAST_SIZE_Y`](../../Reference/str/MstrInquire.md). When [`M_NULL`](../../Reference/str/MstrDraw.md) is passed as the destination image ([`DstImageBufOrListGraId`](../../Reference/str/MstrDraw.md) parameter), this function only records the optional image size for the specified drawing operation; that is, the drawing operation is not performed. This can be used to get the optimal image buffer size for the drawing operation.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context to use when drawing. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `ContextOrResultStrId` *(in, AIL_ID)*

Specifies the String Reader context or String Reader result buffer from which to extract the features to draw. The String Reader context or result buffer must have been previously allocated on the system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md) or [`MstrAllocResult`](../../Reference/str/MstrAllocResult.md), respectively.

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or 2D graphics list in which to draw. The buffer can be any valid 1- or 3-band image buffer allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md). The 2D graphics list must be previously allocated using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md). By drawing into a display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For a font-based context*

| Value | Description |
| --- | --- |
| `M_DRAW_CHAR` | Draws a character representation of the font in the destination image. Characters are drawn from left to right, and from top to bottom. Characters that fall outside the destination image are clipped.

The characters are drawn according to their draw box margin, set using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_DRAW_BOX_MARGIN_X`](../../Reference/str/MstrControl.md) and [`M_DRAW_BOX_MARGIN_Y`](../../Reference/str/MstrControl.md). This margin will determine the amount of white space between each character that is drawn. |

*For a result buffer*

| Value | Description |
| --- | --- |
| `M_DRAW_AIL_FONT_STRING` | Draws all the characters of the String Reader result(s) under the bottom-left corner of the string bounding box ([`M_DRAW_STRING_BOX`](../../Reference/str/MstrDraw.md)). The font associated with the 2D graphics context is used to draw the characters. |
| `M_DRAW_STRING` | Draws all the characters of String Reader result(s). |
| `M_DRAW_STRING_BOX` | Draws a box around the string that is read in the target. Note that the bounding box of the string is the bounding box of all the string's characters, including the draw margin (set with [`M_DRAW_BOX_MARGIN_X`](../../Reference/str/MstrControl.md) and [`M_DRAW_BOX_MARGIN_Y`](../../Reference/str/MstrControl.md) in [`MstrControl`](../../Reference/str/MstrControl.md)). |
| `M_DRAW_STRING_CHAR_BOX` | Draws all the character boxes of the String Reader result(s). The character box drawn is the bounding box of the character plus the draw margin (set with [`M_DRAW_BOX_MARGIN_X`](../../Reference/str/MstrControl.md) and [`M_DRAW_BOX_MARGIN_Y`](../../Reference/str/MstrControl.md) in [`MstrControl`](../../Reference/str/MstrControl.md)). |
| `M_DRAW_STRING_CHAR_POSITION` | Draws a cross at the center of the bounding box of each string's character. The character's angle, scale, and aspect ratio are taken into account. |
| `M_DRAW_STRING_CONTOUR` | Draws the contour of all the characters of the result(s) of the string(s) read. |

### `Index` *(in, AIL_INT)*

Specifies the index of the font feature or result feature to draw.

*For specifying the font or result*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Draws the default features for either fonts or results.

For a String Reader context, the default value is the same as **M_FONT_INDEX()**.

For a String Reader result, the default value is the same as [`M_ALL`](../../Reference/str/MstrDraw.md). |
| `M_FONT_INDEX` | Draws the features of a specific font. |
| `M_ALL` | Draws the features of all results. |
| `Value >= 0` | Draws the features of a specific result. |

### `CharListPtr` *(in, *void)*

Specifies an explicit list of valid characters to draw, at the specified position. This is an optional, null-terminated string.

### `ControlFlag` *(in, AIL_INT64)*

Specifies where in the destination image buffer to draw.

*For specifying where to draw*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Draws characters from the center of the top-left pixel of the destination image buffer; or, if drawing in a 2D graphics list, from the center of the top-left pixel of the image used at the time of annotation. |
| `M_ORIGINAL` | Draws characters at the same offset as their original position in the definition image, for user-defined characters (set using [`MstrEditFont`](../../Reference/str/MstrEditFont.md) with [`M_USER_DEFINED`](../../Reference/str/MstrEditFont.md)). Note that characters that are not user-defined (for example, TrueType) will be drawn at an offset of 0.

> **Note:** To ascertain where user-defined characters are located in the definition image, you must draw the user-defined characters in an overlay on top of the definition image. That is to say, you must draw the characters on top of their original location. For more information, refer to [`M_DEFINITION_OFFSET_X`](../../Reference/str/MstrInquire.md) and [`M_DEFINITION_OFFSET_Y`](../../Reference/str/MstrInquire.md) in [`MstrInquire`](../../Reference/str/MstrInquire.md). |

Note that this operation is only available if using a font-based context.

Note that this operation is only available if the results were obtained using a font-based context.

The 2D graphics context cannot have its input units set to world units ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) set to [`M_WORLD`](../../Reference/gra/MgraControl.md)) when drawing with this operation in either an image buffer or a 2D graphics list. If drawing in a zoomed and offset region, only integer values must be used ([`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_DRAW_OFFSET_X`](../../Reference/gra/MgraControl.md),[`M_DRAW_OFFSET_Y`](../../Reference/gra/MgraControl.md),[`M_DRAW_ZOOM_X`](../../Reference/gra/MgraControl.md), and [`M_DRAW_ZOOM_Y`](../../Reference/gra/MgraControl.md) set to integer values).
