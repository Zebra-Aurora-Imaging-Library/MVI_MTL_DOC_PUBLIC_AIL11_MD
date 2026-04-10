---
doctype: Reference
module: gra
function: MgraLines
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraLines"
---

# MgraLines

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

> Draw one or more lines, a polyline, or a polygon in an image, or add them to a 2D graphics list.

## Syntax

```c
void MgraLines(
    AIL_ID             ContextGraId,             //in
    AIL_ID             DstImageBufOrListGraId,   //out
    AIL_INT            NumberOfLinesOrVertices,  //in
    const AIL_DOUBLE * XPtr,                     //in
    const AIL_DOUBLE * YPtr,                     //in
    const AIL_DOUBLE * X2Ptr,                    //in
    const AIL_DOUBLE * Y2Ptr,                    //in
    AIL_INT64          ControlFlag               //in
)
```

## Description

This function draws one or more lines, a polyline, or a polygon destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of the lines, polyline, or polygon to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

Lines, polylines, and polygons inherit all the relevant settings of the specified 2D graphics context, such as the foreground color. If part of a line, polyline, or polygon falls outside of the destination image (or, when drawn in a 2D graphics list, the associated display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md), respectively. To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md), respectively.

The coordinates of the lines, polyline, or polygon are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraLines`](../../Reference/gra/MgraLines.md) an uncalibrated image, the function will generate an error.

To create a single line without the option of creating multiple lines, a polyline, or a polygon, use [`MgraLine`](../../Reference/gra/MgraLine.md).

Note that prior to Aurora Imaging Library 9.0, this function only supported integer positions; the [`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md), [`X2Ptr`](../../Reference/gra/MgraLines.md), and [`Y2Ptr`](../../Reference/gra/MgraLines.md) parameters only accepted arrays of type _long_. As of Aurora Imaging Library 9.0, this function includes support for positions with floating-point precision. To implement this change and maintain backwards compatibility when using a C++ compiler (*.cpp), [`MgraLines`](../../Reference/gra/MgraLines.md) is available as an inline function which automatically calls [`MgraLinesDouble`](../../Reference/MgraLinesDouble.md), [`MgraLinesInt64`](../../Reference/MgraLinesInt64.md), and [`MgraLinesInt32`](../../Reference/MgraLinesInt32.md), depending on whether [`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md), [`X2Ptr`](../../Reference/gra/MgraLines.md), and [`Y2Ptr`](../../Reference/gra/MgraLines.md) receive arrays of type _AIL_DOUBLE_, _AIL_INT64_, or _AIL_INT32_, respectively. To maintain backwards compatibility when using a C compiler (*.c), [`MgraLines`](../../Reference/gra/MgraLines.md) maps to [`MgraLinesInt32`](../../Reference/MgraLinesInt32.md) when working on a 32-bit system, or [`MgraLinesInt64`](../../Reference/MgraLinesInt64.md) when working on a 64-bit system; you must explicitly call [`MgraLinesDouble`](../../Reference/MgraLinesDouble.md) to pass [`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md), [`X2Ptr`](../../Reference/gra/MgraLines.md), and [`Y2Ptr`](../../Reference/gra/MgraLines.md) arrays of type _AIL_DOUBLE_, respectively. If you are an advanced user and want to retrieve a pointer to [`MgraLines`](../../Reference/gra/MgraLines.md), you must use the Double, Int64, or Int32 version of this function, since [`MgraLines`](../../Reference/gra/MgraLines.md) is actually a macro or an overloaded function.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraLines.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `ContextGraId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics context. This parameter must be set to one of the following values:

*For specifying the 2D graphics context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the default 2D graphics context of the current Aurora Imaging Library application is used.

> **Note:** Note that there is a different default 2D graphics context for each thread. |
| `2D graphics context identifier` | Specifies the identifier of the 2D graphics context, which you have allocated using [`MgraAlloc`](../../Reference/gra/MgraAlloc.md). |

### `DstImageBufOrListGraId` *(out, AIL_ID)*

Specifies the identifier of a valid image buffer in which to draw the lines, polyline, or polygon or the identifier of a valid 2D graphics list in which to add the lines, polyline, or polygon. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `NumberOfLinesOrVertices` *(in, AIL_INT)*

Specifies the number of lines to draw or add ([`M_LINE_LIST`](../../Reference/gra/MgraLines.md)), or the number of vertices in the polyline or polygon to draw or add ([`M_POLYGON`](../../Reference/gra/MgraLines.md) or [`M_POLYLINE`](../../Reference/gra/MgraLines.md)), according to the array parameters ([`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md), [`X2Ptr`](../../Reference/gra/MgraLines.md), and [`Y2Ptr`](../../Reference/gra/MgraLines.md)).

### `XPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the start of the lines, or containing the X-coordinates of the vertices in the polyline or polygon, in the input coordinate system. The number of elements in the array should be equal to the [`NumberOfLinesOrVertices`](../../Reference/gra/MgraLines.md) parameter value.

### `YPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the start of the lines, or containing the Y-coordinates of the vertices in the polyline or polygon, in the input coordinate system. The number of elements in the array should be equal to the [`NumberOfLinesOrVertices`](../../Reference/gra/MgraLines.md) parameter value.

### `X2Ptr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the end of the lines in the input coordinate system. The number of elements in the array should be equal to the [`NumberOfLinesOrVertices`](../../Reference/gra/MgraLines.md) parameter value. If this information is not required ([`M_POLYGON`](../../Reference/gra/MgraLines.md) and [`M_POLYLINE`](../../Reference/gra/MgraLines.md)), set this parameter to [`M_NULL`](../../Reference/gra/MgraLines.md).

### `Y2Ptr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the end of the lines in the input coordinate system. The number of elements in the array should be equal to the [`NumberOfLinesOrVertices`](../../Reference/gra/MgraLines.md) parameter value. If this information is not required ([`M_POLYGON`](../../Reference/gra/MgraLines.md) and [`M_POLYLINE`](../../Reference/gra/MgraLines.md)), set this parameter to [`M_NULL`](../../Reference/gra/MgraLines.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of line to draw in the image or to add to the 2D graphics list.

## Parameter Associations

### For specifying the type of line

Note that any unused parameters should be set to[`M_NULL`](../../Reference/gra/MgraLines.md).

---

### `M_DEFAULT`

Same as [`M_LINE_LIST`](../../Reference/gra/MgraLines.md).

---

### `M_INFINITE_LINES`

Specifies a series of infinite lines, each with no start or end points. Each line is defined by two points (([`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md)) and ([`X2Ptr`](../../Reference/gra/MgraLines.md), [`Y2Ptr`](../../Reference/gra/MgraLines.md))), but the line extends beyond these points.

---

### `M_LINE_LIST`

Specifies a series of lines of finite length. Each line is defined with two points (([`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md)) and ([`X2Ptr`](../../Reference/gra/MgraLines.md), [`Y2Ptr`](../../Reference/gra/MgraLines.md))).

---

### `M_POLYGON`

Specifies a series of connected lines forming a closed polygon. The polygon is defined with two or more vertices ([`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md)). The final vertex is connected to the first.

---

### `M_POLYLINE`

Specifies a series of connected lines forming an open polyline. The polyline is defined with two or more vertices ([`XPtr`](../../Reference/gra/MgraLines.md), [`YPtr`](../../Reference/gra/MgraLines.md)). The final vertex is not connected to the first.  Since the final vertex is not connected to the first, the number of line segments in the polyline is equal to [`NumberOfLinesOrVertices`](../../Reference/gra/MgraLines.md) - 1.

### Combination Constants — For specifying whether the polygon is filled

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set whether the polygon is filled.

| Value | Description |
| --- | --- |
| `M_FILLED` | Specifies that the polygon is filled with the foreground color of the 2D graphics context. |
