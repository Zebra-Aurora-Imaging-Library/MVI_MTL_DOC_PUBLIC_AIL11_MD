---
doctype: Reference
module: gra
function: MgraText
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraText"
---

# MgraText

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

> Draw text in an image or add text to a 2D graphics list.

## Syntax

```c
void MgraText(
    AIL_ID             ContextGraId,            //in
    AIL_ID             DstImageBufOrListGraId,  //out
    AIL_DOUBLE         XStart,                  //in
    AIL_DOUBLE         YStart,                  //in
    AIL_CONST_TEXT_PTR StringPtr                //in
)
```

## Description

This function draws text destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of the text to the specified 2D graphics list.

Note that while the location of text can sometimes change (for example, by applying a camera calibration or zooming the display), the text's font will always remain the same, and the text itself will never be magnified or distorted.

Text is defined by a string ([`StringPtr`](../../Reference/gra/MgraText.md)) and a starting position ([`XStart`](../../Reference/gra/MgraText.md), [`YStart`](../../Reference/gra/MgraText.md)). Text inherits all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the text falls outside of the specified area (image or display), that part is clipped off. By default, a text's background is filled; to make it transparent, use [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_BACKGROUND_MODE`](../../Reference/gra/MgraControl.md).

To modify or inquire graphic context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

A text's coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraText`](../../Reference/gra/MgraText.md) an uncalibrated image, the function will generate an error.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraText.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies a valid 2D graphics context identifier, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to draw the graphic or the identifier of a valid 2D graphics list in which to add the graphic. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `XStart` *(in, AIL_DOUBLE)*

Specifies the X-coordinate at which to start drawing the top-left corner of the first character in the input coordinate system.

### `YStart` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate at which to start drawing the top-left corner of the first character in the input coordinate system.

### `StringPtr` *(in, AIL_CONST_TEXT_PTR)*

Specifies the address of the string containing the text to draw in the destination buffer.

*For specifying the string*

| Value | Description |
| --- | --- |
| `"String"` | Specifies the address of the null-terminated (\0) ASCII string that must be drawn in the destination buffer ([`DstImageBufOrListGraId`](../../Reference/gra/MgraText.md)). There is no restriction on the length of the string. |
