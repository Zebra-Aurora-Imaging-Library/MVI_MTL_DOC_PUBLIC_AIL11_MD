---
doctype: Reference
module: 3dgra
function: M3dgraGetHookInfo
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dgra / M3dgraGetHookInfo"
---

# M3dgraGetHookInfo

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

> Get information about a 3D graphic hook event.

## Syntax

```c
AIL_INT M3dgraGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the event that caused the hook-handler function to be called. [`M3dgraGetHookInfo`](../../Reference/3dgra/M3dgraGetHookInfo.md) should only be called within the scope of a 3D graphic hook-handler function (see [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the 3D graphic event identifier received by the hook-handler function (see [`M3dgraHookFunction`](../../Reference/3dgra/M3dgraHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information about the event to return.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For retrieving information about a 3d graphics list hook event

Regardless of the 3D graphics list event that caused the hook-handler function to be called, the [`InfoType`](../../Reference/3dgra/M3dgraGetHookInfo.md) parameter can be set to the value below.

---

### `M_LIST`

Retrieves the Aurora Imaging Library identifier of the 3D graphics list that generated the event.

| Value | Description |
| --- | --- |
| `3D graphics list identifier` | Specifies the Aurora Imaging Library identifier of the 3D graphics list that generated the event. |

### For retrieving information regarding 3D graphic events

Regardless of the 3D graphics event that caused the hook-handler function to be called, the [`InfoType`](../../Reference/3dgra/M3dgraGetHookInfo.md) parameter can be set to the value below.

---

### `M_LABEL`

Retrieves the label of the 3D graphic that generated the event.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the label of the 3D graphic that generated the event. |

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (\![`M_NULL`](../../Reference/3dgra/M3dgraGetHookInfo.md)) value is returned.
