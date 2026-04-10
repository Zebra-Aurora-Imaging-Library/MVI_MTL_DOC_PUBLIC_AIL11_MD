---
doctype: Reference
module: dlocr
function: MdlocrGetStringIndex
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrGetStringIndex"
---

# MdlocrGetStringIndex

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

> Get the index of a string that touches a specified image position, from a Deep Learning OCR result buffer.

## Syntax

```c
AIL_INT64 MdlocrGetStringIndex(
    AIL_ID      ResultDlocrId,  //in
    AIL_DOUBLE  PositionX,      //in
    AIL_DOUBLE  PositionY,      //in
    AIL_INT64 * StringIndexPtr  //out
)
```

## Description

This function retrieves the index of a string that touches a specified image position, from a Deep Learning OCR result buffer. This is useful, for example, in combination with [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) to define a string model from a position on an image. You can establish the position of the string of interest by drawing the results using [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md).

## Parameters

### `ResultDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR result buffer from which to retrieve results.

### `PositionX` *(in, AIL_DOUBLE)*

Specifies the X-position in the image at which to find a string. You can change whether to use pixel or world coordinates using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md).

### `PositionY` *(in, AIL_DOUBLE)*

Specifies the Y-position in the image at which to find a string. You can change whether to use pixel or world coordinates using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/dlocr/MdlocrControl.md).

### `StringIndexPtr` *(out, *AIL_INT64)*

Specifies the address in which to write the index of the string if one is found touching the specified position. If no string touches the specified position, **M_INVALID** is written at this address. Since [`MdlocrGetStringIndex`](../../Reference/dlocr/MdlocrGetStringIndex.md) also returns the requested information, you can set this parameter to `M_NULL`.

## Return Value

**Type:** `AIL_INT64`

The returned value is the index of the string that touches the specified position. If no string touches, `M_INVALID` is returned.
