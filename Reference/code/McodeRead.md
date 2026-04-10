---
doctype: Reference
module: code
function: McodeRead
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeRead"
---

# McodeRead

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

> Read the code occurrence(s) in an image.

## Syntax

```c
void McodeRead(
    AIL_ID ContextCodeId,  //in
    AIL_ID ImageBufId,     //in
    AIL_ID ResultCodeId    //out
)
```

## Description

This function searches for code occurrences in an image, as specified in the code context, and writes the results in the specified result buffer. The code context's control settings determine how to perform the operation. Retrieve results using [`McodeGetResult`](../../Reference/code/McodeGetResult.md).

Since the global context settings are required, [`McodeRead`](../../Reference/code/McodeRead.md) requires that you pass a code context identifier, and not a code model identifier. The code context passed to [`McodeRead`](../../Reference/code/McodeRead.md) can contain multiple code models depending on their code type (see [`McodeModel`](../../Reference/code/McodeModel.md)for details). [`McodeRead`](../../Reference/code/McodeRead.md) can read one or more occurrences of 1D and Data Matrix code models; for other types of code models, [`McodeRead`](../../Reference/code/McodeRead.md) can read at most one occurrence.

It is strongly recommended to use a region of interest (ROI), set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), with read operations, especially if your image contains more code occurrences than your context is configured to read; otherwise, Aurora Imaging Library will randomly select which code occurrences to read, up to the specified number, from among all the code occurrences in the image. Reading only a portion of your source image is also recommended because it achieves a fast and robust read operation if your image contains other information besides the code occurrences. Note that, if your target image has an ROI, you can have the nominal angle set to the same angle of the target image's ROI, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) set to [`M_ACCORDING_TO_REGION`](../../Reference/code/McodeControl.md).

If you have associated the target image with a camera calibration context, you can specify that certain input settings be interpreted in world units. To do so, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_FINDER_PATTERN_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md), and/or [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) set to [`M_WORLD`](../../Reference/code/McodeControl.md). Note that if you set any of these constants to [`M_WORLD`](../../Reference/code/McodeControl.md) but you don't pass [`McodeRead`](../../Reference/code/McodeRead.md) a calibrated target image, the function will generate an error.

Before performing a read operation, certain control types might have to be set globally for the entire code context or individually for each code model in [`McodeControl`](../../Reference/code/McodeControl.md). When reading multiple occurrences of a Data Matrix code model, setting one or more of these values and reducing the search speed (using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SPEED`](../../Reference/code/McodeControl.md)) will produce more robust results.

| Control type | Notes |
| --- | --- |
| [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md), [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) | For 2D code types only, improves the robustness of the operation. |
| [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md), [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md) | To improve the robustness of the read operation for some code types, especially Data Matrix and Maxicode. Note that Aurora Imaging Library might have difficulty reading code occurrences if the cell size is less than 2 pixels, even if the size is specified. |
| [`M_ENCODING`](../../Reference/code/McodeControl.md) | For code types where [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) | For code types where [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md) | This control type is essential for all code types, and the image will not be decoded if the foreground value is not correctly set. |
| [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) | If the code occurrences to read are not within 5° of the horizontal axis, and/or if the code occurrences have bearer bars. |
| [`M_STRING_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_STRING_SIZE_MAX`](../../Reference/code/McodeControl.md) | For the [`M_BC412`](../../Reference/code/McodeModel.md) code type, the string size must be specified. |
| [`M_SUB_TYPE`](../../Reference/code/McodeControl.md) | For [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code type, specifying the code sub-type can help increase the speed of read operations. |

Note that if the image contains Structured Append sequence of separate but logically linked ECC 200 code occurrences, Aurora Imaging Library will treat the code occurrences as separate, independent code occurrences.

## Parameters

### `ContextCodeId` *(in, AIL_ID)*

Specifies the identifier of the code context.

### `ImageBufId` *(in, AIL_ID)*

Specifies the 8-bit unsigned 1-band image buffer in which to search.

### `ResultCodeId` *(out, AIL_ID)*

Specifies the result buffer in which to write the results of the read.
