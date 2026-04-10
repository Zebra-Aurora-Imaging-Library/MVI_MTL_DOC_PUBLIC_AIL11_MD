---
doctype: Reference
module: func
function: MfuncCall
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncCall"
---

# MfuncCall

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

> Execute the callee function.

## Syntax

```c
AIL_INT MfuncCall(
    AIL_ID ContextFuncId  //in
)
```

## Description

This function executes the callee function of the user-defined Aurora Imaging Library function on the target system. The target system is determined by the values of the user-defined Aurora Imaging Library function parameters, registered in the caller function. (For more information, see [Caller/callee dynamics on a remote system](../../UserGuide/C68_The_AIL_function_development_module/S07_Controller_worker_dynamics_on_a_remote_system.md).) Call [`MfuncCall`](../../Reference/func/MfuncCall.md) from the caller function of the user-defined Aurora Imaging Library function. Note that you must call [`MfuncCall`](../../Reference/func/MfuncCall.md) from the same thread as [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) and [`MfuncFree`](../../Reference/func/MfuncFree.md). If tracing and error reporting are enabled, the tracing and error messages will be reported to screen. You can control the error handling and tracing behavior as you would with other Aurora Imaging Library functions, using [`MappControl`](../../Reference/app/MappControl.md).

Note that if an AIL_ID parameter was registered in the caller function with [`MfuncParamAilId`](../../Reference/MfuncParamAilId.md), the validity of that identifier will be checked during the execution of [`MfuncCall`](../../Reference/func/MfuncCall.md). If the identifier is not valid, the callee function is not executed.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if an error occurred; otherwise, not null.
