---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Guidelines_for_choosing_context_types
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Guidelines for choosing context types"
---

# Guidelines for choosing context types

An OCR font context is an Aurora Imaging Library object that stores the OCR font information, target image character size and spacing, constraints, and processing controls. When allocating your font context (using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md)), you can choose 1 of 2 types:

- The [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type.
- The [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context type.

## General OCR font context type

The [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type requires less information about the target string but works best on clean target images. If you need to specify the location of the string to eliminate erroneous results, use a child buffer to create a smaller area in which to search, using [`MbufChild...`](../../Reference/buf/MbufChild1d.md).

When using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, you should keep in mind the following conditions:

- The target image should have a clearly visible threshold (binarization) between the characters and their background. The threshold must preserve the shape of the characters. Broken and/or touching characters can degrade the results.
- A clean background of the target image. The cleaner the background, the better the results of a search.
- The backgrounds should not have blobs of similar size and intensity as the characters.

If you are using clean target images but they are complex, have lighting variations, or require better binarization, you might consider trying the Aurora Imaging Library String Reader module, which is more suitable for these cases. Note that under the correct conditions, OCR is typically faster.

The following [`MocrControl`](../../Reference/ocr/MocrControl.md) control types are available only when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context:

- Enabling blank character recognition (use [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md)).
- Finding the string length automatically (use [`M_STRING_CHAR_NUMBER`](../../Reference/ocr/MocrControl.md) with [`M_ANY`](../../Reference/ocr/MocrControl.md)).
- Finding the character width automatically (use [`M_TARGET_CHAR_SIZE_X`](../../Reference/ocr/MocrControl.md) with [`M_SAME`](../../Reference/ocr/MocrControl.md)) when using a fixed size font.
- Finding the character height automatically (use [`M_TARGET_CHAR_SIZE_Y`](../../Reference/ocr/MocrControl.md) with [`M_SAME`](../../Reference/ocr/MocrControl.md)) when using a fixed size font.
- Finding the inter-character spacing automatically (use [`M_TARGET_CHAR_SPACING`](../../Reference/ocr/MocrControl.md) with [`M_SAME`](../../Reference/ocr/MocrControl.md) if it is the same between characters or [`M_ANY`](../../Reference/ocr/MocrControl.md) if it differs between characters).

An example image best suited to using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context is:

*[Image: ocrug-general-01.png]*

If your image is greatly degraded or if you know a lot about the location of your target string(s), it is recommended to use the [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) font context type.

## Constrained OCR font context type

The [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context type works well with degraded target images. This type of context requires more information about the target string, but provides a more robust search. [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) is best used when all the details are known about the target string. Especially, the more known about the target string's location and the size and spacing of its characters, the better the results of a search. Since more precise information is required to calibrate your font with that of the target string, you can automatically calibrate the font with the target string using [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md). Note that the automatic calibration of a font can only be done when using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context type.

An example image best suited to using an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context is:

*[Image: ocrug-constrained-01.png]*

If your image contains a string comprised of unevenly spaced characters or a string that has blanks or broken characters, it is recommended to use the [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context type.

## Switching between the two

Defining a good OCR font context takes time. Modifying an existing OCR font context is faster than creating a new OCR font context from scratch. When dealing with two sets of images, for example, with similar OCR font requirements with regards to character representation, but differing in some other aspect (for example, sizing, spacing, processing controls, and/or quality), it might be faster to switch OCR font context types, reset the required constraints and controls, preprocess, and then read the new images. You can switch the type of context using the [`MocrControl`](../../Reference/ocr/MocrControl.md) function with [`M_CONTEXT_CONVERT`](../../Reference/ocr/MocrControl.md).

For example, after reading a series of images with an evenly spaced font (depicted below), to read a group of images where the font is not equally spaced, requires a switch from [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) to [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md).

*[Image: ocrug-space.png]*

> **Note:** Note, when you switch between context types, any unsupported setting is reset to its default.

## Deciding which OCR font context type to use

The following decision tree should help you decide when you should use an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) or an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context type. Note that, if a question does not apply to your situation, skip to the next question in the list.

1. Can the target image be thresholded? That is, can a clear distinction be made between the background of the target image and the characters to be read?
   - **Yes**. Use an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
   - **No**. Use an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
2. Does the target image have intensity problems; that is, does it have reflections or uneven lighting, or is it so heavily degraded that bits of the string are difficult to read with the naked eye?
   - **Yes**. Use an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
   - **No**. Use an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
3. Is the target string made up of characters that vary in size? Does the target string contain variable spacing?
   - **Yes**. Use an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
   - **No**. Use an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
   Note that variable spacing does not include "jitter", which refers to tiny variations in the position of the target characters relative to the expected character position. Jitter can be corrected by calibrating your OCR font context (with [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md)) to match the target string.
   Set [`TargetCharSizeXMin`](../../Reference/ocr/MocrCalibrateFont.md) and [`TargetCharSizeYMin`](../../Reference/ocr/MocrCalibrateFont.md) to measure to the clearest edge of the target characters to be read. Set [`TargetCharSizeXMax`](../../Reference/ocr/MocrCalibrateFont.md) and [`TargetCharSizeYMax`](../../Reference/ocr/MocrCalibrateFont.md) to measure to the faintest edge of the target character to be read - just before the shading (if any) matches the background color.
   *[Image: OCR_min_max_target_values.png]*
4. Are any of the characters to be read broken?
   When there are broken characters, the best way to determine which OCR font context to use is to try both the [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) and the [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) contexts. In a specific situation, one or the other can produce better results.
   If using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_BROKEN_CHAR`](../../Reference/ocr/MocrControl.md) set to [`M_ENABLE`](../../Reference/ocr/MocrControl.md).
5. Are any of the characters to be read touching?
   When there are touching characters, the best way to determine which OCR font context to use is to try both the [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) and the [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) contexts. In a specific situation, one or the other can produce better results.
   If using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context, use [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md) set to [`M_ENABLE`](../../Reference/ocr/MocrControl.md).

If your answers were a combination of both [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) and [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font contexts, use the OCR font context most often recommended. You will have to further manipulate your target image, or your OCR font (mostly using [`MocrControl`](../../Reference/ocr/MocrControl.md)), to improve results.
