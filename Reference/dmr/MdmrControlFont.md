---
doctype: Reference
module: dmr
function: MdmrControlFont
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrControlFont"
---

# MdmrControlFont

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

> Control a global setting of a font in a SureDotOCR context, or control a character in a font.

## Syntax

```c
void MdmrControlFont(
    AIL_ID             ContextDmrId,      //out
    AIL_INT64          FontLabelOrIndex,  //in
    AIL_INT64          CharIndex,         //in
    AIL_CONST_TEXT_PTR CharNamePtr,       //in
    AIL_INT64          ControlType,       //in
    AIL_DOUBLE         ControlValue,      //in
    const void *       ControlValuePtr    //in
)
```

## Description

This function allows you to control a global setting of a font in a SureDotOCR context, or to control a character in a font. This includes adding characters to, and deleting characters from, a font. To add a font to a SureDotOCR context, use [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_FONT_ADD`](../../Reference/dmr/MdmrControl.md) (this adds an empty font). For a successful read operation, the context must have at least one font with one character. The characters held by a font are represented by a dot-matrix. String models use the dot-matrix character representations in the font to define the text to read.

Adding or modifying characters with this function requires specifying their dot-matrix representation in an array. You can also add or modify characters by calling [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) with a SureDotOCR font file (more commonly done). To inquire about fonts, use [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md).

You must preprocess the SureDotOCR context after modifying its fonts and before calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md). To know if a context needs to be preprocessed, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_PREPROCESSED`](../../Reference/dmr/MdmrInquire.md).

## Parameters

### `ContextDmrId` *(out, AIL_ID)*

Specifies the identifier of the SureDotOCR context that contains the fonts to control. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `FontLabelOrIndex` *(in, AIL_INT64)*

Specifies the font (one or all) to control. Set this parameter to one of the values below:

*For specifying the font*

| Value | Description |
| --- | --- |
| `M_FONT_INDEX` | Specifies to control the font by indicating its index. |
| `M_FONT_LABEL` | Specifies to control the font by indicating its label. |
| `M_ALL` | Specifies to control all fonts. |

### `CharIndex` *(in, AIL_INT64)*

Specifies the index of a character in the font, if required. Set this parameter to one of the values below:

*For specifying the index of a character*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the index of a character is not required. |
| `M_ALL` | Specifies all characters. Only specify this value for [`M_CHAR_DELETE`](../../Reference/dmr/MdmrControlFont.md). |
| `Value >= 0` | Specifies the index of a character. |

### `CharNamePtr` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name of a character in the font, if required. Set this parameter to one of the values below:

*For specifying the name of the character*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the name of the character is not required. |
| `"CharName"` | Specifies the name of the character. For example, 'A' would be the name that identifies the first uppercase letter of the English alphabet. When using the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to specify the name of a character, the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md)parameter must be set to [`M_DEFAULT`](../../Reference/dmr/MdmrControlFont.md).

When working with data in an ASCII format, you can specify Unicode character names; these names must be in hexadecimal format and begin with "\x". This is required for specifying character names beyond the Basic Latin range. For example, Basic Latin does not include the smiley face character; to specify it, use "\x263A". When working in a Unicode environment, you can directly specify the smiley face as the name of the character.

Character names must be unique among all character names in the font. Specifying the name of a character that already exists causes an error. To avoid unpredictable results, character names should match the dot-matrix that they represent. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

### `ControlValuePtr` *(in, *void)*

Specifies the address which contains more information about the setting's new value.

## Parameter Associations

### For controlling a global setting of a font

The following [`ControlType`](../../Reference/dmr/MdmrControlFont.md) and corresponding [`ControlValue`](../../Reference/dmr/MdmrControlFont.md) and [`ControlValuePtr`](../../Reference/dmr/MdmrControlFont.md) parameter settings are used to control a global setting of a font. Unless otherwise specified, set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrControlFont.md) parameter to the label or index of a font (one or all), set the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md)parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrControlFont.md), and set the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to [`M_NULL`](../../Reference/dmr/MdmrControlFont.md).

---

### `M_FONT_LABEL_VALUE`

Modifies the label value of the font. Set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrControlFont.md) parameter to a specific font. Specifying all fonts causes an error.

| Value | Description |
| --- | --- |
| `0 < Value < 2097152` | Specifies the font's new label value, as an _integer_. The label must be unique among all font labels in the context. |

---

### `M_FONT_SIZE_COLUMNS`

Sets the number of columns in the font's dot-matrix template. Only specify this value for an empty font (no characters).  The dot-matrix template defines the grid within which to represent every character in the font.  Fonts are typically added with [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). In this case, the dimensions of the characters' dot-matrix in the font file (MDMRF) represent the dimensions of the dot-matrix template of the font.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of columns, as an _integer_. |

---

### `M_FONT_SIZE_ROWS`

Sets the number of rows in the font's dot-matrix template. Only specify this value for an empty font (no characters).  The dot-matrix template defines the grid within which to represent every character in the font.  Fonts are typically added with [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). In this case, the dimensions of the characters' dot-matrix in the font file (MDMRF) represent the dimensions of the dot-matrix template of the font.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of rows, as an _integer_. |

### For controlling a font's characters

The following [`ControlType`](../../Reference/dmr/MdmrControlFont.md) and corresponding [`ControlValue`](../../Reference/dmr/MdmrControlFont.md) and [`ControlValuePtr`](../../Reference/dmr/MdmrControlFont.md) parameter settings are used to control a font's characters. Unless otherwise specified, set the [`FontLabelOrIndex`](../../Reference/dmr/MdmrControlFont.md) parameter to the label or index of a font (one or all), and set either the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md)parameter to a character index or the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to the name of a character.

---

### `M_CHAR_ADD`

Adds a character to the font. To add multiple characters, call this function multiple times. SureDotOCR assigns an index to each character, starting with 0. Use the[`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to specify the name of the character and set the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md) parameter to [`M_DEFAULT`](../../Reference/dmr/MdmrControlFont.md). Fonts should only contain characters you want to read.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_CHAR_DELETE`

Deletes characters in a font.  To delete a specific character in a font, set the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md) parameter to the index of the character to delete, or set the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to the name of the character to delete. To delete all characters in a font, set the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md)parameter to [`M_ALL`](../../Reference/dmr/MdmrControlFont.md) and the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter to [`M_NULL`](../../Reference/dmr/MdmrControlFont.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_CHAR_NAME`

Renames a character in a font.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |
| `"CharName"` | Specifies the character's new name. To indicate the character to rename, use either the [`CharIndex`](../../Reference/dmr/MdmrControlFont.md)parameter or the [`CharNamePtr`](../../Reference/dmr/MdmrControlFont.md) parameter. Character names must be unique among all character names in the font. To avoid unpredictable results, character names should match the dot-matrix that represents them. |

---

### `M_CHAR_TEMPLATE`

Redefines the dot-matrix of a character in a font.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |
