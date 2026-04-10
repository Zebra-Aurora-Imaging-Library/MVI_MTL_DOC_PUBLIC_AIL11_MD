---
doctype: Reference
module: im
function: MimAllocResult
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimAllocResult"
---

# MimAllocResult

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

> Allocate an image processing result buffer.

## Syntax

```c
AIL_ID MimAllocResult(
    AIL_ID    SysId,         //in
    AIL_INT   NbEntries,     //in
    AIL_INT64 ResultType,    //in
    AIL_ID *  ResultImIdPtr  //out
)
```

## Description

This function allocates a result buffer with the specified number of entries, for use with some of the Image Processing module's functions (for example, the statistical functions).

When the image processing result buffer is no longer required, release it using[`MimFree`](../../Reference/im/MimFree.md)unless [`M_UNIQUE_ID`](../../Reference/im/MimAllocResult.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/im/MimAllocResult.md) was specified, the smart identifier manages the image processing result buffer's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the result buffer. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `NbEntries` *(in, AIL_INT)*

Specifies the number of buffer entries to allocate for the specified result buffer. This parameter should be set to one of the following values:

*For specifying the number of buffer entries*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically establishes the size of the result buffer. You can only use this value when the [`ResultType`](../../Reference/im/MimAllocResult.md) parameter is set to [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md), [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md), [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md), [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md), [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md), or [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md). |
| `Value > 0` | Specifies the number of buffer entries. This value should take into account the type of result buffer being allocated. A specific number of entries must be specified when the [`ResultType`](../../Reference/im/MimAllocResult.md)parameter is set to [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md),[`M_EXTREME_LIST`](../../Reference/im/MimAllocResult.md),[`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md),[`M_HIST_LIST`](../../Reference/im/MimAllocResult.md), or [`M_PROJ_LIST`](../../Reference/im/MimAllocResult.md).

A specific number of entries can be specified when the [`ResultType`](../../Reference/im/MimAllocResult.md) parameter is set to [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) to limit the number of results.

A specific number of entries cannot be specified when the [`ResultType`](../../Reference/im/MimAllocResult.md) parameter is set to [`M_COUNT_LIST`](../../Reference/im/MimAllocResult.md), [`M_LOCATE_PEAK_1D_RESULT`](../../Reference/im/MimAllocResult.md), [`M_STATISTICS_RESULT`](../../Reference/im/MimAllocResult.md), or [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md).

Note that for [`NbEntries`](../../Reference/im/MimAllocResult.md) other than [`M_DEFAULT`](../../Reference/im/MimAllocResult.md), [`M_EVENT_LIST`](../../Reference/im/MimAllocResult.md) and [`M_LOCATE_DIFFERENCE_RESULT`](../../Reference/im/MimAllocResult.md) results from a multi-core processing application might differ from run to run. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of data that will be stored in this result buffer. This parameter must be set to one of the following values:

*For specifying the type of data*

| Value | Description |
| --- | --- |
| `M_AUGMENTATION_RESULT` | Specifies that the buffer can hold [`MimAugment`](../../Reference/im/MimAugment.md) results. |
| `M_COUNT_LIST` | Specifies that the buffer can hold [`MimCountDifference`](../../Reference/im/MimCountDifference.md) results. |
| `M_EVENT_LIST` | Specifies that the buffer can hold [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) results. |
| `M_EXTREME_LIST` | Specifies that the buffer can hold [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md) results. |
| `M_FIND_ORIENTATION_LIST` | Specifies that the buffer can hold [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) results. |
| `M_HIST_LIST` | Specifies that the buffer can hold [`MimHistogram`](../../Reference/im/MimHistogram.md) results. |
| `M_LOCATE_DIFFERENCE_RESULT` | Specifies that the buffer can hold [`MimLocateDifference`](../../Reference/im/MimLocateDifference.md) results. |
| `M_LOCATE_PEAK_1D_RESULT` | Specifies that the buffer can hold [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) results. In this case, you must set the [`NbEntries`](../../Reference/im/MimAllocResult.md) parameter to [`M_DEFAULT`](../../Reference/im/MimAllocResult.md). |
| `M_PROJ_LIST` | Specifies that the buffer can hold [`MimProjection`](../../Reference/im/MimProjection.md) results. |
| `M_STATISTICS_RESULT` | Specifies that the buffer can hold [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) results. |
| `M_WAVELET_TRANSFORM_RESULT` | Specifies that the buffer can hold results from [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). Such results can themselves be processed. That is, you can use the wavelet result as the source with which to perform [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |

*For*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Allocates a result buffer of type _float_. You should allocate a _float_ result type buffer when the source buffer specified by [`MimProjection`](../../Reference/im/MimProjection.md), [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md), or [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) is of type _float_. If the specified source buffer is of type _float_ and the result buffer is not, the functions can produce inaccurate results.

Note that allocating a result buffer of type _float_ when the source buffer is not, will slow down the operations performed by the [`MimProjection`](../../Reference/im/MimProjection.md), [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md), or [`MimLocateEvent`](../../Reference/im/MimLocateEvent.md) function. |

### `ResultImIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the image processing result buffer identifier or specifies the data type that the function should use to return the image processing result buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated image processing result
                              buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated image processing result
                              buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_IM_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the image processing result
                              buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated image processing result buffer.

If allocation fails, [`M_NULL`](../../Reference/im/MimAllocResult.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the image processing result buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_IM_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/im/MimAllocResult.md) was specified).

## Remarks

> Note that some of the result buffer types listed above are not available in Aurora Imaging Library Lite. See the result buffer's corresponding operation function for Aurora Imaging Library Lite availability.
