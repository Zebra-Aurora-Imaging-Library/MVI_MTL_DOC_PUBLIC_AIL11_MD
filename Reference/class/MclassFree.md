---
doctype: Reference
module: class
function: MclassFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassFree"
---

# MclassFree

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

> Free a classifier context, a dataset context, a data preparation context, a training context, a statistics classification context, a training result buffer, a prediction result buffer, or a statistics classification result buffer.

## Syntax

```c
void MclassFree(
    AIL_ID ClassObjectId  //in
)
```

## Description

This function deletes the specified classifier context, dataset context, data preparation context, training context, statistics classification context, training result buffer, prediction result buffer, or statistics classification result buffer identifier and releases any memory associated with it. All contexts and result buffers allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified during allocation.

## Parameters

### `ClassObjectId` *(in, AIL_ID)*

Specifies the identifier of the classifier context, dataset context, data preparation context, training context, statistics classification context, training result buffer, prediction result buffer, or statistics classification result buffer to free. These must have been successfully allocated with [`MclassAlloc`](../../Reference/class/MclassAlloc.md), [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md), [`MclassRestore`](../../Reference/class/MclassRestore.md), or [`MclassImport`](../../Reference/class/MclassImport.md)before calling [`MclassFree`](../../Reference/class/MclassFree.md).
