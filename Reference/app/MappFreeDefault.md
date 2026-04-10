---
doctype: Reference
module: app
function: MappFreeDefault
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / app / MappFreeDefault"
---

# MappFreeDefault

> Free default objects.

## Syntax

```c
void MappFreeDefault(
    AIL_ID ContextAppId,  //in
    AIL_ID SysId,         //in
    AIL_ID DispId,        //in
    AIL_ID DigId,         //in
    AIL_ID ImageBufId     //in
)
```

## Description

[`MappFreeDefault`](../../Reference/app/MappFreeDefault.md) is implemented as a macro. This macro frees the default Aurora Imaging Library objects that were allocated with the [`MappAllocDefault`](../../Reference/app/MappAllocDefault.md) macro.

## Parameters

### `ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context to free.

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system to free.

### `DispId` *(in, AIL_ID)*

Specifies the identifier of the display to free. If set to `M_NULL`, no display is freed.

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer to free. If set to `M_NULL`, no digitizer is freed.

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to free. If set to `M_NULL`, no buffer is freed.
