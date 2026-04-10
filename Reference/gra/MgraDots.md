---
doctype: Reference
module: gra
function: MgraDots
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraDots"
---

# MgraDots

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

> Draw one or more dots in an image, or add them to a 2D graphics list.

## Syntax

```c
void MgraDots(
    AIL_ID             ContextGraId,            //in
    AIL_ID             DstImageBufOrListGraId,  //out
    AIL_INT            NumberOfDots,            //in
    const AIL_DOUBLE * PosXArrayPtr,            //in
    const AIL_DOUBLE * PosYArrayPtr,            //in
    AIL_INT64          ControlFlag              //in
)
```

## Description

This function draws one or more dots destructively (raster-based) in the specified image. Alternatively, this function can add a vector-based version of the dots to the specified 2D graphics list, allowing you to, for example, non-destructively annotate a display without pixelation effects upon scaling.

Dots are based on the points positioned at [`PosXArrayPtr`](../../Reference/gra/MgraDots.md) and [`PosYArrayPtr`](../../Reference/gra/MgraDots.md). The dots inherit all the relevant settings of the specified 2D graphics context, such as the foreground color (see [`MgraAlloc`](../../Reference/gra/MgraAlloc.md) for default context settings). If part of the dots fall outside of the specified area (image or display), that part is clipped off.

To modify or inquire 2D graphics context settings, use [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraInquire`](../../Reference/gra/MgraInquire.md). To modify or inquire 2D graphics list settings, use [`MgraControlList`](../../Reference/gra/MgraControlList.md) or [`MgraInquireList`](../../Reference/gra/MgraInquireList.md).

The coordinates of the dots are interpreted with respect to the input coordinate system, specified using [`MgraControl`](../../Reference/gra/MgraControl.md) or [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md). Note that if you set your input coordinate system to [`M_WORLD`](../../Reference/gra/MgraControl.md) and you pass [`MgraDots`](../../Reference/gra/MgraDots.md) an uncalibrated image, the function will generate an error.

To create a single dot without the option of creating multiple dots, use [`MgraDot`](../../Reference/gra/MgraDot.md).

Note that prior to Aurora Imaging Library 9.0, this function only supported integer positions; the [`PosXArrayPtr`](../../Reference/gra/MgraDots.md) and [`PosYArrayPtr`](../../Reference/gra/MgraDots.md) parameters only accepted arrays of type _long_. As of Aurora Imaging Library 9.0, this function includes support for positions with floating-point precision. To implement this change and maintain backwards compatibility when using a C++ compiler (*.cpp), [`MgraDots`](../../Reference/gra/MgraDots.md) is available as an inline function which automatically calls [`MgraDotsDouble`](../../Reference/MgraDotsDouble.md), [`MgraDotsInt32`](../../Reference/MgraDotsInt32.md), and,[`MgraDotsInt64`](../../Reference/MgraDotsInt64.md) depending on whether [`PosXArrayPtr`](../../Reference/gra/MgraDots.md) and [`PosYArrayPtr`](../../Reference/gra/MgraDots.md) receive arrays of type _AIL_DOUBLE_, _AIL_INT32_, or _AIL_INT64_, respectively. To maintain backwards compatibility when using a C compiler (*.c), [`MgraDots`](../../Reference/gra/MgraDots.md) maps to [`MgraDotsInt32`](../../Reference/MgraDotsInt32.md) when working on a 32-bit system, or [`MgraDotsInt64`](../../Reference/MgraDotsInt64.md) when working on a 64-bit system; you must explicitly call [`MgraDotsDouble`](../../Reference/MgraDotsDouble.md) to pass [`PosXArrayPtr`](../../Reference/gra/MgraDots.md) and [`PosYArrayPtr`](../../Reference/gra/MgraDots.md) arrays of type_AIL_DOUBLE_. If you are an advanced user and want to retrieve a pointer to [`MgraDots`](../../Reference/gra/MgraDots.md), you must use the Double, Int64, or Int32 version of this function, since [`MgraDots`](../../Reference/gra/MgraDots.md) is actually a macro or an overloaded function.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`DstImageBufOrListGraId`](../../Reference/gra/MgraDots.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

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

Specifies the identifier of a valid image buffer in which to draw the dots or the identifier of a valid 2D graphics list in which to add the dots. You must have allocated the image buffer or the 2D graphics list using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) or [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), respectively.

### `NumberOfDots` *(in, AIL_INT)*

Specifies the number of dots to draw.

### `PosXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinate(s) of the dots to be drawn in the input coordinate system.

### `PosYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinate(s) of the dots to be drawn in the input coordinate system.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
