---
doctype: Reference
module: dmr
function: MdmrInquireStringModel
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrInquireStringModel"
---

# MdmrInquireStringModel

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

> Inquire about a global setting of a SureDotOCR string model, or inquire about a position (constraint) in a string model.

## Syntax

```c
AIL_INT MdmrInquireStringModel(
    AIL_ID    ContextDmrId,             //in
    AIL_INT64 StringModelLabelOrIndex,  //in
    AIL_INT64 Position,                 //in
    AIL_INT64 PermittedCharEntry,       //in
    AIL_INT64 InquireType,              //in
    void *    UserVarPtr                //out
)
```

## Description

This function inquires about a global setting of a SureDotOCR string model, or inquires about the different positions (constraints) in a string model. String models are defined with [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md).

To inquire about the default constraints for the positions in the string model, call this function and pass [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md) to the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter; to inquire about explicit constraints for a position, pass **M_POSITION_IN_STRING(n)** instead, where _n_ is the explicitly constrained position. Note that constraints typically refer to the characters that are permitted to be read, according to a character type and font.

If you want to loop through all explicitly constrained positions, you can set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to **M_POSITION_CONSTRAINED_ORDER(n)**, where _n_ is the order in which the position was explicitly constrained. Note that the constrained order of a position changes if you reset previously constrained positions to be implicitly constrained, using [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_RESET_POSITION_TO_IMPLICIT_CONSTRAINTS`](../../Reference/dmr/MdmrControlStringModel.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControlStringModel.md) (for example, in [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md)), [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md) will return [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md) to the [`InquireType`](../../Reference/dmr/MdmrInquireStringModel.md) parameter.

## Parameters

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context that contains the string model about which to inquire. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `StringModelLabelOrIndex` *(in, AIL_INT64)*

Specifies the string model about which to inquire. Set this parameter to one of the values below:

*For specifying the string model*

| Value | Description |
| --- | --- |
| `M_STRING_INDEX` | Specifies to inquire about the string model by indicating its index. |
| `M_STRING_LABEL` | Specifies to inquire about the string model by indicating its label. |

### `Position` *(in, AIL_INT64)*

Specifies how to inquire about the string model. Set this parameter to one of the values below:

*For specifying how to inquire about the string model*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to inquire about a global setting of a string model. |
| `M_POSITION_CONSTRAINED_ORDER` | Specifies to inquire about an explicitly constrained position in the string model by indicating the order in which the position was explicitly constrained. |
| `M_POSITION_IN_STRING` | Specifies to inquire about an explicitly constrained position in the string model by indicating its position.

If you specify a position that is not explicitly constrained, you will get an error. To inquire about the default constraints for the positions in the string model, you must set this parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md). |

### `PermittedCharEntry` *(in, AIL_INT64)*

Specifies the permitted character about which to inquire. SureDotOCR keeps track of each permitted character that you specify as an entry ([`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md)). The [`PermittedCharEntry`](../../Reference/dmr/MdmrInquireStringModel.md) parameter allows you to inquire about a specific entry.

*For specifying the permitted character*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not required. |
| `Value >= 0` | Specifies the permitted character about which to inquire. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of inquire to perform.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdmrInquireStringModel`](../../Reference/dmr/MdmrInquireStringModel.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a global setting of a string model

To inquire about a global setting of a string model, set the [`InquireType`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to one of the values below. Unless otherwise specified, set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to the label or index of a string model, set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md), and set the [`PermittedCharEntry`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md).

---

### `M_CHAR_ACCEPTANCE`

Inquires the acceptance level for the score of the string's characters.

| Value | Description |
| --- | --- |
| *(see [`M_CHAR_ACCEPTANCE`](Reference/dmr/MdmrControlStringModel.md))* |  |

---

### `M_NUMBER_OF_CONSTRAINED_POSITIONS`

Inquires the number of explicitly constrained positions. These are set with [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of explicitly constrained positions. |

---

### `M_STRING_ACCEPTANCE`

Inquires the acceptance level for the string's score.

| Value | Description |
| --- | --- |
| *(see [`M_STRING_ACCEPTANCE`](Reference/dmr/MdmrControlStringModel.md))* |  |

---

### `M_STRING_CERTAINTY`

Inquires the certainty level for the string's score.

| Value | Description |
| --- | --- |
| *(see [`M_STRING_CERTAINTY`](Reference/dmr/MdmrControlStringModel.md))* |  |

---

### `M_STRING_INDEX_VALUE`

Inquires the index of the string model. Set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to the string model's label.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the string model you are referring to does not exist in the context. |
| `Value >= 0` | Specifies the index. |

---

### `M_STRING_LABEL_VALUE`

Inquires the label of the string model. Set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to the string model's index.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label. |

---

### `M_STRING_RANK`

Inquires the order in which to read a string, relative to the other strings to read.

| Value | Description |
| --- | --- |
| *(see [`M_STRING_RANK`](Reference/dmr/MdmrControlStringModel.md))* |  |

---

### `M_STRING_SIZE_MAX`

Inquires the maximum number of characters in the string.

| Value | Description |
| --- | --- |
| *(see [`M_STRING_SIZE_MAX`](Reference/dmr/MdmrControlStringModel.md))* |  |
| `M_DEFAULT` | Same as [`M_INVALID`](../../Reference/dmr/MdmrInquireStringModel.md). |
| `M_INVALID` | Specifies an invalid maximum number of characters. Set the maximum number of characters to a valid value, using [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md). |

---

### `M_STRING_SIZE_MIN`

Inquires the minimum number of characters in the string.

| Value | Description |
| --- | --- |
| *(see [`M_STRING_SIZE_MIN`](Reference/dmr/MdmrControlStringModel.md))* |  |

### For inquiring about constraints for the different positions in the string model

To inquire about constraints for the different positions in the string model, set the [`InquireType`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to one of the values below. In this case, set the [`StringModelLabelOrIndex`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to the label or index of a string model.  To inquire about the default constraints for the positions in the string model, set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireStringModel.md), unless otherwise specified. To inquire about explicit constraints for a position, set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to the required position, unless otherwise specified.

---

### `M_CHAR_LIST`

Inquires the names of the characters that can be read. This is returned as a regular string. It consists of all characters including the terminating null character. Characters can come from one or any font ([`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrInquireStringModel.md)).  [`M_CHAR_LIST`](../../Reference/dmr/MdmrInquireStringModel.md) is set as a permitted character constraint ([`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md)). There can be many permitted character constraint entries for a single character position. To specify which to inquire about, use the [`PermittedCharEntry`](../../Reference/dmr/MdmrInquireStringModel.md) parameter.

| Value | Description |
| --- | --- |
| `CharName` | Specifies the character names. Retrieving an empty string ("") indicates that any character can be read. |

---

### `M_CONSTRAINED_ORDER`

Inquires the order in which the position was explicitly constrained. Set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to **M_POSITION_IN_STRING()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the constrained order. This value will be less than [`M_NUMBER_OF_CONSTRAINED_POSITIONS`](../../Reference/dmr/MdmrInquireStringModel.md). |

---

### `M_FONT_LABEL_VALUE`

Inquires the label of the font from which the character to read must come. This is set as a permitted character constraint ([`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md)). There can be many such permitted character constraint entries (allowable fonts) for a single character position. To specify which to inquire about, use the [`PermittedCharEntry`](../../Reference/dmr/MdmrInquireStringModel.md) parameter.  To inquire the corresponding font index, call [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) with [`M_FONT_INDEX_VALUE`](../../Reference/dmr/MdmrInquireFont.md) and the label value returned by [`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrInquireStringModel.md).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the character can come from any font in the context. |
| `Value > 0` | Specifies the label of the font. If SureDotOCR cannot return the requested information (no such font exists in the context), you will get an error. |

---

### `M_IS_OPTIONAL`

Inquires whether a position in a string model is optional. Optional positions can be skipped if their constraints cannot be met (up to a maximum of [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrInquireStringModel.md)-[`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrInquireStringModel.md) skips).

| Value | Description |
| --- | --- |
| *(see [`M_IS_OPTIONAL`](Reference/dmr/MdmrControlStringModel.md))* |  |

---

### `M_NUMBER_OF_PERMITTED_CHARS_ENTRIES`

Inquires the number of permitted character entries. This is the number of permitted characters specified with [`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md). Having no permitted character entries indicates you are reading any character from any font (initial default behavior).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of permitted character entries. |

---

### `M_POSITION`

Inquires the position within the string model, corresponding to the order in which the position was explicitly constrained. Set the [`Position`](../../Reference/dmr/MdmrInquireStringModel.md) parameter to **M_POSITION_CONSTRAINED_ORDER()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the position. |

---

### `M_TYPE`

Inquires the type of characters that can be read. This is set as a permitted character constraint ([`M_ADD_PERMITTED_CHARS_ENTRY`](../../Reference/dmr/MdmrControlStringModel.md)). There can be many such allowable types of characters for a position. To specify the one about which to inquire, use the [`PermittedCharEntry`](../../Reference/dmr/MdmrInquireStringModel.md) parameter.  If you are reading an explicitly defined list of characters, use [`M_CHAR_LIST`](../../Reference/dmr/MdmrInquireStringModel.md) to inquire the names of those characters.

| Value | Description |
| --- | --- |
| *(see [`M_ADD_PERMITTED_CHARS_ENTRY`](Reference/dmr/MdmrControlStringModel.md))* |  |

### Combination Constants — For inquiring about the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character. All strings have the terminating null character, even though you need not explicitly list it when specifying the string.

### Combination Constants — For explicitly inquiring about hexadecimal 16-bit Unicode (UTF-16) character name information

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the hexadecimal 16-bit Unicode (UTF-16) character name information.

The character name of the letter A is "A" (its native name) or "\x0041", which is its representation in hexadecimal format beginning with "\x". Characters between 0 and 127 are considered Basic Latin. A character such as the smiley face (☺, or "\x263A" in hexadecimal) is beyond the Basic Latin range.  In a Unicode environment, the character representations are returned (for example, the smiley face), unless you explicitly specify one of the below combination values (for example, to obtain "\x263A" instead of the actual smiley face). Retrieving information about character names in hexadecimal might be useful if you want to convert your data into ASCII.  Note that the string size information can change when these combination values are added. For example, if the first four characters in the font are "abc☺", using [`M_CHAR_LIST`](../../Reference/dmr/MdmrInquireStringModel.md) + [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrInquireStringModel.md) returns "b" for the character at index 1, and "\x263A" for the character at index 3. Consequently, the string sizes are 2 and 7, respectively (inquired with [`M_CHAR_LIST`](../../Reference/dmr/MdmrInquireStringModel.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrInquireStringModel.md) + [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrInquireStringModel.md)). If [`M_HEX_UTF16_FOR_ALL`](../../Reference/dmr/MdmrInquireStringModel.md) is specified, the preceding example returns "\x0062" and "\x263A" for the respective character names, and 7 for each string size. Recall that the null-terminating character is included in the string size.

| Value | Description |
| --- | --- |
| `M_HEX_UTF16_FOR_ALL` | Retrieves results with the name of all characters in hexadecimal 16-bit Unicode (UTF-16) format. |
| `M_HEX_UTF16_FOR_NON_BASIC_LATIN` | Retrieves results with the names of characters that fall out of the Basic Latin range, in hexadecimal 16-bit Unicode (UTF-16) format. |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring if an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported for the font currently being inquired.

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

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
