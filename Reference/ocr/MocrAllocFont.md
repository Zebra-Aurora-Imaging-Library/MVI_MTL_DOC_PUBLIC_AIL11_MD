---
doctype: Reference
module: ocr
function: MocrAllocFont
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / ocr / MocrAllocFont"
---

# MocrAllocFont

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

> Allocate an OCR font context.

## Syntax

```c
AIL_ID MocrAllocFont(
    AIL_ID    SysId,               //in
    AIL_INT64 FontType,            //in
    AIL_INT   CharNumber,          //in
    AIL_INT   CharCellSizeX,       //in
    AIL_INT   CharCellSizeY,       //in
    AIL_INT   CharOffsetX,         //in
    AIL_INT   CharOffsetY,         //in
    AIL_INT   CharSizeX,           //in
    AIL_INT   CharSizeY,           //in
    AIL_INT   CharThickness,       //in
    AIL_INT   StringLength,        //in
    AIL_INT64 InitFlag,            //in
    AIL_ID *  FontContextOcrIdPtr  //out
)
```

## Description

This function allocates an OCR font context on the specified system. An OCR font context contains the OCR font, target image information, and processing controls.

If the OCR font context already exists as an OCR font file that matches your constraints and character sizes, such as _SEMI_M12-92.mfo_ (SEMI M12-92) or _SEMI_M13-88.mfo_ (SEMI M13-88), restore it using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md). If you need a modified version of a SEMI font (changing its character size, offset, thickness, or the number of characters in the OCR font context) or need a user-defined font, you should allocate a new OCR font context. Note that using either [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) or [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md) automatically sets the OCR font context type to [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md).

A newly allocated OCR font context must have its grayscale character representations initialized using [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md) or [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md). Each part of the OCR font context can be changed using [`MocrControl`](../../Reference/ocr/MocrControl.md) and [`MocrModifyFont`](../../Reference/ocr/MocrModifyFont.md). Constraints are set using [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md).

When allocating an OCR font context, you must specify an OCR font context type. This can be modified later using [`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CONTEXT_CONVERT`](../../Reference/ocr/MocrControl.md). If your target image has a visible threshold difference between the target characters and the background, uniform illumination, and unknown spacing between characters, and unknown string lengths, and if your font is proportional, start by allocating an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context. If your target image contains noisy, scratched, or low contrast characters, or if the illumination is not uniform, or if your font includes broken characters start by allocating an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

You must calibrate the OCR font context when the cell, offset, size, or thickness of the characters in your target image differs from the OCR font. Font calibration can be either automatic (using[`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md)) or manual (using [`MocrControl`](../../Reference/ocr/MocrControl.md)). Note, to use[`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md), you must use an [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md) OCR font context.

If you change any controls or constraints, use [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md) to speed up any following read or verify operation.

If you intend to reuse a font that you modified, save it using [`MocrSaveFont`](../../Reference/ocr/MocrSaveFont.md), and restore it using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md) when you need it.

When the OCR font context is no longer required, release it using[`MocrFree`](../../Reference/ocr/MocrFree.md)unless [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocFont.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocFont.md) was specified, the smart identifier manages the OCR font context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the OCR font context. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `FontType` *(in, AIL_INT64)*

Specifies the OCR font context type and the type of its font. This parameter should be set to one of the following values:

*For specifying the context type and the type of its font*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SEMI_M12_92` | Specifies a font respecting the SEMI M12-92 standard as the type of font. [`StringLength`](../../Reference/ocr/MocrAllocFont.md) must be set to 12, the constraints ([`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md)) for the target image are preset, and a checksum calculation is activated. |
| `M_SEMI_M13_88` | Specifies a font respecting the SEMI M13-88 standard as the type of font. [`StringLength`](../../Reference/ocr/MocrAllocFont.md) must be set to 18, the constraints ([`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md)) for the target image are preset, and a checksum calculation is activated. |
| `M_USER_DEFINED` *(default)* | Specifies a general, user-defined type of font. No maximum string length is set, the scale is set to 1.0, and the character spacing is the same as [`CharCellSizeX`](../../Reference/ocr/MocrAllocFont.md). No checksum is automatically associated with this OCR font context. |

*For*

| Value | Description |
| --- | --- |
| `M_CONSTRAINED` *(default)* | Specifies an OCR font context that works well with degraded target images and requires more information about the target string, but provides a more robust search. Note that using either [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) or [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md) automatically sets the OCR font context type to [`M_CONSTRAINED`](../../Reference/ocr/MocrAllocFont.md). |
| `M_GENERAL` | Specifies an OCR font context that works well with clean target images. It requires less information about the target string, but provides a less robust search.

If you are using clean target images but they are complex, have lighting variations, or require better binarization, you might consider trying the Aurora Imaging Library String Reader module, which is more suitable for these cases. Note that under the correct conditions, OCR is typically faster. |

### `CharNumber` *(in, AIL_INT)*

Specifies how many characters can be stored in the OCR font context.

*For specifying the number of characters to store*

| Value | Description |
| --- | --- |
| `0 < Value <= 256` | Specifies the maximum amount of characters that can be stored. At most 256 characters can be stored. |

### `CharCellSizeX` *(in, AIL_INT)*

Specifies the width of the characters in the OCR font context, in pixels.

*For specifying the width of the characters in the context*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the width, in pixels. |

### `CharCellSizeY` *(in, AIL_INT)*

Specifies the height of the characters in the OCR font context, in pixels.

*For specifying the height of the characters in the context*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the height, in pixels. |

### `CharOffsetX` *(in, AIL_INT)*

Specifies the distance between the edge of each character and their surrounding cell, along the X-axis, in pixels.

*For specifying the X-character offset*

| Value | Description |
| --- | --- |
| `1 <= Value <= 250` | Specifies the horizontal distance, in pixels.

Typically, this is set to the same value as the character thickness.

> **Note:** Note that the minimum distance is 1 pixel. |

### `CharOffsetY` *(in, AIL_INT)*

Specifies the distance between the edge of each character and their surrounding cell, along the Y-axis, in pixels.

*For specifying the Y-character offset*

| Value | Description |
| --- | --- |
| `1 <= Value <= 250` | Specifies the vertical distance, in pixels.

> **Note:** Note that the minimum distance is 1 pixel. |

### `CharSizeX` *(in, AIL_INT)*

Specifies the width of the widest character in the font without its surrounding cell, in pixels.

*For specifying the width of the widest character in the font without its surrounding cell*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the width, in pixels. This should be equal to: [`CharCellSizeX`](../../Reference/ocr/MocrAllocFont.md) `- (` [`CharOffsetX`](../../Reference/ocr/MocrAllocFont.md) `*2)`.

> **Note:** Note that, to be able to search for a string over a range of angles, the font context's character size must be greater than 16x16. |

### `CharSizeY` *(in, AIL_INT)*

Specifies the height of the tallest character in the font without its surrounding cell, in pixels.

*For specifying the height of the tallest character in the font without its surrounding cell*

| Value | Description |
| --- | --- |
| `6 <= Value <= 256` | Specifies the height, in pixels. This should be equal to: [`CharCellSizeY`](../../Reference/ocr/MocrAllocFont.md) `- (` [`CharOffsetY`](../../Reference/ocr/MocrAllocFont.md) `*2)`.

> **Note:** Note that, to be able to search for a string over a range of angles, the font context's character size must be greater than 16x16. |

### `CharThickness` *(in, AIL_INT)*

Specifies the maximum thickness (stroke width) of the characters in the OCR font context.

*For specifying the character thickness*

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the maximum thickness (stroke width) of the characters in the OCR font context, in pixels. |

### `StringLength` *(in, AIL_INT)*

Specifies the maximum length of the string that can be read or verified using the OCR font context.

*For specifying the string length*

| Value | Description |
| --- | --- |
| `0 <= Value <= 100` | Specifies the maximum length of the string that can be read or verified, in characters.

> **Note:** Note that if [`M_SEMI_M12_92`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 12. If [`M_SEMI_M13_88`](../../Reference/ocr/MocrAllocFont.md) is used, the string length must be 18. The maximum string length is 100 characters. |

### `InitFlag` *(in, AIL_INT64)*

Specifies whether the characters are brighter than the background. This parameter should be set to one of the following values:

*For specifying the character brightness*

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the characters to read or verify are darker than the background. |
| `M_FOREGROUND_WHITE` | Specifies that the characters to read or verify are brighter than the background. |

### `FontContextOcrIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the OCR font context identifier or specifies the data type that the function should use to return the OCR font context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated OCR font context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated OCR font context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_OCR_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the OCR font context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the OCR font context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated OCR font context.

If allocation fails, [`M_NULL`](../../Reference/ocr/MocrAllocFont.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the OCR font context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_OCR_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocFont.md) was specified).

An example of one character from the font, the letter 'E', follows:

> **Code example:** [reference.MocrAllocFont.example](reference.MocrAllocFont.example)

Would look as follows: *[Image: dimen.png]*
