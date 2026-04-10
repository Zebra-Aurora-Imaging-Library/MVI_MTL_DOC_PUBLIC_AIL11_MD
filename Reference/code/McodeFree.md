---
doctype: Reference
module: code
function: McodeFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeFree"
---

# McodeFree

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

> Free a code context or a code result buffer.

## Syntax

```c
void McodeFree(
    AIL_ID ContextOrResultCodeId  //in
)
```

## Description

This function deletes the specified code context (and all its models) or code result buffer identifier, and releases any memory associated with it.

To only delete individual models in the context, use [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_DELETE`](../../Reference/code/McodeModel.md).

All code contexts and all code result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/code/McodeAlloc.md) was specified during allocation.

## Parameters

### `ContextOrResultCodeId` *(in, AIL_ID)*

Specifies the identifier of the code context or code result buffer to free. These must have been successfully allocated (with [`McodeAlloc`](../../Reference/code/McodeAlloc.md) or [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md)) prior to calling this function.
