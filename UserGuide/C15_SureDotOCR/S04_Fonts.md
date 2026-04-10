---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: Fonts
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / Fonts"
---

# Fonts

A font is a data structure, within a SureDotOCR context, that stores a list of characters, with each character's name and corresponding dot-matrix representation. A context must contain one or more fonts. A font must contain one or more characters. Only strings with characters represented by a font in the context can be read. For example, if you want to read a string with the number "1", the number "1" must be a character in a font.

Each character must be unique within a font. However the same character can be in multiple fonts. This typically means the fonts represent the characters differently. For example, in _FontBig_, the number "1" can be defined by a column of 10 dots, while in _FontEvenBigger_ the number "1" can be defined by a column of 15 dots. You can choose either of these fonts with which to read the number "1" that you want.

The following lists the most common operations performed on fonts and characters.

- Import characters from a font file into a new or existing font of a SureDotOCR context ([`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md)).
  In general, you should use Aurora Imaging CoPilot to create and update your own fonts. However, you can also use the console-based font utilities with the predefined SureDotOCR font files that Aurora Imaging Library installs; for more information, see [Console-based font utilities](S04_Fonts.md).
- Export fonts to a file ([`MdmrExportFont`](../../Reference/dmr/MdmrExportFont.md)).
- Delete fonts and characters ([`MdmrControl`](../../Reference/dmr/MdmrControl.md) and [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md)), and perform general control and inquire type operations on fonts and characters ([`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md), [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md), and [`MdmrName`](../../Reference/dmr/MdmrName.md)).
  > **Note:** Fonts should only contain characters that you want to read. Delete all unnecessary characters.
- Add and update fonts by programmatically defining character representations in an array ([`MdmrControl`](../../Reference/dmr/MdmrControl.md) and [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md)).

For font modifications to take effect, you must preprocess the context by calling [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md).

## Importing and exporting dot-matrix fonts

You can import characters from a font file into a new or existing font of a SureDotOCR context, using [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). You can import all characters ([`M_IMPORT_ALL_CHARS`](../../Reference/dmr/MdmrImportFont.md)) or an explicit list (for example, "123GO!"). You can, for example, call [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) multiple times to import, into one font, different characters from different fonts. If you try to import a character that already exists, you can choose to overwrite the existing character ([`M_OVERWRITE`](../../Reference/dmr/MdmrImportFont.md)), or you can keep the original ([`M_NO_OVERWRITE`](../../Reference/dmr/MdmrImportFont.md)).

You can also export a font with all its characters from a SureDotOCR context to a font file, using [`MdmrExportFont`](../../Reference/dmr/MdmrExportFont.md). Exported fonts can be imported into any SureDotOCR context.

For easier use with other Aurora Imaging software products (for example, Aurora Design Assistant or Aurora Imaging CoPilot), import and export characters using a SureDotOCR font file. This is essentially a text file with the SureDotOCR font file extension, MDMRF. To create and update these files, you should use the [console-based font utilities](S04_Fonts.md)with the predefined SureDotOCR font files that were installed with Aurora Imaging Library.

You can also create and update font files with a text editor and using the MDMRF extension when saving. Ensure the file is in UTF-16 (little-endian order) and starts with a byte order mark (BOM) that indicates this; the BOM value is U+FEFF. Although often supported, editors might not use the standard terminology, UTF-16. For example, you might have to save the file as Unicode or UCS-2 little-endian.

### Content of SureDotOCR font files

The content of all SureDotOCR font files must adhere to expected specifications. If all the characters in all the strings will be in italic (skewed), you should not add the italic version of the characters to the font. Instead, set the italic angle (skew) as described in [Italic angle](S05_String_models.md). Below is an example of how SureDotOCR expects the character 'A' to appear in a font:

*[Image: MdmrMDMRF.png]*

The following is a line-by-line description of what SureDotOCR files should contain.

| Line | Description |
| --- | --- |
| 1 | This line contains the text, "File Representation". |
| 2 | This line contains the text, "AIL_DOT_FONT". |
| 3 | This line is blank. |
| 4 and above | These lines specify the entry of every character in the font. - Each entry begins with the word "CharValue", a space, and the character name. The character's dot-matrix representation is on the next line. "FF" is for dots that represent the character; the rest use "00". Every dot-matrix of every character must contain at least one "FF", and have the same number of columns and rows. Lines beginning with the hash character ('#') are comments. They should not be within a character's dot-matrix and are typically used sparingly (only when they provide true value).<br/>  *[Image: MdmrMDMRFa.png]*<br/>  Blank lines separate multiple characters in a font. A character's "CharValue" name should properly identify its dot-matrix representation. For example, if the "CharValue" name is '1', its dot-matrix should not represent the letter 'A'. "CharValue" names should be unique within a font.<br/>- Character names can be in UTF-16 hexadecimal; these begin with "\x". Alternatively, you can enter the UTF-16 character directly (for example, the capital Greek letter PSI) if using Windows API functions with the UTF-16 macro, and your source code is saved as unicode.<br/>  *[Image: MdmrMDMRFb.png]*<br/>  Expressing characters in UTF-16 hexadecimal allows you to specify them directly in C++ string literals. This notation can also be useful if your data is in an ASCII format and you want to have Unicode characters beyond the Basic Latin range. When specifying the characters to read, you can seamlessly mix notations. For example, to read the characters "ABC", where just 'A' is represented in hexadecimal, you can call[`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_CHAR_LIST`](../../Reference/dmr/MdmrControlStringModel.md) and specify the character list as AIL_TEXT("\x0041") AIL_TEXT("BC").<br/>  Names for special surrogate pairs in UTF-16 hexadecimal are listed successively. For example, "CharValue \xD83D\xDE0A" specifies a smiley face character.<br/>  *[Image: MdmrMDMRFc.png]*<br/>- Dot-matrix fonts cannot represent certain characters, such as null or the standard space (0x0020). To establish the distance between characters in a target string that represents a space, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_SPACE_SIZE_MAX`](../../Reference/dmr/MdmrControl.md) and [`M_SPACE_SIZE_MIN`](../../Reference/dmr/MdmrControl.md). |

When you call [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) to add a new font to a context, the dimensions of the characters' dot-matrix represent the dimensions of the dot-matrix template of the font. The dot-matrix of every subsequent character that you add to the font must use these dimensions. To inquire about them, call [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md) with [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrInquireFont.md) and [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrInquireFont.md). If you delete all characters in a font, the dimensions of the new characters' dot-matrix re-sets the dimensions of the dot-matrix template of the font.

If you imported a font from a SureDotOCR font file, you can modify one of the imported font's character representations using an array with the character representation to modify, provided it adheres to the font's dot-matrix template. For more information, see [Programmatically managing fonts and characters](S04_Fonts.md).

## Console-based font utilities

You can interactively create and edit fonts, using Aurora Imaging CoPilot. Additionally, Aurora Imaging Library installs SureDotOCR console-based font utilities, such as _DmrEditFontFile_ (accessible from the Aurora Imaging Example Launcher), and predefined SureDotOCR font files located in the installed Contexts folder (for example, _*.mdmrf_ in _C:\Program Files\Aurora Imaging Library\11\Contexts_). You can alternatively use these utilities and files to interactively create and update your fonts. In this case, file and content requirements are automatically respected; ensure that you maintain these requirements as you develop your fonts.

| Console-based font utility | Why should I use this utility? |
| --- | --- |
| _DmrShowFontFile_ | To display the characters in a SureDotOCR font file. |
| _DmrEditFontFile_ | To interactively add or modify characters in a SureDotOCR font file that already exists. |
| _DmrGenFontFile_ | To interactively create a new SureDotOCR font file and add characters to that file. |

To create you own fonts, you would typically:

1. Use _DmrShowFontFile_ to view the predefined MDMRFs (font files).
2. Make a copy of the predefined MDMRFs that most resemble the fonts that you want to have in your SureDotOCR context. Be sure to rename the predefined MDMRFs before modifying them, so you do not overwrite the originals.
3. Use _DmrEditFontFile_ to interactively modify or add characters to the renamed MDMRFs.
   *[Image: MdmrInteractiveCharacterGrid.png]*
4. Call [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md) to import characters from a font file into a new or existing font of a SureDotOCR context.
5. Call [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_CHAR_DELETE`](../../Reference/dmr/MdmrControlFont.md) to remove unneeded characters from your fonts.

If necessary, you can open a renamed MDMRF file and modify it manually using a text-based editor that supports the required format (UTF-16), as previously discussed. This can be particularly convenient if you want to delete numerous characters from a renamed font. In this case, delete the characters before importing them with [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md).

## Dot-matrices in fonts and strings to read should be the same

The dot-matrix of each character in a font should be, dot for dot, the same as the dot-matrix of each character in the string to read. Even subtle discrepancies in dot-matrices can impact the read operation. Before reading, adjust the characters in your fonts accordingly. To do so interactively, use the console-based font utility, _DmrEditFontFile_.

As previously discussed, you will not typically create a font from scratch. For example, you can choose a predefined MDMRF file that is similar to the font you need, and then modify it as you require. When choosing a font, be aware that differences between characters are not necessarily consistent. Some dot-matrices might be very different, some might be barely different, and some might not be different at all. The following is an example of digits defined in 2 different fonts. Can you spot the differences?

*[Image: MdmrIdenticalDotMatrix.png]*

The characters with different dot-matrices are zero, three, and five.

## Delete, control, and inquire about fonts and characters

To delete one or all fonts from a context, call [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_FONT_DELETE`](../../Reference/dmr/MdmrControl.md). To delete one or all characters from a font, call [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_CHAR_DELETE`](../../Reference/dmr/MdmrControlFont.md). You can also delete characters from a font file (MDMRF) using a text based editor.

To perform general controls and inquires on fonts and characters, call [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) and [`MdmrInquireFont`](../../Reference/dmr/MdmrInquireFont.md)), respectively. For example, you can modify a font's label ([`M_FONT_LABEL_VALUE`](../../Reference/dmr/MdmrControlFont.md)), change a character's name ([`M_CHAR_NAME`](../../Reference/dmr/MdmrControlFont.md)), or inquire the total number of dots possible in the font's dot-matrix template ([`M_FONT_SIZE_TEMPLATE`](../../Reference/dmr/MdmrInquireFont.md)).

You can also call [`MdmrName`](../../Reference/dmr/MdmrName.md) to perform specialized name-type operations on fonts. For example, you can set the name of a font, or use a font's name to retrieve either its label or index. By referring to fonts with meaningful names, you can more easily manage them. All font names in a context must be unique. [`MdmrName`](../../Reference/dmr/MdmrName.md)operations do not impact how strings are processed and read.

## Programmatically managing fonts and characters

You can specify an array containing the dot-matrix of a character, using [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with either [`M_CHAR_ADD`](../../Reference/dmr/MdmrControlFont.md), to add a new character to a font, or [`M_CHAR_TEMPLATE`](../../Reference/dmr/MdmrControlFont.md), to modify a character already in a font.

You must populate the array, one row contiguously after the other, with 255 (0xFF) to represent a dot in the character's dot-matrix and 0 to represent a blank. You must organize the character data in the specified array according to the dimensions of the font's dot-matrix template. For example, if the template is 5x7 and you want to programmatically add the number "1", your array must contain 5 entries repeated 7 times, for a total of 35 entries.

*[Image: MdmrArrayChar1.png]*

You would typically add such characters to a font that already had its template established by the SureDotOCR font file that first created and populated the font. To inquire the template's dimensions, use [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrInquireFont.md) and [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrInquireFont.md).

You can also add such characters to an empty font. To do so, you must first create an empty font by calling [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_FONT_ADD`](../../Reference/dmr/MdmrControl.md), and then explicitly set the size of the font's dot-matrix template by calling [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_FONT_SIZE_COLUMNS`](../../Reference/dmr/MdmrControlFont.md) and [`M_FONT_SIZE_ROWS`](../../Reference/dmr/MdmrControlFont.md).
