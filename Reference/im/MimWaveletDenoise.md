---
doctype: Reference
module: im
function: MimWaveletDenoise
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimWaveletDenoise"
---

# MimWaveletDenoise

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

> Perform a wavelet denoising.

## Syntax

```c
void MimWaveletDenoise(
    AIL_ID    WaveletContextImId,              //in
    AIL_ID    SrcImageBufOrWaveletResultImId,  //in
    AIL_ID    DstImageBufOrWaveletResultImId,  //out
    AIL_INT   Level,                           //in
    AIL_ID    DenoisingType,                   //in
    AIL_INT64 ControlFlag                      //in
)
```

## Description

This function performs a denoising on the specified source buffer using a wavelet shrinkage process and places the results in the specified destination buffer. The source and destination can either both be an image or a wavelet result.

This function supports in-place processing; you can specify the identifier of the same buffer for the source and the destination. To specify wavelet controls that can affect this function's results, use [`MimControl`](../../Reference/im/MimControl.md).

To specify wavelet controls that can affect this function's results, use [`MimControl`](../../Reference/im/MimControl.md). If the destination of this function is a result, you can perform drawing operations with them, using [`MimDraw`](../../Reference/im/MimDraw.md).

## Parameters

### `WaveletContextImId` *(in, AIL_ID)*

Specifies the identifier of a wavelet image processing context, when performing a wavelet denoising of an image. The context must have been allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with either [`M_WAVELET_TRANSFORM_CONTEXT`](../../Reference/im/MimAlloc.md) or [`M_WAVELET_TRANSFORM_CUSTOM_CONTEXT`](../../Reference/im/MimAlloc.md). When performing a wavelet denoising of a result, this information is not required; set this parameter to `M_NULL`.

### `SrcImageBufOrWaveletResultImId` *(in, AIL_ID)*

Specifies the identifier of the source with which to perform the wavelet denoising. The identifier must be for an image buffer or a wavelet image processing result buffer. The image buffer can be of any type. The wavelet result buffer must have been allocated on the required system using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md). It must hold [`MimWaveletTransform`](../../Reference/im/MimWaveletTransform.md) results. To retrieve information about wavelet transformation results, use [`MimGetResult`](../../Reference/im/MimGetResult.md) and [`MimGetResultSingle`](../../Reference/im/MimGetResultSingle.md).

### `DstImageBufOrWaveletResultImId` *(out, AIL_ID)*

Specifies the identifier of the destination with which to perform the wavelet denoising. The identifier must be for an image buffer or a wavelet image processing result buffer. The image buffer can be of any type. The wavelet result buffer must have been allocated on the required system using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_WAVELET_TRANSFORM_RESULT`](../../Reference/im/MimAllocResult.md).

### `Level` *(in, AIL_INT)*

Specifies the transformation level at which to perform the denoising.

*For specifying the transformation level*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the level information is not required. Only valid when the source is a result. Denoising is performed at the transformation level indicated in the result. |
| `Value > 0` | Specifies the level. Only valid when the source is an image. This function decomposes the image according to the specified number of levels, performs the denoising, and recomposes the image using the same number of levels.

Aurora Imaging Library establishes the maximum level to be the point at which denoising at subsequent levels does not provide relevant data. The maximum level depends on the source image and the wavelet image processing context. If the specified level exceeds the maximum level, the maximum level is used. |

### `DenoisingType` *(in, AIL_ID)*

Specifies the type of wavelet process with which to perform the denoising.

*For specifying the wavelet shrinkage process*

| Value | Description |
| --- | --- |
| `M_BAYES_SHRINK` | Specifies predefined BayesShrink denoising techniques. Typically effective for minimizing Gaussian noise. |
| `M_NEIGH_SHRINK` | Specifies predefined NeighShrink denoising techniques. Typically effective when noise is difficult to characterize. Denoising mimics the general behavior of wavelet coefficients by taking neighborhood statistics and dismissing outliers. [`M_NEIGH_SHRINK`](../../Reference/im/MimWaveletDenoise.md) is often faster than the other predefined denoising techniques, though results can be less precise. |
| `M_SURE_SHRINK` | Specifies predefined SureShrink denoising techniques. Typically effective for minimizing Mean Square Errors (MSE). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
