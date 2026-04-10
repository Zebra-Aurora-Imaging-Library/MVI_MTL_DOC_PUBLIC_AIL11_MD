---
doctype: Reference
module: buf
function: MbufPutLine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufPutLine"
---

# MbufPutLine

> Write a specified series of values along a specified theoretical line in an image.

## Syntax

```c
void MbufPutLine(
    AIL_ID       ImageBufId,   //out
    AIL_INT      StartX,       //in
    AIL_INT      StartY,       //in
    AIL_INT      EndX,         //in
    AIL_INT      EndY,         //in
    AIL_INT64    Mode,         //in
    AIL_INT *    NbPixelsPtr,  //out
    const void * UserArrayPtr  //in
)
```

## Description

This function reads pixel values from a user-defined array and writes them to the series of pixels, in the specified image, along the theoretical line defined by specified coordinates. The Bresenham algorithm is used to determine the theoretical line. The line can start and end outside of the buffer. The first pixel value in the array will be written to the first valid pixel in the buffer. If the user-array contains more entries than there are valid pixels on the theoretical line in the buffer, the excess entries will be ignored. If the user-array is smaller than the number of valid pixels on the theoretical line, an error will occur.

## Parameters

### `ImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer. This must be a single-band (monochrome) buffer.

### `StartX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the starting position of the line, relative to the top-left pixel of the destination buffer.

### `StartY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the starting position of the line, relative to the top-left pixel of the destination buffer.

### `EndX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the finishing position on the line, relative to the top-left pixel of the destination buffer.

### `EndY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the finishing position on the line, relative to the top-left pixel of the destination buffer.

### `Mode` *(in, AIL_INT64)*

Specifies the operation mode. Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `NbPixelsPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the number of pixels found along the theoretical line. You can set this parameter to `M_NULL` if you don't want this value to be evaluated.

### `UserArrayPtr` *(in, *void)*

Specifies the address of the user array containing the pixels to write to the image buffer. Ensure that the user array is at least as large as the value returned by [`NbPixelsPtr`](../../Reference/buf/MbufPutLine.md). To determine the number of pixel values required, you can set this parameter to `M_NULL` and pass a non-null address to [`NbPixelsPtr`](../../Reference/buf/MbufPutLine.md), which will store the size of the required array. In this case, nothing is written to the image buffer.
