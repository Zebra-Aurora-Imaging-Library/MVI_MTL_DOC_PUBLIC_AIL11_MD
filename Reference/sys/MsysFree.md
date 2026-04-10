---
doctype: Reference
module: sys
function: MsysFree
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / sys / MsysFree"
---

# MsysFree

> Free a system.

## Syntax

```c
void MsysFree(
    AIL_ID SysId  //in
)
```

## Description

This function deallocates a system previously allocated with [`MsysAlloc`](../../Reference/sys/MsysAlloc.md).

Prior to freeing a system, ensure that all buffers, displays, and digitizers allocated on the system are freed, unless [`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) was specified during allocation.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system to free.

## Remarks

> If you are creating a DLL that includes a call to [`MsysFree`](../../Reference/sys/MsysFree.md), ensure that the call is not made from the `DllMain()` function, because [`MsysFree`](../../Reference/sys/MsysFree.md) might unload any DLL loaded with [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) and you cannot unload a DLL from `DllMain()`. If necessary, call [`MsysFree`](../../Reference/sys/MsysFree.md) from a clean-up function in your DLL instead.
