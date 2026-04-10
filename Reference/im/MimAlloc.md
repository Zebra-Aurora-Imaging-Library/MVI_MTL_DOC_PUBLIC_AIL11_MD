---
doctype: Reference
module: im
function: MimAlloc
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimAlloc"
---

# MimAlloc

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

> Allocate an image processing context.

## Syntax

```c
AIL_ID MimAlloc(
    AIL_ID    SysId,          //in
    AIL_INT64 ContextType,    //in
    AIL_INT64 ControlFlag,    //in
    AIL_ID *  ContextImIdPtr  //out
)
```

## Description

This function allocates an image processing context on the specified system.

Note that image processing contexts allocated with [`MimAlloc`](../../Reference/im/MimAlloc.md) can be saved using either [`MimStream`](../../Reference/im/MimStream.md) or [`MimSave`](../../Reference/im/MimSave.md) and restored using [`MimRestore`](../../Reference/im/MimRestore.md).

When the image processing context is no longer required, release it using[`MimFree`](../../Reference/im/MimFree.md)unless [`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified, the smart identifier manages the image processing context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the context. This parameter should be set to one of the following values:

*For specifying the system identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of image processing context. This parameter should be set to one of the following values:

*For specifying the type of context*

| Value | Description |
| --- | --- |
| `M_AUGMENTATION_CONTEXT` | Specifies an image processing context that can be used with [`MimAugment`](../../Reference/im/MimAugment.md). |
| `M_BINARIZE_ADAPTIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) to perform an adaptive binarization. |
| `M_BINARIZE_ADAPTIVE_FROM_SEED_CONTEXT` | Specifies an image processing context that can be used with [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) to perform an adaptive binarization using seeds. |
| `M_DEAD_PIXEL_CONTEXT` | Specifies an image processing context that can be used with [`MimDeadPixelCorrection`](../../Reference/im/MimDeadPixelCorrection.md). |
| `M_DEINTERLACE_CONTEXT` | Specifies an image processing context that can be used with [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md). |
| `M_FILTER_MAJORITY_CONTEXT` | Specifies an image processing context that can be used with [`MimFilterMajority`](../../Reference/im/MimFilterMajority.md). |
| `M_FIND_ORIENTATION_CONTEXT` | Specifies an image processing context that can be used with [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md). |
| `M_FLAT_FIELD_CONTEXT` | Specifies an image processing context that can be used with [`MimFlatField`](../../Reference/im/MimFlatField.md). |
| `M_HISTOGRAM_EQUALIZE_ADAPTIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md). |
| `M_IM_REMAP_CONTEXT` | Specifies an image processing context that can be used with [`MimRemap`](../../Reference/im/MimRemap.md). |
| `M_LINEAR_FILTER_IIR_CONTEXT` | Specifies an image processing context that can be used with [`MimConvolve`](../../Reference/im/MimConvolve.md) or [`MimDifferential`](../../Reference/im/MimDifferential.md). |
| `M_LOCATE_PEAK_1D_CONTEXT` | Specifies an image processing context that can be used with [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md). |
| `M_MATCH_CONTEXT` | Specifies an image processing context that can be used with [`MimMatch`](../../Reference/im/MimMatch.md). |
| `M_REARRANGE_CONTEXT` | Specifies an image processing context that can be used with [`MimRearrange`](../../Reference/im/MimRearrange.md). |
| `M_STATISTICS_CONTEXT` | Specifies an image processing context that can be used with [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md). |
| `M_STATISTICS_CUMULATIVE_CONTEXT` | Specifies an image processing context that can be used with [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md) when calculating statistics from multiple images. |
| `M_UNWARP_ALONG_PATH_CONTEXT` | Specifies an image processing context that can be used with [`MimUnwarpAlongPath`](../../Reference/im/MimUnwarpAlongPath.md). |
| `M_WAVELET_TRANSFORM_CONTEXT` | Specifies an image processing context that can be used with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |
| `M_WAVELET_TRANSFORM_CUSTOM_CONTEXT` | Specifies a custom image processing context that can be used with [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) and [`MimWaveletDenoise`](../../Reference/im/MimWaveletDenoise.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ContextImIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the image processing context identifier or specifies the data type that the function should use to return the image processing context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated image processing context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated image processing context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_IM_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the image processing context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated image processing context.

If allocation fails, [`M_NULL`](../../Reference/im/MimAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the image processing context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_IM_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified).

## Remarks

> Note that some of the context types listed above are not available in Aurora Imaging Library Lite. See the context's corresponding operation function for Aurora Imaging Library Lite availability.
