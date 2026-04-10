---
doctype: Reference
module: dlocr
function: MdlocrHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrHookFunction"
---

# MdlocrHookFunction

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

> Hook a function to an event that occurs using a Deep Learning OCR fine-tuning context.

## Syntax

```c
void MdlocrHookFunction(
    AIL_ID                      ContextDlocrId,  //in
    AIL_INT64                   HookType,        //in
    AIL_DLOCR_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                      UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a Deep Learning OCR fine-tuning event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MdlocrHookFunction`](../../Reference/dlocr/MdlocrHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain information concerning the event, using [`MdlocrGetHookInfo`](../../Reference/dlocr/MdlocrGetHookInfo.md), and take appropriate action before returning control to the application.

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR fine-tuning context onto which to attach a hook-handler. Set this parameter to one of the following values.

### `HookType` *(in, AIL_INT64)*

Specifies the event type. Set this parameter to one of the following values.

*For specifying the event type*

| Value | Description |
| --- | --- |
| `M_FINETUNE_PROGRESS` | Calls the hook-handler function each time progress is made during an [`MdlocrFinetune`](../../Reference/dlocr/MdlocrFinetune.md) operation. |

*For the HookType parameter*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/dlocr/MdlocrHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_DLOCR_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when an event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/dlocr/MdlocrHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required.
