---
doctype: Reference
module: dlocr
function: MdlocrGetHookInfo
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrGetHookInfo"
---

# MdlocrGetHookInfo

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

> Get information about a hooked Deep Learning OCR object event.

## Syntax

```c
AIL_INT64 MdlocrGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of a Deep Learning OCR object hook-handler function call (see [`MdlocrHookFunction`](../../Reference/dlocr/MdlocrHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the Deep Learning OCR object event identifier received by the hook-handler function. See [`MdlocrHookFunction`](../../Reference/dlocr/MdlocrHookFunction.md) for more information. The event will be of the type passed to the [`HookType`](../../Reference/dlocr/MdlocrHookFunction.md) parameter of the hook-handler function.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Note that when getting parameter values, the [`UserVarPtr`](../../Reference/dlocr/MdlocrGetHookInfo.md) parameter should be of the same data type as value of the selected parameter.

## Parameter Associations

### For getting information about a hooked Deep Learning OCR object event

---

### `An M_FINETUNE_PROGRESS event ID`

Specifies that the event that called the hook-handler function was due to an [`M_FINETUNE_PROGRESS`](../../Reference/dlocr/MdlocrHookFunction.md) event type on a fine-tuning context object.

#### `M_PERCENTAGE`

Retrieves the completion percentage of the fine-tuning operation.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the percentage. |

### Combination Constants — For determining whether event results are available to be returned

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an event result is available to be returned.

#### `M_AVAILABLE`

Retrieves whether the requested information is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested information is not available. |
| `M_TRUE` | Specifies that the requested information is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (`M_NULL`) value is returned.
