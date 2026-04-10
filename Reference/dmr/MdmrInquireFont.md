---
doctype: Reference
module: dmr
function: MdmrInquireFont
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrInquireFont"
---

# MdmrInquireFont

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

> Inquire about a global setting of a font in a SureDotOCR context, or inquire about a character in a font.

## Syntax

```c
AIL_INT MdmrInquireFont(
    AIL_ID             ContextDmrId,      //in
    AIL_INT64          FontLabelOrIndex,  //in
    AIL_INT64          CharIndex,         //in
    AIL_CONST_TEXT_PTR CharNamePtr,       //in
    AIL_INT64          InquireType,       //in
    void *             UserVarPtr         //out
)
```

## Description

This function inquires about a global setting of a font in a SureDotOCR context, or inquires about a character in a font. Font settings can be specified with [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md).

Characters you are inquiring about are typically imported into a font with [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). You can also inquire about characters added to a font with [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/dmr/MdmrControlFont.md) (for example, in [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md)), [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) will return [`M_DEFAULT`](../../Reference/dmr/MdmrInquireFont.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/dmr/MdmrInquireFont.md) to the [`InquireType`](../../Reference/dmr/MdmrInquireFont.md) parameter.

## Parameters

### `ContextDmrId` *(in, AIL_ID)*

Specifies the identifier of the SureDotOCR context that contains the font about which to inquire. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `FontLabelOrIndex` *(in, AIL_INT64)*

Specifies the font about which to inquire. Set this parameter to one of the values below:

*For specifying the font*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies to inquire about the font by indicating its index. |
| `M_FONT_LABEL` | Specifies to inquire about the font by indicating its label. |

### `CharIndex` *(in, AIL_INT64)*

Specifies the index of a character in the font, if required. Set this parameter to one of the values below:

*For specifying the index of a character*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that this parameter is not required. |
| `Value >= 0` | Specifies the index of a character. |

### `CharNamePtr` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of a character in the font, if required. Set this parameter to one of the values below:

*For specifying the name of the character*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is not required. |
| `"CharName"` | Specifies the character name. For example, 'A' is the name that identifies the first uppercase letter of the English alphabet. When using the [`CharNamePtr`](../../Reference/dmr/MdmrInquireFont.md) parameter to specify a character, the [`CharIndex`](../../Reference/dmr/MdmrInquireFont.md)parameter must be set to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireFont.md). |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of inquire to perform.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a global setting of a font

To inquire about a global setting of a font, set the [`InquireType`](../../Reference/dmr/MdmrInquireFont.md) parameter to one of the values below. Unless otherwise specified, set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrInquireFont.md) parameter to the label or index of a font, the [`CharIndex`](../../Reference/dmr/MdmrInquireFont.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireFont.md), and the [`CharNamePtr`](../../Reference/dmr/MdmrInquireFont.md) parameter to [`M_NULL`](../../Reference/dmr/MdmrInquireFont.md).

---

### `M_FONT_INDEX_VALUE`

Inquires the index value of a font. Set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrInquireFont.md) parameter to the font's label.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the font to which you are referring does not exist in the context. |
| `0 <= Value <= 255` | Specifies the index of the font. |

---

### `M_FONT_LABEL_VALUE`

Inquires the label value of the font. Set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrInquireFont.md) parameter to the font's index.

| Value | Description |
| --- | --- |
| `0 < Value < 2097152` | Specifies the font's label value. If SureDotOCR cannot return the requested information (no such font exists in the context), you will get an error. |

---

### `M_FONT_SIZE_COLUMNS`

Inquires the number of columns in the font's dot-matrix template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of columns, as an _integer_. |

---

### `M_FONT_SIZE_ROWS`

Inquires the number of rows in the font's dot-matrix template.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of rows, as an _integer_. |

---

### `M_FONT_SIZE_TEMPLATE`

Inquires the total number of dots possible in the font's dot-matrix template. The returned value corresponds to [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrInquireFont.md) * [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrInquireFont.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of dots. |

---

### `M_NUMBER_OF_CHARS`

Inquires the number of characters in the font. Characters are added with [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) or [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of characters. |

### Combination Constants — For inquiring the default value

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the default value of an inquire type (if supported), regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### For inquiring about a font's characters

To inquire about a font's characters, set the [`InquireType`](../../Reference/dmr/MdmrInquireFont.md) parameter to one of the values below. Unless otherwise specified, the [`FontLabelOrIndex`](../../Reference/dmr/MdmrInquireFont.md) parameter must be set to the label or index of a font, and either the [`CharIndex`](../../Reference/dmr/MdmrInquireFont.md)or [`CharNamePtr`](../../Reference/dmr/MdmrInquireFont.md) parameter must specify a character.

---

### `M_CHAR_INDEX_VALUE`

Inquires the index value of the character, based on its name. Set the [`CharIndex`](../../Reference/dmr/MdmrInquireFont.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrInquireFont.md) and the [`CharNamePtr`](../../Reference/dmr/MdmrInquireFont.md) parameter to the name of the character in the font.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the character to which you are referring does not exist in the font. |
| `Value >= 0` | Specifies the index of the character. |

---

### `M_CHAR_NAME`

Inquires the name of the character, based on its index. Set the [`CharIndex`](../../Reference/dmr/MdmrInquireFont.md)parameter to the index of a character in the font and the [`CharNamePtr`](../../Reference/dmr/MdmrInquireFont.md) parameter to [`M_NULL`](../../Reference/dmr/MdmrInquireFont.md).  [`M_CHAR_NAME`](../../Reference/dmr/MdmrInquireFont.md) is returned as a regular string. It consists of all characters including the terminating null character.

| Value | Description |
| --- | --- |
| `Value` | Specifies the name. |

---

### `M_CHAR_TEMPLATE`

Inquires the dot-matrix that represents the character in the font. The dot-matrix is returned as array data, organized one row contiguously after the other, where 255 (0xFF) represents a dot and 0 represents a blank.

| Value | Description |
| --- | --- |
| `Value` | Specifies the character's dot-matrix. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For inquiring the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character. All strings have the terminating null character, even though you need not explicitly list it when specifying the string.

### Combination Constants — For explicitly inquiring about hexadecimal 16-bit Unicode (UTF-16) character name information

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the hexadecimal 16-bit Unicode (UTF-16) character name information.

The character name of the letter A is "A" (its native name) or "\x0041", which is its representation in hexadecimal format beginning with "\x". Characters between 0 and 127 are considered Basic Latin. A character such as the smiley face (☺, or "\x263A" in hexadecimal) is beyond the Basic Latin range.  In a Unicode environment, the character representations are returned (for example, the smiley face), unless you explicitly specify one of the below combination values (for example, to obtain "\x263A" instead of the actual smiley face). Retrieving information about character names in hexadecimal might be useful if you want to convert your data into ASCII.  Note that the string size information can change when these combination values are added. For example, if the first four characters in the font are "abc☺", using [`M_CHAR_NAME`](../../Reference/dmr/MdmrInquireFont.md) + [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrInquireFont.md) returns "b" for the character at index 1, and "\x263A" for the character at index 3. Consequently, the string sizes are 2 and 7, respectively (inquired with [`M_CHAR_NAME`](../../Reference/dmr/MdmrInquireFont.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrInquireFont.md) + [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrInquireFont.md)). If [`M_HEX_UTF16_FOR_ALL`](../../Reference/dmr/MdmrInquireFont.md) is specified, the preceding example returns "\x0062" and "\x263A" for the respective character names, and 7 for each string size. Recall that the null-terminating character is included in the string size.

| Value | Description |
| --- | --- |
| `M_HEX_UTF16_FOR_ALL` | Retrieves results with the name of all characters in hexadecimal 16-bit Unicode (UTF-16) format. |
| `M_HEX_UTF16_FOR_NON_BASIC_LATIN` | Retrieves results with the names of characters that fall out of the Basic Latin range, in hexadecimal 16-bit Unicode (UTF-16) format. |

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

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
