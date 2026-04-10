---
doctype: Reference
module: com
function: McomFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / com / McomFree"
---

# McomFree

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | Yes |
| Iris GTX | Yes |
| Radient eV-CL | No |
| Rapixo CL | No |
| Rapixo CoF | No |
| Rapixo CXP | No |
| USB3 Vision | No |

> Free an Industrial Communication context.

## Syntax

```c
void McomFree(
    AIL_ID ComId  //in
)
```

## Description

This function frees an Industrial Communication context.

> **Note:** This function is only available if you have the Aurora Imaging Library Industrial & Robot Communications package, or another relevant update installed.

All Industrial Communication contexts allocated on a particular system must be freed before the system can be freed, unless [`M_UNIQUE_ID`](../../Reference/com/McomAlloc.md) was specified during allocation.

## Parameters

### `ComId` *(in, AIL_ID)*

Specifies the identifier of the Industrial Communication context.
