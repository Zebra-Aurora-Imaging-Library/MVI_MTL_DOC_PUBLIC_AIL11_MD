---
doctype: Reference
module: gra
function: MgraVectors
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gra / MgraVectors"
---

# MgraVectors

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

> Draw vectors at arbitrary positions in an image, or add them to a 2D graphics list.

## Syntax

```c
void MgraVectors(
    AIL_ID             ContextGraId,            //in
    AIL_ID             DstImageBufOrListGraId,  //out
    AIL_INT            NumVectors,              //in
    const AIL_DOUBLE * XArrayPtr,               //in
    const AIL_DOUBLE * YArrayPtr,               //in
    const AIL_DOUBLE * UArrayPtr,               //in
    const AIL_DOUBLE * VArrayPtr,               //in
    AIL_INT64          ScaleMode,               //in
    AIL_DOUBLE         ScaleValue,              //in
    AIL_INT64          ControlFlag              //in
)
```

## Description

This function draws vectors destructively (raster-based) in the specified image at arbitrary positions. Alternatively, this function can add a vector-based version of the vectors to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling. To draw vectors at equally spaced positions, use [`MgraVectorsGrid`](../../Reference/gra/MgraVectorsGrid.md).

The vectors are based on the specified starting positions ([`XArrayPtr`](../../Reference/gra/MgraVectors.md) and [`YArrayPtr`](../../Reference/gra/MgraVectors.md)), and the specified horizontal and vertical displacements ([`UArrayPtr`](../../Reference/gra/MgraVectors.md) and [`VArrayPtr`](../../Reference/gra/MgraVectors.md)). For example, the values at a given index in the specified arrays define a vector that goes from `(_x_[_index_], _y_[_index_])` to `(_x_[_index_] + _u_[_index_], _y_[_index_] + _v_[_index_])`.

The vectors inherit all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the vectors fall outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

The coordinates of the vectors are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraVectors`](../../Reference/gra/MgraVectors.md) an uncalibrated image, the function will generate an error.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectors.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

### `NumVectors` *(in, AIL_INT)*

Specifies the number of vectors to draw.

### `XArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinates of the starting points of the vectors to draw. The number of elements in the array should be equal to the value specified for the [`NumVectors`](../../Reference/gra/MgraVectors.md) parameter.

### `YArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinates of the starting points of the vectors to draw. The number of elements in the array should be equal to the value specified for the [`NumVectors`](../../Reference/gra/MgraVectors.md) parameter.

### `UArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the displacements, in the X-direction, of the vectors to draw. The number of elements in the array should be equal to the value specified for the [`NumVectors`](../../Reference/gra/MgraVectors.md) parameter.

### `VArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the displacements, in the Y-direction, of the vectors to draw. The number of elements in the array should be equal to the value specified for the [`NumVectors`](../../Reference/gra/MgraVectors.md) parameter.

### `ScaleMode` *(in, AIL_INT64)*

Specifies how to use the scale value ([`ScaleValue`](../../Reference/gra/MgraVectors.md)) to scale the vectors. By default, vectors are not scaled.

*For specifying how to use the scale value to scale the vectors*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies to use the scale value ([`ScaleValue`](../../Reference/gra/MgraVectors.md)) as a scaling factor that directly multiplies the vectors' horizontal and vertical displacements ([`UArrayPtr`](../../Reference/gra/MgraVectors.md) and [`VArrayPtr`](../../Reference/gra/MgraVectors.md)). |
| `M_AUTO` | Specifies that Aurora Imaging Library determines the scale mode. To do this, Aurora Imaging Library makes internal assumptions to establish an ideal scaling factor that optimizes the space taken by the vectors in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectors.md)) while reducing the chance of overlapping vectors. When using [`M_AUTO`](../../Reference/gra/MgraVectors.md), Aurora Imaging Library multiplies the vectors' horizontal and vertical displacements ([`UArrayPtr`](../../Reference/gra/MgraVectors.md) and [`VArrayPtr`](../../Reference/gra/MgraVectors.md)) by both the scale value ([`ScaleValue`](../../Reference/gra/MgraVectors.md)) and the internally established scale factor. |

### `ScaleValue` *(in, AIL_DOUBLE)*

Specifies the value by which to scale the vectors, according to the specified mode ([`ScaleMode`](../../Reference/gra/MgraVectors.md)). By default, vectors are not scaled.

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
| `M_DEFAULT` | Same as [`M_FIXED_LENGTH_ARROWHEADS`](../../Reference/gra/MgraVectors.md) + [`M_SKIP_NULL_VECTORS`](../../Reference/gra/MgraVectors.md). |
| `M_FIXED_LENGTH_ARROWHEADS` | Specifies that arrowheads are drawn identically for all vectors, regardless of each vector's length. |
| `M_PROPORTIONAL_ARROWHEADS` | Specifies that arrowheads are drawn proportionally to each vector's length. |

*For determining how to handle null vectors*

| Value | Description |
| --- | --- |
| `M_DRAW_NULL_VECTORS` | Specifies that null vectors are drawn as a dot in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectors.md)). |
| `M_SKIP_NULL_VECTORS` | Specifies that null vectors are not drawn in the destination ([`DstImageBufOrListGraId`](../../Reference/gra/MgraVectors.md)). |
