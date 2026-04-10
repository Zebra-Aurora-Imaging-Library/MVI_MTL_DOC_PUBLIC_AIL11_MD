---
doctype: Reference
module: dmr
function: MdmrName
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrName"
---

# MdmrName

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

> Perform name operations for fonts and string models.

## Syntax

```c
void MdmrName(
    AIL_ID       ContextDmrId,  //in
    AIL_INT64    Operation,     //in
    AIL_INT64    LabelOrIndex,  //in
    AIL_TEXT_PTR Name,          //in-out
    AIL_INT *    ValuePtr,      //out
    AIL_INT64    ControlFlag    //in
)
```

## Description

This function allows you to perform name operations for fonts and string models in a SureDotOCR context. Use this function to set the name of a font or string model, release the name of a font or string model, or retrieve either the label or index of a font or string model based on its name. These operations allow you to more easily manage your data; they do not impact how strings are processed and read.

## Parameters

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context containing the font or string model with which to perform the name operation. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `Operation` *(in, AIL_INT64)*

Specifies the name operation to perform. Set this parameter to one of the values below. Unless otherwise specified, the [`LabelOrIndex`](../../Reference/dmr/MdmrName.md) parameter can specify a font or string model, the [`Name`](../../Reference/dmr/MdmrName.md) parameter must specify the name information related to the operation, and the [`ValuePtr`](../../Reference/dmr/MdmrName.md) parameter must specify the address at which to write the information requested by the name operation.

*For specifying the name operation*

| Value | Description |
| --- | --- |
| `M_GET_FONT_INDEX` | Retrieves the index of the font, based on the specified name. Set the [`LabelOrIndex`](../../Reference/dmr/MdmrName.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrName.md). |
| `M_GET_FONT_LABEL` | Retrieves the label of the font, based on the specified name. Set the [`LabelOrIndex`](../../Reference/dmr/MdmrName.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrName.md). |
| `M_GET_NAME` | Retrieves the name, and name length, of the specified font or string model.

Use the[`Name`](../../Reference/dmr/MdmrName.md) parameter to retrieve the name. Use the [`ValuePtr`](../../Reference/dmr/MdmrName.md) parameter to retrieve the length of the name. |
| `M_GET_STRING_INDEX` | Retrieves the index of the string model, based on the specified name. Set the [`LabelOrIndex`](../../Reference/dmr/MdmrName.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrName.md). |
| `M_GET_STRING_LABEL` | Retrieves the label of the string model, based on the specified name. Set the [`LabelOrIndex`](../../Reference/dmr/MdmrName.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrName.md). |
| `M_SET_NAME` | Sets the name of the font or string model, or releases the name of the font or string model.

When setting a name, ensure it is not already used. All font names must be unique among font names and all string model names must be unique among string model names.

When releasing a name, set the [`Name`](../../Reference/dmr/MdmrName.md) parameter to [`M_NULL`](../../Reference/dmr/MdmrName.md). Once released, you can use the name again. |

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index value of the font or string model with which to perform the name operation. Set this parameter to `M_DEFAULT` when not required by the name operation.

*For specifying the label or index value*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies a font by indicating its index. |
| `M_FONT_LABEL` | Specifies a font by indicating its label. |
| `M_STRING_INDEX` | Specifies a string model by indicating its index. |
| `M_STRING_LABEL` | Specifies a string model by indicating its label. |

### `Name` *(in-out, AIL_TEXT_PTR)*

Specifies the name. Use this parameter to set, get, or release the name, depending on the name operation. If the operation is getting a label and it fails, SureDotOCR returns 0. If the operation is getting an index and it fails, SureDotOCR returns -1. To release the name of a font or string model, set this parameter to `M_NULL` and specify [`M_SET_NAME`](../../Reference/dmr/MdmrName.md). To determine the length of the current name of the font or string model, set this parameter to [`M_NULL`](../../Reference/dmr/MdmrName.md) and specify [`M_GET_NAME`](../../Reference/dmr/MdmrName.md).

### `ValuePtr` *(out, *AIL_INT)*

Specifies the address at which to write the information requested by the name operation.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
