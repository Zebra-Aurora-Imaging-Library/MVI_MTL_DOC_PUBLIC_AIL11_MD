---
doctype: Reference
module: seq
function: MseqGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqGetHookInfo"
---

# MseqGetHookInfo

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

> Get information about a sequence hook event.

## Syntax

```c
AIL_INT MseqGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function allows you to get information about the sequence event that caused the hook-handler function to be called. [`MseqGetHookInfo`](../../Reference/seq/MseqGetHookInfo.md) should only be called within the scope of a sequence hook-handler function (see [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the sequence event identifier received by the hook-handler function. See[`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md)for more information.

### `InfoType` *(in, AIL_INT64)*

Specifies a combination of two values: the event type and the type of information about the event to return.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information.

## Parameter Associations

### For retrieving the information about a sequence hook event

---

### `M_MODIFIED_BUFFER`

Retrieves information about a modified-buffer type of event. This event only occurs if the hook-handler function was hooked using [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md).

### Combination Constants — For retrieving information about the internal output buffer

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to get information about the internal output buffer.

#### `M_BUFFER_ID`

Retrieves the Aurora Imaging Library identifier of the internal output buffer modified by [`MseqProcess`](../../Reference/seq/MseqProcess.md).

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.
