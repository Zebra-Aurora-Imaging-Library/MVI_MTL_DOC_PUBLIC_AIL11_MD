---
doctype: Reference
module: met
function: MmetName
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / met / MmetName"
---

# MmetName

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

> Set or retrieve the name of a feature or tolerance, or retrieve the label or index of a specified feature or tolerance.

## Syntax

```c
void MmetName(
    AIL_ID       ContextOrResultId,  //out
    AIL_INT64    Operation,          //in
    AIL_INT      LabelOrIndex,       //in
    AIL_TEXT_PTR String,             //in-out
    AIL_INT *    ValuePtr,           //out
    AIL_INT64    ControlFlag         //in
)
```

## Description

This function allows you to set or retrieve the name of a metrology feature or tolerance. It also allows you to retrieve either the label or index of a feature or tolerance based on its name.

Note that [`MmetName`](../../Reference/met/MmetName.md) supports Unicode.

## Parameters

### `ContextOrResultId` *(out, AIL_ID)*

Specifies the identifier of the metrology context or result buffer containing the feature or tolerance to read or modify. The metrology context or the metrology result buffer must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md) or [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md), respectively.

### `Operation` *(in, AIL_INT64)*

Specifies the type of operation to perform.

*For specifying the operation to perform*

| Value | Description |
| --- | --- |
| `M_GET_FEATURE_INDEX` | Retrieves the index of a feature, based on its name. Set the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter to [`M_DEFAULT`](../../Reference/met/MmetName.md).

If there is no feature with the specified name, the returned index is -1. |
| `M_GET_FEATURE_LABEL` | Retrieves the label of a feature, based on its name. Set the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter to [`M_DEFAULT`](../../Reference/met/MmetName.md).

If there is no feature with the specified name, the returned label is 0. |
| `M_GET_NAME` | Retrieves the name of the feature or tolerance defined by the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter.

The name of the feature or tolerance is returned via the [`String`](../../Reference/met/MmetName.md) parameter, and the name's length is returned via the [`ValuePtr`](../../Reference/met/MmetName.md) parameter.

To determine the exact size name, call this function twice, the first time to determine the size of the name, with the [`String`](../../Reference/met/MmetName.md) parameter set to [`M_NULL`](../../Reference/met/MmetName.md), and the second time to return the name. |
| `M_GET_TOLERANCE_INDEX` | Retrieves the index of a tolerance, based on its name. Set the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter to [`M_DEFAULT`](../../Reference/met/MmetName.md).

If there is no tolerance with the specified name, the returned index is -1. |
| `M_GET_TOLERANCE_LABEL` | Retrieves the label of a tolerance, based on its name. Set the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter to [`M_DEFAULT`](../../Reference/met/MmetName.md).

If there is no tolerance with the specified name, the returned label is 0. |
| `M_SET_NAME` | Sets the name of the feature or tolerance defined by the [`LabelOrIndex`](../../Reference/met/MmetName.md) parameter.

This operation requires [`ContextOrResultId`](../../Reference/met/MmetName.md) to be a valid context identifier. In addition, the specified name of the feature or tolerance (passed in the [`String`](../../Reference/met/MmetName.md) parameter) must be unique, and [`ValuePtr`](../../Reference/met/MmetName.md) must be set to [`M_NULL`](../../Reference/met/MmetName.md).

Note that you can release a name by passing [`M_NULL`](../../Reference/met/MmetName.md) to [`String`](../../Reference/met/MmetName.md). |

### `LabelOrIndex` *(in, AIL_INT)*

Specifies the label or index value of the metrology feature or tolerance. Set this parameter to `M_DEFAULT` when not required by the name operation.

*For specifying the label or index value*

| Value | Description |
| --- | --- |
| `M_FEATURE_INDEX` | Specifies a feature by indicating its index. |
| `M_FEATURE_LABEL` | Specifies a feature by indicating its label. |
| `M_TOLERANCE_INDEX` | Specifies a tolerance by indicating its index. |
| `M_TOLERANCE_LABEL` | Specifies a tolerance by indicating its label. |

### `String` *(in-out, AIL_TEXT_PTR)*

Specifies the name to set or retrieve, depending on the Operation parameter.

### `ValuePtr` *(out, *AIL_INT)*

Specifies the address at which to write the requested information.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
