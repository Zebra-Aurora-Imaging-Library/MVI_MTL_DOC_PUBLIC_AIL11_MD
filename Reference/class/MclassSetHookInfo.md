---
doctype: Reference
module: class
function: MclassSetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassSetHookInfo"
---

# MclassSetHookInfo

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

> Set information about a classification object event.

## Syntax

```c
void MclassSetHookInfo(
    AIL_ID     EventId,   //in
    AIL_INT64  InfoType,  //in
    AIL_DOUBLE InfoValue  //in
)
```

## Description

This function allows you to set information about the event that caused the hook-handler function to be called. [`MclassSetHookInfo`](../../Reference/class/MclassSetHookInfo.md) must only be called within the scope of a classification object hook-handler function (see [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the classification object event identifier received by the hook-handler function. See [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) for more information. The event will be of the type passed to the [`HookType`](../../Reference/class/MclassHookFunction.md) parameter of the hook-handler function.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to set.

### `InfoValue` *(in, AIL_DOUBLE)*

Specifies the required value for the type of information to set.

## Parameter Associations

### For a data preparation context using PREPARE_ENTRY_PRE

To set information about the event that caused the hook-handler function to be called, the InfoType and corresponding InfoValue parameter settings can be set to one of the following values. These settings are only available if the hook-handler function was called due to an [`M_PREPARE_ENTRY_PRE`](../../Reference/class/MclassHookFunction.md) event type on a data preparation context object.

---

### `M_CENTER_X`

Sets the X-coordinate of the center position in the given image.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the center's X-coordinate. |

---

### `M_CENTER_Y`

Sets the Y-coordinate of the center position in the given image.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the center's Y-coordinate. |
