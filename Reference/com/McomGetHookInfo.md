---
doctype: Reference
module: com
function: McomGetHookInfo
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / com / McomGetHookInfo"
---

# McomGetHookInfo

> Get information about a communication hook event.

## Syntax

```c
AIL_INT McomGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function retrieves information about the communication event that caused the hook-handler function to be called. [`McomGetHookInfo`](../../Reference/com/McomGetHookInfo.md) should only be called within the scope of an Industrial Communication hook-handler function ([`McomHookFunction`](../../Reference/com/McomHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the identifier of the communication event received by the hook-handler function (see [`McomHookFunction`](../../Reference/com/McomHookFunction.md)).

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address of the variable in which to write the requested information.

## Parameter Associations

### For inquiring information

---

### `M_COM_ERROR_NUMBER`

Retrieves the error code returned by the last Aurora Imaging Library communication function call.

| Value | Description |
| --- | --- |
| `M_ERROR_COM_NETWORK_ROBOT_CONNECT` | Specifies a network error code. This error code is generated when Aurora Imaging Library cannot connect to the robot. |
| `Value` | Specifies the Aurora Imaging Library communication error code. |

---

### `M_COM_PROFINET_SLOT_CHANGED_COUNT`

Retrieves the number of modules (slots) that were detected as changed.

---

### `M_COM_PROFINET_SLOT_CHANGED_LIST`

Retrieves a list containing the module numbers (slot numbers) of all the modules that changed.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.
