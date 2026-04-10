---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Steps_to_reading_a_string_in_an_image
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Steps to reading a string in an image"
---

# Steps to reading a string in an image

The following steps provide a basic methodology for using the Aurora Imaging Library String Reader module:

1. For a font-based context, allocate a String Reader context to hold your string models and fonts, using [`MstrAlloc`](../../Reference/str/MstrAlloc.md). For a fontless context, restore a predefined context, using [`MstrRestore`](../../Reference/str/MstrRestore.md).
2. For a font-based context, add the required fonts to the context and customize them, using [`MstrControl`](../../Reference/str/MstrControl.md) and [`MstrEditFont`](../../Reference/str/MstrEditFont.md) respectively. For a fontless context, specify the general characteristics of the characters to search for in the target image, using [`MstrControl`](../../Reference/str/MstrControl.md).
3. Allocate a String Reader result buffer to hold the results of the read operation, using [`MstrAllocResult`](../../Reference/str/MstrAllocResult.md).
4. Add a string model to the String Reader context, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ADD`](../../Reference/str/MstrControl.md). Note that you can add more than one string model to the context.
5. Set the maximum number of strings to read, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md).
6. Set the minimum and maximum number of expected characters for the string models, using [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_SIZE_MIN`](../../Reference/str/MstrControl.md) and [`M_STRING_SIZE_MAX`](../../Reference/str/MstrControl.md).
7. If necessary, set character constraints, using [`MstrSetConstraint`](../../Reference/str/MstrSetConstraint.md).
8. If necessary, adjust general context controls, using [`MstrControl`](../../Reference/str/MstrControl.md).
9. If necessary, limit the area to be read using either a child buffer, using [`MbufChild...`](../../Reference/buf/MbufChild1d.md), or define a rectangular ROI in vector format, using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). In addition, you can improve the quality of the image using the functions of some other Aurora Imaging Library module (such as the Aurora Imaging Library image processing function [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) or [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md)).
10. Preprocess the String Reader context, using [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md).
11. If necessary, fix read problems, using [`MstrExpert`](../../Reference/str/MstrExpert.md).
12. Perform the read operation in the specified target image, using [`MstrRead`](../../Reference/str/MstrRead.md).
13. Retrieve the required results from the String Reader result buffer, using [`MstrGetResult`](../../Reference/str/MstrGetResult.md).
14. If necessary, draw the results, using [`MstrDraw`](../../Reference/str/MstrDraw.md).
15. If necessary, save your String Reader context, using [`MstrSave`](../../Reference/str/MstrSave.md) or [`MstrStream`](../../Reference/str/MstrStream.md).
16. Free all your allocated objects, using [`MstrFree`](../../Reference/str/MstrFree.md), unless [`M_UNIQUE_ID`](../../Reference/str/MstrAlloc.md) was specified during allocation.

> **Note:** Several String Reader functions require that you pass a null-terminated string array of the required characters. These characters can either be ASCII or Unicode. To specify which, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md). Whenever you pass a string array, you must ensure that it is of the right type for the encoding scheme selected. For more information, see [Encoding](S11_Global_context_settings.md).
