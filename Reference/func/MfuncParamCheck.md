---
doctype: Reference
module: func
function: MfuncParamCheck
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncParamCheck"
---

# MfuncParamCheck

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

> Verify whether parameter checking is required.

## Syntax

```c
AIL_INT MfuncParamCheck(
    AIL_ID ContextFuncId  //in
)
```

## Description

This function allows you to verify, from within your user-defined Aurora Imaging Library function, whether parameter checking is enabled or disabled. To enable or disable the checking of parameters, use the [`MappControl`](../../Reference/app/MappControl.md)   [`M_PARAMETER`](../../Reference/app/MappControl.md)  control type.

Call the [`MfuncParamCheck`](../../Reference/func/MfuncParamCheck.md) function prior to checking the parameters of the specified user-defined Aurora Imaging Library function. The return value of the [`MfuncParamCheck`](../../Reference/func/MfuncParamCheck.md) function should dictate whether to execute the functions that perform the parameter checking, or not. Then, to save the parameter checking time for a time-critical user-defined Aurora Imaging Library function, it is sufficient to disable parameter checking using [`MappControl`](../../Reference/app/MappControl.md) with [`M_CHECK_DISABLE`](../../Reference/app/MappControl.md).

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if no parameter checking is required; otherwise, checking is required.
