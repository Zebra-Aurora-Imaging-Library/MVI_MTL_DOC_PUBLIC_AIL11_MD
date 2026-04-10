---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: OCR_font
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / OCR font"
---

# OCR font

You must specify the Aurora Imaging Library OCR font to read/verify the character strings in target images. Aurora Imaging Library uses fonts (or typesets) to specify the style and size of characters in the images to be read or verified.

An OCR font contains the following information:

- The grayscale representations of the characters.
- Codes identifying each character (ASCII codes for the characters).
- The number of characters in the OCR font.
- Character dimensions.

The above information can be calibrated (with [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md) or [`MocrControl`](../../Reference/ocr/MocrControl.md)), modified (with [`MocrModifyFont`](../../Reference/ocr/MocrModifyFont.md)), and saved (with [`MocrSaveFont`](../../Reference/ocr/MocrSaveFont.md)) for later restoration.

## User-defined Aurora Imaging Library OCR fonts

Unless using one of the two provided predefined OCR fonts, you must define a custom OCR font. You can create a user-defined OCR font from scratch, or make minor modifications to an existing OCR font, already saved to disk.

To create a custom font:

1. Allocate an OCR font context, using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md).
   > **Note:** When allocating an OCR font context, you must specify the maximum number of characters that can be stored in the font, and the dimensions of the font's character representations and their character cells.
2. Grab/create the grayscale character representations of the font in an Aurora Imaging Library image buffer and then copy them from the image buffer to the OCR font context, using [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md). Alternatively, you can import grayscale character representations from a text file or an image file (for example, a TIFF) into an OCR font context using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md).
   > **Note:** You can use the Aurora Imaging Library OCR Reader utility to create new characters to add to an existing font.
   > **Note:** When importing, or copying, character representations to the OCR font context, the OCR font context must have sufficient space to hold the representations of all the specified characters. You can use [`MocrInquire`](../../Reference/ocr/MocrInquire.md) to determine the maximum number of characters that can be stored in the font and the size of each character.
3. Once copied or imported into an OCR font context, the entire OCR font context can be saved on disk using [`MocrSaveFont`](../../Reference/ocr/MocrSaveFont.md), and then later restored using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md).

The following is an example of a character in its Aurora Imaging Library OCR font character cell and the dimensions that you will have to specify during OCR font context allocation. Values are to be specified in pixels. Each square in the grid represents one pixel.

*[Image: dimen.png]*

The parameters [`CharCellSizeX`](../../Reference/ocr/MocrAllocFont.md), [`CharCellSizeY`](../../Reference/ocr/MocrAllocFont.md), [`CharOffsetX`](../../Reference/ocr/MocrAllocFont.md), [`CharOffsetY`](../../Reference/ocr/MocrAllocFont.md), [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md), and [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) of [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md) must comply with the following restrictions:

- 2*[`CharOffsetX`](../../Reference/ocr/MocrAllocFont.md) + [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) &lt;= [`CharCellSizeX`](../../Reference/ocr/MocrAllocFont.md).
- 2*[`CharOffsetY`](../../Reference/ocr/MocrAllocFont.md) + [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) &lt;= [`CharCellSizeY`](../../Reference/ocr/MocrAllocFont.md).
- [`CharCellSizeY`](../../Reference/ocr/MocrAllocFont.md), [`CharCellSizeX`](../../Reference/ocr/MocrAllocFont.md), [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md), [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) must be >= 6 pixels and &lt;= 256 pixels.

> **Note:** When the characters in a font do not have a uniform width or height, [`CharSizeX`](../../Reference/ocr/MocrAllocFont.md) should specify the width of the widest character in the font and [`CharSizeY`](../../Reference/ocr/MocrAllocFont.md) should specify the height of the tallest character in the font. Also, to be able to search for a string over a range of angles, the font context's character size must be greater than 16x16. You can use the Aurora Imaging Library OCRReader utility to determine font character widths and heights.

When copying the character representations from an image buffer, or importing them from an image file, the characters must have the dimensions specified during OCR font context allocation.

The following is an example of how to create a user-defined Aurora Imaging Library OCR font using a font definition image and [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md). In this case, the _CharImageForFontDefinition.mim_file contains the grayscale character representations. The image is loaded into an image buffer and then the character representations are copied to an allocated OCR font context using [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md).

> **Code example:** [userguide.optical_character_recognition.defining_a_font01](userguide.optical_character_recognition.defining_a_font01)

The following is an example of how to create a user-defined Aurora Imaging Library OCR font directly from a font definition image file, using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md). In this case, the _CharImageForFontDefinition.mim_file contains the grayscale character representations. which are imported into an allocated OCR font context using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md). [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md)imports into an allocated OCR font context.

> **Code example:** [userguide.optical_character_recognition.defining_a_font02](userguide.optical_character_recognition.defining_a_font02)

When importing the character representations from an ASCII file, font character representations must be presented as follows:

*[Image: CharValue_65.png]*

> **Note:** Note that in this format, 'pixels' are delimited by a blank space. So '00' counts as one pixel.

This information breaks down into the following:

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

The following is an example of how to create a user-defined Aurora Imaging Library OCR font using character representations from an ASCII file, using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md). In this case, the _AsciiFileForFontDefinition.txt_file contains the ASCII character representations, in the format above. The character representations are imported into an allocated OCR font context using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md).

> **Code example:** [userguide.optical_character_recognition.defining_a_font03](userguide.optical_character_recognition.defining_a_font03)

## Existing Aurora Imaging Library OCR fonts

Once created, an Aurora Imaging Library OCR font can be saved and restored as needed. Restoring this information (using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md)) rather than creating the Aurora Imaging Library OCR font from scratch saves time, especially if the restored font requires no further modifications. Note that the entire OCR font context is restored when restoring the font using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md). Aurora Imaging Library comes with three predefined Semi fonts; for more information, see the next subsection.

## SEMI fonts

Aurora Imaging Library OCR comes with two ISO compatible SEMI fonts ([`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) or [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md)) and one generic SEMI font that has no constraints and no checksum (_SEMI.mfo_). These can be used directly or modified to suit your needs.

### Using a SEMI font

To use a SEMI font directly, restore it using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md) with the [`FileName`](../../Reference/ocr/MocrRestoreFont.md) parameter set to _"SEMI_M12-92.mfo"_, _"SEMI-M13-88.mfo"_, or _"SEMI.mfo"_. These files are located in directory _"\contexts\"_ under the Aurora Imaging Library installation folder. Once restored, the font can be modified using the Aurora Imaging Library OCR functions.

### Creating a SEMI font

To create a new font based on a SEMI font:

1. Create an OCR font context using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md) with:
   - The [`FontType`](../../Reference/ocr/MocrAllocFont.md) parameter set to either [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) or [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md).
   - The [`StringLength`](../../Reference/ocr/MocrAllocFont.md) parameter set to 12 when using [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) and 18 when using [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md).
   - The [`CharNumber`](../../Reference/ocr/MocrAllocFont.md) parameter set to 38. This allows for capital letters (A-Z), digits (0-9), hyphen (-), and period (.).
2. Use either [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md) or [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md) to add character representations from an existing SEMI font.

## Quality and scale are important

Using high-quality character representations will produce the best results. OCR processing relies on using the cleanest font characters possible.

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, broken characters and spaces, even if expected in the target string, should not be defined in the font. Instead, you should enable the ability to read broken characters using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BROKEN_CHAR`](../../Reference/ocr/MocrControl.md), and/or enable the ability to read spaces using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md).

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) font context type, the threshold between the characters and the background must preserve the shape of the characters and have a clearly-visible point of differentiation (binarization).

If the characters in the target image are brighter than the background (for example, white on black), then the character representations included in your font must also be of characters that are brighter than the background. The foreground is specified at context allocation time ([`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md)) and can be changed later using [`MocrModifyFont`](../../Reference/ocr/MocrModifyFont.md) with [`M_INVERT`](../../Reference/ocr/MocrModifyFont.md). This changes both the character representations and the setting specified at allocation time.

If the size of the character representations in the font is not the same as those in the target string, you can calibrate the font (discussed later). Alternatively, when the physical size of the character representations of the OCR font differ from those in the target image, changing the size of the character representations of the OCR font could improve the robustness of the search. To change the size, use [`MocrModifyFont`](../../Reference/ocr/MocrModifyFont.md) with [`M_RESIZE`](../../Reference/ocr/MocrModifyFont.md). Changing the size of the font permanently in the OCR font can be faster than resizing the font before each read/verify operation, as is done when the font is calibrated.

## Visualizing

It might be necessary, at some point during application development, to display the character representations of your Aurora Imaging Library OCR font. To do so, use [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md) to copy the character representations to a displayable image buffer.

## Erasing characters

To remove a character from the OCR font, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_ERASE`](../../Reference/ocr/MocrControl.md) and specify the ASCII code associated with the character representation to remove. An OCR font can contain a limited number of characters; this number is set during OCR font context allocation. Removing unused or erroneously added characters is the easiest way to assure that these characters will not be used when looking for matches in the target string and that there is space for new characters to be added.
