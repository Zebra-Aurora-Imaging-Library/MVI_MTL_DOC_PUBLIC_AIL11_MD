---
doctype: Reference
module: dmr
function: MdmrImportFont
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dmr / MdmrImportFont"
---

# MdmrImportFont

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

> Import characters from a font file into a new or existing font of a SureDotOCR context.

## Syntax

```c
void MdmrImportFont(
    AIL_CONST_TEXT_PTR FileName,          //in
    AIL_INT64          FileFormat,        //in
    AIL_ID             ContextDmrId,      //out
    AIL_INT64          FontLabelOrIndex,  //in
    AIL_CONST_TEXT_PTR CharList,          //in
    AIL_INT64          ControlFlag        //in
)
```

## Description

This function imports characters from a font file into a new or existing font of a SureDotOCR context. The fonts of a context contain the characters that string models use to read dot-matrix text. Fonts should only contain characters you want to read.

For easier use with other Aurora Imaging Library software products, you should import characters from a SureDotOCR font file. This is a text file saved with the extension MDMRF. The file must be in UTF-16 (little-endian order) with a BOM (Byte Order Mark) indicating this; the BOM value is U+FEFF. You can also import a font from a SureDotOCR font file exported from a SureDotOCR context, using[`MdmrExportFont`](../../Reference/dmr/MdmrExportFont.md).

The content of SureDotOCR font files must adhere to the expected format. Below is an example of how the character 'A' should appear in a SureDotOCR font file.

*[Image: MdmrMDMRFReference.png]*

SureDotOCR font files should have "File Representation" and "AIL_DOT_FONT" in lines 1 and 2. Lines 4 and greater should specify the characters; separate each with a blank line. Every character should begin with the word "CharValue" and its name. Names can be in UTF-16 byte hexadecimal by beginning them with "\x" (for example, 'A' would use "\x0041"). The character's dot-matrix must be below the "CharValue" line. Use "FF" for dots that represent the character; otherwise use "00". The dot-matrix of the font's characters should be identical to the dot-matrix of the characters in the strings to read. Every dot-matrix in a font must have the same number of columns and rows.

Aurora Imaging Library installs SureDotOCR console-based font utilities, such as _DmrEditFontFile_ (accessible from the Aurora Imaging Library Example Launcher). Predefined SureDotOCR font files (such as _*.mdmrf_ files) are typically distributed with Aurora Imaging Library and can be found in the installed Contexts folder (for example, _C:\Program Files\Aurora Imaging Library\11\Contexts\"_). Use them to create and update fonts. In this case, file and content requirements are automatically respected; ensure that you maintain these requirements as you develop your fonts. For more information, see [Fonts](../../UserGuide/C15_SureDotOCR/S04_Fonts.md).

When you call [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) to add a new font (or to add characters to an empty font), the dimensions of the characters' dot-matrix represent the dimensions of the dot-matrix template of the font. Subsequent characters that you add to the font must use these dimensions or you will get an error. To inquire the dimensions, call [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) with [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrInquireFont.md) and [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrInquireFont.md). You can also use this function to inquire about other aspects of the font.

To delete a font from a context, or to delete a character from a font, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md)or [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md), respectively. You can also use these functions to set font-related control types, add an empty font to a context, or add characters to a font (or modify existing characters) by defining their representations in an array.

> **Note:** Fonts should only contain characters you want to read. Delete all unnecessary characters.

A SureDotOCR font file must be for a single font. To import multiple fonts, call [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) multiple times. Modifying a context's fonts can affect which strings are read in the target image. You must preprocess the SureDotOCR context after you have finished adjusting a context's fonts and before calling [`MdmrRead`](../../Reference/dmr/MdmrRead.md). To know if a context needs to be preprocessed, call [`MdmrInquire`](../../Reference/dmr/MdmrInquire.md) with [`M_PREPROCESSED`](../../Reference/dmr/MdmrInquire.md).

You cannot represent certain characters, such as null or the standard space ("0x0020"), in a font file. To establish the distance between characters in a target string that represents a space, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the SureDotOCR font file (MDMRF) from which to import the font. The function internally handles the opening and closing of the file. To specify the file name and path of a SureDotOCR font file, set this parameter to one of the values below:

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which you can interactively specify the drive, directory, and name of a SureDotOCR font file. |
| `"FileName"` | Specifies the drive, directory, and name of a SureDotOCR font file (for example, _"C:\mydirectory\MySweetSweetFont.mdmrf"_).

To specify a SureDotOCR font file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\MySweetSweetFont.mdmrf"`). |

### `FileFormat` *(in, AIL_INT64)*

Specifies the format of the file from which to import the font. Set this parameter to the value below.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_DMR_FONT_FILE` | Specifies a SureDotOCR font file format (MDMRF). |

### `ContextDmrId` *(out, AIL_ID)*

Specifies the identifier of the SureDotOCR context to which you are importing the characters. The context must have been previously allocated on the system using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).

### `FontLabelOrIndex` *(in, AIL_INT64)*

Specifies the label or index of an already existing font, or the addition of a new font. Set this parameter to one of the values below:

*For specifying an already existing font, or the addition of a new font*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NEW_LABEL`](../../Reference/dmr/MdmrImportFont.md). |
| `M_FONT_INDEX` | Specifies an already existing font in the context by indicating its index. SureDotOCR imports the new characters to the specified font. |
| `M_FONT_LABEL` | Specifies to add a new font or to update an already existing font by indicating a label. If the specified label refers to a font in the context, SureDotOCR imports the new characters to it. If the specified label does not refer to a font in the context, SureDotOCR adds a new font to the context with that label, and imports the new characters to it. |
| `M_NEW_LABEL` | Specifies to add the imported characters as a new font in the context and to automatically assign it a label. To inquire about the label, use [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) with [`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrInquireFont.md). To change the label, use [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrControlFont.md). |

### `CharList` *(in, AIL_CONST_TEXT_PTR)*

Specifies the characters to import from the SureDotOCR font file. Set this parameter to one of the values below:

*For specifying the characters to import*

| Value | Description |
| --- | --- |
| `M_NULL` | Same as [`M_IMPORT_ALL_CHARS`](../../Reference/dmr/MdmrImportFont.md). |
| `M_IMPORT_ALL_CHARS` | Imports all characters. |
| `"CharList"` | Specifies a null-terminated string indicating the names of the characters to import. Character names must refer to specific letters (such as 'o'), digits (such as '0'), and punctuation marks (such as '%') in the font. You cannot indicate a space.

The specified string can contain one or more character names. List them all without separators. For example, to specify the twenty-second, the ninth, and the third uppercase letters of the alphabet, use the string "VIC". Specifying multiple characters is equivalent to calling this function multiple times, and listing one character each time.

You can list character names in hexadecimal format beginning with "\x". This is necessary if your data is in an ASCII format and you want Unicode characters beyond the Basic Latin range. For example, Basic Latin does not include the smiley face character; to specify it, use "\x263A". You can also list a string of character names with mixed notation; in this case, use "\\x" instead (for example, "VIC\\x263A"). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to handle importing characters that have the same name as characters already in the font. A font in a context cannot have multiple characters with the same name. Set this parameter to one of the values below:

*For specifying how to handle characters with the same name*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_NO_OVERWRITE`](../../Reference/dmr/MdmrImportFont.md). |
| `M_NO_OVERWRITE` | Specifies to not overwrite characters already in the font with characters that you are importing, when the characters have the same name. |
| `M_OVERWRITE` | Specifies to overwrite characters already in the font with characters that you are importing, when the characters have the same name. |
