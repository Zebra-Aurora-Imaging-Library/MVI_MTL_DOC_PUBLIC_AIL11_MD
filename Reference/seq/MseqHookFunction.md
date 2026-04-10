---
doctype: Reference
module: seq
function: MseqHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqHookFunction"
---

# MseqHookFunction

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

> Hook a function to a sequence event.

## Syntax

```c
void MseqHookFunction(
    AIL_ID                    ContextSeqId,    //in
    AIL_INT                   HookType,        //in
    AIL_SEQ_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified sequence event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain more information about the event from within the hook-handler function using [`MseqGetHookInfo`](../../Reference/seq/MseqGetHookInfo.md).

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `HookType` *(in, AIL_INT)*

Specifies the sequence event to which to hook the function. This parameter can be set to one of the following values:

*For specifying the sequence event to which to hook the function*

| Value | Description |
| --- | --- |
| `M_FRAME_END` | Hooks the function to the event that occurs when a frame has finished being processed. |

*For the HookType parameter*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/seq/MseqHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_SEQ_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/seq/MseqHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
