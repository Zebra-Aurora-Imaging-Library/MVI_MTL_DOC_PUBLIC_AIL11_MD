---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: Steps_to_reading_a_dot_matrix_string_in_an_image
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / Steps to reading a dot matrix string in an image"
---

# Steps to read dot-matrix strings from an image

The following steps provide a basic methodology for using the Aurora Imaging Library SureDotOCR module:

1. Allocate a SureDotOCR context, using [`MdmrAlloc`](../../Reference/dmr/MdmrAlloc.md).
2. Allocate a SureDotOCR result buffer to hold the results of the read operation, using [`MdmrAllocResult`](../../Reference/dmr/MdmrAllocResult.md).
3. Import one or more fonts into the context, using [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). Typically, you should use the SureDotOCR console-based font utilities with the predefined SureDotOCR font files that Aurora Imaging Library installs to interactively create your fonts.
   You can also add an empty font to the context, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_FONT_ADD`](../../Reference/dmr/MdmrControl.md) to which you can then add characters.
4. If necessary, add or replace characters in a font, using [`MdmrImportFont`](../../Reference/dmr/MdmrImportFont.md). Alternatively, you can add characters to a font using [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) with [`M_CHAR_ADD`](../../Reference/dmr/MdmrControlFont.md) and an array containing the dot-matrix of the character.
   [`MdmrControlFont`](../../Reference/dmr/MdmrControlFont.md) provides additional functionality, such as allowing you to delete characters, or rename them.
5. Set general read settings:
   1. Set the diameter of the dots that make up the characters in the strings to read, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER`](../../Reference/dmr/MdmrControl.md).
      Optionally, you can also specify a tolerance for the size of the dot diameter using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_DOT_DIAMETER_SPREAD`](../../Reference/dmr/MdmrControl.md).
   2. Set the dimensions of the area (text block) that encompasses all dot-matrix text, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_TEXT_BLOCK_HEIGHT`](../../Reference/dmr/MdmrControl.md) and [`M_TEXT_BLOCK_WIDTH`](../../Reference/dmr/MdmrControl.md).
   3. If necessary, modify other context settings, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md). For example, you can modify the foreground with which to read strings.
6. Add one or more string models to the context, using [`MdmrControl`](../../Reference/dmr/MdmrControl.md) with [`M_STRING_ADD`](../../Reference/dmr/MdmrControl.md).
7. Set model-specific settings for each model:
   1. Set the minimum and maximum number of characters in the string model, using [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md) with [`M_STRING_SIZE_MIN`](../../Reference/dmr/MdmrControlStringModel.md) and [`M_STRING_SIZE_MAX`](../../Reference/dmr/MdmrControlStringModel.md).
   2. If necessary, modify other string model settings, using [`MdmrControlStringModel`](../../Reference/dmr/MdmrControlStringModel.md). For example, you can apply grammar-like constraints, such as specifying that a position in a string must contain a specific character from a specific font.
8. Preprocess the SureDotOCR context, using [`MdmrPreprocess`](../../Reference/dmr/MdmrPreprocess.md).
9. Perform the read operation on the specified target image, using [`MdmrRead`](../../Reference/dmr/MdmrRead.md).
10. Retrieve the required results from the SureDotOCR result buffer, using [`MdmrGetResult`](../../Reference/dmr/MdmrGetResult.md).
11. If necessary, draw the results, using [`MdmrDraw`](../../Reference/dmr/MdmrDraw.md). For example, you can specify [`M_DRAW_STRING_BOX`](../../Reference/dmr/MdmrDraw.md) to draw a bounding box around the string; then, specify [`M_DRAW_AIL_FONT_STRING`](../../Reference/dmr/MdmrDraw.md) to draw an annotation of the string under the bounding box. For more information on annotation, see [Annotations](S07_Retrieving_results_and_annotation.md).
12. If necessary, save your SureDotOCR context, using [`MdmrSave`](../../Reference/dmr/MdmrSave.md) or [`MdmrStream`](../../Reference/dmr/MdmrStream.md).
13. Free all your allocated objects, using [`MdmrFree`](../../Reference/dmr/MdmrFree.md), unless [`M_UNIQUE_ID`](../../Reference/dmr/MdmrAlloc.md) was specified during allocation.
