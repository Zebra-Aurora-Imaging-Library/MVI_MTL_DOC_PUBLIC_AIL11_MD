---
doctype: Reference
module: disp
function: MdispHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / disp / MdispHookFunction"
---

# MdispHookFunction

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

> Hook a function to a display event.

## Syntax

```c
void MdispHookFunction(
    AIL_ID                     DisplayId,       //out
    AIL_INT                    HookType,        //in
    AIL_DISP_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                     UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified display event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MdispHookFunction`](../../Reference/disp/MdispHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

Note that this function is not available on network displays.

## Parameters

### `DisplayId` *(out, AIL_ID)*

Specifies the identifier of the target display.

### `HookType` *(in, AIL_INT)*

Specifies the display event type. This parameter can be set to the following:

*For specifying the display event type*

| Value | Description |
| --- | --- |
| `M_KEY_CHAR` | Calls the hook-handler function each time the user presses a character key, while the mouse cursor is in the display. |
| `M_KEY_DOWN` | Calls the hook-handler function each time the user presses a keyboard key, while the mouse cursor is in the display. |
| `M_KEY_UP` | Calls the hook-handler function each time the user releases a keyboard key, while the mouse cursor is in the display. |
| `M_MOUSE_LEFT_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the left mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_LEFT_BUTTON_UP` | Calls the hook-handler function each time the user releases the left mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_LEFT_DOUBLE_CLICK` | Calls the hook-handler function each time the user double-clicks the left mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_MIDDLE_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the middle mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_MIDDLE_BUTTON_UP` | Calls the hook-handler function each time the user releases the middle mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_MOVE` | Calls the hook-handler function each time the mouse cursor moves, while in the display. |
| `M_MOUSE_RIGHT_BUTTON_DOWN` | Calls the hook-handler function each time the user clicks the right mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_RIGHT_BUTTON_UP` | Calls the hook-handler function each time the user releases the right mouse button, while the mouse cursor is in the display. |
| `M_MOUSE_WHEEL` | Calls the hook-handler function each time the user scrolls the mouse wheel (up or down), while the mouse cursor is in the display. |

*For detaching the hook-handler function*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/disp/MdispHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_DISP_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function, pointed to by [`HookHandlerPtr`](../../Reference/disp/MdispHookFunction.md), must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/disp/MdispHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
