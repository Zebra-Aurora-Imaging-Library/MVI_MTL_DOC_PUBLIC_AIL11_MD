---
doctype: Reference
module: code
function: McodeGrade
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / code / McodeGrade"
---

# McodeGrade

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

> Grade the code occurrence(s) in an image.

## Syntax

```c
void McodeGrade(
    AIL_ID    ContextCodeId,                   //in
    AIL_ID    ImageBufId,                      //in
    AIL_ID    SrcResultCodeId,                 //in
    AIL_INT   SrcModelCodeIndexOrResultIndex,  //in
    AIL_ID    DstResultCodeId,                 //out
    AIL_INT64 ControlFlag                      //in
)
```

## Description

This function computes different quality-grades for the code occurrences in an image, as specified in the code context, and writes the results in the destination result buffer. Retrieve results using [`McodeGetResult`](../../Reference/code/McodeGetResult.md). [`McodeGrade`](../../Reference/code/McodeGrade.md) can also grade code occurrences found using [`McodeRead`](../../Reference/code/McodeRead.md).

> **Note:** Note that [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

By default, [`McodeGrade`](../../Reference/code/McodeGrade.md) internally performs a code read operation before performing the code grade operation. However, you can skip this step if you pass [`McodeGrade`](../../Reference/code/McodeGrade.md) the result buffer from a previous successful call to [`McodeRead`](../../Reference/code/McodeRead.md); this is more efficient if for some reason, you need to explicitly call [`McodeRead`](../../Reference/code/McodeRead.md) prior to calling [`McodeGrade`](../../Reference/code/McodeGrade.md). In this case for all code types except 2D matrix, ensure that [`McodeRead`](../../Reference/code/McodeRead.md) was executed with the control type [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to high to avoid potential failure.

Since the global context settings are required, [`McodeGrade`](../../Reference/code/McodeGrade.md) requires that you pass a code context identifier, and not a code model identifier. The code context passed to [`McodeGrade`](../../Reference/code/McodeGrade.md) can contain multiple code models depending on their code type (see [`McodeModel`](../../Reference/code/McodeModel.md)for details). [`McodeGrade`](../../Reference/code/McodeGrade.md) can grade one or more occurrences of 1D and Data Matrix code models; for other types of code models, [`McodeGrade`](../../Reference/code/McodeGrade.md) can grade at most one occurrence. If a target image is specified and the code context contains multiple code models, you can grade all the occurrences of a specific code model or grade all the occurrences of all the code models ([`M_ALL`](../../Reference/code/McodeGrade.md)). If a source code result buffer is specified and it contains the results of several code occurrences, you can specify the index of the code occurrence to grade or grade all the occurrences ([`M_ALL`](../../Reference/code/McodeGrade.md)).

Before calling [`McodeGrade`](../../Reference/code/McodeGrade.md), ensure that the following control types are set appropriately for your code occurrences.

| Control type | Notes |
| --- | --- |
| [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md) | Specifies the foreground color of the code occurrence. This control type must be set if the foreground color is not black; the code occurrence will not be graded if the foreground value is not correctly set. |
| [`M_ENCODING`](../../Reference/code/McodeControl.md) | Specifies the type of encoding scheme for the code occurrence. This control type must be set if the default encoding scheme for the code type differs from the one used by the code occurrence or if [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) | Specifies the type of error correction scheme for the code occurrence. This control type must be set if the default error correction scheme for the code type differs from the one used by the code occurrence or if [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_STRING_SIZE...`](../../Reference/code/McodeControl.md). | Specifies the maximum and minimum size of the string (number of characters) encoded in each code occurrence. These control types must be set if [`M_STRING_SIZE_MIN`](../../Reference/code/McodeControl.md) and/or [`M_STRING_SIZE_MAX`](../../Reference/code/McodeControl.md) cannot be set to [`M_ANY`](../../Reference/code/McodeControl.md). |
| [`M_SEARCH_ANGLE...`](../../Reference/code/McodeControl.md). | Specifies the nominal search angle and angular search range. These control types must be set if the code occurrence is at an angle greater or less than 0.0 ±5 degrees. |

For more information on code model and code context settings that can improve results, see [Customizing read and grade operation settings](../../UserGuide/C17_Codes/S09_Customizing_read_operation_settings.md).

If you have associated the target image with a camera calibration context, you can specify that certain input settings be interpreted in world units. To do so, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md), and/or [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) set to [`M_WORLD`](../../Reference/code/McodeControl.md). Note that if you set any of these constants to [`M_WORLD`](../../Reference/code/McodeControl.md) but you don't pass [`McodeGrade`](../../Reference/code/McodeGrade.md) a calibrated target image, the function will generate an error. If you pass a source code result buffer to [`McodeGrade`](../../Reference/code/McodeGrade.md)instead of a target image, the code context should have the same settings as the context passed to [`McodeRead`](../../Reference/code/McodeRead.md); the settings should not be changed between the two operations. A source code result buffer (passed to this function) should contain results obtained from a calibrated target image for best results.

It is strongly recommended to use a region of interest (ROI), set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), with [`McodeGrade`](../../Reference/code/McodeGrade.md), especially if your image contains more code occurrences than your context is configured to grade; otherwise, Aurora Imaging Library will randomly select the specified number of code occurrences to grade. Grading only a portion of your image is also recommended because it achieves a fast and robust [`McodeGrade`](../../Reference/code/McodeGrade.md) operation if your image contains other information besides the code occurrences (for example, text). Note that, if your image has an ROI, you can have the nominal angle set to the same angle as that of the image's ROI, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) set to [`M_ACCORDING_TO_REGION`](../../Reference/code/McodeControl.md).

You should note that if the code occurrence cannot be decoded by Aurora Imaging Library, all grades will be returned as F.

If you cannot read a bar code with a cell size that is size 2 or smaller, you can enlarge the image using [`MimResize`](../../Reference/im/MimResize.md) and try again.

## Parameters

### `ContextCodeId` *(in, AIL_ID)*

Specifies the identifier of the code context, allocated using [`McodeAlloc`](../../Reference/code/McodeAlloc.md).

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of the image buffer containing the code occurrence(s) to be graded. The image buffer must be an 8-bit, unsigned, 1-band image buffer. Note that, this image buffer should contain the same image used for the previous [`McodeRead`](../../Reference/code/McodeRead.md) operation.

### `SrcResultCodeId` *(in, AIL_ID)*

Specifies a source code result buffer from which to retrieve [`McodeRead`](../../Reference/code/McodeRead.md) results. The result buffer must have been allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md). Note that[`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) and [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) should have been set before the [`McodeRead`](../../Reference/code/McodeRead.md) operation. In addition for all code types except 2D matrix, the results must have been obtained from the same source image in high accuracy ([`McodeControl`](../../Reference/code/McodeControl.md) [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md)). This parameter can be set to `M_NULL`.

### `SrcModelCodeIndexOrResultIndex` *(in, AIL_INT)*

Specifies the index of the code model (in the code context) or the result occurrence (in the source code result buffer) to use when grading.

*For specifying the model or the result containing information about the code occurrence to grade*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to grade all occurrences of all code models in the code context or all occurrences of code models in the source code result buffer. |
| `Value >= 0` | Specifies which code model (from the code context) or result index (from the result buffer) to use, when grading the code occurrence. |

### `DstResultCodeId` *(out, AIL_ID)*

Specifies the result buffer in which to write the resulting grading information. The result buffer must have been allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
