---
doctype: Reference
module: dlocr
function: MdlocrInquireStringModel
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrInquireStringModel"
---

# MdlocrInquireStringModel

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

> Inquire about a global setting of a Deep Learning OCR string model.

## Syntax

```c
AIL_INT64 MdlocrInquireStringModel(
    AIL_ID    ContextDlocrId,           //in
    AIL_INT64 StringModelLabelOrIndex,  //in
    AIL_INT64 InquireType,              //in
    void *    UserVarPtr                //out
)
```

## Description

This function inquires about a global setting of a Deep Learning OCR string model. Global settings of a string model are set using [`MdlocrControlStringModel`](../../Reference/dlocr/MdlocrControlStringModel.md).

## Parameters

### `ContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR context that contains the string model about which to inquire. The context must have been previously allocated on the system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model about which to inquire. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies to inquire about the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to inquire about the string model by indicating its label. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdlocrInquireStringModel`](../../Reference/dlocr/MdlocrInquireStringModel.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a global setting of a string model

To inquire about a global setting of a string model, set the [`InquireType`](../../Reference/dlocr/MdlocrInquireStringModel.md) parameter to one of the values below. Unless otherwise specified, set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrInquireStringModel.md) parameter to the label or index of a string model.

---

### `M_CHAR_HEIGHT_MAX`

Inquires the maximum height of an uppercase character in the string model.

| Value | Description |
| --- | --- |
| `M_SAME_AS_CONTEXT` *(default)* | Specifies to use the maximum character height specified using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MAX`](../../Reference/dlocr/MdlocrControl.md). |
| `Value >= M_CHAR_HEIGHT_MIN` | Specifies the maximum character height, in pixels. |

---

### `M_CHAR_HEIGHT_MIN`

Inquires the minimum height of an uppercase character in the string model.

| Value | Description |
| --- | --- |
| `M_SAME_AS_CONTEXT` *(default)* | Specifies to use the minimum character height specified using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with [`M_DETECTION_CHAR_HEIGHT_MIN`](../../Reference/dlocr/MdlocrControl.md). |
| `0.0 <= Value <= M_CHAR_HEIGHT_MAX` | Specifies the minimum height in pixels. |

---

### `M_MAX_NUMBER_OF_OCCURRENCES`

Inquires the maximum number of occurrences of the specified string model.

| Value | Description |
| --- | --- |
| `M_INFINITE` *(default)* | Specifies that there is no maximum number of occurrences. |
| `Value >= M_MIN_NUMBER_OF_OCCURRENCES` | Specifies the maximum number of occurrences. |

---

### `M_MIN_NUMBER_OF_OCCURRENCES`

Inquires the minimum number of occurrences of the specified string model.

| Value | Description |
| --- | --- |
| `0 <= Value <= M_MAX_NUMBER_OF_OCCURRENCES` *(default)* | Specifies the minimum number of occurrences. |

---

### `M_STRING_INDEX_VALUE`

Inquires the index of the string model. Set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrInquireStringModel.md) parameter to label of the string model.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the label is invalid. |
| `Value >= 0` | Specifies the index of the string model. |

---

### `M_STRING_LABEL_VALUE`

Inquires the label of the string model. Set the [`StringModelLabelOrIndex`](../../Reference/dlocr/MdlocrInquireStringModel.md) parameter to index of the string model.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of the string model. |

---

### `M_STRING_SIZE_MAX`

Inquires the maximum number of characters in a string.

| Value | Description |
| --- | --- |
| `M_STRING_SIZE_MIN <= Value <= 256` | Specifies the maximum number of characters. |

---

### `M_STRING_SIZE_MIN`

Inquires the minimum number of characters in a string.

| Value | Description |
| --- | --- |
| `1 <= Value <= M_STRING_SIZE_MAX` | Specifies the minimum number of characters. |

---

### `M_TEXT_ANCHOR_MODE`

Inquires the text anchoring mode.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that no anchoring is used. |
| `M_TEXT_PREFIX` | Specifies that the string is prefixed by a known substring. |
| `M_TEXT_SUFFIX` | Specifies that the string is suffixed by a known substring. |

---

### `M_TEXT_ANCHOR_VALUE`

Inquires the text to be used as a textual anchor.

| Value | Description |
| --- | --- |
| `"String"` *(default)* | Specifies the text used as an anchor. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring if an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported for the string model currently being inquired.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For casting information to the required data type.

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
