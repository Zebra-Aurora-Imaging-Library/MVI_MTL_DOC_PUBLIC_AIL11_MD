---
doctype: Reference
module: obj
function: MobjGetHookInfo
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / obj / MobjGetHookInfo"
---

# MobjGetHookInfo

> Get information about a hook event.

## Syntax

```c
AIL_INT MobjGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook function to be called. [`MobjGetHookInfo`](../../Reference/obj/MobjGetHookInfo.md) should only be called within the scope of an object hook-handler function (see [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the object-related event identifier received by the hook-handler function (see [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For inquiring information

---

### `M_OBJECT_ID`

Inquires the object identifier that called the hook-handler function.

| Value | Description |
| --- | --- |
| `Object identifier` | Specifies the object identifier that called the hook-handler function. |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.
