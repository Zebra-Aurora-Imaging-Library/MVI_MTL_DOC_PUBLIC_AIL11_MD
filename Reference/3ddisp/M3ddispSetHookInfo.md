---
doctype: Reference
module: 3ddisp
function: M3ddispSetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispSetHookInfo"
---

# M3ddispSetHookInfo

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

> Set information about an event that caused a hook-handler function to be called.

## Syntax

```c
void M3ddispSetHookInfo(
    AIL_ID     EventId,   //out
    AIL_INT64  InfoType,  //in
    AIL_DOUBLE InfoValue  //in
)
```

## Description

This function allows you to set information about the event that caused the hook-handler function to be called. [`M3ddispSetHookInfo`](../../Reference/3ddisp/M3ddispSetHookInfo.md) must only be called within the scope of a 3d display object hook-handler function (see [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md)).

## Parameters

### `EventId` *(out, AIL_ID)*

Specifies the display event identifier received by the hook-handler function. See [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md) for more information. The event will be of the type passed to the [`HookType`](../../Reference/3ddisp/M3ddispHookFunction.md) parameter of the hook-handler function.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to set.

### `InfoValue` *(in, AIL_DOUBLE)*

Specifies the required value for the type of information to set.

## Parameter Associations

### For setting whether the display will process any mouse or keyboard input events

To set information about the event that caused the hook-handler function to be called, the [`InfoType`](../../Reference/3ddisp/M3ddispSetHookInfo.md) and corresponding [`InfoValue`](../../Reference/3ddisp/M3ddispSetHookInfo.md) parameter settings can be set to one of the following values. These settings are only available if the hook-handler function was called due to mouse or keyboard input event ([`M_KEY_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_KEY_UP`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_KEY_CHAR`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_LEFT_BUTTON_UP`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_LEFT_DOUBLE_CLICK`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_RIGHT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_RIGHT_BUTTON_UP`](../../Reference/3ddisp/M3ddispHookFunction.md) [`M_MOUSE_MIDDLE_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md) [`M_MOUSE_MIDDLE_BUTTON_UP`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_WHEEL`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_MOVE`](../../Reference/3ddisp/M3ddispHookFunction.md)).

---

### `M_CONSUMED`

Sets whether this callback will prevent the display from processes a mouse or keyboard input event after the hook-handler function has returned.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that the display will process the mouse or keyboard input event after the hook-handler function has returned. The callback will not consume the mouse or keyboard input event. |
| `M_NO_CLEAR` | Specifies that for mouse button down events ([`M_MOUSE_LEFT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md),[`M_MOUSE_RIGHT_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md), [`M_MOUSE_MIDDLE_BUTTON_DOWN`](../../Reference/3ddisp/M3ddispHookFunction.md)), the display's view can still be moved by moving the mouse after the mouse button down event. However, the initial click will not trigger the rotation indicator. Similar to [`M_TRUE`](../../Reference/3ddisp/M3ddispSetHookInfo.md) but after moving the mouse the rotation indicator will also trigger.  For example, by clicking on a point, you can consume the input event and display the coordinates of the pixel, and by moving the mouse you can simultaneously trigger the rotation indicator and move the view on the display.  > **Note:** Note that this only applies when [`M_ROTATION_INDICATOR`](../../Reference/3ddisp/M3ddispControl.md) is set [`M_ENABLE_ON_MOUSE_CLICK`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_TRUE` | Specifies that the display will not process the mouse or keyboard input event after the hook-handler function has returned. The callback will consume the mouse or keyboard input event. |
