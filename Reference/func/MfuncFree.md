---
doctype: Reference
module: func
function: MfuncFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / func / MfuncFree"
---

# MfuncFree

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

> Free an Aurora Imaging Library function context.

## Syntax

```c
void MfuncFree(
    AIL_ID ContextFuncId  //in
)
```

## Description

This function frees the function context allocated for a user-defined Aurora Imaging Library function. [`MfuncFree`](../../Reference/func/MfuncFree.md) must be the last Aurora Imaging Library function called in the caller function. Note that you must call [`MfuncFree`](../../Reference/func/MfuncFree.md) from the same thread as [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md) and [`MfuncCall`](../../Reference/func/MfuncCall.md).

All Aurora Imaging Library function contexts allocated on a particular system must be freed before the system can be freed.

## Parameters

### `ContextFuncId` *(in, AIL_ID)*

Specifies the identifier of the user-defined Aurora Imaging Library function whose function context you want to free.
