---
doctype: Reference
module: obj
function: MobjHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjHookFunction"
---

# MobjHookFunction

> Hook a function to an object-related event.

## Syntax

```c
void MobjHookFunction(
    AIL_ID                    ObjectId,        //out
    AIL_INT                   HookType,        //in
    AIL_OBJ_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified object-related event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain more information about the event from within the hook-handler function using [`MobjGetHookInfo`](../../Reference/obj/MobjGetHookInfo.md).

## Parameters

### `ObjectId` *(out, AIL_ID)*

Specifies the identifier of the object on which to hook a function.

### `HookType` *(in, AIL_INT)*

Specifies the object-related event on which to hook the function. This parameter can be set to one of the following values.

*For specifying the object-related event to hook*

| Value | Description |
| --- | --- |
| `M_OBJECT_FREE` | Hooks the function to the event that occurs when an object is freed. |

*For specifying the message mailbox event to hook*

| Value | Description |
| --- | --- |
| `M_MESSAGE_RECEIVED` | Hooks the function to the event that occurs when a message is received by the message mailbox. |
| `M_READ_TIMEOUT` | Hooks the function to the event that occurs when a read operation times out. |
| `M_WRITE_TIMEOUT` | Hooks the function to the event that occurs when a write operation times out. |

*For specifying that the function should be unhooked*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/obj/MobjHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_OBJ_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/obj/MobjHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
