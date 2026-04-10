---
doctype: Reference
module: gra
function: MgraVectorsGrid
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraVectorsGrid"
---

# MgraVectorsGrid

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

> Draw vectors at equally spaced positions in an image, or add them to a 2D graphics list.

## Syntax

```c
void MgraVectorsGrid(
    AIL_ID     ContextGraId,            //in
    AIL_ID     DstImageBufOrListGraId,  //out
    AIL_ID     UImageBufId,             //in
    AIL_ID     VImageBufId,             //in
    AIL_INT    Stride,                  //in
    AIL_INT64  ScaleMode,               //in
    AIL_DOUBLE ScaleValue,              //in
    AIL_INT64  ControlFlag              //in
)
```

## Description

This function draws vectors destructively (raster-based) in the specified image at equally spaced positions. Alternatively, this function can add a vector-based version of the vectors to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling. To draw vectors at arbitrary positions, use [`MgraVectors`](../../Reference/gra/MgraVectors.md).

The vectors are based on the specified image buffers ([`UImageBufId`](../../Reference/gra/MgraVectorsGrid.md) and [`VImageBufId`](../../Reference/gra/MgraVectorsGrid.md)) containing values that correspond to the horizontal and vertical displacements of the vectors. Aurora Imaging Library uses the displacements at a given location to calculate the starting and ending positions of the vectors. For example, the values located at `(_x_, _y_)` in the specified buffers ([`UImageBufId`](../../Reference/gra/MgraVectorsGrid.md) and [`VImageBufId`](../../Reference/gra/MgraVectorsGrid.md)) define a vector that goes from `(_x_, _y_)` to `(_x_ + _u_[_x_][_y_], y + _v_[_x_][_y_])`. The number of pixels in the buffers `(_SizeX_ x _SizeY_)` determine the number of vectors to draw.

Typically, the image buffers from which the vectors' positions and lengths are inferred are the result of a previous image processing operation, such as an edge detection (for example, [`MimConvolve`](../../Reference/im/MimConvolve.md)).

The vectors inherit all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the vectors fall outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

The starting coordinates of the first vector drawn are (0,0). Coordinates are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md) uncalibrated images, the function will generate an error.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectorsGrid.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For the identifier of the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies the identifier of the 2D graphics context, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to draw the vectors or the identifier of a valid 2D graphics list in which to add the vectors. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `UImageBufId` *(in, AIL_ID)*

Specifies the identifier of a valid image buffer containing the values corresponding to the displacements, in the X-direction, of the vectors to draw. You must have allocated the image buffer using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).

### `VImageBufId` *(in, AIL_ID)*

Specifies the identifier of a valid image buffer containing the values corresponding to the displacements, in the Y-direction, of the vectors to draw. You must have allocated the image buffer using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md).

### `Stride` *(in, AIL_INT)*

Specifies a value that establishes the positions for which vectors are drawn. Aurora Imaging Library always draws a vector for the first position (0,0), and then draws subsequent vectors that are a multiple of the specified stride, in both the X- and Y-direction. For example, if you specify a stride of 2, a vector is drawn for position (0,0), and for every second position after that, in the X-direction and in the Y-direction. Specifying a stride of 1 draws a vector for every position. Increasing the stride increases the space between the vectors drawn.

### `ScaleMode` *(in, AIL_INT64)*

Specifies how to use the scale value ([`ScaleValue`](../../Reference/gra/MgraVectorsGrid.md)) to scale the vectors. By default, vectors are not scaled.

*For specifying how to use the scale value to scale the vectors*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies to use the scale value ([`ScaleValue`](../../Reference/gra/MgraVectorsGrid.md)) as a scaling factor that directly multiplies the vectors' horizontal and vertical displacements ([`UImageBufId`](../../Reference/gra/MgraVectorsGrid.md) and [`VImageBufId`](../../Reference/gra/MgraVectorsGrid.md)). |
| `M_AUTO` | Specifies that Aurora Imaging Library determines the scale mode. To do this, Aurora Imaging Library makes internal assumptions to establish an ideal scaling factor that optimizes the space taken by the vectors in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectorsGrid.md)) while reducing the chance of overlapping vectors. When using [`M_AUTO`](../../Reference/gra/MgraVectorsGrid.md), Aurora Imaging Library multiplies the vectors' horizontal and vertical displacements ([`UImageBufId`](../../Reference/gra/MgraVectorsGrid.md) and [`VImageBufId`](../../Reference/gra/MgraVectorsGrid.md)) by both the scale value ([`ScaleValue`](../../Reference/gra/MgraVectorsGrid.md)) and the internally established scale factor. |

### `ScaleValue` *(in, AIL_DOUBLE)*

Specifies the value by which to scale the vectors, according to the specified mode ([`ScaleMode`](../../Reference/gra/MgraVectorsGrid.md)). By default, vectors are not scaled.

*For specifying the value by which to scale the vectors, according to the specified mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the scale value. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to configure the arrowheads of the vectors, and how to handle null vectors.

*For specifying how to configure the arrowheads of the vectors*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_FIXED_LENGTH_ARROWHEADS`](../../Reference/gra/MgraVectorsGrid.md) + [`M_SKIP_NULL_VECTORS`](../../Reference/gra/MgraVectorsGrid.md). |
| `M_FIXED_LENGTH_ARROWHEADS` | Specifies that arrowheads are drawn identically for all vectors, regardless of each vector's length. |
| `M_PROPORTIONAL_ARROWHEADS` | Specifies that arrowheads are drawn proportionally to each vector's length. |

*For determining how to handle null vectors*

| Value | Description |
| --- | --- |
| `M_DRAW_NULL_VECTORS` | Specifies that null vectors are drawn as a dot in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectorsGrid.md)). |
| `M_SKIP_NULL_VECTORS` | Specifies that null vectors are not drawn in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectorsGrid.md)). |
