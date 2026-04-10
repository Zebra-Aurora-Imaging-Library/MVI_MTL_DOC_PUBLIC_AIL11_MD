---
doctype: Reference
module: func
function: MfuncFreeId
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncFreeId"
---

# MfuncFreeId

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

> Free the Aurora Imaging Library identifier associated with a user-defined Aurora Imaging Library object.

## Syntax

```c
void MfuncFreeId(
    AIL_ID ContextFuncId,    //in
    AIL_ID UserObjectFuncId  //in
)
```

## Description

This function frees an Aurora Imaging Library identifier associated with a user-defined object using the [`MfuncAllocId`](../../Reference/func/MfuncAllocId.md) function.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies an Aurora Imaging Library function identifier. This parameter can be set to the following values:

*For the Aurora Imaging Library function identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default Aurora Imaging Library function identifier. Use [`M_DEFAULT`](../../Reference/func/MfuncFreeId.md) to free user-defined Aurora Imaging Library objects created outside of any particular user-defined Aurora Imaging Library function. |
| `Function identifier` | Specifies the identifier of a user-defined Aurora Imaging Library function. |

### `UserObjectFuncId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library identifier of the user-defined Aurora Imaging Library object to free.
