---
doctype: Reference
module: im
function: MimDifferential
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimDifferential"
---

# MimDifferential

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

> Perform a differential operation.

## Syntax

```c
void MimDifferential(
    AIL_ID     Drv1ImageBufIdOrFilterContextImId,  //in
    AIL_ID     SrcOrDrv2ImageBufId,                //in
    AIL_ID     SrcOrDrv3ImageBufId,                //in
    AIL_ID     Drv4ImageBufId,                     //in
    AIL_ID     Drv5ImageBufId,                     //in
    AIL_ID     Dst1ImageBufId,                     //out
    AIL_ID     Dst2ImageBufId,                     //out
    AIL_DOUBLE Param,                              //in
    AIL_INT64  Operation,                          //in
    AIL_INT64  ControlFlag                         //in
)
```

## Description

This function performs a differential operation on an image, based on either automatically calculated or specified derivative images. This function can be used, for example, to calculate the gradient magnitude and orientation (angle) of edges in an image, or to sharpen an image.

You can have [`MimDifferential`](../../Reference/im/MimDifferential.md) automatically calculate the required derivatives (in X and Y) of the source image by passing a linear IIR filter image processing context (previously allocated using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md)). [`MimDifferential`](../../Reference/im/MimDifferential.md)calculates the derivatives using IIR filters. You can partially define how the derivatives are calculated by configuring the IIR filter image processing context using [`MimControl`](../../Reference/im/MimControl.md). [`MimDifferential`](../../Reference/im/MimDifferential.md) ignores the [`M_FILTER_OPERATION`](../../Reference/im/MimControl.md) setting.

You can have [`MimDifferential`](../../Reference/im/MimDifferential.md) use precalculated derivatives by passing derivative image buffers: most operations require one image buffer with the derivative in X, and another with the derivative in Y (for the first derivatives, this is synonymous with passing the vertical and horizontal edge detection images, respectively). Passing precalculated derivatives is useful if you need to calculate the derivatives using a specific filter operation (for example, using [`MimConvolve`](../../Reference/im/MimConvolve.md)with a predefined FIR filter or a custom kernel), or use the derivatives with multiple functions (in which case, it is more efficient to calculate the derivatives only once).

> **Note:** Note that some operations require the first derivative images, while others require the second derivative images.

## Parameters

### `Drv1ImageBufIdOrFilterContextImId` *(in, AIL_ID)*

Specifies the identifier of an image buffer that stores a derivative, or a linear IIR filter context. The image buffer must not have a region of interest (ROI) associated with it. Specifying an image buffer with an ROI will cause an error.

### `SrcOrDrv2ImageBufId` *(in, AIL_ID)*

Specifies the identifier of an image buffer that stores the source image, or a derivative. The image buffer must not have a region of interest (ROI) associated with it. Specifying an image buffer with an ROI will cause an error.

### `SrcOrDrv3ImageBufId` *(in, AIL_ID)*

Specifies the identifier of an image buffer that stores the source image (for a sharpening operation), or a derivative. The image buffer must not have a region of interest (ROI) associated with it. Specifying an image buffer with an ROI will cause an error.

### `Drv4ImageBufId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_NULL`.

### `Drv5ImageBufId` *(in, AIL_ID)*

Reserved for future expansion and must be set to `M_NULL`.

### `Dst1ImageBufId` *(out, AIL_ID)*

Specifies the identifier of the first destination image buffer. The image buffer must not have a region of interest (ROI) associated with it. Specifying an image buffer with an ROI will cause an error.

### `Dst2ImageBufId` *(out, AIL_ID)*

Specifies the identifier of the second destination image buffer. The image buffer must not have a region of interest (ROI) associated with it. Specifying an image buffer with an ROI will cause an error.

### `Param` *(in, AIL_DOUBLE)*

Specifies an attribute of the operation to perform. Set this parameter to `M_DEFAULT` if not used.

### `Operation` *(in, AIL_INT64)*

Specifies the differential operation to perform.

### `ControlFlag` *(in, AIL_INT64)*

Specifies a control setting for the operation to perform. Set this parameter to `M_DEFAULT` if not used.

## Parameter Associations

### For specifying the differential operation

Set unused source and destination parameters to [`M_NULL`](../../Reference/im/MimDifferential.md). Set other unused parameters to [`M_DEFAULT`](../../Reference/im/MimDifferential.md).

---

### `M_GRADIENT`

Performs a gradient operation.

#### `Drv1ImageBufIdOrFilterContextImId - Image buffer ID`

Specifies the identifier of a float or signed image buffer, indicating the first derivative in X.

#### `Drv1ImageBufIdOrFilterContextImId - Linear IIR filter context ID`

Specifies the identifier of a linear IIR filter context ([`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md)).

---

### `M_GRADIENT_SQR`

Performs a square gradient operation.

#### `Drv1ImageBufIdOrFilterContextImId - Image buffer ID`

Specifies the identifier of a float or signed image buffer, indicating the first derivative in X.

#### `Drv1ImageBufIdOrFilterContextImId - Linear IIR filter context ID`

Specifies the identifier of a linear IIR filter context ([`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md)).

---

### `M_LAPLACIAN`

Performs a Laplacian operation.

#### `Drv1ImageBufIdOrFilterContextImId - Image buffer ID`

Specifies the identifier of a float or signed image buffer, indicating the second derivative in X.

#### `Drv1ImageBufIdOrFilterContextImId - Linear IIR filter context ID`

Specifies the identifier of a linear IIR filter context ([`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md)).

---

### `M_SHARPEN`

Performs a sharpening operation.

#### `Drv1ImageBufIdOrFilterContextImId - Image buffer ID`

Specifies the identifier of a float or signed image buffer, indicating the second derivative in X.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the sharpening value. |

#### `Drv1ImageBufIdOrFilterContextImId - Linear IIR filter context ID`

Specifies the identifier of a linear IIR filter context ([`M_LINEAR_FILTER_IIR_CONTEXT`](../../Reference/im/MimAlloc.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library will establish an appropriate sharpening value. |
| `Value > 0.0` | Specifies the sharpening value. |
| `M_DEFAULT` | Specifies to perform the sharpening operation. |
| `M_PREPROCESS` | Specifies to preprocesses the linear IIR filter context explicitly. Note, if not called explicitly, Aurora Imaging Library performs this operation automatically upon the first call to [`MimDifferential`](../../Reference/im/MimDifferential.md). |

## Remarks

> In-place processing is supported, but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).
