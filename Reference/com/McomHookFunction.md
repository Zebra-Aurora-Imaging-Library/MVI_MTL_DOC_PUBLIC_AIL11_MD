---
doctype: Reference
module: com
function: McomHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / com / McomHookFunction"
---

# McomHookFunction

> Hooks a function to a communication event.

## Syntax

```c
void McomHookFunction(
    AIL_ID                    ComId,           //in
    AIL_INT                   HookType,        //in
    AIL_COM_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified communication event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs. The events that can be hooked represent actions by the PLC or robot in the Aurora Imaging Library application.

You can hook more than one function to an event by making separate calls to [`McomHookFunction`](../../Reference/com/McomHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain more information about the event from within the hook-handler function using [`McomGetHookInfo`](../../Reference/com/McomGetHookInfo.md).

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the industrial communication context. The context must have been allocated using [`McomAlloc`](../../Reference/com/McomAlloc.md) with [`M_COM_PROTOCOL_PROFINET`](../../Reference/com/McomAlloc.md).

### `HookType` *(in, AIL_INT)*

Specifies the data change event to which to hook the function.

*For specifying the data change event to hook*

| Value | Description |
| --- | --- |
| `M_COM_DATA_CHANGE` | Calls the hook-handler function when a change has been made to an output module. Note that this type of event is only supported when using the PROFINET protocol. |
| `M_COM_ERROR_TRIGGER` | Calls the hook-handler function when an Aurora Imaging Library communication error occurs. |

*For specifying that the function should be hooked or unhooked*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/com/McomHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_COM_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/com/McomHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
