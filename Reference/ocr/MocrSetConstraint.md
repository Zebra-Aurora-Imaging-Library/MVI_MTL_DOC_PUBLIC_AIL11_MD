---
doctype: Reference
module: ocr
function: MocrSetConstraint
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrSetConstraint"
---

# MocrSetConstraint

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

> Set character position constraints.

## Syntax

```c
void MocrSetConstraint(
    AIL_ID             FontContextOcrId,  //out
    AIL_INT            CharPos,           //in
    AIL_INT64          CharPosType,       //in
    AIL_CONST_TEXT_PTR CharValidString    //in
)
```

## Description

This function specifies which character value constraints to apply to each position of the strings in the target image, when using the specified OCR font context. This function specifies which characters can appear at given positions in the string to be read, and associates these constraints with the OCR font context. Using this function increases both speed and reliability as long as the strings in the target image have a known format and obey certain grammatical rules.

Note that constraints are set in regards to the character's position in the string. When dealing with multiple strings, the same constraints are applied to each string.

If you change any constraints, use [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md) to speed up any following read or verify operation.

## Parameters

### `FontContextOcrId` *(out, AIL_ID)*

Specifies the OCR font context identifier with which to associate the constraints.

### `CharPos` *(in, AIL_INT)*

Specifies the character position in the string for which a constraint is being set. Valid values range from zero to the length of the string -1.

### `CharPosType` *(in, AIL_INT64)*

Specifies the type of character that can appear at the specified string position.

*For specifying the type of character that is accepted*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that all characters present in the font are valid. |
| `M_DIGIT` | Specifies that only characters "0" to "9" (ASCII codes 48 to 57) are valid. |
| `M_LETTER` | Specifies that only characters "A" to "Z" and "a" to "z" (ASCII codes 65 to 90 and 97 to 122) are valid. |

*For*

| Value | Description |
| --- | --- |
| `M_LOWERCASE` | Specifies that only characters "a" to "z" (ASCII codes 97 to 122) are valid. |
| `M_UPPERCASE` | Specifies that only characters "A" to "Z" (ASCII codes 65 to 90) are valid. |

### `CharValidString` *(in, AIL_CONST_TEXT_PTR)*

Specifies that any character matching the given [`CharPosType`](../../Reference/ocr/MocrSetConstraint.md) will be accepted as valid, or explicitly lists valid characters for the specified position.

*For specifying whether all characters matching the CharPosType will be accepted*

| Value | Description |
| --- | --- |
| `M_NULL` | Sets all font characters matching the [`CharPosType`](../../Reference/ocr/MocrSetConstraint.md) parameter value as valid. |
| `"StringOfCharacters"` | Specifies an explicit list of valid characters for the specified position. This string must be null-terminated. The accepted characters must match the [`CharPosType`](../../Reference/ocr/MocrSetConstraint.md) parameter definition and must exist in the font. |
