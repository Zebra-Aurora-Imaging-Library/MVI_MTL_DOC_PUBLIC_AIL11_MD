---
doctype: Reference
module: code
function: McodeGetResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / code / McodeGetResult"
---

# McodeGetResult

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

> Get the specified type of result for the specified code occurrence(s), or in the case of an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, for the specified code model(s), from a code result buffer.

## Syntax

```c
void McodeGetResult(
    AIL_ID    ResultCodeId,    //in
    AIL_INT   ResultIndex,     //in
    AIL_INT   RowOrScanIndex,  //in
    AIL_INT64 ResultType,      //in
    void *    UserVarArrayPtr  //out
)
```

## Description

This function retrieves the result(s) of the specified type for the specified code occurrence(s), or in the case of an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, for the specified code model(s), from a code result buffer. For results from an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, you can also retrieve a result for a specific scanline or row of a specified code occurrence.

In some cases, you can also retrieve the specified result for all occurrences, or in the case of [`McodeTrain`](../../Reference/code/McodeTrain.md), for all code models. The order in which these results are returned depends on the code operation that returned them. Use [`M_CODE_MODEL_ID`](../../Reference/code/McodeGetResult.md) to establish the corresponding code model of each returned result (or set of results for result types that return multiple values such as [`M_DATA_CODEWORDS`](../../Reference/code/McodeGetResult.md)). For more information on the order in which results are returned, see the Retrieving results section of [Chapter 15: Codes](../../UserGuide/C17_Codes/S11_Retrieving_results.md).

The code operation must have been performed prior to calling [`McodeGetResult`](../../Reference/code/McodeGetResult.md); otherwise, an error will occur.

Besides code type restrictions listed explicitly in the result types below, keep in mind that [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

The results from an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation are the recommended control type settings for a code context and its code models, based on the occurrences found in all the training images. To retrieve the results of the read operation that [`McodeTrain`](../../Reference/code/McodeTrain.md) internally performed on each training image, use [`M_CODE_RESULT_ID`](../../Reference/code/McodeGetResult.md). This retrieves the identifiers of internal code read result buffers; there is one per training image. You can then pass [`McodeGetResult`](../../Reference/code/McodeGetResult.md) one of these result buffer identifiers and retrieve any type of result available for an[`McodeRead`](../../Reference/code/McodeRead.md) operation.

From an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, a resulting quality grade (such as, [`M_CONTRAST_UNIFORMITY_GRADE`](../../Reference/code/McodeGetResult.md)) is typically based on a calculation described in the standard selected using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md). The calculation returns one result per codeword or one result per code occurrence, depending on what was being graded. In most cases, there is a non-grade result which has more information about the test performed (such as, [`M_CONTRAST_UNIFORMITY`](../../Reference/code/McodeGetResult.md)). The grade is based on a scale from 4.0 to 0.0 that is converted to an alphabetic grade (A through F).

When dealing with results that are derived from other results, retrieve the results for the associated results first. For example, before retrieving [`M_SCAN_DEFECTS`](../../Reference/code/McodeGetResult.md), retrieve [`M_SCAN_ERN_MAXIMUM`](../../Reference/code/McodeGetResult.md) and [`M_SCAN_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md). This assists in narrowing down where your code occurrence (or potentially the configuration of your code context) is lacking.

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the top-left pixel in the target image.

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`McodeControl`](../../Reference/code/McodeControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md) control type set to [`M_PIXEL`](../../Reference/code/McodeControl.md). However, note that if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md) to [`M_WORLD`](../../Reference/code/McodeControl.md) without specifying a calibrated target image in which to calculate the results, [`McodeGetResult`](../../Reference/code/McodeGetResult.md) will generate an error.

Note that [`McodeGetResult`](../../Reference/code/McodeGetResult.md) in combination with [`McodeWrite`](../../Reference/code/McodeWrite.md) can be used to determine the required image buffer size to encode a given string. First, call [`McodeWrite`](../../Reference/code/McodeWrite.md) with [`ImageBufId`](../../Reference/code/McodeWrite.md) set to [`M_NULL`](../../Reference/code/McodeWrite.md) and then call [`McodeGetResult`](../../Reference/code/McodeGetResult.md) to retrieve the minimum width ([`M_WRITE_SIZE_X`](../../Reference/code/McodeGetResult.md)) and height ([`M_WRITE_SIZE_Y`](../../Reference/code/McodeGetResult.md)) required to draw the code. For simplicity, the codes drawn using [`McodeWrite`](../../Reference/code/McodeWrite.md) are referred to as "code occurrences" in the [`ResultType`](../../Reference/code/McodeGetResult.md) tables below.

## Parameters

### `ResultCodeId` *(in, AIL_ID)*

Specifies the identifier of the code result buffer from which to retrieve results.

### `ResultIndex` *(in, AIL_INT)*

Specifies the code occurrence(s) or code model(s) for which to retrieve results.

*For specifying the occurrence or model index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For code occurrence-specific or code model-specific result types, this value is the same as [`M_ALL`](../../Reference/code/McodeGetResult.md). For other result types, this value is the same as [`M_GENERAL`](../../Reference/code/McodeGetResult.md). |
| `M_ALL` | Specifies to get results related to all code occurrences.

> **Note:** Note that when this value is used, results from an [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation are returned in the order in which the code occurrences were found, regardless of the code model/code type. |
| `M_GENERAL` | Specifies to get general results related to all code occurrences, or in the case of [`McodeTrain`](../../Reference/code/McodeTrain.md), the recommended control type settings for the code context.

> **Note:** Note that the order in which these results are returned depends on the code operation that returned them. |
| `0 <= CodeModelIndex < M_NUMBER_OF_CODE_MODELS` | Specifies the index of the code model in the [`McodeTrain`](../../Reference/code/McodeTrain.md)result buffer from which to retrieve results. Code models are indexed in the result buffer in the same order as they were added to the training context. Use [`M_CODE_MODEL_ID`](../../Reference/code/McodeGetResult.md) to establish the corresponding code model identifier.

See [Retrieving results](../../UserGuide/C17_Codes/S11_Retrieving_results.md) for more information on result indices. |
| `0 <= CodeOccurrenceIndex < M_NUMBER` | Specifies the index of the code occurrence in the [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeDetect`](../../Reference/code/McodeDetect.md) result buffer from which to retrieve results. Code occurrences are indexed in the result buffer in the order in which they were found, regardless of the code model/code type. Use [`M_CODE_MODEL_ID`](../../Reference/code/McodeGetResult.md) and [`M_CODE_TYPE`](../../Reference/code/McodeGetResult.md) to establish the corresponding code model and code type, respectively.

See [Retrieving results](../../UserGuide/C17_Codes/S11_Retrieving_results.md) for more information on result indices. |

### `RowOrScanIndex` *(in, AIL_INT)*

Specifies the scanline(s) or row(s) of the code occurrence in the [`McodeGrade`](../../Reference/code/McodeGrade.md) result buffer for which to retrieve results. For result types that are not scanline-specific or row-specific, set this parameter to [`M_GENERAL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md).

*For specifying the scanline result index or row result index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.

For a scanline-specific or row-specific result type, this value is the same as [`M_ALL`](../../Reference/code/McodeGetResult.md). For other result types, this value is the same as [`M_GENERAL`](../../Reference/code/McodeGetResult.md). |
| `M_ALL` | Specifies to retrieve the results related to all scanlines or rows of the specified code occurrence. |
| `M_GENERAL` | Specifies to retrieve general results. |
| `0 <= Value < M_NUMBER_OF_ROWS` | Specifies the row for which to retrieve results of the specified code occurrence. 2D cross-row code types, composite code types, and the stacked sub-types of the GS1 Databar code type have multiple rows; the remaining 1D code types have one row.

> **Note:** Note that this value cannot be used if [`ResultIndex`](../../Reference/code/McodeGetResult.md) is set to [`M_ALL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md). |
| `0 <= Value < M_NUMBER_OF_SCANS` | Specifies the scanline for which to retrieve results of the specified code occurrence.

> **Note:** Note that this value cannot be used if [`ResultIndex`](../../Reference/code/McodeGetResult.md) is set to [`M_ALL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md). |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `UserVarArrayPtr` *(out, *void)*

Specifies the address in which to place the specified result.

## Parameter Associations

### For retrieving the number of code occurrences from an

To retrieve the number of code occurrences, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values. To retrieve the result obtained from an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, [`ResultIndex`](../../Reference/code/McodeGetResult.md) should be set to the index of a specific code model or [`M_ALL`](../../Reference/code/McodeGetResult.md) (or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md)); whereas, to retrieve the result obtained from an [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, [`ResultIndex`](../../Reference/code/McodeGetResult.md) should be set to [`M_GENERAL`](../../Reference/code/McodeGetResult.md).

---

### `Code result buffer ID`

Specifies a code detect, read, grade, train, or write result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with[`M_CODE_..._RESULT`](../../Reference/code/McodeAllocResult.md).

#### `M_NUMBER`

Retrieves the total number of code occurrences. Note that multiple occurrences of the same model are counted separately.  Note that only 1D code types (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md) code types), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type support multiple occurrences.  Since training the corresponding control type is only supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this result type is only available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type after an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. For the remaining operations, this result type is available for all code types.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `Value > 0` | Specifies the maximum number of code occurrences for an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation; for all other operations, specifies the number of code occurrences. |

### For retrieving general results from an

To retrieve a general result that is returned from an [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values.

---

### `Code detect result buffer ID for general results`

Specifies a code detect result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeDetect`](../../Reference/code/McodeDetect.md) results.

#### `M_STATUS`

Retrieves the status of the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation.  It can be helpful to retrieve the number of code occurrences by calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md), along with the status. For example, if you specify that the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation should detect two code occurrences ([`NumCodesToDetect`](../../Reference/code/McodeDetect.md)) and only one was detected, the status returned is [`M_STATUS_DETECT_FAILED`](../../Reference/code/McodeGetResult.md). Calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md) will return 1.

| Value | Description |
| --- | --- |
| `M_STATUS_DETECT_FAILED` | Specifies that the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation failed. |
| `M_STATUS_DETECT_OK` | Specifies that the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation was successful. |
| `M_STATUS_TIMEOUT_END` | Specifies that the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation timed out. |

#### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TIMEOUT`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the timeout limit was not reached. |
| `M_TRUE` | Specifies that the timeout limit was reached. |

---

### `Code grade result buffer ID for general results`

Specifies a code grade result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeGrade`](../../Reference/code/McodeGrade.md) results.

#### `M_STATUS`

Retrieves the status of the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  It can be helpful to retrieve the number of code occurrences by calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md), along with the status. For example, if you set an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation to grade two code occurrences ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md)) and only one was graded, the status returned is [`M_STATUS_GRADE_FAILED`](../../Reference/code/McodeGetResult.md). Calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md) will return 1.  To retrieve the confidence score of an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation as a grade, use [`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_STATUS_GRADE_FAILED` | Specifies that the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation failed. |
| `M_STATUS_GRADE_OK` | Specifies that the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation was successful. |
| `M_STATUS_NO_RESULT_AVAILABLE` | Specifies that there are no results available. The [`McodeGrade`](../../Reference/code/McodeGrade.md) operation has not been performed. |
| `M_STATUS_TIMEOUT_END` | Specifies that the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation timed out. |

#### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TIMEOUT`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT_END`](Reference/code/McodeGetResult.md))* |  |

---

### `Code read result buffer ID for general results`

Specifies a code read result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeRead`](../../Reference/code/McodeRead.md) results.

#### `M_STATUS`

Retrieves the status of the [`McodeRead`](../../Reference/code/McodeRead.md) operation.  It can be helpful to retrieve the number of code occurrences by calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md), along with the status. For example, if you set an [`McodeRead`](../../Reference/code/McodeRead.md) operation to read two code occurrences ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md)) and only one was found, the status returned is [`M_STATUS_NOT_FOUND`](../../Reference/code/McodeGetResult.md). Calling [`M_NUMBER`](../../Reference/code/McodeGetResult.md) will return 1.  > **Note:** If the operation that produced the results expected to operate on multiple code occurrences ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md)) and it incurred an error for more than one, the least-common error is returned. For example, if you set an [`McodeRead`](../../Reference/code/McodeRead.md) operation to read two code occurrences and one fails because the code was not found ([`M_STATUS_NOT_FOUND`](../../Reference/code/McodeGetResult.md)) and the other fails because the encoding type was unknown ([`M_STATUS_ENC_UNKNOWN`](../../Reference/code/McodeGetResult.md)), the latter is returned.  To retrieve the confidence score of an [`McodeRead`](../../Reference/code/McodeRead.md) operation, use [`M_SCORE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_STATUS_CRC_FAILED` | Specifies that the [`McodeRead`](../../Reference/code/McodeRead.md) operation failed when validating the CRC. |
| `M_STATUS_ECC_UNKNOWN` | Specifies an unknown error correction type. |
| `M_STATUS_ENC_UNKNOWN` | Specifies an unknown encoding type. |
| `M_STATUS_ENCODING_ERROR` | Specifies that an error occurred when decoding the code. |
| `M_STATUS_NO_RESULT_AVAILABLE` | Specifies that there are no results available. The [`McodeRead`](../../Reference/code/McodeRead.md) operation has not been performed. |
| `M_STATUS_NOT_FOUND` | Specifies that the code was not found. |
| `M_STATUS_READ_OK` | Specifies that the [`McodeRead`](../../Reference/code/McodeRead.md) operation was successful. |
| `M_STATUS_TIMEOUT_END` | Specifies that the[`McodeRead`](../../Reference/code/McodeRead.md) operation timed out. |

#### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TIMEOUT`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT_END`](Reference/code/McodeGetResult.md))* |  |

---

### `Code train result buffer ID for general results`

Specifies a code train result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeTrain`](../../Reference/code/McodeTrain.md) results.

#### `M_CODE_MODEL_ID`

Retrieves the identifier of the trained code model. Note that this result is not valid when the result buffer is on a remote system.

#### `M_CODE_RESULT_ID`

Retrieves the identifiers of the internal code read result buffers associated with the training images; there is one result buffer per training image. Each identifier can be used to retrieve the results of the read operation that [`McodeTrain`](../../Reference/code/McodeTrain.md) internally performed on the corresponding training image.

#### `M_FAILED_IMAGES_ID`

Retrieves the identifiers of the training images that have been unsuccessfully read.

#### `M_FAILED_IMAGES_INDEX`

Retrieves the indices of the training images that have been unsuccessfully read.

#### `M_FAILED_NUMBER_OF_IMAGES`

Retrieves the number of images that have been unsuccessfully read.

#### `M_MINIMUM_CONTRAST`

Retrieves the recommended setting for the minimum possible contrast between the foreground and background in the target image ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md)).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `0 <= Value <= 255` | Specifies the minimum contrast value. |

#### `M_NUMBER_OF_CODE_MODELS`

Retrieves the number of code models added to the training context.

#### `M_NUMBER_OF_TRAINING_IMAGES`

Retrieves the number of images used in the current train operation.

#### `M_PASSED_IMAGES_ID`

Retrieves the identifiers of the training images that have been successfully read.

#### `M_PASSED_IMAGES_INDEX`

Retrieves the indices of the training images that have been successfully read.

#### `M_PASSED_NUMBER_OF_IMAGES`

Retrieves the number of images that have been successfully read.

#### `M_SEARCH_ANGLE_MODE`

Retrieves the recommended setting for whether to enable the search angular range algorithm ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md)) for the code context.

| Value | Description |
| --- | --- |
| `M_ENABLE` | Specifies that the search angular range algorithm is used. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |

#### `M_SPEED`

Retrieves the recommended setting for the search speed ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SPEED`](../../Reference/code/McodeControl.md)) for the code context.  This result type is available for all code types except the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the initialization mode. |
| `M_LOW` | Specifies a low search speed. |
| `M_MEDIUM` | Specifies a medium search speed. |
| `M_VERY_LOW` | Specifies a very low search speed. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_STATUS`

Retrieves the status of the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation.

| Value | Description |
| --- | --- |
| `M_STATUS_TIMEOUT_END` | Specifies that the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation timed out. |
| `M_STATUS_TRAIN_FAILED` | Specifies that the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation failed. |
| `M_STATUS_TRAIN_OK` | Specifies that the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation was successful. |

#### `M_THRESHOLD_MODE`

Retrieves the recommended threshold mode setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md)) for the code context.

| Value | Description |
| --- | --- |
| `M_ADAPTIVE` | Specifies the use of a fast dynamic local threshold. |
| `M_GLOBAL_SEGMENTATION` | Specifies the use of a global threshold value. |
| `M_GLOBAL_WITH_LOCAL_RESEGMENTATION` | Specifies that the source image was globally thresholded and then the edges in the binarized image were resegmented according to the intensities of the surrounding bars and spaces in the original source image. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code context. |

#### `M_THRESHOLD_VALUE`

Retrieves the recommended threshold value setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_THRESHOLD_VALUE`](../../Reference/code/McodeControl.md)) for the code context.  Note that, this result type is only available when [`M_THRESHOLD_MODE`](../../Reference/code/McodeGetResult.md) is not set to [`M_ADAPTIVE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_AUTO_COMPUTE` | Specifies to calculate the threshold value automatically. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code context. |
| `0 <= Value <= 255` | Specifies the threshold value. |

#### `M_TIMEOUT_END`

Retrieves whether the timeout limit was reached. You can set the timeout limit using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TRAIN_TIMEOUT`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| *(see [`M_TIMEOUT_END`](Reference/code/McodeGetResult.md))* |  |

#### `M_TRAIN_ENABLED_NUMBER_OF_MODEL_CONTROL_TYPES`

Retrieves, for each code model, the number of code model control types that were enabled for training.

#### `M_TRAINED_NUMBER_OF_MODEL_CONTROL_TYPES`

Retrieves, for each code model, the number of code model control types that can be modified by the training results; that is, the code model control types that will be changed after calling [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md).

#### `M_TRAINING_SCORE`

Retrieves the percentage of successful internal[`McodeRead`](../../Reference/code/McodeRead.md) operations. The percentage is the number of successfully-read images out of the total number of images used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the percentage of successful internal[`McodeRead`](../../Reference/code/McodeRead.md) operations. |

---

### `Code write result buffer ID for general results`

Specifies a code write result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_WRITE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeWrite`](../../Reference/code/McodeWrite.md) results.

#### `M_STATUS`

Retrieves the status of the [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

| Value | Description |
| --- | --- |
| `M_STATUS_NO_RESULT_AVAILABLE` | Specifies that there are no results available. The [`McodeWrite`](../../Reference/code/McodeWrite.md) operation has not been performed. |
| `M_STATUS_WRITE_FAILED` | Specifies that the [`McodeWrite`](../../Reference/code/McodeWrite.md) operation failed. |
| `M_STATUS_WRITE_OK` | Specifies that the [`McodeWrite`](../../Reference/code/McodeWrite.md) operation was successful. |

### For retrieving code model-specific results from an

To retrieve a code model-specific result that is returned from an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values. In this case, the [`ResultIndex`](../../Reference/code/McodeGetResult.md)parameter must be set to the index of a specific code model or [`M_ALL`](../../Reference/code/McodeGetResult.md) (or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md)).

---

### `Code train result buffer ID for model-specific results`

Specifies a code train result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeTrain`](../../Reference/code/McodeTrain.md) results.

#### `M_CELL_NUMBER_X`

Retrieves the recommended number of cells setting in the X-direction ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the code can have any number of cells in the X-direction. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `Value > 0` | Specifies the number of cells in the X-direction. |

#### `M_CELL_NUMBER_X_MAX`

Retrieves the recommended setting for the maximum number of cells for which to search, in the X-direction of a 2D code ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_X_MAX`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for 2D code types.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |

#### `M_CELL_NUMBER_X_MIN`

Retrieves the recommended setting for the minimum number of cells for which to search, in the X-direction of a 2D code ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_X_MIN`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for 2D code types.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |

#### `M_CELL_NUMBER_Y`

Retrieves the recommended number of cells setting in the Y direction ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies that the code can have any number of cells in the Y-direction. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `Value > 0` | Specifies the number of cells in the Y-direction. |

#### `M_CELL_NUMBER_Y_MAX`

Retrieves the recommended setting for the maximum number of cells for which to search, in the Y-direction of a 2D code ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_Y_MAX`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for 2D code types.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |

#### `M_CELL_NUMBER_Y_MIN`

Retrieves the recommended setting for the minimum number of cells for which to search, in the Y-direction of a 2D code ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_Y_MIN`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for 2D code types.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |

#### `M_CELL_SIZE_MAX`

Retrieves the recommended setting for the maximum cell size ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md)) for the trained code model(s).  For 2D matrix code types, this corresponds to the nominal maximum module size, whereas for other code types, this corresponds to the size of the cell in X.

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_CELL_SIZE_MIN`

Retrieves the recommended setting for the minimum cell size ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md)) for the trained code model(s).  For 2D matrix code types, this corresponds to the nominal minimum module size, whereas for other code types, this corresponds to the size of the cell in X.

| Value | Description |
| --- | --- |
| `Value` | Specifies the minimum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_CODE_FLIP`

Retrieves the recommended code flip setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CODE_FLIP`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_ANY` | Specifies to allow Aurora Imaging Library to decide whether the code needs to be flipped or read in the opposite direction to be read properly. |
| `M_FLIP` | Specifies that the code occurrence was reversed. |
| `M_NO_FLIP` | Specifies that the code occurrence was not reversed. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_CODE_MODEL_NUMBER_OF_OCCURRENCES`

Retrieves the total number of occurrences found of the code model(s) in all the images used for training.

#### `M_CODE_SEARCH_MODE`

Retrieves the recommended setting for the code search mode ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CODE_SEARCH_MODE`](../../Reference/code/McodeControl.md)) for the trained code model(s).  This result type is available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) and the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_BEST` | Specifies to search and return the best quality code occurrences. |
| `M_FAST` | Specifies to search and return the first code occurrences decoded. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_CODE_TYPE`

Retrieves the code type of the specified code model(s). The code type is retrieved as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

#### `M_DATAMATRIX_SHAPE`

Retrieves the recommended setting for the shape of the Data Matrix code type ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DATAMATRIX_SHAPE`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_ANY` *(default)* | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type can be any shape. |
| `M_RECTANGLE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a rectangular shape. |
| `M_SQUARE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a square shape. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_DECODE_ALGORITHM`

Retrieves the recommended setting for the decoding algorithm ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_CODE_DEFORMED` | Specifies to use the algorithm to decode deformed code occurrences. |
| `M_CODE_NOT_DEFORMED` | Specifies to use the algorithm to decode non-deformed code occurrences. |
| `M_SCANLINE_AT_ANGLE` | Specifies to use the algorithm to decode [`M_PDF417`](../../Reference/code/McodeModel.md) or [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code occurrences using rotated scanlines, without any localization. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_DOT_SIZE_MAX`

Retrieves the recommended setting for the maximum dot size ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SIZE_MAX`](../../Reference/code/McodeControl.md)) for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) model.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `Value > 0` | Specifies the maximum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_DOT_SIZE_MIN`

Retrieves the recommended setting for the minimum dot size ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SIZE_MIN`](../../Reference/code/McodeControl.md)) for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) model.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `Value > 0` | Specifies the minimum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_DOT_SPACING_MAX`

Retrieves the recommended setting for the maximum distance between 2 dots in a matrix code type composed of dots ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `-2 <= Value <= 3` | Specifies the distance. |

#### `M_DOT_SPACING_MIN`

Retrieves the recommended setting for the minimum distance between 2 dots in a matrix code type composed of dots ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md)) for the trained code model.  This result type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `-2 <= Value <= 3` | Specifies the distance. |

#### `M_ENCODING`

Retrieves the recommended encoding scheme ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md)) for the trained code model(s).  Note that since some versions of the GS1 Databar code types only differ by the bar height (and not the structure), the same result is returned for these similar code types. GS1 Databar Omnidirectional and GS1 Databar Truncated will return [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md). GS1 Databar Stacked and GS1 Databar Stacked Omnidirectional will return [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md).  It is possible to obtain a more accurate result to distinguish between these structurally similar code types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md).  Since training the corresponding control type is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this result type is not available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type after an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation.

| Value | Description |
| --- | --- |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeGetResult.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeGetResult.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |
| `M_ANY` | Specifies any type of encoding scheme. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_ERROR_CORRECTION`

Retrieves the recommended error correction scheme ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md)) for the trained code model(s).  Since training the corresponding control type is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this result type is not available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type after an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation.

| Value | Description |
| --- | --- |
| `M_ECC_4STATE` | Specifies  the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding. |
| `M_ECC_200` | Specifies  a Reed-Solomon-based algorithm as an error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies  an additional digit to check whether there is an error or not. |
| `M_ECC_COMPOSITE` | Specifies  the default error correction scheme for the 1D and 2D portions of the composite code. |
| `M_ECC_H` | Specifies  the highest-level error correction scheme. |
| `M_ECC_L` | Specifies  the lowest-level error correction scheme. |
| `M_ECC_M` | Specifies  a medium-low level error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction. |
| `M_ECC_Q` | Specifies  a medium-high level error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies  a Reed-Solomon type of error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies  a Reed-Solomon type of error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information. |
| `M_ANY` | Specifies that the error correction type is detected automatically. |
| `M_ECC_UNKNOWN` | Specifies an unknown error correction scheme. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_FOREGROUND_VALUE`

Retrieves the recommended foreground color setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the foreground color is black. |
| `M_FOREGROUND_WHITE` | Specifies that the foreground color is white. |
| `M_FOREGROUND_ANY` | Specifies the foreground color as black or white. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_SEARCH_ANGLE`

Retrieves the recommended setting for the nominal search angle ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal angle, in degrees, relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_SEARCH_ANGLE_DELTA_NEG`

Retrieves the recommended setting for the negative angular range of the search ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |
| `5.0 <= Value <= 180.0` | Specifies a negative angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). |

#### `M_SEARCH_ANGLE_DELTA_POS`

Retrieves the recommended setting for the positive angular range of the search ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md)) for the trained code model(s).

| Value | Description |
| --- | --- |
| `M_TRAIN` | Specifies that the corresponding control type was not trained since [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) was not selected for training; alternatively, it means that the corresponding control type does not apply to the code model(s). |
| `5.0 <= Value <= 180.0` | Specifies a positive angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). |

#### `M_SEARCH_ANGLE_STEP`

Retrieves the recommended setting for the angle increment/decrement ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md)) used when searching for PDF417 or TruncatedPDF417 codes through an angular range.

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies that no explicit increment/decrement is used. |
| `0.1 <= Value <= 180.0` | Specifies the explicit angle increment/decrement, in degrees. |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

#### `M_USE_PRESEARCH`

Retrieves the recommended presearch setting ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md)) for the trained code model. The presearch setting determines whether the localization operation is performed prior to the decoding step of an operation.  This result type is available for 2D matrix code types excluding the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the operation is not performed. |
| `M_FINDER_PATTERN_BASE` | Specifies that the localization operation is only performed on the base pattern of the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code (an "L" starting at the top-most left corner, and ending on the bottom-most right corner of the code). |
| `M_STAT_BASE` | Specifies to localize the code within the image with the statistical characteristics of a 2D bar code (for example, local variance and the presence of a lot of edges). |
| `M_TRAIN` | Specifies that the corresponding control type was not selected for training or does not apply to the code model(s). |

### For retrieving code occurrence-specific results from an

To retrieve a code occurrence-specific result that is returned from an [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values. In this case, the [`ResultIndex`](../../Reference/code/McodeGetResult.md)parameter must be set to the index of a specific occurrence or [`M_ALL`](../../Reference/code/McodeGetResult.md) (or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md)).

---

### `Code detect result buffer ID for occurrence-specific results`

Specifies a code detect result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeDetect`](../../Reference/code/McodeDetect.md) results.

#### `M_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_CODE_TYPE`

Retrieves the code type of the specified code occurrence(s). The code type is retrieved as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md). After an [`McodeDetect`](../../Reference/code/McodeDetect.md) operation, you can retrieve the code type's corresponding name using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_CODE_TYPE_NAME`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

#### `M_CODE_TYPE_NAME`

Retrieves the name of the code type of the specified code occurrence(s) detected. You can use this name, for example, to annotate the display of the image used with [`McodeDetect`](../../Reference/code/McodeDetect.md). To retrieve the code type as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md), use [`M_CODE_TYPE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_BC412_NAME` | Specifies a BC412 code type. |
| `M_CODABAR_NAME` | Specifies a Codabar code type. |
| `M_CODE39_NAME` | Specifies a Code 39 code type. |
| `M_CODE93_NAME` | Specifies a Code 93 code type. |
| `M_CODE128_NAME` | Specifies a Code 128 code type. |
| `M_EAN8_NAME` | Specifies an EAN 8 code type. |
| `M_EAN13_NAME` | Specifies an EAN 13 code type. |
| `M_EAN14_NAME` | Specifies an EAN 14 code type. |
| `M_GS1_128_NAME` | Specifies a GS1-128 code type. |
| `M_IATA25_NAME` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25_NAME` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25_NAME` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_UPC_A_NAME` | Specifies a UPC-A code type. |
| `M_UPC_E_NAME` | Specifies a UPC-E code type. |

#### `M_ENCODING`

Retrieves the type of encoding of the specified code occurrence(s).  Note that since some versions of the GS1 Databar code types only differ by the bar height (and not the structure), the same result is returned for these similar code types. GS1 Databar Omnidirectional and GS1 Databar Truncated will return [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md). GS1 Databar Stacked and GS1 Databar Stacked Omnidirectional will return [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md).  It is possible to obtain a more accurate result to distinguish between these structurally similar code types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeGetResult.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeGetResult.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |

#### `M_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s).

---

### `Code grade result buffer ID for occurrence-specific results`

Specifies a code grade result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeGrade`](../../Reference/code/McodeGrade.md) results.

#### `M_ANGLE`

Retrieves the angle, in degrees, of the specified code occurrence(s), relative to the output coordinate system specified using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

#### `M_APERTURE_SIZE_USED`

Retrieves the aperture size used for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the aperture size used. |

#### `M_ASTERISK`

Retrieves whether the you should append an asterisk to the overall symbol grade, for the specified code occurrence(s). Typically, an asterisk is appended to the overall symbol grade when the region containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides) has a symbol contrast value (reflectance) that might interfere with the reading of the code. This is the case, for example, if the region is too bright or too dark.  This result type is available for 2D matrix code types, except Maxicode.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_FALSE` | Specifies that no asterisk needs to be appended to the overall symbol grade. |
| `M_TRUE` | Specifies that an asterisk needs to be appended to the overall symbol grade. |

#### `M_AXIAL_NONUNIFORMITY`

Retrieves the axial non-uniformity of the specified code occurrence(s). The axial non-uniformity is calculated using the following formula:  *[Image: McodeGetResult_M_AXIAL_NONUNIFORMITY.png]*  Where _X<sub>avg</sub> _ is the average cell size in the X-dimension and _Y<sub>avg</sub> _ is the average measurement cell size in the Y-dimension.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the axial non-uniformity measure. |

#### `M_AXIAL_NONUNIFORMITY_GRADE`

Retrieves the axial non-uniformity of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_CELL_CONTRAST`

Retrieves the cell contrast value (CC) of the specified code occurrence(s). The cell contrast value is calculated as the difference between the average intensity of lighter cells minus the average intensity of darker cells, divided by the average intensity of lighter cells.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the cell contrast value. |

#### `M_CELL_CONTRAST_GRADE`

Retrieves the cell contrast value of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_CELL_DEFECTS`

Retrieves the cell defects value of the specified code occurrence(s). The cell defects value is the ratio between the number of incorrect pixels and the total number of pixels that fall within the bounds of the Data Matrix grid, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the cell defects value, expressed as a percentage. |

#### `M_CELL_HEIGHT`

Retrieves the Data Matrix cell height (DMCH) of the specified code occurrence(s). This is the average height of the cells in the Data Matrix. The Data Matrix cell height is calculated from the 4 Data Matrix corner points and the number of rows.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value > 0.0` | Specifies the cell height. |

#### `M_CELL_MODULATION_GRADE`

Retrieves the modulation of the specified code occurrence(s), as a grade calculated using the ISO/IEC 29158 standard. Modulation is a measure of the uniformity of reflectance in the dark and light areas of the code.  To retrieve the modulation of a code occurrence calculated using the ISO/IEC 15415 standard, use [`M_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md) and [`M_REFLECTANCE_MARGIN_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_CELL_NUMBER_X`

Retrieves the number of cells in the X-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells in the X-direction. |

#### `M_CELL_NUMBER_Y`

Retrieves the number of cells in the Y-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells in the Y-direction. |

#### `M_CELL_SIZE`

Retrieves the size of the cell in X (module size (Z)) of the specified code occurrence(s).

#### `M_CELL_SIZE_MAX`

Retrieves the maximum cell size of the code occurrence.  For 2D matrix code types, this corresponds to the nominal maximum module size, whereas for other code types, this corresponds to the same result as [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_CELL_SIZE_MIN`

Retrieves the minimum cell size of the code occurrence.  For 2D matrix code types, this corresponds to the nominal minimum module size, whereas for other code types, this corresponds to the same result as [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the minimum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_CELL_WIDTH`

Retrieves the Data Matrix cell width (DMCW) of the specified code occurrence(s). This is the average width of the cells in the Data Matrix. The Data Matrix cell width is calculated from the 4 Data Matrix corner points and the number of columns.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value > 0.0` | Specifies the cell width, in pixels. |

#### `M_CODE_FLIP`

Retrieves for the specified code occurrence(s) whether it was necessary to flip it/read it in the opposite direction.

| Value | Description |
| --- | --- |
| `M_FLIP` | Specifies that the code occurrence was reversed. |
| `M_NO_FLIP` | Specifies that the code occurrence was not reversed. |

#### `M_CODE_MODEL_ID`

Retrieves the identifier of the code model of the code occurrence. Note that this result is not valid when the result buffer is on a remote system. To find out which code model was used when the result buffer is on a remote system, use [`M_CODE_MODEL_INDEX`](../../Reference/code/McodeGetResult.md) instead.

#### `M_CODE_MODEL_INDEX`

Retrieves the index of the code model of the specified code occurrence(s).

#### `M_CODE_TYPE`

Retrieves the code type of the specified code occurrence(s). The code type is retrieved as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

#### `M_CODEWORD_DECODABILITY`

Retrieves a measure of the print quality of the codeword relative to the decoding algorithm, for the specified code occurrence(s). Note that this excludes any start/stop patterns.  To retrieve the decodability of the start/stop patterns of a code occurrence, use [`M_SCAN_DECODABILITY`](../../Reference/code/McodeGetResult.md).  The following result types are only available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md) (the default) before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the measure of the print quality of each codeword relative to the decoding algorithm. |

#### `M_CODEWORD_DECODABILITY_GRADE`

Retrieves the decodability of the codeword in the specified code occurrence(s), as a grade. Note that this excludes any start/stop patterns.  To retrieve the decodability of the start/stop patterns of a code occurrence as a grade, use [`M_SCAN_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CODEWORD_DEFECTS`

Retrieves a measure of the defects of the codeword in the specified code occurrence(s), as a ratio between the deviations in the expected signal. The larger the result, the greater the defects of the codeword and the less likely that the codeword can be decoded without error. Note that this excludes any start/stop patterns.  To retrieve a measure of the defects of the specified code occurrence(s) as a single grade, use [`M_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the defects of the start/stop patterns, use [`M_SCAN_DEFECTS`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the deviation in the expected signal that denotes a codeword in your code occurrence. |

#### `M_CODEWORD_DEFECTS_GRADE`

Retrieves a measure of the defects of the codeword in the specified code occurrence(s), as a grade. Note that this excludes any start/stop patterns.  To retrieve a measure of the defects of the specified code occurrence(s) as a single grade, use [`M_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the defects of the start/stop patterns, use [`M_SCAN_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CODEWORD_MODULATION`

Retrieves the modulation (MOD) of the codeword in the specified code occurrence(s). Modulation is a measure of the uniformity of reflectance in the dark and light areas of the code. For 2D cross-row and composite code types, modulation is calculated as the ratio of the minimum edge contrast value (EC<sub>min</sub>) to symbol contrast value ([`M_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md)) within the code.  To retrieve the modulation of the start/stop patterns of 2D cross-row and composite code types, use [`M_SCAN_MODULATION`](../../Reference/code/McodeGetResult.md).  This result type is available for Aztec, Data Matrix, DotCode, QR code, Micro QR, 2D cross-row, and composite code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the modulation. |

#### `M_CODEWORD_MODULATION_GRADE`

Retrieves the modulation (MOD) of the codeword in the specified code occurrence(s), as a grade.  To retrieve the modulation of the specified code occurrence(s) as a single grade, use [`M_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the modulation of the start/stop patterns of 2D cross-row and composite code types, as a grade, use [`M_SCAN_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).  This result type is available for Aztec, Data Matrix, DotCode, QR code, Micro QR, 2D cross-row, and composite code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CODEWORD_REFLECTANCE_MARGIN`

Retrieves the reflectance margin of the codeword in the specified code occurrence(s).  This result type is available for 2D matrix code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the reflectance margin. |

#### `M_CODEWORD_REFLECTANCE_MARGIN_GRADE`

Retrieves the reflectance margin of the codeword in the specified code occurrence(s), as a grade.  To retrieve the modulation of the specified code occurrence(s) as a single grade, use [`M_REFLECTANCE_MARGIN_GRADE`](../../Reference/code/McodeGetResult.md).  This result type is available for 2D matrix code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CODEWORD_YIELD`

Retrieves the codeword yield result of the specified code occurrence(s). The codeword yield result determines how well the code can be read at an angle relative to the horizontal and vertical axis of the code. When all other results are good, a poor codeword yield result can indicate a problem along the Y-axis of your code. For more information, refer to the [ISO/IEC 15415](ISO/IEC 15415) specification.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the codeword yield. |

#### `M_CODEWORD_YIELD_GRADE`

Retrieves the codeword yield, as a single grade for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CONTRAST_UNIFORMITY`

Retrieves the contrast uniformity of the specified code occurrence(s). The contrast uniformity is the minimum (MOD) value for the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the contrast uniformity. |

#### `M_CONTRAST_UNIFORMITY_GRADE`

Retrieves the contrast uniformity of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_CORNER_P1_X`

Retrieves the X-coordinate of the first corner (top-left) of the specified code occurrence(s). This result type returns the same value as [`M_TOP_LEFT_X`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P1_Y`

Retrieves the Y-coordinate of the first corner (top-left) of the specified code occurrence(s). This result type returns the same value as [`M_TOP_LEFT_Y`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P2_X`

Retrieves the X-coordinate of the second corner (bottom-left) of the specified code occurrence(s). This result type returns the same value as [`M_BOTTOM_LEFT_X`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P2_Y`

Retrieves the Y-coordinate of the second corner (bottom-left) of the specified code occurrence(s). This result type returns the same value as [`M_BOTTOM_LEFT_Y`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P3_X`

Retrieves the X-coordinate of the third corner (bottom-right) of the specified code occurrence(s). This result type returns the same value as [`M_BOTTOM_RIGHT_X`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P3_Y`

Retrieves the Y-coordinate of the third corner (bottom-right) of the specified code occurrence(s). This result type returns the same value as [`M_BOTTOM_RIGHT_Y`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P4_X`

Retrieves the X-coordinate of the fourth corner (top-right) of the specified code occurrence(s). This result type returns the same value as [`M_TOP_RIGHT_X`](../../Reference/code/McodeGetResult.md).

#### `M_CORNER_P4_Y`

Retrieves the Y-coordinate of the fourth corner (top-right) of the specified code occurrence(s). This result type returns the same value as [`M_TOP_RIGHT_Y`](../../Reference/code/McodeGetResult.md).

#### `M_DATA_CODEWORDS`

Retrieves the data codewords from the graphical representation of the specified code occurrence(s). Note that each codeword is returned as a numerical value, rather than as one or more alpha-numeric characters. The returned data codewords could be used to validate the code, as with PDF417, or to allow interpretation of the data codewords using a character set other than the default set supported by Aurora Imaging Library. For more information, refer to the Extended Channel Interpretation (ECI) protocol in ISO/IEC 15438:2006.  To retrieve the decoded characters from the specified code occurrence(s), use [`M_STRING`](../../Reference/code/McodeGetResult.md).

#### `M_DECODABILITY_GRADE`

Retrieves a measure of the print quality of the specified code occurrence(s) relative to the decoding algorithm, as a grade.  To determine the decodability for each codeword in the specified code occurrence(s), use [`M_CODEWORD_DECODABILITY`](../../Reference/code/McodeGetResult.md). To retrieve the decodability of the start/stop patterns of the specified code occurrence(s) as a grade, use [`M_SCAN_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_DECODE_GRADE`

Retrieves the code decodability of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies that the code can be read. |
| `M_CODE_GRADE_F` | Specifies that the code cannot be read. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_DECODING_MASK_SCORE`

Retrieves the score of each decoded mask of the specified code occurrence(s).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the score of each decoded mask of the specified code occurrence(s). |

#### `M_DECODING_MASK_SCORE_THRESHOLD`

Retrieves the threshold score of each decoded mask of the specified code occurrence(s).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md)code type.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the threshold score of each decoded mask of the specified code occurrence(s). |

#### `M_DECODING_MASK_TYPE`

Retrieves the decoding mask type of the specified code occurrence(s).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that no mask was applied to the code occurrence(s). |
| `M_MASK_0` | Specifies that the mask used for decoding is 0. |
| `M_MASK_0_PRIME` | Specifies that the mask used for decoding is 0 prime (0'). |
| `M_MASK_1` | Specifies that the mask used for decoding is 1. |
| `M_MASK_1_PRIME` | Specifies that the mask used for decoding is 1 prime (1'). |
| `M_MASK_2` | Specifies that the mask used for decoding is 2. |
| `M_MASK_2_PRIME` | Specifies that the mask used for decoding is 2 prime (2'). |
| `M_MASK_3` | Specifies that the mask used for decoding is 3. |
| `M_MASK_3_PRIME` | Specifies that the mask used for decoding is 3 prime (3'). |

#### `M_DEFECTS_GRADE`

Retrieves a measure of the defects in the specified code occurrence(s), as a grade.  To retrieve the measure of defects for each codeword as a grade, use [`M_CODEWORD_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the measure of defects in the start/stop patterns as a grade, use [`M_SCAN_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_DOT_SPACING_USED`

Retrieves the value between[`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md) used to decode the specified code occurrence(s).  The expected dot spacing is set using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md)and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md).  This result type is available for Aztec, Data Matrix, Maxicode, QR code, and Micro QR code types.

#### `M_ELEMENT_NUMBER_X`

Retrieves the number of elements in the X-direction.  For 1D code types, this result retrieves the number of bars and spaces. For GS1 Databar Stacked code types and composite code types whose 1D portion uses a GS1 Databar Stacked code type, this result retrieves the number of bars and spaces in all rows. For composite code types, this result retrieves the number of bars and spaces in the 1D portion of the code.  For 2D cross-row and 2D matrix code types, this result retrieves the same value as [`M_CELL_NUMBER_X`](../../Reference/code/McodeGetResult.md).

#### `M_ELEMENT_NUMBER_Y`

Retrieves the number of elements in the Y-direction.  For composite code types, this result retrieves the number of rows in the 1D portion of the code. For 2D matrix code types, this result retrieves the number of cells.  For 1D and 2D cross-row code types, this result retrieves the same value as [`M_CELL_NUMBER_Y`](../../Reference/code/McodeGetResult.md). Note that for 1D code types, excluding GS1 Databar, this result will always return 1.

#### `M_ENCODING`

Retrieves the type of encoding of the specified code occurrence(s).  Note that since some versions of the GS1 Databar code types only differ by the bar height (and not the structure), the same result is returned for these similar code types. GS1 Databar Omnidirectional and GS1 Databar Truncated will return [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md). GS1 Databar Stacked and GS1 Databar Stacked Omnidirectional will return [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md).  It is possible to obtain a more accurate result to distinguish between these structurally similar code types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeGetResult.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeGetResult.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |

#### `M_ERROR_CORRECTION`

Retrieves the type of error correction scheme of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_ECC_4STATE` | Specifies  the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding. |
| `M_ECC_200` | Specifies  a Reed-Solomon-based algorithm as an error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies  an additional digit to check whether there is an error or not. |
| `M_ECC_COMPOSITE` | Specifies  the default error correction scheme for the 1D and 2D portions of the composite code. |
| `M_ECC_H` | Specifies  the highest-level error correction scheme. |
| `M_ECC_L` | Specifies  the lowest-level error correction scheme. |
| `M_ECC_M` | Specifies  a medium-low level error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction. |
| `M_ECC_Q` | Specifies  a medium-high level error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies  a Reed-Solomon type of error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies  a Reed-Solomon type of error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information. |
| `M_ECC_UNKNOWN` | Specifies an unknown error correction scheme. |

#### `M_EXTENDED_AREA_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_CODEWORD_MODULATION`

Retrieves the modulation (MOD) of the codeword in the specified code occurrence(s) of any 2D matrix code type, except Maxicode, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_CODEWORD_MODULATION_GRADE`

Retrieves the modulation (MOD) of the codeword in the specified code occurrence(s) of any 2D matrix code type, except Maxicode, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_A1_GRADE`

Retrieves the fixed pattern damage in the A1 segment of the specified code occurrence(s) of a QR code or Micro QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_A2_GRADE`

Retrieves the fixed pattern damage in the A2 segment of the specified code occurrence(s) of a QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_A3_GRADE`

Retrieves the fixed pattern damage in the A3 segment of the specified code occurrence(s) of a QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_A_GRADE`

Retrieves the fixed pattern damage in the A segment of the specified code occurrence(s) of an Aztec code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_AVERAGE_GRADE`

Retrieves the average grade of the fixed pattern damage (FDP) in the specified code occurrence(s) of a Data Matrix code type, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_B1_GRADE`

Retrieves the fixed pattern damage in the B1 segment of the specified code occurrence(s) of a QR code or Micro QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_B2_GRADE`

Retrieves the fixed pattern damage in the B2 segment of the specified code occurrence(s) of a QR code or Micro QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_B_GRADE`

Retrieves the fixed pattern damage in each B segment of the specified code occurrence(s) of an Aztec code type with a full-range encoding scheme ([`M_ENC_AZTEC_FULL_RANGE`](../../Reference/code/McodeGetResult.md)), as a grade; the greatest and lowest reflectance are measured over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_C_GRADE`

Retrieves the fixed pattern damage in the C segment of the specified code occurrence(s) of a QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_CLOCKTRACK_SOLID_GRADE`

Retrieves the fixed pattern damage in the clock pattern and adjacent solid area segments of the specified code occurrence(s) of a Data Matrix code type, as a grade; the greatest and lowest reflectance are measured over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_GRADE`

Retrieves the fixed pattern damage in the specified code occurrence(s) of an Aztec, Data Matrix, DotCode, QR, or Micro QR code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_INTERSTITIAL_DOTS_GRADE`

Retrieves the fixed pattern damage of the interstitial dot positions, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_L1_GRADE`

Retrieves the fixed pattern damage in the L1 segment of the specified code occurrence(s) of a Data Matrix code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_L2_GRADE`

Retrieves the fixed pattern damage in the L2 segment of the specified code occurrence(s) of a Data Matrix code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_QUIET_ZONES_GRADE`

Retrieves the fixed pattern damage of the 3-dot wide quiet zones on all four sides, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_QZL1_GRADE`

Retrieves the fixed pattern damage in the QZL1 segment of the specified code occurrence(s) of a Data Matrix code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_FIXED_PATTERN_DAMAGE_QZL2_GRADE`

Retrieves the fixed pattern damage in the QZL2 segment of the specified code occurrence(s) of a Data Matrix code type, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_MODULATION_GRADE`

Retrieves the modulation (MOD) of the specified code occurrence(s) of any 2D matrix code type, except Maxicode, as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_QUIET_ZONE_INCLUDED`

Retrieves whether the quiet zone and the extended area (that is, 20 times the cell size beyond the quiet zone on all sides) were included in the source image of the code operation, for the specified code occurrence(s).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_FALSE` | Specifies that the quiet zone and extended area were not included. |
| `M_TRUE` | Specifies that the quiet zone and extended area were included. |

#### `M_EXTENDED_AREA_REFLECTANCE_MAXIMUM`

Retrieves the highest reflectance (Rmax) of the specified code occurrence(s) of any 2D matrix code type, except Maxicode, over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies the greatest reflectance value within the extended area. |

#### `M_EXTENDED_AREA_REFLECTANCE_MINIMUM`

Retrieves the lowest reflectance (Rmin) of the specified code occurrence(s) of any 2D matrix code type, except Maxicode, over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies the lowest reflectance value within the extended area. |

#### `M_EXTENDED_AREA_SYMBOL_CONTRAST`

Retrieves the symbol contrast value (SC) of the specified code occurrence(s), after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the symbol contrast value. |

#### `M_EXTENDED_AREA_SYMBOL_CONTRAST_GRADE`

Retrieves the symbol contrast value (SC) of the specified code occurrence(s), as a grade, after measuring the greatest and lowest reflectance over an area containing the code and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_EXTENDED_AREA_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_FINDER_PATTERN_DEFECTS`

Retrieves the finder pattern defects value of the specified code occurrence(s). The finder pattern defects value is the ratio between the number of incorrect pixels and the total number of pixels that fall within the bounds of the L finder pattern of the Data Matrix grid, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the finder pattern defects value, expressed as a percentage. |

#### `M_FIXED_PATTERN_DAMAGE_A1_GRADE`

Retrieves the fixed pattern damage in the A1 segment for the specified code occurrence(s) of a QR code or Micro QR code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code. For a QR code, the alignment pattern is also analyzed.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_A2_GRADE`

Retrieves the fixed pattern damage in the A2 segment for the specified code occurrence(s) of a QR code type as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_A3_GRADE`

Retrieves the fixed pattern damage in the A3 segment for the specified code occurrence(s) of a QR code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_A_GRADE`

Retrieves the fixed pattern damage in the A segment for the specified code occurrence(s) of an Aztec code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern and orientation patterns of the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_AVERAGE_GRADE`

Retrieves the average grade of the fixed pattern damage (FDP) in the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_B1_GRADE`

Retrieves the fixed pattern damage in the B1 segment for the specified code occurrence(s) of a QR code or Micro QR code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code. For a QR code, the alignment pattern is also analyzed.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_B2_GRADE`

Retrieves the fixed pattern damage in the B2 segment for the specified code occurrence(s) of a QR code or Micro QR code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code. For a QR code, the alignment pattern is also analyzed.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_B_GRADE`

Retrieves the fixed pattern damage in each B segment of the specified code occurrence(s) of an Aztec code type with a full-range encoding scheme ([`M_ENC_AZTEC_FULL_RANGE`](../../Reference/code/McodeGetResult.md)), as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, and orientation patterns of the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_C_GRADE`

Retrieves the fixed pattern damage in the C segment for the specified code occurrence(s) of a QR code type, as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_CLOCKTRACK_SOLID_GRADE`

Retrieves the grade of the clock pattern and adjacent solid area segments for the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_GRADE`

Retrieves the fixed pattern damage for the specified code occurrence(s), as a grade. The fixed pattern damage (FPD) is a measure of the amount of damage present in the finder pattern, clock pattern, and quiet zone of the code. For a QR code, the alignment pattern is also analyzed.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_INTERSTITIAL_DOTS_GRADE`

Retrieves the fixed pattern damage grade of the interstitial dot positions of the code occurrence(s).  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_FIXED_PATTERN_DAMAGE_L1_GRADE`

Retrieves the grade of the L1 segment for the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_L2_GRADE`

Retrieves the grade of the L2 segment for the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_QUIET_ZONES_GRADE`

Retrieves the fixed pattern damage grade of the 3-dot wide quiet zones on all four sides of code occurrence.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_FIXED_PATTERN_DAMAGE_QZL1_GRADE`

Retrieves the grade of the QZL1 segment for the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FIXED_PATTERN_DAMAGE_QZL2_GRADE`

Retrieves the grade of the QZL2 segment for the specified code occurrence(s) of a Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FOREGROUND_VALUE`

Retrieves the foreground color of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the foreground color is black. |
| `M_FOREGROUND_WHITE` | Specifies that the foreground color is white. |

#### `M_FORMAT_INFORMATION_1_GRADE`

Retrieves the readability of the format information in segment 1 of the specified code occurrence(s), as a grade. The format information is an embedded pattern that contains information about the characteristics of the code that are essential for further decoding.  This result type is available for the QR code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FORMAT_INFORMATION_2_GRADE`

Retrieves the readability of the format information in segment 2 of the specified code occurrence(s), as a grade. The format information is an embedded pattern that contains information about the characteristics of the code that are essential for further decoding.  This result type is available for the QR code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_FORMAT_INFORMATION_GRADE`

Retrieves the readability of the format information of the specified code occurrence(s), as a grade. The format information is an embedded pattern that contains information about the characteristics of the code that are essential for further decoding.  This result type is available for QR code and Micro QR code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_GRADING_STANDARD_EDITION_USED`

Retrieves the grading standard edition used to grade the code occurrence(s).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the most recent grading standard edition that is supported, which is dependent on the code type. |
| `M_ISO_15415_2011_15416_2000` | Specifies that the ISO/IEC 15415:2011 and ISO/IEC 15416:2000 specifications are used. |
| `M_ISO_15415_2011_15416_2016` | Specifies that the ISO/IEC 15415:2011 and ISO/IEC 15416:2016 specifications are used. |
| `M_ISO_15416_2000` | Specifies that the ISO/IEC 15416:2000 specification is used. |
| `M_ISO_15416_2016` | Specifies that the ISO/IEC 15416:2016 specification is used. |
| `M_ISO_29158_2011` | Specifies that the ISO/IEC 29158:2011 specification is used. |
| `M_ISO_29158_2020` | Specifies that the ISO/IEC 29158:2020 specification is used. |
| `M_SEMI_T10_0701` | Specifies that the Semi T10-0701 specification is used. |

#### `M_GRID_NONUNIFORMITY`

Retrieves the grid non-uniformity value of the specified code occurrence(s). Grid non-uniformity is calculated as the amount of deviation of the code's actual grid of an ideal grid.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the grid non-uniformity value. |

#### `M_GRID_NONUNIFORMITY_GRADE`

Retrieves the grid non-uniformity of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_HORIZONTAL_MARK_GROWTH`

Retrieves the horizontal mark growth (HMG) value of the specified code occurrence(s). The horizontal mark growth value is the ratio between the median mark cell width (MCW) and the sum of this width and the median space cell width (SCW) in the horizontal alternating pattern, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the horizontal mark growth value, expressed as a percentage. |

#### `M_HORIZONTAL_MARK_MISPLACEMENT`

Retrieves the horizontal mark misplacement (HMM) value of the specified code occurrence(s). The horizontal mark misplacement value is the average horizontal displacement of the marks' centers from the their ideal cell center point, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the horizontal mark misplacement value, expressed as a percentage. |

#### `M_IS_ECI`

Retrieves whether the decoded string of the specified code occurrence(s) contains character set ECIs.  This result type is available for Aztec, Data Matrix, DotCode, Maxicode, Micro PDF417, PDF417, QR code, and truncated PDF417 code types.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the decoded string does not contains character set ECIs. |
| `M_TRUE` | Specifies that the decoded string contains character set ECIs. |

#### `M_IS_GS1`

Retrieves whether the specified code occurrence(s) follows the industry standard for a GS1 code.  This result type is available for Aztec, Data Matrix, Code 128, EAN 14, GS1-128, GS1-Databar, QR code, DotCode, and composite code types.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the specified code occurrence(s) does not follow the industry standard for a GS1 code. |
| `M_TRUE` | Specifies that the specified code occurrence(s) follows the industry standard for a GS1 code. |

#### `M_MEAN_LIGHT_CALIBRATION`

Retrieves the mean intensity of the centers of the white elements of the reference code occurrence (MLcal).  This result is determined during the reflectance calibration phase of the ISO/IEC 29158 standard. To start the target grading phase, you must retrieve this result and load it into the code context of the target code. Use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeControl.md) to load this result into the target code context; alternatively, you can call [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DPM_CALIBRATION_RESULTS`](../../Reference/code/McodeControl.md).  > **Note:** This result type is available for 1D (except for 4-state, Pharmacode, Postnet, and Planet), Aztec, Data Matrix, DotCode, QR code, and Micro QR code types.

| Value | Description |
| --- | --- |
| `0 <= Value <= 255` | Specifies the mean intensity of the centers of the white elements. |

#### `M_MEAN_LIGHT_TARGET`

Retrieves the mean intensity of the centers of the white elements of the specified code occurrence(s) (MLtarget).  This result is determined during the target grading phase of the ISO/IEC 29158 standard.

| Value | Description |
| --- | --- |
| `0 <= Value <= 255` | Specifies the mean intensity of the centers of the white elements. |

#### `M_MINIMUM_REFLECTANCE`

Retrieves the amount of reflectance in the white elements of the specified code occurrence(s).  This result type will return [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md) if [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_LIGHTING_CONFIGURATION`](../../Reference/code/McodeControl.md) is set to [`M_90_DEGREE`](../../Reference/code/McodeControl.md) and [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_29158_2020`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the amount of reflectance in the white elements, as a percentage. |

#### `M_MINIMUM_REFLECTANCE_GRADE`

Retrieves the minimum reflectance of the specified code occurrence(s), as a grade.  This result type will return [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md) if [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_LIGHTING_CONFIGURATION`](../../Reference/code/McodeControl.md) is set to [`M_90_DEGREE`](../../Reference/code/McodeControl.md) and [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_29158_2020`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_MODULATION_GRADE`

Retrieves the modulation (MOD) of the specified code occurrence(s), as a grade calculated using the ISO standard. Modulation is a measure of the uniformity of reflectance in the dark and light areas of the code. For more information, refer to the [ISO/IEC 15415](ISO/IEC 15415) specification.  To retrieve the modulation of each codeword as a grade, use [`M_CODEWORD_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the modulation of the start/stop patterns of 2D cross-row and composite code types, as a grade, use [`M_SCAN_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the modulation of a code calculated using the ISO/IEC 29158 standard, use [`M_CELL_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).  This result type is available for Aztec, Data Matrix, QR code, Micro QR, 2D cross-row, and composite code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_NUMBER_OF_CODEWORDS`

Retrieves the number of codewords in the specified code occurrence(s); this includes data, overhead, and error correction codewords.

#### `M_NUMBER_OF_DATA_CODEWORDS`

Retrieves the number of data codewords in the specified code occurrence(s).

#### `M_NUMBER_OF_DECODED_ROWS`

Retrieves the number of successfully decoded rows of the specified code occurrence(s).  For GS1 Databar code types, the number of rows is the number of stacks in the specified code occurrence(s). For composite code types, this is the number of rows in the 1D portion of the code occurrence(s).

#### `M_NUMBER_OF_DECODED_SCANS`

Retrieves the total number of successfully decoded scanlines of the specified code occurrence(s). This is the sum of the number of scanlines per row, for every row within the code occurrence. For composite code types, this is the number of scanlines in the 1D portion of the code occurrence(s).

#### `M_NUMBER_OF_ERASURES`

Retrieves the number of erasures in the specified code occurrence(s). Erasures are missing or unreadable codewords at known positions.

#### `M_NUMBER_OF_ERROR_CORRECTION_CODEWORDS`

Retrieves the number of error correction codewords in the specified code occurrence(s). Error correction can be used to compensate for defects found during the decoding process.

#### `M_NUMBER_OF_ERRORS`

Retrieves the number of errors found in the specified code occurrence(s). Note that the number of errors does not include the number of erasures.

#### `M_NUMBER_OF_FIXED_PATTERN_DAMAGE_B_SEGMENT`

Retrieves the number of B segments in the specified code occurrence(s) of an Aztec code type with a full-range encoding scheme ([`M_ENC_AZTEC_FULL_RANGE`](../../Reference/code/McodeGetResult.md)); you can use this result type to establish the required array size to retrieve the fixed pattern damage in each B segment of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 18` | Specifies the number of B segment damage errors reported within a full Aztec code occurrence. |

#### `M_NUMBER_OF_INTERLEAVED_BLOCKS`

Retrieves the number of interleaved Reed-Solomon blocks in the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of interleaved Reed-Solomon blocks. |

#### `M_NUMBER_OF_ROWS`

Retrieves the number of rows graded in the specified code occurrence(s).  For GS1 Databar code types, the number of rows is the number of stacks in the specified code occurrence(s). For cross-row code types, the number of rows is the number of rows graded in the start/stop pattern grading.

#### `M_NUMBER_OF_SCANS`

Retrieves the total number of scanlines analyzed in the specified code occurrence(s). This is the sum of the number of scanlines per row, for every row within the code occurrence. The number of scanlines determines the number of results within an array for all the [`M_SCAN...`](../../Reference/code/McodeGetResult.md) values described in this function.

#### `M_OVERALL_SYMBOL_GRADE`

Retrieves the overall symbol grade of the specified code occurrence(s). The code type determines the calculation used to obtain the grade.  For 1D code types, the overall symbol grade is the average grade of the scan reflectance profile (using [`M_SCAN_REFLECTANCE_PROFILE_GRADE`](../../Reference/code/McodeGetResult.md)).  For composite code types, the overall symbol grade is the worst grade overall, obtained when analyzing either the 1D or the 2D portions.  For 2D matrix code types, the overall symbol grade is the worst of all the returned grades.  For cross-row codes, the overall symbol grade is the worst of all possible grades (with the exception of [`M_SCAN...GRADE`](../../Reference/code/McodeGetResult.md)).  To retrieve the overall symbol grade for each row in the code, use [`M_ROW_OVERALL_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the overall symbol grade. |

#### `M_POSITION_X`

Retrieves the X-coordinate of the specified code occurrence(s). For all types of codes, except the Data Matrix and composite code types, this result is the center of the code in the X-direction. For Data Matrix codes, after an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation this result is the center, in the X-direction, of the top-left cell. For composite codes, this result is the center, in the X-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_POSITION_Y`

Retrieves the Y-coordinate of the specified code occurrence(s). For all types of codes, except the Data Matrix and composite code types, this result is the center of the code in the Y-direction. For Data Matrix codes, after an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation this result is the center, in the Y-direction, of the top-left cell. For composite codes, this result is the center, in the Y-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_PRINT_GROWTH`

Retrieves the print growth value of the specified code occurrence(s). The print growth is a value determining whether the graphical features of the code have either grown or shrunk, relative to the theoretical model, sufficiently to hinder readability.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `-1.0 <= Value <= 1.0` | Specifies the print growth value. |

#### `M_PRINT_GROWTH_GRADE`

Retrieves the print growth value of the specified code occurrence(s), as a grade.  This result type is available for Aztec, Maxicode, and DotCode code types, and Data Matrix code types that do not use [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) with [`M_ECC_200`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_PRINT_GROWTH_HORIZONTAL`

Retrieves the horizontal print growth value of the code occurrence. The print growth is a value determining whether the graphical features of the code have either grown or shrunk, relative to the theoretical model, sufficiently to hinder readability.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `-1.0 <= Value <= 1.0` | Specifies the print growth value. |

#### `M_PRINT_GROWTH_HORIZONTAL_GRADE`

Retrieves the horizontal print growth value of the code occurrence, as a grade.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_PRINT_GROWTH_VERTICAL`

Retrieves the vertical print growth value of the code occurrence. The print growth is a value determining whether the graphical features of the code have either grown or shrunk, relative to the theoretical model, sufficiently to hinder readability.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `-1.0 <= Value <= 1.0` | Specifies the print growth value. |

#### `M_PRINT_GROWTH_VERTICAL_GRADE`

Retrieves the vertical print growth value of the code occurrence, as a grade.  This result type is available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_QUIET_ZONE_BOTTOM_LEFT_X`

Retrieves the bottom-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_LEFT_Y`

Retrieves the bottom-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_X`

Retrieves the bottom-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_Y`

Retrieves the bottom-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_INCLUDED`

Retrieves whether the quiet zone was included in the source image of the code operation, for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_FALSE` | Indicates the quiet zone was not included. |
| `M_TRUE` | Indicates the quiet zone was included. |

#### `M_QUIET_ZONE_TOP_LEFT_X`

Retrieves the top-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_LEFT_Y`

Retrieves the top-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_X`

Retrieves the top-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_Y`

Retrieves the top-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_R_MAX`

Retrieves the highest reflectance (Rmax) of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies the highest reflectance (Rmax). |

#### `M_R_MIN`

Retrieves the lowest reflectance (Rmin) of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies the lowest reflectance (Rmin). |

#### `M_RECOMMENDED_APERTURE_SIZE`

Retrieves the recommended aperture size for the specified code occurrence(s), following the [ISO/IEC 15415](ISO/IEC 15415) specification for a 2D matrix code type and the [ISO/IEC 15416](ISO/IEC 15416) specification for all other available code types.  Note that, for 1D, 2D cross-row, and composite code types, this value depends on [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md). If [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is not set (or is set to [`M_UNKNOWN`](../../Reference/code/McodeControl.md)), the value returned is `(0.5 x _cell size_)`.  If the [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is set to a value, the recommended aperture size is computed using the [ISO/IEC 15416](ISO/IEC 15416) specification.  For 2D matrix code types, the value returned is `(0.8 x _cell size_)`, regardless of the value of [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md).

#### `M_REFLECTANCE_CALIBRATION`

Retrieves the maximum reflectance value derived during the reflectance calibration phase (Rcal), for the specified code occurrence(s).  This result is determined during the reflectance calibration phase of the ISO/IEC 29158 standard. To start the target grading phase, you must retrieve this result and load it into the code context of the target code. Use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeControl.md) to load this result into the target code context; alternatively, you can call [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DPM_CALIBRATION_RESULTS`](../../Reference/code/McodeControl.md).  > **Note:** This result type is available for 1D (except for 4-state, Pharmacode, Postnet, and Planet), Aztec, Data Matrix, DotCode, QR code, and Micro QR code types.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies the maximum reflectance value derived during the reflectance calibration phase (Rcal). |

#### `M_REFLECTANCE_MARGIN_GRADE`

Retrieves the assessment of the codeword print quality for the specified code occurrence(s), based on the reflectance margin, as a grade. For more information, refer to the ISO/IEC 15415 specification.  This result type is available for 2D matrix code types, except Maxicode.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCORE`

Retrieves the confidence score of the code operation for the specified code occurrence(s). The code type determines how this value is calculated.  - For linear 1D and GS1 Databar code types, the score is based on:   - The number of redundant scanlines. Redundant scanlines can be omitted from a code, while still yielding the same result.   - The difference between the actual code and the theoretical code model. - For 1D Planet, Postnet, and 4-state code types, the score is 1 if the code was read and 0 if it was not. - For 2D code types, the score is calculated using: `1 - (_Number of errors _)/(_Number of error correction codewords_)`.   If the code could not be decoded successfully, the score is 0. - For 2D cross-row types, the score is calculated using: `1 - (_Number of errors _ + _Number of erasures _)/(_Number of codewords_)`.   If the code could not be decoded successfully, the score is 0. - For the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type, the score is calculated using: `1 - (_Number of errors_ + _Number of erasures_)/(_Number of error correction codewords_)`.   If the code could not be decoded successfully, the score is 0. - For composite codes, the score is the score of either the 1D part or the 2D part; whichever was lower.  To retrieve the error status of the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, use [`M_STATUS`](../../Reference/code/McodeGetResult.md). To retrieve the confidence score of an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation as a grade, use [`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the confidence score. 0 indicates 0% confidence and 1 indicates 100% confidence. |

#### `M_SIZE_X`

Retrieves the size of the specified code occurrence(s) in the X-direction.  For composite code types, this result is the width of the 1D component of the code.

#### `M_SIZE_Y`

Retrieves the size of the specified code occurrence(s) in the Y-direction.  For composite code types, this result is the height of the 1D component of the code.

#### `M_START_STOP_PATTERN_GRADE`

Retrieves the average of each row's worst scan reflectance profile grade for the specified code occurrence(s), as a grade. A start/stop pattern is a set pattern that marks the end points of the code. For each scan reflectance profile, the start/stop pattern grade is computed for each graded row, then the average start/stop pattern grade for all the scan reflectance profiles is computed.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the average of each row's worst scan reflectance profile grade. |

#### `M_STRING`

Retrieves the decoded string of the specified code occurrence(s).  If the string is decoded from a composite code model, the string is returned in the following format: 1D string|2D string, where | is used as the separator.  Note that for this result type, [`ResultIndex`](../../Reference/code/McodeGetResult.md) cannot be set to [`M_ALL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md).

#### `M_SYMBOL_CONTRAST`

Retrieves the symbol contrast value (SC) of the specified code occurrence(s). In the case of matrix code types, the symbol contrast value is calculated as the average 10% of the lighter pixels minus the average 10% of the darker pixels.  If using a Data Matrix code type, this result type is also available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_SEMI_T10_GRADING`](../../Reference/code/McodeControl.md) before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called. In this case, this result is expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result could not be computed for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the symbol contrast value. |
| `0.0 <= Value <= 100.0` | Specifies the symbol contrast value, expressed as a percentage. |

#### `M_SYMBOL_CONTRAST_GRADE`

Retrieves the symbol contrast value (SC) of the specified code occurrence(s), as a grade. The higher the contrast, the better it is for scanning purposes and the better the grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SYMBOL_CONTRAST_SNR`

Retrieves the symbol contrast signal to noise ratio (SNR) of the specified code occurrence(s). It is a relative measure of the symbol contrast (signal) to the maximum deviation in the light or dark grayscale levels in the symbol (noise).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `Value > 0.0` | Specifies the symbol contrast value. |

#### `M_THRESHOLD_MODE`

Retrieves the threshold mode used to internally binarize the source image for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_ADAPTIVE` | Specifies the use of a fast dynamic local threshold. |
| `M_GLOBAL_SEGMENTATION` | Specifies the use of a global threshold value. |
| `M_GLOBAL_WITH_LOCAL_RESEGMENTATION` | Specifies that the source image was globally thresholded and then the edges in the binarized image were resegmented according to the intensities of the surrounding bars and spaces in the original source image. |

#### `M_THRESHOLD_VALUE`

Retrieves the threshold value used to internally binarize the source image for the specified code occurrence(s).  Note that, this result type is only available when [`M_THRESHOLD_MODE`](../../Reference/code/McodeGetResult.md) is not set to [`M_ADAPTIVE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `0 <= Value <= 255` | Specifies the threshold value. |

#### `M_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_UNUSED_ERROR_CORRECTION`

Retrieves the ratio of unused error correction to the total amount of error correction available for the specified code occurrence(s). This tests the extent to which regional or spot damage in the code has eroded the reading safety margin that error correction provides.  Note that when retrieving this result from an[`McodeGrade`](../../Reference/code/McodeGrade.md) operation that used the SEMI standard ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) set to [`M_SEMI_T10_GRADING`](../../Reference/code/McodeControl.md)), this result is expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the ratio of unused error correction within the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the ratio of unused error correction within the specified code occurrence(s), expressed as a percentage. |

#### `M_UNUSED_ERROR_CORRECTION_GRADE`

Retrieves the ratio of unused error correction to the total amount of error correction available for the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_VERSION_INFORMATION_1_GRADE`

Retrieves the readability of the version information in segment 1 of the specified code occurrence(s), as a grade. The version information is an embedded pattern used to identify the location of the error correction bits in the code.  > **Note:** This result type is only available for the QR code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_VERSION_INFORMATION_2_GRADE`

Retrieves the readability of the version information in segment 2 of the specified code occurrence(s), as a grade. The version information is an embedded pattern used to identify the location of the error correction bits in the code.  > **Note:** This result type is only available for the QR code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_VERSION_INFORMATION_GRADE`

Retrieves the readability of the version information for the specified code occurrence(s), as a grade. The version information is an embedded pattern used to identify the location of the error correction bits in the code.  For QR codes version 1 through version 7, [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md) is returned.  > **Note:** This result type is only available for the QR code type.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies an interpolation value between grading values rounded to the nearest 0.1. |

#### `M_VERTICAL_MARK_GROWTH`

Retrieves the vertical mark growth (VMG) value of the specified code occurrence(s). The vertical mark growth value is the ratio between the median mark cell height (MCH) and the sum of this height and the median space cell height (SCH) in the vertical alternating pattern, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the vertical mark growth value, expressed as a percentage. |

#### `M_VERTICAL_MARK_MISPLACEMENT`

Retrieves the vertical mark misplacement (VMM) value of the specified code occurrence(s). The vertical mark misplacement value is the average vertical displacement of the marks' centers from the their ideal cell center point, expressed as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `M_CODE_GRADE_NOT_COMPUTABLE` | Specifies that the result was not computable for the specified code occurrence(s). |
| `0.0 <= Value <= 100.0` | Specifies the vertical mark misplacement value, expressed as a percentage. |

---

### `Code read result buffer ID for occurrence-specific results`

Specifies a code read result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeRead`](../../Reference/code/McodeRead.md) results.

#### `M_ANGLE`

Retrieves the angle, in degrees, of the specified code occurrence(s), relative to the output coordinate system specified using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

#### `M_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_CELL_NUMBER_X`

Retrieves the number of cells in the X-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| *(see [`M_CELL_NUMBER_X`](Reference/code/McodeGetResult.md))* |  |

#### `M_CELL_NUMBER_Y`

Retrieves the number of cells in the Y-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| *(see [`M_CELL_NUMBER_Y`](Reference/code/McodeGetResult.md))* |  |

#### `M_CELL_SIZE`

Retrieves the size of the cell in X (module size (Z)) of the specified code occurrence(s).

#### `M_CELL_SIZE_MAX`

Retrieves the maximum cell size of the code occurrence.  For 2D matrix code types, this corresponds to the nominal maximum module size, whereas for other code types, this corresponds to the same result as [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the maximum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_CELL_SIZE_MIN`

Retrieves the minimum cell size of the code occurrence.  For 2D matrix code types, this corresponds to the nominal minimum module size, whereas for other code types, this corresponds to the same result as [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the minimum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

#### `M_CODE_FLIP`

Retrieves for the specified code occurrence(s) whether it was necessary to flip it/read it in the opposite direction.

| Value | Description |
| --- | --- |
| *(see [`M_CODE_FLIP`](Reference/code/McodeGetResult.md))* |  |

#### `M_CODE_MODEL_ID`

Retrieves the identifier of the code model of the code occurrence. Note that this result is not valid when the result buffer is on a remote system. To find out which code model was used when the result buffer is on a remote system, use [`M_CODE_MODEL_INDEX`](../../Reference/code/McodeGetResult.md) instead.

#### `M_CODE_MODEL_INDEX`

Retrieves the index of the code model of the specified code occurrence(s).

#### `M_CODE_TYPE`

Retrieves the code type of the specified code occurrence(s). The code type is retrieved as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

#### `M_DATA_CODEWORDS`

Retrieves the data codewords from the graphical representation of the specified code occurrence(s). Note that each codeword is returned as a numerical value, rather than as one or more alpha-numeric characters. The returned data codewords could be used to validate the code, as with PDF417, or to allow interpretation of the data codewords using a character set other than the default set supported by Aurora Imaging Library. For more information, refer to the Extended Channel Interpretation (ECI) protocol in ISO/IEC 15438:2006.  To retrieve the decoded characters from the specified code occurrence(s), use [`M_STRING`](../../Reference/code/McodeGetResult.md).

#### `M_DOT_SPACING_USED`

Retrieves the value between[`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md) used to decode the specified code occurrence(s).  The expected dot spacing is set using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md)and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md).  This result type is available for Aztec, Data Matrix, Maxicode, QR code, and Micro QR code types.

#### `M_ELEMENT_NUMBER_X`

Retrieves the number of elements in the X-direction.  For 1D code types, this result retrieves the number of bars and spaces. For GS1 Databar Stacked code types and composite code types whose 1D portion uses a GS1 Databar Stacked code type, this result retrieves the number of bars and spaces in all rows. For composite code types, this result retrieves the number of bars and spaces in the 1D portion of the code.  For 2D cross-row and 2D matrix code types, this result retrieves the same value as [`M_CELL_NUMBER_X`](../../Reference/code/McodeGetResult.md).

#### `M_ELEMENT_NUMBER_Y`

Retrieves the number of elements in the Y-direction.  For composite code types, this result retrieves the number of rows in the 1D portion of the code. For 2D matrix code types, this result retrieves the number of cells.  For 1D and 2D cross-row code types, this result retrieves the same value as [`M_CELL_NUMBER_Y`](../../Reference/code/McodeGetResult.md). Note that for 1D code types, excluding GS1 Databar, this result will always return 1.

#### `M_ENCODING`

Retrieves the type of encoding of the specified code occurrence(s).  Note that since some versions of the GS1 Databar code types only differ by the bar height (and not the structure), the same result is returned for these similar code types. GS1 Databar Omnidirectional and GS1 Databar Truncated will return [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md). GS1 Databar Stacked and GS1 Databar Stacked Omnidirectional will return [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md).  It is possible to obtain a more accurate result to distinguish between these structurally similar code types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeGetResult.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeGetResult.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |

#### `M_ERROR_CORRECTION`

Retrieves the type of error correction scheme of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_ECC_4STATE` | Specifies  the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding. |
| `M_ECC_200` | Specifies  a Reed-Solomon-based algorithm as an error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies  an additional digit to check whether there is an error or not. |
| `M_ECC_COMPOSITE` | Specifies  the default error correction scheme for the 1D and 2D portions of the composite code. |
| `M_ECC_H` | Specifies  the highest-level error correction scheme. |
| `M_ECC_L` | Specifies  the lowest-level error correction scheme. |
| `M_ECC_M` | Specifies  a medium-low level error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction. |
| `M_ECC_Q` | Specifies  a medium-high level error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies  a Reed-Solomon type of error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies  a Reed-Solomon type of error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information. |
| `M_ECC_UNKNOWN` | Specifies an unknown error correction scheme. |

#### `M_EXTENDED_AREA_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_QUIET_ZONE_INCLUDED`

Retrieves whether the quiet zone and the extended area (that is, 20 times the cell size beyond the quiet zone on all sides) were included in the source image of the code operation, for the specified code occurrence(s).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

| Value | Description |
| --- | --- |
| *(see [`M_EXTENDED_AREA_QUIET_ZONE_INCLUDED`](Reference/code/McodeGetResult.md))* |  |

#### `M_EXTENDED_AREA_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_EXTENDED_AREA_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s), including the extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  > **Note:** This result type is only available when [`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md) is enabled.

#### `M_FOREGROUND_VALUE`

Retrieves the foreground color of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_FOREGROUND_BLACK` | Specifies that the foreground color is black. |
| `M_FOREGROUND_WHITE` | Specifies that the foreground color is white. |

#### `M_IS_ECI`

Retrieves whether the decoded string of the specified code occurrence(s) contains character set ECIs.  This result type is available for Aztec, Data Matrix, DotCode, Maxicode, Micro PDF417, PDF417, QR code, and truncated PDF417 code types.

| Value | Description |
| --- | --- |
| *(see [`M_IS_ECI`](Reference/code/McodeGetResult.md))* |  |

#### `M_IS_GS1`

Retrieves whether the specified code occurrence(s) follows the industry standard for a GS1 code.  This result type is available for Aztec, Data Matrix, Code 128, EAN 14, GS1-128, GS1-Databar, QR code, DotCode, and composite code types.

| Value | Description |
| --- | --- |
| *(see [`M_IS_GS1`](Reference/code/McodeGetResult.md))* |  |

#### `M_NUMBER_OF_CODEWORDS`

Retrieves the number of codewords in the specified code occurrence(s); this includes data, overhead, and error correction codewords.

#### `M_NUMBER_OF_DATA_CODEWORDS`

Retrieves the number of data codewords in the specified code occurrence(s).

#### `M_NUMBER_OF_DECODED_ROWS`

Retrieves the number of successfully decoded rows of the specified code occurrence(s).  For GS1 Databar code types, the number of rows is the number of stacks in the specified code occurrence(s). For composite code types, this is the number of rows in the 1D portion of the code occurrence(s).

#### `M_NUMBER_OF_DECODED_SCANS`

Retrieves the total number of successfully decoded scanlines of the specified code occurrence(s). This is the sum of the number of scanlines per row, for every row within the code occurrence. For composite code types, this is the number of scanlines in the 1D portion of the code occurrence(s).

#### `M_NUMBER_OF_ERASURES`

Retrieves the number of erasures in the specified code occurrence(s). Erasures are missing or unreadable codewords at known positions.

#### `M_NUMBER_OF_ERROR_CORRECTION_CODEWORDS`

Retrieves the number of error correction codewords in the specified code occurrence(s). Error correction can be used to compensate for defects found during the decoding process.

#### `M_NUMBER_OF_ERRORS`

Retrieves the number of errors found in the specified code occurrence(s). Note that the number of errors does not include the number of erasures.

#### `M_POSITION_X`

Retrieves the X-coordinate of the specified code occurrence(s). For all types of codes, except the Data Matrix and composite code types, this result is the center of the code in the X-direction. For Data Matrix codes, after an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation this result is the center, in the X-direction, of the top-left cell. For composite codes, this result is the center, in the X-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_POSITION_Y`

Retrieves the Y-coordinate of the specified code occurrence(s). For all types of codes, except the Data Matrix and composite code types, this result is the center of the code in the Y-direction. For Data Matrix codes, after an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation this result is the center, in the Y-direction, of the top-left cell. For composite codes, this result is the center, in the Y-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_QUIET_ZONE_BOTTOM_LEFT_X`

Retrieves the bottom-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_LEFT_Y`

Retrieves the bottom-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_X`

Retrieves the bottom-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_Y`

Retrieves the bottom-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_INCLUDED`

Retrieves whether the quiet zone was included in the source image of the code operation, for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_FALSE` | Indicates the quiet zone was not included. |
| `M_TRUE` | Indicates the quiet zone was included. |

#### `M_QUIET_ZONE_TOP_LEFT_X`

Retrieves the top-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_LEFT_Y`

Retrieves the top-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_X`

Retrieves the top-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_Y`

Retrieves the top-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_RECOMMENDED_APERTURE_SIZE`

Retrieves the recommended aperture size for the specified code occurrence(s), following the [ISO/IEC 15415](ISO/IEC 15415) specification for a 2D matrix code type and the [ISO/IEC 15416](ISO/IEC 15416) specification for all other available code types.  Note that, for 1D, 2D cross-row, and composite code types, this value depends on [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md). If [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is not set (or is set to [`M_UNKNOWN`](../../Reference/code/McodeControl.md)), the value returned is `(0.5 x _cell size_)`.  If the [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is set to a value, the recommended aperture size is computed using the [ISO/IEC 15416](ISO/IEC 15416) specification.  For 2D matrix code types, the value returned is `(0.8 x _cell size_)`, regardless of the value of [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md).

#### `M_SCORE`

Retrieves the confidence score of the code operation for the specified code occurrence(s). The code type determines how this value is calculated.  - For linear 1D and GS1 Databar code types, the score is based on:   - The number of redundant scanlines. Redundant scanlines can be omitted from a code, while still yielding the same result.   - The difference between the actual code and the theoretical code model. - For 1D Planet, Postnet, and 4-state code types, the score is 1 if the code was read and 0 if it was not. - For 2D code types, the score is calculated using: `1 - (_Number of errors _)/(_Number of error correction codewords_)`.   If the code could not be decoded successfully, the score is 0. - For 2D cross-row types, the score is calculated using: `1 - (_Number of errors _ + _Number of erasures _)/(_Number of codewords_)`.   If the code could not be decoded successfully, the score is 0. - For the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type, the score is calculated using: `1 - (_Number of errors_ + _Number of erasures_)/(_Number of error correction codewords_)`.   If the code could not be decoded successfully, the score is 0. - For composite codes, the score is the score of either the 1D part or the 2D part; whichever was lower.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the confidence score. 0 indicates 0% confidence and 1 indicates 100% confidence. |

#### `M_SIZE_X`

Retrieves the size of the specified code occurrence(s) in the X-direction.  For composite code types, this result is the width of the 1D component of the code.

#### `M_SIZE_Y`

Retrieves the size of the specified code occurrence(s) in the Y-direction.  For composite code types, this result is the height of the 1D component of the code.

#### `M_STRING`

Retrieves the decoded string of the specified code occurrence(s).  If the string is decoded from a composite code model, the string is returned in the following format: 1D string|2D string, where | is used as the separator.  Note that for this result type, [`ResultIndex`](../../Reference/code/McodeGetResult.md) cannot be set to [`M_ALL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md).

#### `M_THRESHOLD_MODE`

Retrieves the threshold mode used to internally binarize the source image for the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_ADAPTIVE` | Specifies the use of a fast dynamic local threshold. |
| `M_GLOBAL_SEGMENTATION` | Specifies the use of a global threshold value. |
| `M_GLOBAL_WITH_LOCAL_RESEGMENTATION` | Specifies that the source image was globally thresholded and then the edges in the binarized image were resegmented according to the intensities of the surrounding bars and spaces in the original source image. |

#### `M_THRESHOLD_VALUE`

Retrieves the threshold value used to internally binarize the source image for the specified code occurrence(s).  Note that, this result type is only available when [`M_THRESHOLD_MODE`](../../Reference/code/McodeGetResult.md) is not set to [`M_ADAPTIVE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `0 <= Value <= 255` | Specifies the threshold value. |

#### `M_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s).

---

### `Code write result buffer ID for occurrence-specific results`

Specifies a code write result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_WRITE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeWrite`](../../Reference/code/McodeWrite.md) results.

#### `M_ANGLE`

Retrieves the angle, in degrees, of the specified code occurrence(s), relative to the output coordinate system specified using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md).

#### `M_BOTTOM_LEFT_X`

Retrieves the X-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_LEFT_Y`

Retrieves the Y-coordinate of the bottom-left corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_X`

Retrieves the X-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_BOTTOM_RIGHT_Y`

Retrieves the Y-coordinate of the bottom-right corner of the specified code occurrence(s).

#### `M_CELL_NUMBER_X`

Retrieves the number of cells in the X-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells in the X-direction. |

#### `M_CELL_NUMBER_Y`

Retrieves the number of cells in the Y-direction of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of cells in the Y-direction. |

#### `M_CELL_SIZE`

Retrieves the size of the cell in X (module size (Z)) of the specified code occurrence(s).

#### `M_CODE_TYPE`

Retrieves the code type of the specified code occurrence(s). The code type is retrieved as a numeric that can be passed to [`McodeModel`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

#### `M_ELEMENT_NUMBER_X`

Retrieves the number of elements in the X-direction.  For 1D code types, this result retrieves the number of bars and spaces. For GS1 Databar Stacked code types and composite code types whose 1D portion uses a GS1 Databar Stacked code type, this result retrieves the number of bars and spaces in all rows. For composite code types, this result retrieves the number of bars and spaces in the 1D portion of the code.  For 2D cross-row and 2D matrix code types, this result retrieves the same value as [`M_CELL_NUMBER_X`](../../Reference/code/McodeGetResult.md).

#### `M_ELEMENT_NUMBER_Y`

Retrieves the number of elements in the Y-direction.  For composite code types, this result retrieves the number of rows in the 1D portion of the code. For 2D matrix code types, this result retrieves the number of cells.  For 1D and 2D cross-row code types, this result retrieves the same value as [`M_CELL_NUMBER_Y`](../../Reference/code/McodeGetResult.md). Note that for 1D code types, excluding GS1 Databar, this result will always return 1.

#### `M_ENCODING`

Retrieves the type of encoding of the specified code occurrence(s).  Note that since some versions of the GS1 Databar code types only differ by the bar height (and not the structure), the same result is returned for these similar code types. GS1 Databar Omnidirectional and GS1 Databar Truncated will return [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md). GS1 Databar Stacked and GS1 Databar Stacked Omnidirectional will return [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md).  It is possible to obtain a more accurate result to distinguish between these structurally similar code types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) set to [`M_HIGH`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeGetResult.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeGetResult.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |

#### `M_ERROR_CORRECTION`

Retrieves the type of error correction scheme of the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_ECC_4STATE` | Specifies  the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding. |
| `M_ECC_200` | Specifies  a Reed-Solomon-based algorithm as an error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies  an additional digit to check whether there is an error or not. |
| `M_ECC_COMPOSITE` | Specifies  the default error correction scheme for the 1D and 2D portions of the composite code. |
| `M_ECC_H` | Specifies  the highest-level error correction scheme. |
| `M_ECC_L` | Specifies  the lowest-level error correction scheme. |
| `M_ECC_M` | Specifies  a medium-low level error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction. |
| `M_ECC_Q` | Specifies  a medium-high level error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies  a Reed-Solomon type of error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies  a Reed-Solomon type of error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information. |
| `M_ECC_UNKNOWN` | Specifies an unknown error correction scheme. |

#### `M_IS_GS1`

Retrieves whether the specified code occurrence(s) follows the industry standard for a GS1 code.  This result type is available for Aztec, Data Matrix, Code 128, EAN 14, GS1-128, GS1-Databar, QR code, DotCode, and composite code types.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the specified code occurrence(s) does not follow the industry standard for a GS1 code. |
| `M_TRUE` | Specifies that the specified code occurrence(s) follows the industry standard for a GS1 code. |

#### `M_POSITION_X`

Retrieves the X-coordinate of the specified code occurrence(s). For all types of codes, except composite code types, this result is the center of the code in the X-direction. For composite codes, this result is the center, in the X-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_POSITION_Y`

Retrieves the Y-coordinate of the specified code occurrence(s). For all types of codes, except composite code types, this result is the center of the code in the Y-direction. For composite codes, this result is the center, in the Y-direction, of the 1D component of the code. Note that to retrieve the top-left and bottom-right coordinates, use [`M_BOTTOM...`](../../Reference/code/McodeGetResult.md) and [`M_TOP...`](../../Reference/code/McodeGetResult.md).

#### `M_QUIET_ZONE_BOTTOM_LEFT_X`

Retrieves the bottom-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_LEFT_Y`

Retrieves the bottom-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_X`

Retrieves the bottom-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_BOTTOM_RIGHT_Y`

Retrieves the bottom-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_LEFT_X`

Retrieves the top-left position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_LEFT_Y`

Retrieves the top-left position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_X`

Retrieves the top-right position along the X-axis of the specified code occurrence(s), including the quiet zone.

#### `M_QUIET_ZONE_TOP_RIGHT_Y`

Retrieves the top-right position along the Y-axis of the specified code occurrence(s), including the quiet zone.

#### `M_SIZE_X`

Retrieves the size of the specified code occurrence(s) in the X-direction.  For composite code types, this result is the width of the 1D component of the code.

#### `M_SIZE_Y`

Retrieves the size of the specified code occurrence(s) in the Y-direction.  For composite code types, this result is the height of the 1D component of the code.

#### `M_STRING`

Retrieves the decoded string of the specified code occurrence(s).  If the string is decoded from a composite code model, the string is returned in the following format: 1D string|2D string, where | is used as the separator.  Note that for this result type, [`ResultIndex`](../../Reference/code/McodeGetResult.md) cannot be set to [`M_ALL`](../../Reference/code/McodeGetResult.md) or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md).

#### `M_TOP_LEFT_X`

Retrieves the X-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_LEFT_Y`

Retrieves the Y-coordinate of the top-left corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_X`

Retrieves the X-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_TOP_RIGHT_Y`

Retrieves the Y-coordinate of the top-right corner of the specified code occurrence(s).

#### `M_WRITE_SIZE_X`

Specifies the minimum width for the destination image.

#### `M_WRITE_SIZE_Y`

Specifies the minimum height for the destination image.

### For retrieving row-specific results from an

To retrieve a row result that is returned from an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values. In this case, the [`ResultIndex`](../../Reference/code/McodeGetResult.md) parameter must be set to the index of a specific occurrence or [`M_ALL`](../../Reference/code/McodeGetResult.md), and the [`RowOrScanIndex`](../../Reference/code/McodeGetResult.md)parameter must be set to the index of a specific row or to [`M_ALL`](../../Reference/code/McodeGetResult.md) (or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md)). If [`RowOrScanIndex`](../../Reference/code/McodeGetResult.md) is set to an index of a specific row, [`ResultIndex`](../../Reference/code/McodeGetResult.md) cannot be set to [`M_ALL`](../../Reference/code/McodeGetResult.md); it must be set to an index of a specific occurrence.

---

### `Code grade result buffer ID for row-specific results`

Specifies a code grade result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeGrade`](../../Reference/code/McodeGrade.md) results.

#### `M_ROW_NUMBER_OF_DECODED_SCANS`

Retrieves the number of successfully decoded scanlines per row, for the specified row(s) within the specified code occurrence(s). For composite code types, this is the number of scanlines per row in the 1D portion of the code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the code occurrence. |
| `Value > 0` | Specifies the number of successfully decoded scanlines for the row. |

#### `M_ROW_NUMBER_OF_SCANS`

Retrieves the number of scanlines analyzed per row, for the specified row(s) within the specified code occurrence(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the code occurrence. |
| `Value > 0` | Specifies the number of scanlines analyzed for the row. |

#### `M_ROW_OVERALL_GRADE`

Retrieves the overall grade per row, for the specified row(s) within the specified code occurrence(s).  To retrieve the average of all the row overall grades, use [`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md).  For stacked sub-types of the GS1 Databar code type, the overall row grade is the overall grade per stack of the code.  For all other 1D code types, the overall row grade is the overall grade for the entire code.  For 2D cross-row code types, the overall row grade is the worst grade of the row.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the code occurrence. |
| `0.0 <= Value <= 4.0` | Specifies the overall grade for the row. |

---

### `Code read result buffer ID for row-specific results`

Specifies a code read result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeRead`](../../Reference/code/McodeRead.md) results.

#### `M_ROW_NUMBER_OF_DECODED_SCANS`

Retrieves the number of successfully decoded scanlines per row, for the specified row(s) within the specified code occurrence(s). For composite code types, this is the number of scanlines per row in the 1D portion of the code occurrence(s).

### For retrieving scanline-specific results from an

To retrieve a scanline result that is returned from an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values. In this case, the [`ResultIndex`](../../Reference/code/McodeGetResult.md) parameter must be set to the index of a specific occurrence or [`M_ALL`](../../Reference/code/McodeGetResult.md), and the [`RowOrScanIndex`](../../Reference/code/McodeGetResult.md)parameter must be set to the index of a specific scanline or to [`M_ALL`](../../Reference/code/McodeGetResult.md) (or [`M_DEFAULT`](../../Reference/code/McodeGetResult.md)). If [`RowOrScanIndex`](../../Reference/code/McodeGetResult.md) is set to an index of a specific scanline, [`ResultIndex`](../../Reference/code/McodeGetResult.md) cannot be set to [`M_ALL`](../../Reference/code/McodeGetResult.md); it must be set to an index of a specific occurrence.

---

### `Code grade result buffer ID for scanline-specific results`

Specifies a code grade result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_GRADE_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeGrade`](../../Reference/code/McodeGrade.md) results.

#### `M_DECODED_SCANS_END_X`

Retrieves the X-coordinate of the end point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_END_Y`

Retrieves the Y-coordinate of the end point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_SCORE`

Retrieves the score of the specified decoded scanline(s) of the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the score of the decoded scanline. |

#### `M_DECODED_SCANS_START_X`

Retrieves the X-coordinate of the start point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_START_Y`

Retrieves the Y-coordinate of the start point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_SCAN_DECODABILITY`

Retrieves the decodability result for the scan reflectance profile.  To retrieve the decodability for each codeword, use [`M_CODEWORD_DECODABILITY`](../../Reference/code/McodeGetResult.md). To retrieve the decodability of the specified code occurrence(s) as a grade, use [`M_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).  Note that certain code types have additional decodability measures that are taken into account when computing the decodability result. See the [ISO 15417](ISO 15417) (Code 128) and [ISO 15420](ISO 15420) (EAN and UPC code types) specifications.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the decodability value. |

#### `M_SCAN_DECODABILITY_GRADE`

Retrieves the decodability result for the scan reflectance profile, as a grade.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md), the grade is retrieved as an interpolated value.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to a grading standard edition prior to ISO/IEC 15416:2016, the grade is retrieved as a letter grade.  To retrieve the decodability for each codeword as a grade, use [`M_CODEWORD_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the decodability for a cross-row code occurrence as a grade, use [`M_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the decodability grade as an interpolated value rounded to the nearest 0.1 when [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

#### `M_SCAN_DECODE_GRADE`

Retrieves whether the decoding algorithm succeeded or failed for the scan reflectance profile, as a grade.  To retrieve the error status of the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, use [`M_STATUS`](../../Reference/code/McodeGetResult.md). To retrieve the confidence score of the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, use [`M_SCORE`](../../Reference/code/McodeGetResult.md).  Note that [`M_SCAN_EDGE_DETERMINATION_GRADE`](../../Reference/code/McodeGetResult.md) is used in calculating this value.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies that the decoding algorithm succeeded. |
| `M_CODE_GRADE_F` | Specifies that the decoding algorithm failed. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_DEFECTS`

Retrieves a measure of the defects in the scan reflectance profile. The result is based on the ERN maximum ([`M_SCAN_ERN_MAXIMUM`](../../Reference/code/McodeGetResult.md)) and the symbol contrast value ([`M_SCAN_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md)).  To retrieve the measure for each codeword, use [`M_CODEWORD_DEFECTS`](../../Reference/code/McodeGetResult.md). To retrieve the measure for the specified code occurrence(s) as a grade, use [`M_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies a measure of the defects. A value of 1.0 indicates that the scan is perfect; whereas, a value of 0.0 indicates that scan is very bad. |

#### `M_SCAN_DEFECTS_GRADE`

Retrieves a measure of the defects in the scan reflectance profile, as a grade.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md), the grade is retrieved as an interpolated value.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to a grading standard edition prior to ISO/IEC 15416:2016, the grade is retrieved as a letter grade.  To retrieve the measure of defects in each codeword, as a grade, use [`M_CODEWORD_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the measure of defects in the specified code occurrence(s), use [`M_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the defects grade as an interpolated value rounded to the nearest 0.1 when [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

#### `M_SCAN_EDGE_CONTRAST_MINIMUM`

Retrieves the minimum edge contrast value of the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the minimum edge contrast value. |

#### `M_SCAN_EDGE_CONTRAST_MINIMUM_GRADE`

Retrieves the minimum edge contrast value of the scan reflectance profile, as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_EDGE_DETERMINATION_GRADE`

Retrieves the edge determination of the scan reflectance profile, as a grade. Edge determination examines the number of bars and spaces in your code. The presence of scratches on the bars and spaces, including the quiet zone, could affect this value. Note that this grade is not considered when calculating the overall code grade ([`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md)), instead it is included in the decode grade ([`M_SCAN_DECODE_GRADE`](../../Reference/code/McodeGetResult.md)).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_EDGE_DETERMINATION_WARNING`

Retrieves the edge determination warning of the scan reflectance profile when[`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md)is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md).  An edge determination warning is issued when the minimum reflectance margin for any element is less than 5% of the symbol contrast value. This warning might indicate that the symbol is close to a failing grade for edge determination.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that an edge determination warning was not issued. |
| `M_TRUE` | Specifies that an edge determination warning was issued. |

#### `M_SCAN_ERN_MAXIMUM`

Retrieves the highest ERN (element reflectance non-uniformity) in the scan reflectance profile. The ERN is an unexpected peak in the scan reflectance profile. The result is expressed as the ratio of the maximum ERN relative to the symbol contrast value `(_ERN<sup>max</sup> _/_SC_)`.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the element reflectance non-uniformity maximum. |

#### `M_SCAN_GUARD_PATTERN`

Retrieves the size of the interior guard pattern, expressed as a factor of the cell (module size (_Z_)), in the scan reflectance profile. Guard patterns consist of two one-cell wide groupings at the end of the specified code occurrence(s). GS1 Databar code types have guard patterns at the ends of each row of the specified code occurrence(s). See the [ISO 24724](ISO 24724) specification. Note that the value is negative if the left guard pattern is greater than the right guard pattern, to permit easy identification.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the size of the interior guard pattern, expressed as a factor of the cell (module size (_Z_)) in the scan reflectance profile. |

#### `M_SCAN_GUARD_PATTERN_GRADE`

Retrieves the largest interior guard pattern in the scan reflectance profile, as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_INTERCHARACTER_GAP`

Retrieves the size of the largest inter-character gap in the scan reflectance profile, expressed as a factor of the cell size (module size (_Z_)) in the scan reflectance profile. See [ISO 16388](ISO 16388) and [AIM BC3-1995](AIM BC3-1995) for more details.  Note that this result type will always return the value of [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md) when [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is not set, and the following condition is true:  (`5.3 _Z_&lt;= _Largest inter-character gap_ &lt;= 3_Z_`).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the largest inter-character gap. |

#### `M_SCAN_INTERCHARACTER_GAP_GRADE`

Retrieves the largest inter-character gap in the scan reflectance profile, as a grade. The largest inter-character gap is calculated as a factor of the cell size (module size (_Z_)) in the scan reflectance profile. See [ISO 16388](ISO 16388) and [AIM BC3-1995](AIM BC3-1995) for more details.  Note that this result type will always return the value of [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md) when [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) is not set, and the following condition is true:  (`5.3 _Z_&lt;= _Largest inter-character gap_ &lt;= 3_Z_`).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies that the maximum inter-character gap is under three times the module size (3Z). |
| `M_CODE_GRADE_F` | Specifies that the maximum inter-character gap is over 5.3 times the module size (5.3Z). |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_MINIMUM_REFLECTANCE_MARGIN`

Retrieves the minimum reflectance margin found in the scan reflectance profile when[`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md)is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the minimum reflectance margin. |

#### `M_SCAN_MODULATION`

Retrieves the modulation (MOD) of the scan reflectance profile. The modulation is the ratio of the minimum edge contrast value ([`M_SCAN_EDGE_CONTRAST_MINIMUM`](../../Reference/code/McodeGetResult.md)) to the symbol contrast value ([`M_SCAN_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md)).  To retrieve the modulation of each codeword, use [`M_CODEWORD_MODULATION`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the modulation value. |

#### `M_SCAN_MODULATION_GRADE`

Retrieves the modulation (MOD) of the scan reflectance profile, as a grade.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md), the grade is retrieved as an interpolated value.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to a grading standard edition prior to ISO/IEC 15416:2016, the grade is retrieved as a letter grade.  To retrieve the modulation of each codeword as a grade, use [`M_CODEWORD_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md). To retrieve the modulation for the specified code occurrence(s) as a grade, use [`M_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the modulation grade as an interpolated value rounded to the nearest 0.1 when [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

#### `M_SCAN_PRINT_CONTRAST_SIGNAL`

Retrieves the print contrast signal (PCS) of the scan reflectance profile. Note that this is an approximation based on the maximum reflectance ([`M_SCAN_REFLECTANCE_MAXIMUM`](../../Reference/code/McodeGetResult.md)) and the symbol contrast value ([`M_SCAN_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md)). For more details, see the [ISO/IEC 15416](ISO/IEC 15416) specification.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the print contrast signal. |

#### `M_SCAN_PROFILE_END_X`

Retrieves the X-coordinate of the end of the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the X-coordinate. |

#### `M_SCAN_PROFILE_END_Y`

Retrieves the Y-coordinate of the end of the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the Y-coordinate. |

#### `M_SCAN_PROFILE_START_X`

Retrieves the X-coordinate of the start of the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the X-coordinate. |

#### `M_SCAN_PROFILE_START_Y`

Retrieves the Y-coordinate of the start of the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the Y-coordinate. |

#### `M_SCAN_QUIET_ZONE`

Retrieves the ratio, for the scan reflectance profile, between the expected quiet zone of the theoretical code model and the measured quiet zone of the specified code occurrence(s) (`_measured quiet zone size_ / _expected quiet zone size_`).  The measurement is taken from both sides. Note that if the returned value is negative, the left quiet zone is worse than the right. Refer to the appropriate specifications for more details ([ISO 15417](ISO 15417), [ISO 15420](ISO 15420), [ISO 16388](ISO 16388), and [ISO 16390](ISO 16390), respectively).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value` | Specifies the ratio. |

#### `M_SCAN_QUIET_ZONE_GRADE`

Retrieves the quiet zone size of the scan reflectance profile, as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_REFLECTANCE_MAXIMUM`

Retrieves the highest reflectance (_R<sub>max</sub> _) of the scan reflectance profile, as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the highest reflectance (Rmax) of the scan reflectance profile, as a percentage. |

#### `M_SCAN_REFLECTANCE_MINIMUM`

Retrieves the lowest reflectance (_R<sub>min</sub> _) of the scan reflectance profile, as a percentage.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the lowest reflectance (Rmin) of the scan reflectance profile, as a percentage. |

#### `M_SCAN_REFLECTANCE_MINIMUM_GRADE`

Retrieves a pass or fail grade for the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies that the scan reflectance profile passed. |
| `M_CODE_GRADE_F` | Specifies that the scan reflectance profile failed. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

#### `M_SCAN_REFLECTANCE_PROFILE_GRADE`

Retrieves for the scan reflectance profile, its lowest grade among all its [`M_SCAN_..._GRADE`](../../Reference/code/McodeGetResult.md) results (except for [`M_SCAN_EDGE_DETERMINATION_GRADE`](../../Reference/code/McodeGetResult.md)).  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md), the grade is retrieved as an interpolated value.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to a grading standard edition prior to ISO/IEC 15416:2016, the grade is retrieved as a letter grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the scan reflectance profile grade as an interpolated value rounded to the nearest 0.1 when [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

#### `M_SCAN_REFLECTANCE_PROFILE_LENGTH`

Retrieves the number of values in the scan reflectance profile.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value > 0` | Specifies the number of values in the scan reflectance profile. |

#### `M_SCAN_REFLECTANCE_PROFILE_VALUES`

Retrieves the reflectance values in the specified scan reflectance profile(s).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0 <= Value <= 255` | Specifies a reflectance value of the scan reflectance profile. |

#### `M_SCAN_SYMBOL_CONTRAST`

Retrieves the symbol contrast value (_SC_) of the scan reflectance profile. Note that this value is used in conjunction with the maximum reflectance ([`M_SCAN_REFLECTANCE_MAXIMUM`](../../Reference/code/McodeGetResult.md) to determine the print contrast signal ([`M_SCAN_PRINT_CONTRAST_SIGNAL`](../../Reference/code/McodeGetResult.md)). For more details, see [ISO/IEC 15416](ISO/IEC 15416).  When dealing with 2D matrix codes, to retrieve the symbol contrast value for the entire code occurrence, use [`M_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 1.0` | Specifies the symbol contrast value. |

#### `M_SCAN_SYMBOL_CONTRAST_GRADE`

Retrieves the symbol contrast value of the scan reflectance profile, as a grade.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md), the grade is retrieved as an interpolated value.  When [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to a grading standard edition prior to ISO/IEC 15416:2016, the grade is retrieved as a letter grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `0.0 <= Value <= 4.0` | Specifies the symbol contrast grade as an interpolated value rounded to the nearest 0.1 when [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md), [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md), or [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

#### `M_SCAN_WIDE_TO_NARROW_RATIO`

Retrieves the ratio between the average of the widest and the average of the narrowest bar/space, for the scan reflectance profile of the specified code occurrence(s). See [ISO 16388](ISO 16388) and [ISO 16390](ISO 16390) for more details.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |
| `Value >= 0.0` | Specifies the ratio of the average of the widest and the average of the narrowest bar/space in the scan reflectance profile. |

#### `M_SCAN_WIDE_TO_NARROW_RATIO_GRADE`

Retrieves the wide to narrow ratio, for the scan reflectance profile of the specified code occurrence(s), as a grade.

| Value | Description |
| --- | --- |
| `M_CODE_GRADE_A` | Specifies the best grade for the result. |
| `M_CODE_GRADE_B` | Specifies a good grade for the result. |
| `M_CODE_GRADE_C` | Specifies a fair grade for the result. |
| `M_CODE_GRADE_D` | Specifies a poor grade for the result. |
| `M_CODE_GRADE_F` | Specifies the worst grade for the result. |
| `M_CODE_GRADE_NOT_AVAILABLE` | Specifies that the result is not available for the specified code occurrence(s). |

---

### `Code read result buffer ID for scanline-specific results`

Specifies a code read result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_READ_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeRead`](../../Reference/code/McodeRead.md) results.

#### `M_DECODED_SCANS_END_X`

Retrieves the X-coordinate of the end point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_END_Y`

Retrieves the Y-coordinate of the end point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_SCORE`

Retrieves the score of the specified decoded scanline(s) of the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the score of the decoded scanline. |

#### `M_DECODED_SCANS_START_X`

Retrieves the X-coordinate of the start point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

#### `M_DECODED_SCANS_START_Y`

Retrieves the Y-coordinate of the start point of the specified decoded scanline(s), for the specified code occurrence(s). For composite code types, this is for the scanline(s) in the 1D portion of the code occurrence(s).

### For retrieving information about the code context or code model control types that were enabled for training using the

To retrieve information about the code context or code model control types that were enabled for training using the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, the [`ResultCodeId`](../../Reference/code/McodeGetResult.md) and [`ResultType`](../../Reference/code/McodeGetResult.md) parameters can be set to the following values, where [`ResultIndex`](../../Reference/code/McodeGetResult.md) is set to [`M_GENERAL`](../../Reference/code/McodeGetResult.md)in the case of a code context, and to a specific model index or [`M_ALL`](../../Reference/code/McodeGetResult.md)in the case of a code model. [`RowOrScanIndex`](../../Reference/code/McodeGetResult.md)must be set to [`M_GENERAL`](../../Reference/code/McodeGetResult.md).

---

### `Code train result buffer ID for trained control type results`

Specifies a code train result buffer, allocated using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md), and used to store [`McodeTrain`](../../Reference/code/McodeTrain.md) results.

#### `M_TRAIN_ENABLED_CONTROL_TYPES`

Retrieves the code context or code model control types that were enabled for training.

#### `M_TRAIN_ENABLED_CONTROL_TYPES_ORIGINAL_VALUE`

Retrieves the original values of the code context or code model control types that were enabled for training.

#### `M_TRAIN_ENABLED_CONTROL_TYPES_STATE`

Retrieves the states of the code context or code model control types that were enabled for training.

| Value | Description |
| --- | --- |
| `M_NOT_OPTIMIZABLE` | Specifies that the original value is the same as the trained value. |
| `M_OPTIMIZABLE` | Specifies that the original value is different from the trained value. |

#### `M_TRAIN_ENABLED_CONTROL_TYPES_TRAINED_VALUE`

Retrieves the trained values of the code context or code model control types that were enabled for training.

#### `M_TRAINED_CONTROL_TYPES`

Retrieves the code context or code model control types that can be modified by the training results; that is, the code model control types that will be changed after calling[`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md).

### Combination Constants — For use with string result types

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify whether unprintable characters should be returned.

| Value | Description |
| --- | --- |
| `M_ESCAPE_SEQUENCE` | Retrieves the string with its unprintable characters represented by their ASCII character codes. Any back slashes (\) in the string are doubled, while unprintable characters, such as carriage returns, are put in a `\xNN` format, where _NN_ is the hexadecimal value of the ASCII character code for the unprintable character. For example, the string "\Hello&lt;CR>" (where &lt;CR> is a carriage return) is retrieved as "\\Hello\x0D". |

### Combination Constants — For retrieving the specified result for the 2D component of a composite code occurrence, from an McodeRead() or McodeGrade() operation

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the specified result for the 2D component of a composite code occurrence, from an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

If this combination constant is not added to a result type, the results of the linear component (1D component) are returned followed by those of the 2D component. Note that this does not include general results (number of rows, number of scanlines) where results are compounded into a single value.  Note that this combination constant is only available for results of an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_2D_COMPONENT` | Retrieves the result of the 2D component of a composite code occurrence. |

### Combination Constants — For retrieving the specified result for the linear component (1D component) of a composite code occurrence, from an

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the specified result of the linear component (1D component) of a composite code occurrence, from an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

If this combination constant is not added to a result type, the results of the linear component (1D component) are returned followed by those of the 2D component. Note that this does not include general results (number of rows, number of scanlines) where results are compounded into a single value.  Note that this combination constant is only available for results of an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_LINEAR_COMPONENT` | Retrieves the result of the linear component (1D component) of a composite code occurrence. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For determining the string length

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the string length of a string.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0"), and unprintable characters, if the [`M_ESCAPE_SEQUENCE`](../../Reference/code/McodeGetResult.md) combination constant is specified.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

#### `M_TYPE_TEXT_CHAR`

Cast the requested results to an _AIL_TEXT_CHAR_.

For an occurrence of a 1D code type from an [`McodeRead`](../../Reference/code/McodeRead.md) operation, this value is dependent on [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md).

This result type is available for Code 39, Codabar, Industrial 2 of 5, and Interleaved 2 of 5.

This result type is available for Code 39 and Codabar code types.

This result type is available for all 1D and composite code types.

This result type is available for all 2D and composite code types.

This result type is available for 2D cross-row and composite code types.

This result type is available for 2D matrix code types.

This result type is available for 1D GS1 Databar code types and composite code types containing a GS1 Databar code.

This result type is available for 1D, 2D cross-row, and composite code types.

This result type is available for 1D code types (except GS1 Databar) and composite code types encoded with a format of EAN/UPC or UCC/EAN-128.

This result type is available for Aztec, Data Matrix, QR code, and Micro QR code types.

This result type is available for Aztec, Data Matrix, DotCode, QR code, and Micro QR code types.

This result type is available for Data Matrix code types.

When performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation from the result of an [`McodeRead`](../../Reference/code/McodeRead.md) operation, the required controls must be set before the [`McodeRead`](../../Reference/code/McodeRead.md) operation.

This result type is only available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md) (the default) before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called.

This result type is only available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_DPM_GRADING`](../../Reference/code/McodeControl.md) before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called.

This result type is only available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md) or[`M_ISO_DPM_GRADING`](../../Reference/code/McodeControl.md)before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called.

This result type is only available if [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_SEMI_T10_GRADING`](../../Reference/code/McodeControl.md) before [`McodeGrade`](../../Reference/code/McodeGrade.md) is called.

When dealing with results that are derived from other results, retrieve the results for the associated results first to narrow down where your code occurrence (or code context configuration) is lacking.

This result type relates to the position of the scan profiles, enabling you to visualize or draw the available analysis profiles using [`McodeDraw`](../../Reference/code/McodeDraw.md) with [`M_DRAW_SCAN_PROFILES`](../../Reference/code/McodeDraw.md). Note that the start and stop positions of any given scan are related to the steps of the analysis were performed.

This result type is available for 2D and composite code types, with the exception of Data Matrix code types that do not use [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) with [`M_ECC_200`](../../Reference/code/McodeControl.md).

This result type is used in the grade overlay procedure for measuring codeword print quality, as described in the [ISO/IEC 15415](ISO/IEC 15415) specification.

This result type requires that the specified code be graded using proper code verification techniques. For more information on 1D code types, refer to the ISO/IEC 15416 specification. For more information on 2D matrix code types, refer to the ISO/IEC 15415 specification.

This result type requires that the specified code be graded using proper code verification techniques. For more information, refer to the ISO/IEC 15415 specification.

When [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_DPM_GRADING`](../../Reference/code/McodeControl.md) and [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_29158_2020`](../../Reference/code/McodeControl.md) or [`M_DEFAULT`](../../Reference/code/McodeControl.md), this grade is retrieved as an interpolated value to the nearest 0.1 between grade levels. For [`M_ISO_29158_2011`](../../Reference/code/McodeControl.md), this result type is retrieved as an interpolated integer value between 0 and 4. For all other settings, this result type is retrieved as a letter grade ([`M_CODE_GRADE_...`](../../Reference/code/McodeGetResult.md)).

> **Note:** Note that these result types require that [`ResultIndex`](../../Reference/code/McodeGetResult.md) be set to [`M_GENERAL`](../../Reference/code/McodeGetResult.md).

Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER_OF_CODEWORDS`](../../Reference/code/McodeGetResult.md). Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with[`M_NUMBER_OF_FIXED_PATTERN_DAMAGE_B_SEGMENT`](../../Reference/code/McodeGetResult.md). Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER_OF_ROWS`](../../Reference/code/McodeGetResult.md). Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER_OF_SCANS`](../../Reference/code/McodeGetResult.md). Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER_OF_DECODED_SCANS`](../../Reference/code/McodeGetResult.md). Array size = sum of values returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER_OF_DECODED_SCANS`](../../Reference/code/McodeGetResult.md).
