---
doctype: Reference
module: gra
function: MgraHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gra / MgraHookFunction"
---

# MgraHookFunction

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

> Hook a function to a 2D graphics list event.

## Syntax

```c
void MgraHookFunction(
    AIL_ID                    GraListId,       //in
    AIL_INT                   HookType,        //in
    AIL_GRA_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified 2D graphics list event (for example, a modification to the 2D graphics list's contents). Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MgraHookFunction`](../../Reference/gra/MgraHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

> **Note:** Unlike most other functions that modify an Aurora Imaging Library object, you can call this function concurrently from multiple threads on the same Aurora Imaging Library 2D graphics list ([`GraListId`](../../Reference/gra/MgraHookFunction.md)) without using an [`M_MUTEX`](../../Reference/thr/MthrAlloc.md) object, as long as all the other parameters of the concurrent calls do not also share data.

## Parameters

### `GraListId` *(in, AIL_ID)*

Specifies the identifier of the 2D graphics list to which to hook a function. The 2D graphics list must have been previously allocated on the required system using [`MgraAllocList`](../../Reference/gra/MgraAllocList.md).

### `HookType` *(in, AIL_INT)*

Specifies the 2D graphics list event to which to hook the function. This parameter can be set to one of the following values:

*For specifying the 2D graphics list event*

| Value | Description |
| --- | --- |
| `M_GRAPHIC_LIST_MODIFIED` | Calls the hook-handler function each time the specified 2D graphics list is modified in any way, either by an Aurora Imaging Library function or by interactive manipulations. Any of the other events will also trigger this event, and it will always be the last event to be triggered. |
| `M_GRAPHIC_MODIFIED` | Calls the hook-handler function each time the specified 2D graphics list's graphics are modified by an Aurora Imaging Library function or by interactive manipulations. This includes the creation of a new graphic (for example, using [`MgraLine`](../../Reference/gra/MgraLine.md) or by interactive manipulations), its deletion (using [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_DELETE`](../../Reference/gra/MgraControlList.md) or [`MgraClear`](../../Reference/gra/MgraClear.md)) and any modification to a graphic using [`MgraControlList`](../../Reference/gra/MgraControlList.md), except for changing the selection ([`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_GRAPHIC_SELECTED`](../../Reference/gra/MgraControlList.md)).

Note that this event can be triggered even if no modification occurred. For example, calling [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_POSITION_X`](../../Reference/gra/MgraControlList.md) and passing the current value returned by [`MgraInquireList`](../../Reference/gra/MgraInquireList.md) with [`M_POSITION_X`](../../Reference/gra/MgraInquireList.md) will not modify the graphic, but will still trigger the event. |
| `M_GRAPHIC_SELECTION_MODIFIED` | Calls the hook-handler function each time a graphic is selected or deselected, either by an Aurora Imaging Library function or by interactive manipulations. |
| `M_INTERACTIVE_GRAPHIC_STATE_MODIFIED` | Calls the hook-handler function each time the specified 2D graphics list's interactive state is modified by an Aurora Imaging Library function or by interactive manipulations. |

*For detaching the hook-handler function*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/gra/MgraHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_GRA_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/gra/MgraHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
