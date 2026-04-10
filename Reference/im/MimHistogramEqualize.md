---
doctype: Reference
module: im
function: MimHistogramEqualize
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimHistogramEqualize"
---

# MimHistogramEqualize

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

> Perform a histogram equalization of an image.

## Syntax

```c
void MimHistogramEqualize(
    AIL_ID     SrcImageBufId,  //in
    AIL_ID     DstImageBufId,  //out
    AIL_INT64  Operation,      //in
    AIL_DOUBLE Alpha,          //in
    AIL_DOUBLE Min,            //in
    AIL_DOUBLE Max             //in
)
```

## Description

This function performs a histogram equalization of the specified source image. Results are written to a destination buffer, which can be either an image buffer or a LUT buffer.

This function first performs a histogram of the source image buffer. The histogram is then used to calculate a transformation LUT that can be used to enhance the source image (with [`MimLutMap`](../../Reference/im/MimLutMap.md)). If the destination buffer is a LUT, the transformation LUT is copied into the destination LUT. If the destination buffer is an image, the source buffer is remapped through the transformation LUT to produce the destination image.

Note that floating-point values will be cast to _AIL_UINT32_ values before performing the histogram. Therefore, unexpected results can occur if a floating-point value is larger than the _AIL_UINT32_ range.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

If you specify multiple image buffers with an ROI, this function performs a histogram using the source ROI, and writes to the destination only those pixels that intersect between the source ROI and the destination ROI. This is consistent with the behavior of this function when ROIs are not specified.

If [`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md) is not producing the results you expect, try performing an adaptive histogram equalization, using [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md)). Though an adaptive histogram equalization can take longer to execute (more calculations to process), it can help minimize the over-amplification of noise and the potential for saturation. Unlike [`MimHistogramEqualize`](../../Reference/im/MimHistogramEqualize.md), [`MimHistogramEqualizeAdaptive`](../../Reference/im/MimHistogramEqualizeAdaptive.md) is not available with Aurora Imaging Library Lite.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter will be treated as an unsigned image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer or LUT buffer identifier.

### `Operation` *(in, AIL_INT64)*

Specifies the equalization operation to perform. This parameter can be set to one of the following values. Note that the cumulative probability distribution, _P<sub>f</sub>(f)_, of the input image is approximated by its cumulative histogram: *[Image: mimhistogramequalize_prdis.png]* Refer to *Digital Image Processing* (Pratt, William K., 1978, John Wiley & Sons).

*For specifying the type of equalization to perform*

| Value | Description |
| --- | --- |
| `M_EXPONENTIAL` | Specifies an equalization density function which generates an Exponential distribution.

Output probability density model: *[Image: mimhistogramequalize_exou.png]*

Transfer function: *[Image: mimhistogramequalize_extr.png]* |
| `M_HYPER_CUBE_ROOT` | Specifies an equalization density function which generates a Hyperbolic Cube Root distribution.

Output probability density model: *[Image: mimhistogramequalize_hcrou.png]*

Transfer function: *[Image: mimhistogramequalize_hcrtr.png]* |
| `M_HYPER_LOG` | Specifies an equalization density function which generates a Hyperbolic Logarithmic distribution.

Output probability density model: *[Image: mimhistogramequalize_hlou.png]*

Transfer function: *[Image: mimhistogramequalize_hltr.png]* |
| `M_RAYLEIGH` | Specifies an equalization density function which generates a Rayleigh distribution.

Output probability density model: *[Image: mimhistogramequalize_raou.png]*

Transfer function: *[Image: mimhistogramequalize_ratr.png]* |
| `M_UNIFORM` | Specifies an equalization density function which generates a Uniform distribution.

Output probability density model: *[Image: mimhistogramequalize_unou.png]*

Transfer function: *[Image: mimhistogramequalize_untr.png]* |

### `Alpha` *(in, AIL_DOUBLE)*

Specifies the value to use for alpha (_α_) in the equations for [`M_EXPONENTIAL`](../../Reference/im/MimHistogramEqualize.md) and [`M_RAYLEIGH`](../../Reference/im/MimHistogramEqualize.md). For other [`Operation`](../../Reference/im/MimHistogramEqualize.md) parameter values, set [`Alpha`](../../Reference/im/MimHistogramEqualize.md) to `M_NULL`.

*For specifying the alpha value*

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the value to use for alpha (_α_).

For an [`M_EXPONENTIAL`](../../Reference/im/MimHistogramEqualize.md) operation, a greater alpha value yields a lower occurrence of the most frequent pixels of the histogram in the resulting image buffer. When operating on an 8-bit unsigned image, a typical value for alpha is between 0.001 and 0.1.

For an [`M_RAYLEIGH`](../../Reference/im/MimHistogramEqualize.md) operation, the greater the alpha is, the greater the occurrence of the most frequent pixels of the histogram in the resulting image buffer. When operating on an 8-bit unsigned image, a typical value for alpha is between 10 and 1000. |

### `Min` *(in, AIL_DOUBLE)*

Specifies the lowest pixel value to equalize.

*For specifying the lowest pixel value to equalize*

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the lowest pixel value. |

### `Max` *(in, AIL_DOUBLE)*

Specifies the highest pixel value to equalize.

*For specifying the highest pixel value to equalize*

| Value | Description |
| --- | --- |
| `Min <= Value <= max buffer value` | Specifies the highest pixel value. |
