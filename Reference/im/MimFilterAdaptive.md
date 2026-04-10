---
doctype: Reference
module: im
function: MimFilterAdaptive
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimFilterAdaptive"
---

# MimFilterAdaptive

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

> Perform adaptive filtering.

## Syntax

```c
void MimFilterAdaptive(
    AIL_ID     AdaptiveFilterContextImId,  //in
    AIL_ID     SrcImageBufId,              //in
    AIL_ID     DstImageBufId,              //out
    AIL_DOUBLE Param1,                     //in
    AIL_DOUBLE Param2,                     //in
    AIL_DOUBLE Param3,                     //in
    AIL_INT64  ControlFlag                 //in
)
```

## Description

This function performs adaptive filtering to remove noise in an image, while preserving edges as much as possible.

Depending on the application, [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md) will require different value settings for the [`Param1`](../../Reference/im/MimFilterAdaptive.md), [`Param2`](../../Reference/im/MimFilterAdaptive.md), [`Param3`](../../Reference/im/MimFilterAdaptive.md) and [`ControlFlag`](../../Reference/im/MimFilterAdaptive.md)parameters. It is recommended that you experiment with different settings to achieve the best level of noise reduction and edge preservation for your application.

Note that adaptive filters are slower than other similar operations, such as rank filters ([`MimRank`](../../Reference/im/MimRank.md)) and most of the filters available using [`MimConvolve`](../../Reference/im/MimConvolve.md). If edge preservation is less important than speed for your application, try the predefined or custom filters available in [`MimConvolve`](../../Reference/im/MimConvolve.md).

## Parameters

### `AdaptiveFilterContextImId` *(in, AIL_ID)*

Specifies the predefined adaptive filter context to use.

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer.

### `Param1` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform.

### `Param3` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to determine the new value for a pixel determined to be noise.

## Parameter Associations

### For specifying parameters of a bilateral, noise peak elimination, guided, or fast guided adaptive filter operation

The following [`AdaptiveFilterContextImId`](../../Reference/im/MimFilterAdaptive.md) and associated [`Param1`](../../Reference/im/MimFilterAdaptive.md), [`Param2`](../../Reference/im/MimFilterAdaptive.md), [`Param3`](../../Reference/im/MimFilterAdaptive.md), and [`ControlFlag`](../../Reference/im/MimFilterAdaptive.md) settings are used to determine the type of adaptive filtering to perform.

---

### `M_BILATERAL`

Specifies a bilateral adaptive filter operation. A bilateral filter smooths an image while preserving its features. It is effectively an edge-preserving filter.  Bilateral adaptive filtering is based on spatial and intensity weights given to each pixel in a neighborhood, and results in greater or lesser degrees of smoothing and edge preservation, depending on the values set with [`Param1`](../../Reference/im/MimFilterAdaptive.md), [`Param2`](../../Reference/im/MimFilterAdaptive.md), and [`Param3`](../../Reference/im/MimFilterAdaptive.md). This function performs an operation similar to [`MimConvolve`](../../Reference/im/MimConvolve.md), except the kernel used in calculations is variable and adapts to the content of the source image.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the amount of smoothing. A larger value applies more smoothing than a smaller value.  Note that, beyond a certain value, all the spatial weights in the neighborhood become equal; further increasing [`Param1`](../../Reference/im/MimFilterAdaptive.md) would be useless, but not detrimental. |
| `0.0 < Value <= 1.0` | Specifies the amount of acceptable edge loss. To preserve edges, set [`Param2`](../../Reference/im/MimFilterAdaptive.md) to a low value.  > **Note:** Typically, this parameter is set to a decimal value much less than one. |
| `Value > 0` | Specifies the spatial window (neighborhood size), in pixels. This parameter should typically be set to an odd value so the kernel is centered on a whole pixel. Even values are supported but not advised. |

---

### `M_FAST_GUIDED`

Specifies a fast guided adaptive filter operation. A fast guided filter smooths an image while preserving its edges.  Fast guided filtering separates image information into edges and homogenous areas. Two sub-images are created, one by blurring the homogenous areas of the image and the other containing the location of the edges. A new image is then constructed by combining these two sub-images with the original image. The fast guided filter improves the computational speed of the guided filter by sub-sampling the original image and then up-sampling before constructing the final image. This is at the cost of a slightly degraded result.  > **Note:** Note, only 8-bit and 16-bit unsigned image buffers are supported for guided filter operations.

| Value | Description |
| --- | --- |
| `Value >= 1.0` | Specifies the scale factor. |
| `0.0 < Value <= 1.0` | Specifies the amount of acceptable edge loss. To preserve edges, set [`Param2`](../../Reference/im/MimFilterAdaptive.md) to a low value.  > **Note:** Typically, this parameter is set to a decimal value much less than one. |
| `Value > 0` | Specifies the spatial window (neighborhood size), in pixels. This parameter should typically be set to an odd value so the kernel is centered on a whole pixel. Even values are supported but not advised. |

---

### `M_GUIDED`

Specifies a guided adaptive filter operation. A guided filter smooths an image while preserving its edges.  Guided filtering separates image information into edges and homogenous areas. Two sub-images are created, one by blurring the homogenous areas of the image and the other containing the location of the edges. A new image is then constructed by combining these two sub-images with the original image.  > **Note:** Note, only 8-bit and 16-bit unsigned image buffers are supported for guided filter operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `0.0 < Value <= 1.0` | Specifies the amount of acceptable edge loss. To preserve edges, set [`Param2`](../../Reference/im/MimFilterAdaptive.md) to a low value.  > **Note:** Typically, this parameter is set to a decimal value much less than one. |
| `Value > 0` | Specifies the spatial window (neighborhood size), in pixels. This parameter should typically be set to an odd value so the kernel is centered on a whole pixel. Even values are supported but not advised. |

---

### `M_NOISE_PEAK_REMOVAL`

Specifies a noise peak elimination adaptive filter operation. A noise peak elimination filter iteratively replaces the central pixel in a 3x3 neighborhood, if the pixel is a minimum or maximum within that neighborhood, and differs from the replacement value by more than a specified threshold. The replacement value is based on the neighboring pixels; how the replacement value is calculated can be set using the [`M_NOISE_PEAK_REMOVAL`](../../Reference/im/MimFilterAdaptive.md) parameter.  Optionally, you can specify a gap value. A pixel must be smaller/greater than the minimum/maximum of its neighbors by at least this amount to be considered the minimum/maximum of its neighborhood.  The operation performed is similar to using [`MimRank`](../../Reference/im/MimRank.md) with [`M_3X3_RECT`](../../Reference/im/MimRank.md), except that the rank value is variable and adapts to the content of the source image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` | Specifies the number of iterations. |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the gap value. |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the replacement threshold. |
| `M_DEFAULT` |  |
| `M_EXTREME` | Specifies that the replacement value for each pixel is the minimum or maximum value of the neighboring pixels. The extreme closest to the pixel's current value is used. |
| `M_MEAN` | Specifies that the replacement value for each pixel is the average value of the neighboring pixels. |
| `M_MEDIAN` *(default)* | Specifies that the replacement value for each pixel is the median value of the neighboring pixels. |
