---
doctype: Reference
module: im
function: MimArithMultiple
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimArithMultiple"
---

# MimArithMultiple

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

> Perform a point-to-point arithmetic operation using multiple source images.

## Syntax

```c
void MimArithMultiple(
    AIL_DOUBLE Src1ImageBufId,         //in
    AIL_DOUBLE Src2ImageBufIdOrConst,  //in
    AIL_DOUBLE Src3ImageBufIdOrConst,  //in
    AIL_DOUBLE Src4Const,              //in
    AIL_DOUBLE Src5Const,              //in
    AIL_ID     DstImageBufId,          //out
    AIL_INT64  Operation,              //in
    AIL_INT64  OperationFlag           //in
)
```

## Description

This function performs the specified point-to-point operation using multiple images, images and constants, or constants, storing results in the specified destination image buffer. Note that this function does not take 1-bit buffers in any of its parameters.

You can limit this function's results to a region of an image buffer using a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). If you specify multiple image buffers with an ROI, results are limited to the portion of the ROIs that intersect.

## Parameters

### `Src1ImageBufId` *(in, AIL_DOUBLE)*

Specifies the data source of the first operand; this parameter must be given an image buffer identifier. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Src2ImageBufIdOrConst` *(in, AIL_DOUBLE)*

Specifies the data source of the second operand; this parameter can be given an image buffer identifier or a constant. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Src3ImageBufIdOrConst` *(in, AIL_DOUBLE)*

Specifies the data source of the third operand; this parameter can be given an image buffer identifier or a constant. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Src4Const` *(in, AIL_DOUBLE)*

Specifies the data source of the fourth operand; this parameter must be given a constant.

### `Src5Const` *(in, AIL_DOUBLE)*

Specifies the data source of the fifth operand; this parameter must be given a constant.

### `DstImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination of the results; this parameter must be given an image buffer identifier. If you specify an image buffer that has an ROI associated with it, the ROI must be in raster format; otherwise, you will get an error.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform.

### `OperationFlag` *(in, AIL_INT64)*

Reserved for future expansion. Must be set to `M_DEFAULT`.

## Parameter Associations

### For the Operation, Src1ImageBufId, Src2ImageBufId, Src3ImageBufId, Src4ImageBufId, and Src5ImageBufId parameters

---

### `M_MULTIPLY_ACCUMULATE_1`

Performs a point-to-point multiply and accumulate 1 operation using the equation that follows.  *[Image: zcolor_DestImage.PNG]*

---

### `M_MULTIPLY_ACCUMULATE_2`

Performs a point-to-point multiply and accumulate 2 operation using the equation that follows.  *[Image: zcolor_DestImage1.PNG]*

---

### `M_OFFSET_GAIN`

Performs a per-pixel gain and offset correction operation using the equation that follows.  *[Image: zcolor_DestImage2.PNG]*

---

### `M_WEIGHTED_AVERAGE`

Performs a weighted average operation using the equation that follows.  *[Image: zcolor_DestImage3.PNG]*

### Combination Constants — For the Operation parameter

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the operation result should be saturated if necessary.

| Value | Description |
| --- | --- |
| `M_SATURATION` | Forces the operation to saturate any resulting pixel values that overflow or underflow the possible range of the destination buffer. The pixel values are clipped to fit within the buffer's range, rather than wrapped around the range.  If you do not specify [`M_SATURATION`](../../Reference/im/MimArithMultiple.md) and there is an overflow or underflow, the resulting pixel values are either clipped (saturated) or wrapped around the buffer's range (not saturated), depending on which is faster for your hardware. |

## Remarks

> Optimized in-place processing is supported (source equals destination), but the source and destination image buffers cannot partially overlap (a situation that can only occur when using child buffers).

Note that the image buffer's data type must either match that of the first operand, or it must be of type [`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md).

Note that the constant is cast to the same type as the buffer specified for the first operand.
