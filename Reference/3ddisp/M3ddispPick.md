---
doctype: Reference
module: 3ddisp
function: M3ddispPick
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispPick"
---

# M3ddispPick

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

> Pick a 3D graphic at a given pixel in a 3D display.

## Syntax

```c
void M3ddispPick(
    AIL_ID    Context3ddispId,  //in
    AIL_ID    Disp3dId,         //in
    AIL_ID    Result3ddispId,   //out
    AIL_INT64 PixelX,           //in
    AIL_INT64 PixelY,           //in
    AIL_INT64 ControlFlag       //in
)
```

## Description

This function identifies which 3D graphic (if any) is at a specific position in the 3D display and writes the results in the specified result buffer. You can retrieve results using [`M3ddispGetResult`](../../Reference/3ddisp/M3ddispGetResult.md). Aurora Imaging Library identifies the 3D graphic by casting a ray from the 3D display's viewpoint towards the given pixel and detecting with which 3D graphic that the ray intersects. Note that when there is more than one 3D graphic at a given pixel, the detected 3D graphic is the one that appears in front. If the 3D graphics are on the same layer, the 3D graphic closest to the viewpoint is detected. If the 3D graphics are on different layers, the 3D graphic on the highest layer is detected, regardless of its position in the world.

Use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) to configure the settings of the picking context before calling this function. You can filter out 3D graphics that you don't want to pick. For example, you can use [`M_GRAPHIC_TYPE`](../../Reference/3ddisp/M3ddispControl.md) to only detect 3D graphics of a certain type. If a 3D graphic of the specified type is behind a filtered out 3D graphic (either on a lower layer or further away on the same layer), the ray will pass through the filtered out 3D graphic and [`M3ddispPick`](../../Reference/3ddisp/M3ddispPick.md) will detect the specified graphic. You can set [`M_BLOCKABLE`](../../Reference/3ddisp/M3ddispControl.md) to [`M_ENABLE`](../../Reference/3ddisp/M3ddispControl.md) to allow the filtered out 3D graphic to block the ray.

This function is useful for identifying which 3D graphic was clicked on. To do so, you can set [`PixelX`](../../Reference/3ddisp/M3ddispPick.md) and [`PixelY`](../../Reference/3ddisp/M3ddispPick.md) to the X- and Y-positions of your cursor ([`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) with [`M_MOUSE_POSITION_X`](../../Reference/3ddisp/M3ddispGetHookInfo.md) and [`M_MOUSE_POSITION_Y`](../../Reference/3ddisp/M3ddispGetHookInfo.md)) when an [`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md) event is triggered.

## Parameters

### `Context3ddispId` *(in, AIL_ID)*

Specifies the identifier of the picking context. The picking context must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md) with [`M_PICKING_CONTEXT`](../../Reference/3ddisp/M3ddispAlloc.md).

### `Disp3dId` *(in, AIL_ID)*

Specifies the identifier of the 3D display. The 3D display must have been previously allocated using [`M3ddispAlloc`](../../Reference/3ddisp/M3ddispAlloc.md).

### `Result3ddispId` *(out, AIL_ID)*

Specifies the identifier of the picking result buffer in which to write the results. The picking result buffer must have been previously allocated using [`M3ddispAllocResult`](../../Reference/3ddisp/M3ddispAllocResult.md) with [`M_PICKING_RESULT`](../../Reference/3ddisp/M3ddispAllocResult.md).

### `PixelX` *(in, AIL_INT64)*

Specifies the X-coordinate of the pixel through which to cast the ray. If the specified pixel coordinate is greater than the 3D display's current [`M_SIZE_X`](../../Reference/3ddisp/M3ddispInquire.md), an error is generated.

### `PixelY` *(in, AIL_INT64)*

Specifies the Y-coordinate of the pixel through which to cast the ray. If the specified pixel coordinate is greater than the 3D display's current [`M_SIZE_Y`](../../Reference/3ddisp/M3ddispInquire.md), an error is generated.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
