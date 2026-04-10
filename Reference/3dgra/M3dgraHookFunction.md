---
doctype: Reference
module: 3dgra
function: M3dgraHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraHookFunction"
---

# M3dgraHookFunction

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

> Hook a function to a 3D graphics list event.

## Syntax

```c
void M3dgraHookFunction(
    AIL_ID                      List3dgraId,     //in
    AIL_INT                     HookType,        //in
    AIL_3DGRA_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                      UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified 3D graphics list event (for example, a modification to a 3D graphic in a 3D graphics list). Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You must not make any blocking calls to a 3D display showing the 3D graphics list, unless the hook-handler function is called as a result of interactive editing from the 3D display.

## Parameters

### `List3dgraId` *(in, AIL_ID)*

Specifies the identifier of the 3D graphics list to which to hook a function. The 3D graphics list must have been previously allocated on the required system using [`M3dgraAlloc`](../../Reference/3dgra/M3dgraAlloc.md), or you can specify the identifier of the 3D display's internal graphics list (inquired using [`M3ddispInquire`](../../Reference/3ddisp/M3ddispInquire.md) with [`M_3D_GRAPHIC_LIST_ID`](../../Reference/3ddisp/M3ddispInquire.md)).

### `HookType` *(in, AIL_INT)*

Specifies the 3D graphics list event to which to hook the function. This parameter can be set to one of the following values:

*For specifying the 3D graphics list event*

| Value | Description |
| --- | --- |
| `M_EDITABLE_GRAPHIC_MODIFIED` | Calls the hook-handler function each time a graphic is interactively modified in the specified 3D graphics list. |
| `M_HANDLE_GRAPHIC_CLICKED` | Calls the hook-handler function each time the handle graphic is clicked. The graphic can be a handle or a target of another graphic that is a handle. |
| `M_HANDLE_GRAPHIC_ROTATED` | Calls the hook-handler function each time the handle graphic is rotated. The graphic can be a handle or a target of another graphic that is a handle. |
| `M_HANDLE_GRAPHIC_TRANSLATED` | Calls the hook-handler function each time the handle graphic is translated in any direction. The graphic can be a handle or a target of another graphic that is a handle. |
| `M_MODEL_REMOVE` | Calls the hook-handler function each time a model is removed from the specified 3D graphics list. a model can be removed with [`M3dgraRemove`](../../Reference/3dgra/M3dgraRemove.md) or [`M3ddispSelect`](../../Reference/3ddisp/M3ddispSelect.md) with [`M_SELECT`](../../Reference/3ddisp/M3ddispSelect.md) or [`M_REMOVE`](../../Reference/3ddisp/M3ddispSelect.md). |

*For detaching the hook-handler function*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/3dgra/M3dgraHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_3DGRA_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/3dgra/M3dgraHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
