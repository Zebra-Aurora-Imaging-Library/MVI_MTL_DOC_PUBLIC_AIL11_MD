---
doctype: Reference
module: ocr
function: MocrImportFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrImportFont"
---

# MocrImportFont

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

> Import character representations into an OCR font context.

## Syntax

```c
void MocrImportFont(
    AIL_CONST_TEXT_PTR FileName,         //in
    AIL_INT64          FileFormat,       //in
    AIL_INT64          Operation,        //in
    AIL_CONST_TEXT_PTR CharListString,   //in
    AIL_ID             FontContextOcrId  //out
)
```

## Description

This function imports character representations from a file to initialize or overwrite those of an existing OCR font context. This function's main use is to initialize custom fonts.

Font character representations can come from either a grayscale image or a text file.

When taken from an image file, character representations are associated with the ASCII characters in the string, specified by [`CharListString`](../../Reference/ocr/MocrImportFont.md). The image file must be a grid of character representations matching the specifications of the font. The character representations are read from left to right and top to bottom, and the number of characters in the source image must equal the number of characters in the string, specified by [`CharListString`](../../Reference/ocr/MocrImportFont.md).

When taken from an ASCII file, font character representations must be presented as in the table below. *[Image: CharValue_65.png]*

| Row | Description |
| --- | --- |
| 01 | Specifies ASCII file format. |
| 02 | Blank row. |
| 03 | Specifies the start of a new character representation and its associated (generally ASCII) character. |
| 04 to 36 | Specifies the alpha-numerical representation of the character. |
| 37 | Blank row. |
| 38 | Specifies the start of a new character representation and its associated (generally ASCII) character. |
| 39 to 71 | Specifies the alpha-numerical representation of the character. |
| etc. | This pattern is repeated for every character in the font. |

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file from which to import the character representation. The function handles (internally) the opening and closing of the file.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `FileFormat` *(in, AIL_INT64)*

Specifies how the data is stored in the file. This parameter can be set to one of the following values:

*For specifying how the data is stored*

| Value | Description |
| --- | --- |
| `M_AIL` | Specifies an Aurora Imaging Library format image file.

Note that only one-band images can be used. |
| `M_FONT_ASCII` | Specifies a user-drawn ASCII file.

Note that the [`CharListString`](../../Reference/ocr/MocrImportFont.md) must be set to [`M_NULL`](../../Reference/ocr/MocrImportFont.md). |
| `M_TIFF` | Specifies a TIFF format image file.

Note that only one-band images can be used. |

### `Operation` *(in, AIL_INT64)*

Specifies the type of import operation to be performed. This parameter must be set to the following value:

*For specifying the type of import operation*

| Value | Description |
| --- | --- |
| `M_LOAD_CHARACTER` | Specifies that the font character representations should be imported. |

### `CharListString` *(in, AIL_CONST_TEXT_PTR)*

Specifies the ASCII characters to associate with the font character representations in the file. This parameter can be set to one of the following values:

*For specifying the ASCII characters*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this parameter is ignored. Use this setting when the [`FileFormat`](../../Reference/ocr/MocrImportFont.md) parameter is set to [`M_FONT_ASCII`](../../Reference/ocr/MocrImportFont.md). |
| `"StringOfCharacters"` | Specifies the list of ASCII characters to associate with the font character representations in the image file. This string must be null-terminated. Note that the number of characters in this list must match the number of font character representations present in the source file. |

### `FontContextOcrId` *(out, AIL_ID)*

Specifies the identifier of the OCR font context.
