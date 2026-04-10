---
doctype: Reference
module: func
function: MfuncParamValue
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncParamValue"
---

# MfuncParamValue

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

> Read the value of the specified Aurora Imaging Library function parameter.

## Syntax

```c
void MfuncParamValue(
    AIL_ID  ContextFuncId,  //in
    AIL_INT ParamIndex,     //in
    void *  ParamValuePtr   //out
)
```

## Description

This function allows you to read the value of a parameter registered in the caller function of your user-defined Aurora Imaging Library function, from the callee function. The requested value is written to the address specified by the [`ParamValuePtr`](../../Reference/func/MfuncParamValue.md) parameter.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function.

### `ParamIndex` *(in, AIL_INT)*

Specifies the index of the parameter within the user-defined Aurora Imaging Library function's parameter list. The index of the first parameter is considered to be one.

### `ParamValuePtr` *(out, *void)*

Specifies the address of the variable in which to write the value of the specified parameter. The type of the variable depends on the type of the parameter registered in the caller function.

## Return Value

**Type:** `void`

Returns the value of the specified parameter.
