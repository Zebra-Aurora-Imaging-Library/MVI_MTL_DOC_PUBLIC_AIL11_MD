---
doctype: Reference
module: 3ddisp
function: M3ddispPickRect
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispPickRect"
---

# M3ddispPickRect

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

> Pick 3D graphics within a given pixel range defined by a rectangular region in a 3D display.

## Syntax

```c
void M3ddispPickRect(
    AIL_ID    Context3ddispId,  //in
    AIL_ID    Disp3dId,         //in
    AIL_ID    Result3ddispId,   //out
    AIL_INT64 PixelX1,          //in
    AIL_INT64 PixelY1,          //in
    AIL_INT64 PixelX2,          //in
    AIL_INT64 PixelY2,          //in
    AIL_INT64 ControlFlag       //in
)
```

## Description

This function identifies which 3D graphics (if any) are in a specific rectangular region in the 3D display and writes the results in the specified result buffer. You can retrieve results using [`M3ddispGetResult`](../../Reference/3ddisp/M3ddispGetResult.md). Aurora Imaging Library identifies the 3D graphics by casting a frustum from the 3D display's viewpoint towards the given region and detecting the 3D graphic(s) that intersect the frustum (either completely or partially). Note that 3D graphics occluded by other 3D graphics are still picked, provided that they are within the frustum.

You can set a region that encapsulates the 3D display in its entirety, a portion of it, or none of it at all. Any 3D graphics intersecting the resulting frustum will be picked, including those outside the visible portion of the 3D display.

Use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) to configure the settings of the picking context before calling this function. You can filter out 3D graphics that you don't want to pick. For example, you can use [`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispControl.md) to only detect 3D graphics of a certain type. If 3D graphics of the specified type are behind filtered out 3D graphics (either on a lower layer or further away on the same layer), the frustum will pass through the filtered out graphics and [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) will detect the specified graphics.

To define the rectangular region, use [`M3ddispPickRect`](../../Reference/3ddisp/M3ddispPickRect.md) to specify the X- and Y-coordinates of 2 separate pixels. These will form opposite corners of the rectangle from which to pick. You can select the required pixels interactively. To do so, set the 4 pixel coordinate parameters (for example, [`PixelX1`](../../Reference/3ddisp/M3ddispPickRect.md)) to the X- and Y-positions of your cursor for 2 consecutive mouse click events. Use [`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) with [`M_MOUSE_POSITION_X`](../../Reference/3ddisp/M3ddispGetHookInfo.md) and [`M_MOUSE_POSITION_Y`](../../Reference/3ddisp/M3ddispGetHookInfo.md) to get the position of the mouse when an [`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md) event is triggered.

## Parameters

### `Context3ddispId` *(in, AIL_ID)*

Specifies the identifier of the picking area context. The picking area context must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) with [`M_PICKING_AREA_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md).

### `Disp3dId` *(in, AIL_ID)*

Specifies the identifier of the 3D display. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

### `Result3ddispId` *(out, AIL_ID)*

Specifies the identifier of the picking area result buffer in which to write the results. The picking area result buffer must have been previously allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_AREA_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md).

### `PixelX1` *(in, AIL_INT64)*

Specifies the first X-coordinate of the region from which to generate the picking frustum. If the specified pixel coordinate is outside the bounds of the display (less than 0 or greater than [`M_SIZE_X`](../../Reference/3ddisp/M3ddispInquire.md)), a warning is generated. 3D graphics outside of the bounds of the display are still detected and picked.

### `PixelY1` *(in, AIL_INT64)*

Specifies the first Y-coordinate of the region from which to generate the picking frustum. If the specified pixel coordinate is outside of the bounds of the display (less than 0 or greater than [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispInquire.md)), a warning is generated. 3D graphics outside of the bounds of the display are still detected and picked.

### `PixelX2` *(in, AIL_INT64)*

Specifies the second X-coordinate of the region from which to generate the picking frustum. If the specified pixel coordinate is outside of the bounds of the display (less than 0 or greater than [`M_SIZE_X`](../../Reference/3ddisp/M3ddispInquire.md)), a warning is generated. 3D graphics outside of the bounds of the display are still detected and picked.

### `PixelY2` *(in, AIL_INT64)*

Specifies the second Y-coordinate of the region from which to generate the picking frustum. If the specified pixel coordinate is outside of the bounds of the display (less than 0 or greater than [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispInquire.md)), a warning is generated. 3D graphics outside of the bounds of the display are still detected and picked.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
