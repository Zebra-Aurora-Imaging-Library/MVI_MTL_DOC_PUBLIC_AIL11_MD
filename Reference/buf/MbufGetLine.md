---
doctype: Reference
module: buf
function: MbufGetLine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufGetLine"
---

# MbufGetLine

> Read the pixels along a specified theoretical line, count the pixels, and store either their values or coordinates in a user-defined array.

## Syntax

```c
void MbufGetLine(
    AIL_ID    ImageBufId,   //in
    AIL_INT   StartX,       //in
    AIL_INT   StartY,       //in
    AIL_INT   EndX,         //in
    AIL_INT   EndY,         //in
    AIL_INT64 Mode,         //in
    AIL_INT * NbPixelsPtr,  //out
    void *    UserArrayPtr  //out
)
```

## Description

This function reads the series of pixels along a theoretical line (defined by coordinates) from an image and stores either their values or the X/Y coordinates of each value in a user-defined array. The Bresenham algorithm is used to determine the theoretical line. The line can start and end outside of the buffer, but only pixel values/coordinates within the buffer are store in the user-defined array.

If the source buffer is compressed, Aurora Imaging Library decompresses the data before copying it to the user-supplied array.

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer. This must be a single-band (monochrome) buffer.

### `StartX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the starting position of the line, relative to the top-left pixel of the source buffer.

### `StartY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the starting position of the line, relative to the top-left pixel of the source buffer.

### `EndX` *(in, AIL_INT)*

Specifies the horizontal pixel offset of the finishing position of the line, relative to the top-left pixel of the source buffer.

### `EndY` *(in, AIL_INT)*

Specifies the vertical pixel offset of the finishing position of the line, relative to the top-left pixel of the source buffer.

### `Mode` *(in, AIL_INT64)*

Specifies the operation mode.

*For specifying the operation mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to read or count the pixels along a specified theoretical line and store their values in a user-defined array. |
| `M_POSITION_X` | Specifies to read the pixels along a specified theoretical line and store the X-coordinates of the pixels along that line in a user-defined array of integer values. |
| `M_POSITION_Y` | Specifies to read the pixels along a specified theoretical line and store the Y-coordinates of the pixels along that line in a user-defined array of integer values. |

### `NbPixelsPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the number of pixels found along the theoretical line. You can set this parameter to `M_NULL` if you don't want this value to be evaluated.

### `UserArrayPtr` *(out, *void)*

Specifies the address of the user array in which to store the pixels or the coordinates from the image buffer. Ensure that the user array is large enough to receive the data to be stored. To determine the required size of the array, you can set this parameter to `M_NULL` and pass a non-null address to [`NbPixelsPtr`](../../Reference/buf/MbufGetLine.md), which will store the required size of the array. In this case, nothing is read from the image buffer.
