---
doctype: Reference
module: seq
function: MseqAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqAlloc"
---

# MseqAlloc

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

> Allocate a sequence context.

## Syntax

```c
AIL_ID MseqAlloc(
    AIL_ID     SystemId,        //in
    AIL_INT64  SequenceType,    //in
    AIL_INT64  Operation,       //in
    AIL_UINT32 OutputFormat,    //in
    AIL_INT64  InitFlag,        //in
    AIL_ID *   ContextSeqIdPtr  //out
)
```

## Description

This function allocates a sequence context on the specified system. A sequence context establishes the sequence operation that should be performed when a sequence processing operation is started using [`MseqProcess`](../../Reference/seq/MseqProcess.md) and contains all the information necessary to perform it.

Define the source of an input or the destination(s) of an output for a sequence operation using [`MseqDefine`](../../Reference/seq/MseqDefine.md). Specify the operation settings of the sequence operation and adjust the settings of the source(s) and destination(s) using [`MseqControl`](../../Reference/seq/MseqControl.md).

> **Note:** H.264 video encoding is optimized for Intel CPUs and can be subject to performance and stability issues when used with other CPUs.

When the sequence context is no longer required, release it using[`MseqFree`](../../Reference/seq/MseqFree.md)unless [`M_UNIQUE_ID`](../../Reference/seq/MseqAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/seq/MseqAlloc.md) was specified, the smart identifier manages the sequence context's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the sequence context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SequenceType` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.

### `Operation` *(in, AIL_INT64)*

Specifies the operation that the sequence processing session should perform.

*For specifying the operation performed by the sequence processing session*

| Value | Description |
| --- | --- |
| `M_SEQ_COMPRESS` | Specifies that the sequence processing operation will compress a sequence of images into an H.264 elementary video stream. |
| `M_SEQ_DECOMPRESS` | Specifies that the sequence processing operation will decompress an H.264 elementary video stream. |

### `OutputFormat` *(in, AIL_UINT32)*

Specifies the format to use to create the internal output buffer.

*For specifying the output format to use*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default output format. For example, for a compression operation, the format is H.264. |

### `InitFlag` *(in, AIL_INT64)*

Specifies the underlying hardware or software used for the operation.

*For specifying the underlying hardware or software used for the operation.*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_QSV` *(default)* | Specifies to perform the operation using the Intel QSV engine. If your processor does not have a QSV engine, Aurora Imaging Library will perform the operation in software. You can force the operation to be performed in hardware or software by combining [`M_QSV`](../../Reference/seq/MseqAlloc.md) with [`M_HARDWARE`](../../Reference/seq/MseqAlloc.md)or [`M_SOFTWARE`](../../Reference/seq/MseqAlloc.md), respectively. |

*For specifying whether the operation is performed in hardware or software*

| Value | Description |
| --- | --- |
| `M_HARDWARE` | Specifies to perform the operation in hardware; Aurora Imaging Library will select the first hardware accelerated device it detects. If no hardware acceleration is present on your computer, an error is generated.

> **Note:** To use the Intel QSV engine with Aurora Imaging Library under Linux, you must install the appropriate Intel GPU driver and libva package for your Linux distribution. |
| `M_SOFTWARE` | Specifies to perform the operation in software. |

### `ContextSeqIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the sequence context identifier or specifies the data type that the function should use to return the sequence context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated sequence context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated sequence context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_SEQ_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the sequence context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated sequence context.

If allocation fails, [`M_NULL`](../../Reference/seq/MseqAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the sequence context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_SEQ_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/seq/MseqAlloc.md) was specified).

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_QSV`](../../Reference/seq/MseqAlloc.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
