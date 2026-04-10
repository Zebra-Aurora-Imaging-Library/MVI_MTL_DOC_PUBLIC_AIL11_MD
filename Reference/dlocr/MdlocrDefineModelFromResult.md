---
doctype: Reference
module: dlocr
function: MdlocrDefineModelFromResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrDefineModelFromResult"
---

# MdlocrDefineModelFromResult

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

> Add a string model based on a resulting string from a previous call to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

## Syntax

```c
void MdlocrDefineModelFromResult(
    AIL_ID    ContextDlocrId,           //in
    AIL_INT64 StringModelLabelOrIndex,  //in
    AIL_ID    ResultId,                 //in
    AIL_INT64 StringIndex,              //in
    AIL_INT64 ControlFlag               //in
)
```

## Description

This function allows you to add a string model based on a resulting string from a previous call to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md).

The string model is automatically assigned the same number of characters ([`M_STRING_SIZE_...`](../../Reference/dlocr/MdlocrControlStringModel.md)) as the specified resulting string. The string model is also automatically assigned a minimum and maximum character height ([`M_CHAR_HEIGHT_...`](../../Reference/dlocr/MdlocrControlStringModel.md)) that is the same as the specified resulting string plus a tolerance. To have the constraints inferred from the resulting string as well, set the [`ControlFlag`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) parameter to [`M_INFER_CONSTRAINTS`](../../Reference/dlocr/MdlocrDefineModelFromResult.md). Note that the position of the specified resulting string is not used.

You don't need to add a model to read all strings in the target image. You only need to add a model if you want to restrict the string that is found and read. [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md) allows you to define the string model so that attributes are automatically set from the specified resulting string. If you only want to restrict the number of characters in the string, you can use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md). Regardless of how you add a model, you can always constrain the characters that appear at certain positions using [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) and adjust general string model settings using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

The string model index starts at 0, and each subsequent string model added to the context is given a sequential index number, in the order that the model was added. If a model is deleted, all entries with higher indices are shifted down one. When adding the string model, you can overwrite an existing model by specifying its label or index.

If multiple strings are present in the target image, you can establish which is the required string by drawing the results using [`MdlocrDraw`](../../Reference/dlocr/MdlocrDraw.md).

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context to which the string model must be added.

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model to replace. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_STRING_INDEX` | Specifies the index of the string model to replace. An error is generated if a string model is not defined at the specified index. |
| `M_STRING_LABEL` | Specifies the label of the string model to replace. An error is generated if a string model is not defined at the specified label. |
| `M_ADD` *(default)* | Specifies to add the new string model. |

### `ResultId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR result context from which the string model will be created.

### `StringIndex` *(in, AIL_INT64)*

Specifies the index of the string occurrence from which the string model will be created.

### `ControlFlag` *(in, AIL_INT64)*

Specifies what is set when defining a string model from a result. Set this parameter to one of the values below:

*For specifying what is set when defining a string model from a result string*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to set [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md), [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md), [`M_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md), and [`M_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md) for the new model based on the string occurrence. |
| `M_INFER_CONSTRAINTS` | Specifies to set constraints based on the specified string occurrence. [`M_LETTERS_UPPERCASE`](../../Reference/dlocr/MdlocrControlConstraint.md), [`M_LETTERS_LOWERCASE`](../../Reference/dlocr/MdlocrControlConstraint.md), and [`M_DIGITS`](../../Reference/dlocr/MdlocrControlConstraint.md) can be inferred from the string occurrence. If a character in the string occurrence is neither a letter or a digit, then the inferred constraint type is set to [`M_SAME_AS_DEFAULT_CONSTRAINT`](../../Reference/dlocr/MdlocrControlConstraint.md) (same as [`M_ANY`](../../Reference/dlocr/MdlocrControlConstraint.md)). |

## Return Value

**Type:** `void`

Reserved for future expansion and returns `M_NULL`.
