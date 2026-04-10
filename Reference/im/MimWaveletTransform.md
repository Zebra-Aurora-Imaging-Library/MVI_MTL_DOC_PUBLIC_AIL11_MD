---
doctype: Reference
module: im
function: MimWaveletTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimWaveletTransform"
---

# MimWaveletTransform

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

> Perform a wavelet transformation.

## Syntax

```c
void MimWaveletTransform(
    AIL_ID    WaveletContextImId,              //in
    AIL_ID    SrcImageBufOrWaveletResultImId,  //in
    AIL_ID    DstImageBufOrWaveletResultImId,  //out
    AIL_INT64 TransformType,                   //in
    AIL_INT   Level,                           //in
    AIL_INT64 ControlFlag                      //in
)
```

## Description

This function performs a forward or reverse wavelet transformation using the specified source buffer and places the results in the specified destination buffer. The source and destination can be an image or wavelet result.

This function supports in-place processing; you can specify the identifier of the same buffer for the source and the destination.

Wavelets refer to mathematical algorithms that process data in both the space and frequency domain, offering a compromise between each. By using wavelets, [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results can therefore represent space and frequency. This makes [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) especially effective at filtering images. It is typically useful for signal coding, signal denoising, data compression, and pattern recognition.

To specify wavelet controls that can affect [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results, use [`MimControl`](../../Reference/im/MimControl.md). To perform drawing operations with this function's results, use [`MimDraw`](../../Reference/im/MimDraw.md).

To retrieve information about a specific wavelet result, use [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md). A specific wavelet result refers to a transformed rendition of the source, based on either a wavelet coefficient (diagonal, horizontal, or vertical) at a specific level, or on an approximation of the wavelet transformation. For general types of wavelet results, such as the actual number of transformation levels used, call [`MimGetResult`](../../Reference/im/MimGetResult.md).

## Parameters

### `WaveletContextImId` *(in, AIL_ID)*

Specifies the identifier of a wavelet image processing context. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md).

### `SrcImageBufOrWaveletResultImId` *(in, AIL_ID)*

Specifies the identifier of the source with which to perform the wavelet transformation. The identifier must be for an image buffer or a wavelet image processing result buffer. The image buffer can be of any type. The wavelet result buffer must have been allocated on the required system using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md). It must hold [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results.

### `DstImageBufOrWaveletResultImId` *(out, AIL_ID)*

Specifies the identifier of the destination in which to write the result of the wavelet transformation. The identifier must be for an image buffer or a wavelet image processing result buffer. The image buffer can be of any type. The wavelet result buffer must have been allocated on the required system using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md).

### `TransformType` *(in, AIL_INT64)*

Specifies the type of wavelet transformation to perform.

*For specifying the wavelet transformation type*

| Value | Description |
| --- | --- |
| `M_FORWARD` | Specifies a forward wavelet transformation. This decomposes the source according to the number of levels established. The source can be an image or result. The destination must be a result. |
| `M_REVERSE` | Specifies a reverse wavelet transformation. This recomposes the source according to the number of levels established. The source must be a result. The destination can be an image or a result. |

### `Level` *(in, AIL_INT)*

Specifies the number of transformation levels with which to calculate results. Results are written in the specified destination buffer. The level applies to either an [`M_FORWARD`](../../Reference/im/MimWaveletTransform.md) or [`M_REVERSE`](../../Reference/im/MimWaveletTransform.md) transformation.

*For specifying the transformation level*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies a level of 0. Only supported when using [`M_REVERSE`](../../Reference/im/MimWaveletTransform.md) and the destination is an image. In any other case, [`M_DEFAULT`](../../Reference/im/MimWaveletTransform.md)(or 0) causes an error. |
| `Value >= 0` | Specifies the transformation level. Values depend on the transformation, the source, and the destination.

With [`M_FORWARD`](../../Reference/im/MimWaveletTransform.md), the source can be an image or result; the destination must be a result. In all cases, the level must be greater than 0 and less than or equal to the maximum level. If the source is a result, the level must also be greater than the level in the source. If the specified level exceeds the maximum level, Aurora Imaging Library uses the maximum level instead. Aurora Imaging Library establishes the maximum level to be the point at which transformations at subsequent levels do not provide relevant data. This depends on the source and the wavelet image processing context.

With [`M_REVERSE`](../../Reference/im/MimWaveletTransform.md), the source must be a result; the destination can be a result or an image. If the destination is a result, the level must be greater than 0 and less than the level in the source. If the specified level exceeds the level in the source (maximum level), Aurora Imaging Library uses the level in the source instead. If the destination is an image, the level must be 0 (or [`M_DEFAULT`](../../Reference/im/MimWaveletTransform.md)). Setting the level to 0 (or [`M_DEFAULT`](../../Reference/im/MimWaveletTransform.md)) when the destination is a result causes an error.

To determine the level in the source result, use it with [`MimGetResult`](../../Reference/im/MimGetResult.md) and [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md). To determine the level in the destination result, use it with [`M_NUMBER_OF_LEVELS`](../../Reference/im/MimGetResult.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
