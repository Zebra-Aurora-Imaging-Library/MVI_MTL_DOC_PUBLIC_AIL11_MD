---
doctype: Reference
module: app
function: MappHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappHookFunction"
---

# MappHookFunction

> Hook a function to an event.

## Syntax

```c
void MappHookFunction(
    AIL_ID                    ContextAppId,    //in
    AIL_INT                   HookType,        //in
    AIL_APP_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified application event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MappHookFunction`](../../Reference/app/MappHookFunction.md) for each function that you want to hook. These hooked functions are automatically chained and kept in an internal list. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list are executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, the order of the remaining functions in the list is preserved.

You can obtain information concerning the event, using [`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md), and take appropriate action before returning control to the application.

In multi-thread environments, an [`MappHookFunction`](../../Reference/app/MappHookFunction.md) call hooks or unhooks the function specified by [`HookHandlerPtr`](../../Reference/app/MappHookFunction.md) to all application threads running Aurora Imaging Library, unless specifically limited to the calling thread.

This function is typically used to trap errors that occur in an application, without checking every function execution with [`MappGetError`](../../Reference/app/MappGetError.md), or to detect the start or end of certain functions.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to use.

*For specifying the application context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappHookFunction.md). |

### `HookType` *(in, AIL_INT)*

Specifies the event type. This parameter can be set to one of the following values.

*For specifying the event type*

| Value | Description |
| --- | --- |
| `M_CALLEE_ERROR_CURRENT` | Calls the hook-handler function each time an error occurs in a user-defined function. Note that hook-handler functions attached to this type of event are called first before the error dialog occurs (if enabled using [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md)). It is also called before any error handling code within the user-defined function. |
| `M_ERROR_CURRENT` | Calls the hook-handler function each time an error occurs. Note that hook-handler functions attached to this type of event are called first before the error dialog occurs (if enabled using [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md)). |
| `M_ERROR_FATAL` | Calls the hook-handler function when a user presses the **Cancel** button in an error dialog. For this event to occur, [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md) must be set to [`M_PRINT_ENABLE`](../../Reference/app/MappControl.md). |
| `M_OBJECT_PUBLISH_DAIL` | Calls the hook-handler function when an object to be used with Distributed Aurora Imaging Library has its publication state modified. This event occurs when the publication state is modified using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_DAIL_PUBLISH`](../../Reference/obj/MobjControl.md), or when the published object is freed as its publication state was set to [`M_NO`](../../Reference/obj/MobjControl.md). |
| `M_TRACE_END` | Calls the hook-handler function at the end of each function.

When setting the [`HookType`](../../Reference/app/MappHookFunction.md) parameter to [`M_TRACE_END`](../../Reference/app/MappHookFunction.md), the only function that can be called in a hook function is [`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md), otherwise infinite recursion will occur. |
| `M_TRACE_START` | Calls the hook-handler function at the start of each function.

When setting the [`HookType`](../../Reference/app/MappHookFunction.md) parameter to [`M_TRACE_START`](../../Reference/app/MappHookFunction.md), the only function that can be called in a hook function is [`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md), otherwise infinite recursion will occur. |
| `M_WEB_CLIENT_CONNECTED` | Calls the hook-handler function when an Aurora Imaging Web Library client application connects to your application.

Before an Aurora Imaging Web Library client can connect to your application, you must enable Aurora Imaging Web Library functionality using [`MappControl`](../../Reference/app/MappControl.md) with [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md) and [`M_ENABLE`](../../Reference/app/MappControl.md). |
| `M_WEB_CLIENT_DISCONNECTED` | Calls the hook-handler function when an Aurora Imaging Web Library client application disconnects from your application. |

*For the HookType parameter*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/app/MappHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

*For hooking or unhooking the function to application threads*

| Value | Description |
| --- | --- |
| `M_THREAD_CURRENT` | Limits the hooking or unhooking of the function to the calling thread. For example, to call the hook-handler function only for errors occurring in the current thread, specify [`M_ERROR_CURRENT`](../../Reference/app/MappHookFunction.md) + [`M_THREAD_CURRENT`](../../Reference/app/MappHookFunction.md) as the [`HookType`](../../Reference/app/MappHookFunction.md) parameter. |

### `HookHandlerPtr` *(in, AIL_APP_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when an event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/app/MappHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required.
