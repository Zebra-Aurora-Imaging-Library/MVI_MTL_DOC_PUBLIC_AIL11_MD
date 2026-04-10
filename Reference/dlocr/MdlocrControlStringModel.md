---
doctype: Reference
module: dlocr
function: MdlocrControlStringModel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrControlStringModel"
---

# MdlocrControlStringModel

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

> Control a global setting of a Deep Learning OCR string model.

## Syntax

```c
void MdlocrControlStringModel(
    AIL_ID     ContextDlocrId,           //out
    AIL_INT64  StringModelLabelOrIndex,  //in
    AIL_INT64  ControlType,              //in
    AIL_DOUBLE ControlValue              //in
)
```

## Description

This function allows you to control a global setting of a Deep Learning OCR string model. String models represent the strings to read from a target image. Initially, any character from any font can be read.

String models are held in a Deep Learning OCR context. To add string models to a context based on a previous call to [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md), use [`MdlocrDefineModelFromResult`](../../Reference/dlocr/MdlocrDefineModelFromResult.md); to add a string model without relying on a previous [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operation, use [`MdlocrDefineModel`](../../Reference/dlocr/MdlocrDefineModel.md). To inquire about global settings of string models, use [`MdlocrInquireStringModel`](../../Reference/dlocr/MdlocrInquireStringModel.md).

You must preprocess the Deep Learning OCR context after you have finished modifying its string models and before calling [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md). To know if a context needs to be preprocessed, call [`MdlocrInquire`](../../Reference/dlocr/MdlocrInquire.md) with [`M_PREPROCESSED`](../../Reference/dlocr/MdlocrInquire.md).

## Parameters

### `ContextDlocrId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context that contains the string model to control. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model (one or all) to control. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies to control the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to control the string model by indicating its label. |
| `M_ALL` | Specifies to control all string models. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the setting.

## Parameter Associations

### For general string model settings

To control a global setting of a string model, set the [`ControlType`](../../Reference/dlocr/MdlocrControlStringModel.md) parameter to one of the values below. Unless otherwise specified, set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrControlStringModel.md) parameter to the label or index of a string model.

---

### `M_CHAR_HEIGHT_MAX`

Sets the maximum height of an uppercase character in the string model. Note this value must be less than or equal to the value set using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_CONTEXT` *(default)* | Specifies to use the maximum character height specified using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControl.md). |
| `Value >= M_CHAR_HEIGHT_MIN` | Specifies the maximum character height, in pixels. |

---

### `M_CHAR_HEIGHT_MIN`

Sets the minimum height of an uppercase character in the string model. Note, this value must be greater than or equal to the value set using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SAME_AS_CONTEXT` *(default)* | Specifies to use the minimum character height specified using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControl.md). |
| `0.0 <= Value <= M_CHAR_HEIGHT_MAX` | Specifies the minimum height in pixels. |

---

### `M_MAX_NUMBER_OF_OCCURRENCES`

Sets the maximum number of occurrences of the specified string model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies that there is no maximum number of occurrences. |
| `Value >= M_MIN_NUMBER_OF_OCCURRENCES` | Specifies the maximum number of occurrences. |

---

### `M_MIN_NUMBER_OF_OCCURRENCES`

Sets the minimum number of occurrences of the specified string model.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= M_MAX_NUMBER_OF_OCCURRENCES` *(default)* | Specifies the minimum number of occurrences. |

---

### `M_STRING_LABEL_VALUE`

Sets the label of the specified string model. You must set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrControlStringModel.md) parameter to the index of the string model.

| Value | Description |
| --- | --- |
| `0 < Value < 2097152` | Specifies the label of the string model to control. |

---

### `M_STRING_SIZE_MAX`

Sets the maximum number of characters in a string.

| Value | Description |
| --- | --- |
| `M_STRING_SIZE_MIN <= Value <= 256` | Specifies the maximum number of characters. |

---

### `M_STRING_SIZE_MIN`

Sets the minimum number of characters in a string.  > **Note:** Note, when setting [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md) less than [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md), [`MdlocrRead`](../../Reference/dlocr/MdlocrRead.md) operations with [`M_MODEL_BASED`](../../Reference/dlocr/MdlocrRead.md) will be allowed to skip model positions where [`M_IS_OPTIONAL`](../../Reference/dlocr/MdlocrControlConstraint.md) is set to [`M_TRUE`](../../Reference/dlocr/MdlocrControlConstraint.md). [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md)expects the number of positions with [`M_IS_OPTIONAL`](../../Reference/dlocr/MdlocrControlConstraint.md) set to [`M_TRUE`](../../Reference/dlocr/MdlocrControlConstraint.md) to be greater than or equal to the difference between [`M_STRING_SIZE_MAX`](../../Reference/dlocr/MdlocrControlStringModel.md) and [`M_STRING_SIZE_MIN`](../../Reference/dlocr/MdlocrControlStringModel.md).

| Value | Description |
| --- | --- |
| `1 <= Value <= M_STRING_SIZE_MAX` | Specifies the minimum number of characters. |

---

### `M_TEXT_ANCHOR_MODE`

Sets the text anchoring mode. If a text anchor is used, it will not be included in the string result.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no anchoring is used. |
| `M_TEXT_PREFIX` | Specifies that the string is prefixed by a known substring.  Specify the substring using[`M_TEXT_ANCHOR_VALUE`](../../Reference/dlocr/MdlocrControlStringModel.md). |
| `M_TEXT_SUFFIX` | Specifies that the string is suffixed by a known substring.  Specify the substring using[`M_TEXT_ANCHOR_VALUE`](../../Reference/dlocr/MdlocrControlStringModel.md). |

---

### `M_TEXT_ANCHOR_VALUE`

Sets the text to be used as a textual anchor.

| Value | Description |
| --- | --- |
| `"String"` *(default)* | Specifies the text used as an anchor. An empty string indicates that any string that fits the string model can be read. |
