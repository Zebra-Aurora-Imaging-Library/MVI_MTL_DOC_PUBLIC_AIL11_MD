---
doctype: Reference
module: 3ddisp
function: M3ddispHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3ddisp / M3ddispHookFunction"
---

# M3ddispHookFunction

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

> Hook a function to a 3D display event.

## Syntax

```c
void M3ddispHookFunction(
    AIL_ID                       Disp3dId,        //out
    AIL_INT64                    HookType,        //in
    AIL_3DDISP_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                       UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified 3D display event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

Note that if a mouse or keyboard event is also associated with user interaction, the hook-handler function is called before processing the user interaction. You can use [`M3ddispSetHookInfo`](../../Reference/3ddisp/M3ddispSetHookInfo.md) with [`M_CONSUMED`](../../Reference/3ddisp/M3ddispSetHookInfo.md) set to [`M_TRUE`](../../Reference/3ddisp/M3ddispSetHookInfo.md) to prevent the user interaction from being processed entirely.

You can hook more than one function to an event by making separate calls to [`M3ddispHookFunction`](../../Reference/3ddisp/M3ddispHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

Hook functions are called from the 3D display's internal thread; as such, any expensive or blocking task can freeze the 3D display. All 3D display and 3D graphics functions are available from within the callback and will not race with anything for this 3D display; however, controlling or inquiring another 3D display might be a blocking call in some cases (in particular, all calls to [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) and [`M3ddispGetView`](../../Reference/3ddisp/M3ddispGetView.md) are blocking calls). You should not use hook functions with two 3D displays that might try to inquire each other at the same time. For example, 3D display A should not be trying to get the [`M_VIEWPOINT`](../../Reference/3ddisp/M3ddispGetView.md) of 3D display B while 3D display B is trying to copy the [`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispCopy.md) of 3D display A (this would deadlock the two displays).

## Parameters

### `Disp3dId` *(out, AIL_ID)*

Specifies the identifier of the target 3D display.

### `HookType` *(in, AIL_INT64)*

Specifies the 3D display event type. This parameter can be set to the following:

*For specifying the 3D display event type*

| Value | Description |
| --- | --- |
| `M_ENTER_SAFE_MODE` | Calls the hook-handler function when an unrecoverable error is encountered and the 3D display enters safe mode. When in safe mode, all calls to the 3D Display module will generate an error (except [`M3ddispGetHookInfo`](../../Reference/3ddisp/M3ddispGetHookInfo.md) and [`M3ddispFree`](../../Reference/3ddisp/M3ddispFree.md)). When this event occurs, you should save your work and restart the application.

Note that although all 3D displays will enter safe mode, only the 3D display that encountered the error will generate this event. |
| `M_FOV_CHANGE` | Calls the hook-handler function each time the 3D display's field of view (FoV) changes. This includes setting the projection mode ([`M_PROJECTION_MODE`](../../Reference/3ddisp/M3ddispControl.md)), setting the FoV angle in [`M_PERSPECTIVE`](../../Reference/3ddisp/M3ddispControl.md) or [`M_PARALLEL`](../../Reference/3ddisp/M3ddispControl.md), setting the parallel scale in [`M_PARALLEL_PRECISE`](../../Reference/3ddisp/M3ddispControl.md), changing the distance in [`M_PARALLEL`](../../Reference/3ddisp/M3ddispControl.md), or changing the window size.

Note that the hook function will typically be called right before the next render after the field of view changes. To guarantee that the 3D display is up to date, you can force a render, using [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_UPDATE`](../../Reference/3ddisp/M3ddispControl.md) set to [`M_NOW`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_FRAMETIME_HIGH` | Calls the hook-handler function when the frametime goes above [`M_FRAMETIME_THRESHOLD_HIGH`](../../Reference/3ddisp/M3ddispControl.md).

Note that this event only occurs when the frametime first passes the threshold. This is reset whenever a new function is hooked to this event, any control related to frametime averaging is changed, or the frametime goes below [`M_FRAMETIME_THRESHOLD_LOW`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_FRAMETIME_LOW` | Calls the hook-handler function when the frametime goes below [`M_FRAMETIME_THRESHOLD_LOW`](../../Reference/3ddisp/M3ddispControl.md).

Note that this event only occurs when the frametime first passes the threshold. This is reset whenever a new function is hooked to this event, any control related to frametime averaging is changed, or the frametime goes above [`M_FRAMETIME_THRESHOLD_HIGH`](../../Reference/3ddisp/M3ddispControl.md). |
| `M_KEY_CHAR` | Calls the hook-handler function each time the user presses a character key, while the mouse cursor is in the 3D display. |
| `M_KEY_DOWN` | Calls the hook-handler function each time the user presses a keyboard key, while the mouse cursor is in the 3D display. |
| `M_KEY_UP` | Calls the hook-handler function each time the user releases a keyboard key, while the mouse cursor is in the 3D display. |
| `M_MOUSE_LEFT_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the left mouse button, while the mouse cursor is in the 3D display. |
| `M_MOUSE_LEFT_BUTTON_UP` | Calls the hook-handler function each time the user releases the left mouse button, while the mouse cursor is in the 3D display. |
| `M_MOUSE_LEFT_DOUBLE_CLICK` | Calls the hook-handler function each time the user double-clicks the left mouse button, while the mouse cursor is in the 3D display.

Note that [`M_MOUSE_LEFT_DOUBLE_CLICK`](../../Reference/3ddisp/M3ddispHookFunction.md) might not work with Aurora Imaging Web Library displays. |
| `M_MOUSE_MIDDLE_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the middle mouse button, while the mouse cursor is in the 3D display. |
| `M_MOUSE_MIDDLE_BUTTON_UP` | Calls the hook-handler function each time the user releases the middle mouse button, while the mouse cursor is in the 3D display. |
| `M_MOUSE_MOVE` | Calls the hook-handler function each time the mouse cursor moves, while the moue cursor is in the 3D display. |
| `M_MOUSE_RIGHT_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the right mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_RIGHT_BUTTON_UP` | Calls the hook-handler function each time the user releases the right mouse button, while the mouse cursor is in the 3D display. |
| `M_MOUSE_WHEEL` | Calls the hook-handler function each time the user scrolls the mouse wheel (up or down), while the mouse cursor is in the display. |
| `M_VIEW_CHANGE` | Calls the hook-handler function each time the view of the 3D display changes. This event occurs when the position or orientation of the view is modified, either using [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md), or through direct user interaction with the 3D display using the keyboard and mouse. If several changes are made to the view sequentially, there is no guarantee that this event will be generated for every intermediate change; if there is a single callback, it will be associated with the final view change.

This event is not generated for a field of view change (such as resizing the window). See [`M_FOV_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md) instead.

[`M_VIEW_CHANGE`](../../Reference/3ddisp/M3ddispHookFunction.md) is not recursive; calling [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) for this 3D display from within the callback will not generate more events.

This event can be triggered even if no actual modification occurred. For example, if you attempt to set the view to the same position and orientation as the current view (using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md) with [`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispCopy.md) to store the current transformation matrix of the view, and then setting the view to match that transformation matrix, using [`M3ddispCopy`](../../Reference/3ddisp/M3ddispCopy.md) or [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md) with [`M_VIEW_MATRIX`](../../Reference/3ddisp/M3ddispSetView.md)), there is a chance the hook function will be called again, even though the view did not visibly change.

Note that when calling [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md), it is not guaranteed that the view change event will have been generated yet when the function returns. If you need to guarantee that the view change event has been generated, use [`M3ddispControl`](../../Reference/3ddisp/M3ddispControl.md) with [`M_SYNCHRONIZE`](../../Reference/3ddisp/M3ddispControl.md) after the call to [`M3ddispSetView`](../../Reference/3ddisp/M3ddispSetView.md). |
| `M_WINDOW_RESIZED` | Calls the hook-handler function each time the window is resized. |

*For detaching the hook-handler function*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/3ddisp/M3ddispHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_3DDISP_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function, pointed to by [`HookHandlerPtr`](../../Reference/3ddisp/M3ddispHookFunction.md), must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/3ddisp/M3ddispHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_DEFAULT` if not used.
