---
doctype: Reference
module: buf
function: MbufHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufHookFunction"
---

# MbufHookFunction

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Yes |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Hook a function to a buffer or container event.

## Syntax

```c
void MbufHookFunction(
    AIL_ID                    ContainerOrBufId,  //in
    AIL_INT                   HookType,          //in
    AIL_BUF_HOOK_FUNCTION_PTR HookHandlerPtr,    //in
    void *                    UserDataPtr        //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified buffer or container event (for example, a modification to the buffer's contents). Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

## Parameters

### `ContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the buffer or container.

### `HookType` *(in, AIL_INT)*

Specifies the buffer event to which to hook the function. This parameter can be set to one of the following values:

*For specifying the buffer event*

| Value | Description |
| --- | --- |
| `M_FEATURE_CHANGE` | Hooks the hook-handler function to the event that occurs when the value of a GenICam feature changes.

To enable an event to occur when the value of a specific feature changes, use [`MbufControlFeature`](../../Reference/buf/MbufControlFeature.md) with [`M_FEATURE_CHANGE_HOOK`](../../Reference/buf/MbufControlFeature.md) set to [`M_ENABLE`](../../Reference/buf/MbufControlFeature.md). Repeat for each feature that you want to enable a feature change event. To enable an event to occur when the value of any feature changes, use [`M_FEATURE_CHANGE`](../../Reference/buf/MbufHookFunction.md) + [`M_ALL`](../../Reference/buf/MbufHookFunction.md). This setting is only available for hooking to an image buffer.

To retrieve the name of the feature that caused the event, use [`MbufGetHookInfo`](../../Reference/buf/MbufGetHookInfo.md) with [`M_GC_FEATURE_CHANGE_NAME`](../../Reference/buf/MbufGetHookInfo.md). |
| `M_LAYOUT_MODIFIED` | Calls the hook-handler function when at least 1 component of a container has been added or removed. The hook-handler function is called once at the end of a function regardless of the number of changes. |
| `M_MODIFIED_BUFFER` | Calls the hook-handler function at the end of any Aurora Imaging Library function that modifies the specified buffer or container.

> **Note:** For a container, this event will occur any time the container is used a destination for an Aurora Imaging Library function (unless none of the container's components is modified), any time a component is added to or removed from the container, as well as any time one of the container's components is modified directly using its Aurora Imaging Library identifier. The hook-handler function will only be called once per call to an Aurora Imaging Library function, unless [`M_ON_COMPONENT`](../../Reference/buf/MbufHookFunction.md)is specified. |

*For hooking the function to changes in components*

| Value | Description |
| --- | --- |
| `M_ON_COMPONENT` | Specifies to hook to changes in each component of the container individually, instead of to the container as a whole. When a component of the container changes, the hook function will be called as though it were hooked to that component. Using[`MbufGetHookInfo`](../../Reference/buf/MbufGetHookInfo.md)with [`M_BUFFER_ID`](../../Reference/buf/MbufGetHookInfo.md) within the hook-handler function will return the buffer identifier of the component. |

*For*

| Value | Description |
| --- | --- |
| `M_ALL` | Specifies to hook the specifies function to all feature change events. |

*For specifying that the function should be unhooked*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/buf/MbufHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_BUF_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/buf/MbufHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.
