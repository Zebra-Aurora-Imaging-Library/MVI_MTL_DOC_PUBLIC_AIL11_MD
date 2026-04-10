---
doctype: Reference
module: dlocr
function: MdlocrInquireConstraint
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dlocr / MdlocrInquireConstraint"
---

# MdlocrInquireConstraint

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

> Inquire about a positional constraint of a Deep Learning OCR string model.

## Syntax

```c
AIL_INT64 MdlocrInquireConstraint(
    AIL_ID    ContextDlocrId,           //in
    AIL_INT64 StringModelLabelOrIndex,  //in
    AIL_INT64 PositionInString,         //in
    AIL_INT64 InquireType,              //in
    void *    UserVarPtr                //out
)
```

## Description

This function allows you to inquire about a positional constraint of a Deep Learning OCR string model.

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

### `PositionInString` *(in, AIL_INT64)*

Specifies the position of the character in the string model. Set this parameter to one of the values below:

*For specifying the position in a string model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies all positions that have not been manually constrained. |
| `Value >= 0` | Specifies the position in the string model. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdlocrInquireConstraint`](../../Reference/dlocr/MdlocrInquireConstraint.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about positional constraints in a string model

The following [`InquireType`](../../Reference/dlocr/MdlocrInquireConstraint.md) parameter settings are used to inquire about positional constraints of a string model.

---

### `M_CHAR_LIST`

Inquires the list of expected characters (case sensitive). If [`M_CONSTRAINT_TYPE`](../../Reference/dlocr/MdlocrInquireConstraint.md) is set to [`M_ANY`](../../Reference/dlocr/MdlocrInquireConstraint.md), this will return an empty string indicating all supported values are used.

| Value | Description |
| --- | --- |
| `"CharList"` | Specifies the list of expected characters. |

---

### `M_CONSTRAINT_TYPE`

Inquires the type of constraint applied to the position.

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies any character. |
| `M_DIGITS` | Specifies any digit. |
| `M_LETTERS` | Specifies any letter. |
| `M_LETTERS_LOWERCASE` | Specifies any lowercase letter. |
| `M_LETTERS_UPPERCASE` | Specifies any uppercase letter. |
| `M_CHAR_LIST` | Specifies any character in a list passed to the [`ControlValuePtr`](../../Reference/dlocr/MdlocrControlConstraint.md) parameter of [`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md). |
| `M_SAME_AS_DEFAULT_CONSTRAINT` *(default)* | Specifies to use the default constraint set using[`MdlocrControlConstraint`](../../Reference/dlocr/MdlocrControlConstraint.md) with its [`PositionInString`](../../Reference/dlocr/MdlocrInquireConstraint.md) set to [`M_DEFAULT`](../../Reference/dlocr/MdlocrInquireConstraint.md) and [`M_CONSTRAINT_TYPE`](../../Reference/dlocr/MdlocrInquireConstraint.md) set to the required constraint. |

---

### `M_IS_OPTIONAL`

Inquires whether a position in a string model is optional.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that the model position is not optional. |
| `M_TRUE` | Specifies that the model position is optional. |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type (if supported), regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character. All strings have the terminating null character, even though you need not explicitly list it when specifying the string.

### Combination Constants — For inquiring if an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported for the constraint currently being inquired.

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

### Combination Constants — For casting the requested information to the required data type

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

#### `M_TYPE_AIL_UINT8`

Casts the requested information to an _AIL_UINT8_.

## Return Value

**Type:** `AIL_INT64`

The returned value is the requested information, cast to an _AIL_INT64_. If the requested information does not fit into an _AIL_INT64_, this function will return `M_NULL`or truncate the information.
