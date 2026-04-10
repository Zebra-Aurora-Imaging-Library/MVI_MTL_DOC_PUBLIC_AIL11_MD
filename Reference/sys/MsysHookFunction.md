---
doctype: Reference
module: sys
function: MsysHookFunction
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysHookFunction"
---

# MsysHookFunction

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | Partial |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | Partial |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Hook a function to a system event.

## Syntax

```c
void MsysHookFunction(
    AIL_ID                    SysId,           //in
    AIL_INT                   HookType,        //in
    AIL_SYS_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                    UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a specified system event. Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MsysHookFunction`](../../Reference/sys/MsysHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain more information about the event from within the hook-handler function using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md).

Note that functions hooked to an event execute on a distinct thread. This permits the functions to run concurrently from the operation that fired the event and from functions hooked to other events. Although there is a small queue to permit a certain amount of overlap, hooked functions should not take longer to execute than the period in which two of their associated events can occur. You cannot determine the instance of the event that fired the function.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to hook a function.

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `HookType` *(in, AIL_INT)*

Specifies the system event to which to hook the function. This parameter can be set to one of the following values.

*For specifying the system event to hook*

| Value | Description |
| --- | --- |
| `M_CAMERA_PRESENT` | Hooks the function to the presence of the camera. |
| `M_FEATURE_CHANGE` | Hooks the function to the event that occurs when the value of a GenICam feature changes.

To enable an event to occur when the value of a specific feature changes, use [`MsysControlFeature`](../../Reference/sys/MsysControlFeature.md) with [`M_FEATURE_CHANGE_HOOK`](../../Reference/sys/MsysControlFeature.md) set to [`M_ENABLE`](../../Reference/sys/MsysControlFeature.md). Repeat for each feature for which you want to enable a feature change event. To enable an event to occur when the value of any feature changes, use [`M_FEATURE_CHANGE`](../../Reference/sys/MsysHookFunction.md) + [`M_ALL`](../../Reference/sys/MsysHookFunction.md). This setting is only available for hooking to an image buffer.

To retrieve the name of the feature that caused the event, use [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_GC_FEATURE_CHANGE_NAME`](../../Reference/sys/MsysGetHookInfo.md). |
| `M_IO_CHANGE` | Hooks the function to the event that occurs when an I/O signal changes in accordance with its specified interrupt mode, set using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_IO_INTERRUPT_ACTIVATION`](../../Reference/sys/MsysControl.md). Within the scope of the hook-handler function, it is prudent to verify that the appropriate I/O signal triggered the hook-handler function, using [`MsysGetHookInfo`](../../Reference/sys/MsysGetHookInfo.md) with [`M_IO_INTERRUPT_SOURCE`](../../Reference/sys/MsysGetHookInfo.md).

If more than one input signal triggers an interrupt at the same time, the hook-handler function (or chain of hook-handler functions) is called for each input signal that generated an interrupt. As a result, any and all user-specified function(s) hooked using this hook type will be executed for each interrupt. |
| `M_TIMER_END` | Hooks the function to the event that occurs when the specified timer completes. |
| `M_TIMER_START` | Hooks the function to the event that occurs when the specified timer starts its active period (after the timer delay). |
| `M_UART_DATA_RECEIVED` | Hooks the function to the event that occurs when the specified UART receives data. Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_UART_PRESENT`](../../Reference/sys/MsysInquire.md) to return the number of UARTs available.

Note that this value can be used in combination; see below. |

*For specifying to hook on a specific feature change event associated with a GenTL configuration file*

| Value | Description |
| --- | --- |
| `M_GENTL_INTERFACE_NUMBER` | Specifies that the hook will occur on a feature change event for a feature associated with a specific instance of the GenTL interface configuration file (XML file). |
| `M_ALL` | Specifies that the hook will occur on all feature change events. |
| `M_GENTL_SYSTEM` | Specifies that the hook will occur on a feature change event of a feature associated with the GenTL system configuration file (XML file). |

*For specifying the UART upon which the system event occurs*

| Value | Description |
| --- | --- |
| `M_UART_NB` | Specifies the UART upon which the system event occurs.

Use [`MsysInquire`](../../Reference/sys/MsysInquire.md) with [`M_UART_PRESENT`](../../Reference/sys/MsysInquire.md) to determine the number of UARTS on the system. |

*For specifying the timer to which to hook the function*

| Value | Description |
| --- | --- |
| `M_TIMERn` | Specifies the timer to hook. |

*For specifying that the function should be unhooked*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/sys/MsysHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_SYS_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when the specified event occurs. The hook-handler function must be declared as follows:

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/sys/MsysHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if not used.

This constant is only available on a Host system if the Host system was previously allocated on a Zebra 4Sight EV6/EV7. It is not available on Zebra 4Sight XV6/XV7.
