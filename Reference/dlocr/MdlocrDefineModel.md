---
doctype: Reference
module: dlocr
function: MdlocrDefineModel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrDefineModel"
---

# MdlocrDefineModel

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

> Add a string model to, or delete a string model from, a Deep Learning OCR context.

## Syntax

```c
void MdlocrDefineModel(
    AIL_ID    ContextDlocrId,           //in
    AIL_INT64 StringModelLabelOrIndex,  //in
    AIL_INT64 Operation,                //in
    AIL_INT64 StringSizeMin,            //in
    AIL_INT64 StringSizeMax             //in
)
```

## Description

This function allows you to add a string model to, or delete a string model from, a Deep Learning OCR context.

You don't need to add a model to read all strings in the target image. In addition, if you want to restrict the string that is found and read, you should typically use [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md), which allows you to select a resulting string as the model. [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md) is useful if you only want to restrict the number of characters in the string. Regardless of how you add a model, you can always constrain the characters that appear at certain positions using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) and adjust general string model settings, such as, setting text anchors, using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

Note that although you need to define the minimum and maximum number of characters of the string model when you add a new model with this function, you can change these limits using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md) with [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md).

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context in which to add or delete a model. The Deep Learning OCR context must have been previously allocated on the required system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model (one or all) to control.

### `Operation` *(in, AIL_INT64)*

Specifies whether to add or delete a string model from a Deep Learning OCR context.

### `StringSizeMin` *(in, AIL_INT64)*

Specifies the minimum number of characters of a new string model. If the [`Operation`](../../Reference/dlocr/MdlocrDefineModel.md) parameter is set to [`M_DELETE`](../../Reference/dlocr/MdlocrDefineModel.md), [`StringSizeMin`](../../Reference/dlocr/MdlocrDefineModel.md) must be set to [`M_DEFAULT`](../../Reference/dlocr/MdlocrDefineModel.md).

*For specifying the minimum number of characters*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not needed (that is, when [`Operation`](../../Reference/dlocr/MdlocrDefineModel.md) set to [`M_DELETE`](../../Reference/dlocr/MdlocrDefineModel.md)). |
| `Value >= 0` | Specifies the minimum number of characters. |

### `StringSizeMax` *(in, AIL_INT64)*

Specifies the maximum length of a new string model. If the [`Operation`](../../Reference/dlocr/MdlocrDefineModel.md) parameter is set to [`M_DELETE`](../../Reference/dlocr/MdlocrDefineModel.md), [`StringSizeMax`](../../Reference/dlocr/MdlocrDefineModel.md) must be set to [`M_DEFAULT`](../../Reference/dlocr/MdlocrDefineModel.md).

*For specifying the maximum number of characters*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not needed (that is, when [`Operation`](../../Reference/dlocr/MdlocrDefineModel.md) set to [`M_DELETE`](../../Reference/dlocr/MdlocrDefineModel.md)). |
| `Value >= 0` | Specifies the maximum number of characters. |

## Parameter Associations

### For creating or deleting a string model

---

### `M_ADD`

Specifies to add a new string model to the context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_STRING_LABEL` | Specifies to add a string model with the specified label. |
| `M_NEW_LABEL` *(default)* | Specifies to add a string model with an automatically assigned label. To inquire about the label, use [`MdlocrInquireStringModel`](../../Reference/dlocr/MdlocrInquireStringModel.md) with [`M_STRING_LABEL_VALUE`](../../Reference/dlocr/MdlocrInquireStringModel.md). |

---

### `M_DELETE`

Specifies to delete a string model from a Deep Learning OCR context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_STRING_INDEX` | Specifies to delete the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to delete the string model by indicating its label. |
| `M_ALL` *(default)* | Specifies to delete all string models from the Deep Learning OCR context. |

## Return Value

**Type:** `void`

Reserved for future expansion and returns `M_NULL`.
