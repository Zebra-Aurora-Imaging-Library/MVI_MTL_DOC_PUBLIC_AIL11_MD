---
doctype: Reference
module: ocr
function: MocrCopyFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrCopyFont"
---

# MocrCopyFont

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

> Copy a font character to or from an image buffer.

## Syntax

```c
void MocrCopyFont(
    AIL_ID             ImageBufId,        //in
    AIL_ID             FontContextOcrId,  //in
    AIL_INT64          Operation,         //in
    AIL_CONST_TEXT_PTR CharListString     //in
)
```

## Description

This function copies a character representation of one, or many, font characters to/from the specified image buffer. This buffer can then be used to initialize, change or obtain a visual representation of the font's characters.

If character representations from the image buffer are being copied to the OCR font context, the OCR font context must be large enough to hold the representations of all the specified characters. You can use [`MocrInquire`](../../Reference/ocr/MocrInquire.md) to determine the maximum number of characters that can be stored in the font and the size of each character.

The characters are stored in contiguous cells in the image buffer.

The character representations are read from either a font or an image buffer, assuming that the source image is a grid of character representations matching the specified font size. The character representations are read from left to right, and the characters in [`CharListString`](../../Reference/ocr/MocrCopyFont.md) must exist in the font.

If the character representation being added already exists in the font, it will be replaced by the new character representation. If the character representation does not already exist in the font, it is appended to the end of the font. If the maximum string length, as set using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md), is equal to or greater than the number of characters in the font (use [`MocrInquire`](../../Reference/ocr/MocrInquire.md) with [`M_CHAR_NUMBER_IN_FONT`](../../Reference/ocr/MocrInquire.md)), an error will occur and the copy operation will fail.

Note that it is crucial that the character representation respects the foreground value as set in [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md). If the foreground value of the character representation does not match, the font will be unusable.

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer to/from which characters are copied.

### `FontContextOcrId` *(in, AIL_ID)*

Specifies the identifier of the OCR font context to/from which the characters are copied.

### `Operation` *(in, AIL_INT64)*

Specifies the direction of the copy operation. This parameter can be set to one of the following values:

*For specifying the direction of the copy operation*

| Value | Description |
| --- | --- |
| `M_COPY_FROM_FONT` | Copies character(s) from a font context to an image buffer.

Note that the maximum number of characters that can be copied from a font context to an image buffer is limited to the number of characters in the font.

Note that The buffer height must be at least the character Cell size Y multiplied by the number of lines necessary to hold all the characters. For more details see [`MocrInquire`](../../Reference/ocr/MocrInquire.md) with [`M_CHAR_CELL_SIZE_Y`](../../Reference/ocr/MocrInquire.md). |
| `M_COPY_TO_FONT` | Copies character(s) from an image buffer to a font context. |

*For the Operations parameter*

| Value | Description |
| --- | --- |
| `M_ALL_CHAR` | Copies all font characters to/from the image buffer, stacked from left to right.

Note that when [`M_ALL_CHAR`](../../Reference/ocr/MocrCopyFont.md) is added, the [`CharListString`](../../Reference/ocr/MocrCopyFont.md) parameter should be set to [`M_NULL`](../../Reference/ocr/MocrCopyFont.md) and is ignored. |

*For*

| Value | Description |
| --- | --- |
| `M_SORT` | Sorts font by its ASCII character values. |

### `CharListString` *(in, AIL_CONST_TEXT_PTR)*

Specifies a string containing the list of characters to be copied.

*For specifying the string*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter. This is the only possible setting when copying all characters with [`M_ALL_CHAR`](../../Reference/ocr/MocrCopyFont.md). |
| `"CharListString"` | Specifies the string containing the list of characters to be copied. This string must be null-terminated. Character representations copied to the font are associated with the characters, and their ASCII code, in the string. If a character exists in both the list and the font, the character representation will be overwritten with the new character representation. New characters will be added to the font provided that there is sufficient space in the OCR font context.

If [`M_ALL_CHAR`](../../Reference/ocr/MocrCopyFont.md) is added to the [`Operation`](../../Reference/ocr/MocrCopyFont.md) parameter, this setting cannot be used. |
