---
doctype: Reference
module: code
function: McodeDetect
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeDetect"
---

# McodeDetect

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

> Detect 1D code occurrences in an image.

## Syntax

```c
void McodeDetect(
    AIL_ID          ImageBufId,           //in
    AIL_INT         SizeOfCodeTypeArray,  //in
    const AIL_INT * CodeTypeArrayPtr,     //in
    AIL_INT         NumCodesToDetect,     //in
    AIL_INT64       TimeoutValue,         //in
    AIL_INT64       ControlFlag,          //in
    AIL_ID          DetectResultCodeId    //out
)
```

## Description

This function detects code occurrences of most 1D code types in a specified image. It detects their code type and encoding scheme; it also establishes their position with high accuracy. You can automatically add a code model for each detected code type to a code context, using [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md). Alternatively, you can use the detected code type and encoding scheme information to manually set up your code models with an appropriate code type and encoding scheme for the code occurrences in your images.

You can retrieve results using [`McodeGetResult`](../../Reference/code/McodeGetResult.md). Retrieve the code type and encoding scheme with [`M_CODE_TYPE`](../../Reference/code/McodeGetResult.md) and [`M_ENCODING`](../../Reference/code/McodeGetResult.md), respectively. Prior to retrieving other results, you should ensure that [`McodeDetect`](../../Reference/code/McodeDetect.md) was successful by verifying that [`M_STATUS`](../../Reference/code/McodeGetResult.md) returns [`M_STATUS_DETECT_OK`](../../Reference/code/McodeGetResult.md).

Some code types are a subset of another. If a code occurrence has only the common features of these code types, [`McodeDetect`](../../Reference/code/McodeDetect.md)cannot distinguish if the code occurrence is of one type or the other at decode-time. In these cases, [`McodeDetect`](../../Reference/code/McodeDetect.md) reports the following:

| When the following code types cannot be differentiated | Code type reported |
| --- | --- |
| [`M_UPC_A`](../../Reference/code/McodeModel.md) and [`M_EAN13`](../../Reference/code/McodeModel.md) | [`M_UPC_A`](../../Reference/code/McodeModel.md) |
| [`M_EAN14`](../../Reference/code/McodeModel.md), [`M_GS1_128`](../../Reference/code/McodeModel.md), and [`M_CODE128`](../../Reference/code/McodeModel.md) | [`M_EAN14`](../../Reference/code/McodeModel.md) |
| [`M_GS1_128`](../../Reference/code/McodeModel.md) and [`M_CODE128`](../../Reference/code/McodeModel.md) | [`M_GS1_128`](../../Reference/code/McodeModel.md) |

You can remove this ambiguity by restricting the possible code types with [`CodeTypeArrayPtr`](../../Reference/code/McodeDetect.md). Another reason to restrict the possible code types is if you are not interested in detecting certain code types; this will accelerate the[`McodeDetect`](../../Reference/code/McodeDetect.md) operation.

> **Note:** Note that this function does not detect postal, GS1 Databar, Pharmacode, 2D, or composite code types. For the list of code types supported by [`McodeDetect`](../../Reference/code/McodeDetect.md), use [`McodeInquire`](../../Reference/code/McodeInquire.md) with [`M_SUPPORTED_CODE_TYPES_DETECT`](../../Reference/code/McodeInquire.md).

This function tries to detect the specified number of code occurrences. If it detects more than the specified number of code occurrences in the specified image, the function only stores the results of those with the best quality. If it detects less than the specified number, [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md)returns [`M_STATUS_DETECT_FAILED`](../../Reference/code/McodeGetResult.md).

## Parameters

### `ImageBufId` *(in, AIL_ID)*

Specifies the identifier of an image buffer that contains the code occurrences to detect. The image buffer must be an 8-bit unsigned 1-band buffer.

### `SizeOfCodeTypeArray` *(in, AIL_INT)*

Specifies the size of the array passed to [`CodeTypeArrayPtr`](../../Reference/code/McodeDetect.md).

*For specifying the size of the code type array*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value = 0` *(default)* | Specifies that an array is not passed to [`CodeTypeArrayPtr`](../../Reference/code/McodeDetect.md). [`McodeDetect`](../../Reference/code/McodeDetect.md) will search for all code types supported by it.

This value is only applicable when [`CodeTypeArrayPtr`](../../Reference/code/McodeDetect.md) is set to [`M_NULL`](../../Reference/code/McodeDetect.md). |
| `Value > 0` | Specifies the size of the array. |

### `CodeTypeArrayPtr` *(in, *AIL_INT)*

Specifies the address of an array listing the possible code types to detect in the image. Instead of passing an array with [`M_SUPPORTED_CODE_TYPES_DETECT`](../../Reference/code/McodeDetect.md), you can set this parameter to `M_NULL` and set [`SizeOfCodeTypeArray`](../../Reference/code/McodeDetect.md) to 0.

*For specifying the code types to detect*

| Value | Description |
| --- | --- |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_SUPPORTED_CODE_TYPES_DETECT` | Specifies all 1D code types supported by this function. |

### `NumCodesToDetect` *(in, AIL_INT)*

Specifies the total number of code occurrences to detect in the image, regardless of their code type.

*For specifying the number of code occurrences to detect*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the number of code occurrences to detect. |

### `TimeoutValue` *(in, AIL_INT64)*

Specifies the timeout period, in msec.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DetectResultCodeId` *(out, AIL_ID)*

Specifies the code detect result buffer in which to write the results. The result buffer must have been allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md).
