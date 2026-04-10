---
doctype: Reference
module: im
function: MimConvolve
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimConvolve"
---

# MimConvolve

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

> Perform a general convolution operation.

## Syntax

```c
void MimConvolve(
    AIL_ID SrcImageBufId,                //in
    AIL_ID DstImageBufId,                //out
    AIL_ID FilterContextImOrKernelBufId  //in
)
```

## Description

This function performs a general convolution operation on the source buffer using the specified filter, storing results in the specified destination buffer. This function supports two types of filters: Finite Impulse Response (FIR) filters and Infinite Impulse Response (IIR) filters. FIR filters operate on a finite neighborhood, while IIR filters take into account all values in an image. You can specify either a predefined FIR filter or a custom FIR or IIR filter.

Note that predefined FIR filters are the fastest type, and are recommended as the first to try if you are not sure which filter will best suit your needs. When using custom filters, relatively small FIR filters are faster and allow control over the kernel contents. If the FIR kernel that you require becomes too large, try an IIR filter. If speed is not an issue, try the adaptive filters available using [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md), which remove noise in an image while preserving edges as much as possible.

This function provides a number of predefined FIR filters. For predefined FIR filters, the overscan is set to transparent overscan pixel values, saturation is enabled for results, and the kernel's default center pixel is set as the top-left pixel of the central elements in a neighborhood. When using predefined FIR filters, you cannot change operation control settings except for the overscan.

This function also accepts a custom FIR or IIR filter with which you can customize operation control settings. To define a custom FIR filter, you must allocate a kernel buffer using [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) or [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). Note that the kernel buffer size can be constrained by the available resources. To define a custom IIR filter, you must allocate a linear IIR filter image processing context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md).

To define a custom FIR filter, you must load the kernel buffer with values, using [`MbufPut`](../../Reference/buf/MbufPut.md). When using a custom FIR filter, you can change operation control settings using [`MbufControl`](../../Reference/buf/MbufControl.md). To define a custom IIR filter, use [`MimControl`](../../Reference/im/MimControl.md) with a linear IIR filter image processing context and specify appropriate [`M_FILTER_TYPE`](../../Reference/im/MimControl.md), [`M_FILTER_OPERATION`](../../Reference/im/MimControl.md), [`M_FILTER_RESPONSE_TYPE`](../../Reference/im/MimControl.md), [`M_FILTER_SMOOTHNESS_TYPE`](../../Reference/im/MimControl.md), and [`M_FILTER_SMOOTHNESS`](../../Reference/im/MimControl.md) operation control type settings.

For predefined and custom FIR filters, when using 32-bit integer buffers and saturation is disabled, the accumulator buffer is a 32-bit internal buffer (AIL_INT); whereas, when saturation is enabled, the accumulator buffer is a 64-bit floating-point internal buffer (double). For custom IIR filters, operations are always performed using floating-point precision.

For custom FIR filters, this function will internally separate a large kernel into two 1-dimensional kernels, if possible, to increase the speed of the convolution operation. For more information on separating kernels, see [Finite Impulse Response (FIR) filters](../../UserGuide/C04_Advanced_image_processing/S02_Custom_spatial_filters.md).

Custom FIR filters containing a single kernel value (such as uniform kernels or average kernels) are dealt with by a specifically targeted algorithmic optimization.

If the source and destination are multi-band buffers, the same filter is applied to every band of the source buffer.

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the data source of the operation. This parameter must be given an image buffer identifier.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results. This parameter must be given an image buffer identifier.

### `FilterContextImOrKernelBufId` *(in, AIL_ID)*

Specifies the filter to use. Set this parameter to a predefined filter, the identifier of the kernel buffer which defines a custom FIR filter, or a linear IIR filter image processing context, which defines a custom IIR filter.

*For specifying a custom filter*

| Value | Description |
| --- | --- |
| `Kernel Buffer ID` | Specifies the identifier of the kernel buffer for a custom FIR filter, allocated using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_KERNEL`](../../Reference/buf/MbufAlloc1d.md). For more details, refer to [Custom spatial filters](../../UserGuide/C04_Advanced_image_processing/S02_Custom_spatial_filters.md). |
| `Linear IIR filter image processing context ID` | Specifies a linear IIR filter image processing context for a custom IIR filter, allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md). Set up the context using [`MimControl`](../../Reference/im/MimControl.md). |

*For specifying a predefined FIR filter*

| Value | Description |
| --- | --- |
| `M_EDGE_DETECT_PREWITT_FAST` | Computes the gradient of the image using the following operation, which is based on a Prewitt filter.

*[Image: edgedetect2.png]* |
| `M_EDGE_DETECT_SOBEL_FAST` | Computes the gradient of the image using the following operation, which is based on a Sobel filter.

*[Image: edgedetect1.png]* |
| `M_HORIZONTAL_EDGE_PREWITT` | Computes the absolute values of the vertical derivative of the image using a Prewitt filter, to detect the horizontal edges.

*[Image: Prewitthorizontaledge.png]* |
| `M_HORIZONTAL_EDGE_SOBEL` | Computes the absolute values of the vertical derivative of the image using a Sobel filter, to detect the horizontal edges.

*[Image: SobelHorizontalFilter.png]* |
| `M_LAPLACIAN_4` | Computes the Laplacian values of the image using the following 4-connected Laplacian edge detection filter.

*[Image: laplacianedge1.png]* |
| `M_LAPLACIAN_8` | Computes the Laplacian values of the image using the following 8-connected Laplacian edge detection filter.

*[Image: laplacianedge2.png]* |
| `M_LAPLACIAN_ISO_8` | Computes the Laplacian values of the image using the following 8-connected isotropic Laplacian edge detection filter.*[Image: LaplacianIso8.png]* |
| `M_PREWITT_X` | Computes the values of the horizontal derivative of the image using a Prewitt filter.

*[Image: PrewittX.png]* |
| `M_PREWITT_Y` | Computes the values of the vertical derivative of the image using a Prewitt filter.

*[Image: PrewittY.png]* |
| `M_SHARPEN_4` | Performs a sharpening operation on the image using the following 4-connected Laplacian filter combined with the original image.

Note that this operation results in a sharpening effect that is not as strong as the one achieved with [`M_SHARPEN_8`](../../Reference/im/MimConvolve.md).

*[Image: sharpen2.png]* |
| `M_SHARPEN_8` | Performs a sharpening operation on the image using the following 8-connected Laplacian filter combined with the original image.

Note that this operation results in a stronger sharpening effect than [`M_SHARPEN_4`](../../Reference/im/MimConvolve.md).

*[Image: sharpen1.png]* |
| `M_SMOOTH` | Performs a smoothing operation on the image using the following smoothing filter.

*[Image: smooth.png]* |
| `M_SOBEL_X` | Computes the values of the horizontal derivative of the image using a Sobel filter.

*[Image: SobelX.png]* |
| `M_SOBEL_Y` | Computes the values of the vertical derivative of the image using a Sobel filter.

*[Image: SobelY.png]* |
| `M_VERTICAL_EDGE_PREWITT` | Computes the absolute values of the horizontal derivative of the image using a Prewitt filter, to detect the vertical edges.

*[Image: Prewittverticaledge.png]* |
| `M_VERTICAL_EDGE_SOBEL` | Computes the absolute values of the horizontal derivative of the image using a Sobel filter, to detect the vertical edges.

*[Image: SobelVerticalFilter.png]* |

*For specifying how to determine the value of a destination pixel when its associated point falls outside the source buffer*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_DISABLE` | Specifies to disable overscanning for this operation. You should disable overscanning to accelerate the operation for applications in which the overscan data is not important in the resulting buffer. |
| `M_OVERSCAN_ENABLE` *(default)* | Specifies to enable overscanning for this operation. The overscan uses pixels from the ancestor of the source buffer. If the source buffer is not a child buffer, or if the point falls outside the ancestor buffer, a mirror type overscan is used. A mirror overscan specifies that the overscan pixels will be a mirror copy of the source buffer's border pixels. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
