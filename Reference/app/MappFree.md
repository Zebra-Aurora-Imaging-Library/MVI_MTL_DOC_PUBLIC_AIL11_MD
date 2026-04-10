---
doctype: Reference
module: app
function: MappFree
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappFree"
---

# MappFree

> Free an application.

## Syntax

```c
void MappFree(
    AIL_ID ContextAppId  //in
)
```

## Description

This function deallocates an Aurora Imaging Library application previously allocated with [`MappAlloc`](../../Reference/app/MappAlloc.md).

[`MappFree`](../../Reference/app/MappFree.md) must be the last function called; no other function can be executed after a call to this function. Prior to freeing an application, ensure that all systems, buffers, displays, and digitizers allocated in the application are freed, unless [`M_UNIQUE_ID`](../../Reference/app/MappAlloc.md) was specified during allocation.

Note, if you use [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) to allocate the default application, you must use [`MappFreeDefault`](../../Reference/app/MappFreeDefault.md) to free the application.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to free.

## Remarks

> If you are creating a DLL that includes a call to [`MappFree`](../../Reference/app/MappFree.md), ensure that the call is not made from the `DllMain()` function, because [`MappFree`](../../Reference/app/MappFree.md) might unload any DLL loaded with [`MappAlloc`](../../Reference/app/MappAlloc.md) and you cannot unload a DLL from `DllMain()`. If necessary, call [`MappFree`](../../Reference/app/MappFree.md) from a clean-up function in your DLL instead.
