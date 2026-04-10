---
doctype: Reference
module: im
function: MimTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimTransform"
---

# MimTransform

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

> Perform a Fast Fourier transform (FFT), a Discrete Cosine transform (DCT), or a polar-cartesian coordinates transformation.

## Syntax

```c
void MimTransform(
    AIL_ID    SrcImageRBufId,  //in
    AIL_ID    SrcImageIBufId,  //in
    AIL_ID    DstImageRBufId,  //out
    AIL_ID    DstImageIBufId,  //out
    AIL_INT64 TransformType,   //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function performs a forward or reverse FFT or DCT transformation on an image. It also perfoms a cartesian-to-polar or polar-to-cartesian point-to-point coordinates transformation.

## Parameters

### `SrcImageRBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer for the real component of the image.

### `SrcImageIBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer for the imaginary component of the image.

### `DstImageRBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer for the real component of the image.

### `DstImageIBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer for the imaginary component of the image.

### `TransformType` *(in, AIL_INT64)*

Specifies the type of transform to perform on the image.

### `ControlFlag` *(in, AIL_INT64)*

Specifies if the transform is a forward transform or a reverse transform.

## Parameter Associations

### For selecting the transformation type.

---

### `M_DCT8X8`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Performs a discrete cosine transform on each 8x8 pixel block in the image.

#### `M_DEFAULT`

Same as [`M_FORWARD`](../../Reference/im/MimTransform.md).

#### `M_FORWARD`

Performs a forward transform on the image.

| Value | Description |
| --- | --- |
| `8-bit signed/unsigned image buffer ID` | Performs calculations in fixed-point format and returns to the destinations in 23.9 fixed point format. |
| `16-bit signed/unsigned image buffer ID` | Performs calculations in fixed-point format and returns to the destinations in 25.7 fixed point format. |
| `32-bit signed image buffer ID` | Conserves the fixed point format between the destination buffer and the source buffer. |
| `Floating-point image buffer ID` | Specifies that processing will be performed in floating-point arithmetic. The destination buffer should also be a floating-point buffer. |

#### `M_REVERSE`

Performs a reverse transform on the image.

| Value | Description |
| --- | --- |
| `8-bit signed/unsigned image buffer ID` | The format of the source buffer is assumed to be in 23.9 fixed point format. |
| `16-bit signed/unsigned image buffer ID` | The format of the source buffer is assumed to be in 25.7 fixed point format. |
| `32-bit signed image buffer ID` | The fixed point format of the destination buffer is the same as that of the source. |
| `Floating-point image buffer ID` | If [`SrcImageRBufId`](../../Reference/im/MimTransform.md) is a floating-point buffer, this must also be a floating-point buffer and processing will be performed in floating-point arithmetic. |

---

### `M_FFT`

Performs a Fast Fourier transform.

#### `M_DEFAULT`

Same as [`M_FORWARD`](../../Reference/im/MimTransform.md).

#### `M_FORWARD`

Performs a forward transform on the image. For floating-point destination buffers, the processing will be performed in floating-point arithmetic.

| Value | Description |
| --- | --- |
| `8-bit signed/unsigned image buffer ID` | Calculations are performed in fixed-point format and returned to the destinations in 23.9 fixed point format. |
| `16-bit signed/unsigned image buffer ID` | Calculations are performed in fixed-point format and returned to the destinations in 25.7 fixed point format. |
| `32-bit signed image buffer ID` | The fixed point format of the destination buffer will be the same as that of the source buffer. |
| `Floating-point image buffer ID` | The destination buffer should also be a floating-point buffer and processing will be performed in floating-point arithmetic. |
| `M_NULL` | Specifies that a faster version of the forward transform will be performed. |
| `Image buffer ID` | Specifies the identifier of the source buffer. |

#### `M_REVERSE`

Performs a reverse transform on the image.

| Value | Description |
| --- | --- |
| `8-bit signed/unsigned image buffer ID` | The format of the source buffer is assumed to be in 23.9 fixed point format. |
| `16-bit signed/unsigned image buffer ID` | The format of the source buffer is assumed to be in 25.7 fixed point format. |
| `32-bit signed image buffer ID` | The fixed point format of the destination buffer is the same as that of the source. |
| `Floating-point image buffer ID` | If [`SrcImageRBufId`](../../Reference/im/MimTransform.md) is a floating-point buffer, this must also be a floating-point buffer and processing will be performed in floating-point arithmetic. |
| `M_NULL` | Specifies that a faster version of the reverse transform will be performed. |
| `Image buffer ID` | Specifies the identifier of the destination buffer. |

---

### `M_POLAR`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Performs a cartesian-to-polar or a polar-to-cartesian transform on the coordinates of the source buffer.

#### `M_DEFAULT`

Same as [`M_FORWARD`](../../Reference/im/MimTransform.md).

#### `M_FORWARD`

Converts the real and imaginary components of cartesian coordinates to the magnitude and phase components of polar coordinates.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that a faster version of the forward transform will be performed.  > **Note:** Only one of the destination buffers can be set to [`M_NULL`](../../Reference/im/MimTransform.md). |
| `Image buffer ID` | Specifies the identifier of the destination buffer. It must be a 32-bit floating-point buffer. |
| `M_NULL` | Specifies that a faster version of the forward transform will be performed.  > **Note:** Only one of the destination buffers can be set to [`M_NULL`](../../Reference/im/MimTransform.md). |
| `Image buffer ID` | Specifies the identifier of the destination buffer. The phase results are comprised in the range of 0-360. |

#### `M_REVERSE`

Converts the magnitude and phase components of polar coordinates to the real and imaginary components of cartesian coordinates.

### Combination Constants — For the ControlFlag parameter

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to set the transform characteristics.

Note that only [`M_NORMALIZE`](../../Reference/im/MimTransform.md) is supported with both [`M_FFT`](../../Reference/im/MimTransform.md) and [`M_DCT8X8`](../../Reference/im/MimTransform.md). All other values are supported only with [`M_FFT`](../../Reference/im/MimTransform.md).

| Value | Description |
| --- | --- |
| `M_1D_COLUMNS` | Performs a 1D transform on all columns of the image.  If [`M_1D_COLUMNS`](../../Reference/im/MimTransform.md) has been specified with the forward transform, then it must be specified for the reverse transform as well. |
| `M_1D_ROWS` | Performs a 1D transform on all rows of the image.  If [`M_1D_ROWS`](../../Reference/im/MimTransform.md) has been specified with the forward transform, then it must be specified for the reverse transform as well. |
| `M_CENTER` | Centers the real and imaginary components of the image in the frequency domain by repositioning their respective buffers' quadrants. The DC component is positioned at (SizeX/2-1, SizeY/2-1), where SizeX and SizeY are the width and height of the source buffer, respectively.  *[Image: FFT_and_FFT_with_M_CENTER.png]*  If the size of the destination buffers is different from that of the source buffer, data that falls outside of the destination buffers is clipped; exceeding areas of the destination buffers are unaffected. See [Managing data buffers](../../UserGuide/C23_Data_buffers/S07_Managing_data_buffers.md)for more information.  If [`M_CENTER`](../../Reference/im/MimTransform.md) has been specified for the forward transform, then it must be specified for the reverse transform as well. |
| `M_MAGNITUDE` | Specifies whether to compute or use the magnitude.  When used with [`M_FORWARD`](../../Reference/im/MimTransform.md), computes the magnitude of the forward FFT (the square root of `_R_ <sup>2</sup>+_I_ <sup>2</sup>`). The magnitude is returned in the [`DstImageRBufId`](../../Reference/im/MimTransform.md) destination buffer, thereby overwriting the real component of the destination image.  When used with [`M_REVERSE`](../../Reference/im/MimTransform.md), computes the real component of the transform from the magnitude and phase (`_R_ = _M_ cos(_P_)`). The result will be used to perform the reverse FFT.  For a reverse FFT, [`SrcImageRBufId`](../../Reference/im/MimTransform.md) and [`SrcImageIBufId`](../../Reference/im/MimTransform.md) must contain the magnitude and phase of a forward transform, respectively.  [`M_MAGNITUDE`](../../Reference/im/MimTransform.md) must be used with [`M_PHASE`](../../Reference/im/MimTransform.md) for a reverse FFT.  For a forward FFT, [`DstImageIBufId`](../../Reference/im/MimTransform.md) can be set to [`M_NULL`](../../Reference/im/MimTransform.md) if [`M_MAGNITUDE`](../../Reference/im/MimTransform.md) is used without [`M_PHASE`](../../Reference/im/MimTransform.md).  [`M_MAGNITUDE`](../../Reference/im/MimTransform.md) can only be used in transforms processed with floating-point buffers. |
| `M_NORMALIZE` | Normalizes results (divide the final result by 8 for DCT and by (_m_ x _n_) for FFT where _m_ x _n _is the size of the image). [`M_NORMALIZE`](../../Reference/im/MimTransform.md) must be used with fixed-point arithmetic (all integer buffers) to avoid overflows.  If [`M_NORMALIZE`](../../Reference/im/MimTransform.md) has been specified with the forward transform, then it must be specified for the reverse transform as well. |
| `M_PHASE` | Specifies whether to compute or use the phase.  When used with [`M_FORWARD`](../../Reference/im/MimTransform.md), computes the phase of the forward transform (`_P_ = atan(_I_/_R_)`). The phase is returned in the [`DstImageIBufId`](../../Reference/im/MimTransform.md) buffer, thereby overwriting the imaginary component of the destination image. The phase is returned in the range of 0° to 360°.  When used with [`M_REVERSE`](../../Reference/im/MimTransform.md), computes the imaginary component of the transform from the magnitude and phase (`_I_ = _M_ sin(_P_)`). The result will be used to perform the reverse FFT.  For a reverse FFT, [`SrcImageRBufId`](../../Reference/im/MimTransform.md) and [`SrcImageIBufId`](../../Reference/im/MimTransform.md) must contain the magnitude and phase of a forward transform, respectively.  [`M_PHASE`](../../Reference/im/MimTransform.md) must be used with [`M_MAGNITUDE`](../../Reference/im/MimTransform.md) for a reverse FFT.  For a forward FFT, [`DstImageRBufId`](../../Reference/im/MimTransform.md) can be set to [`M_NULL`](../../Reference/im/MimTransform.md) if [`M_PHASE`](../../Reference/im/MimTransform.md) is used without [`M_MAGNITUDE`](../../Reference/im/MimTransform.md).  [`M_PHASE`](../../Reference/im/MimTransform.md) can only be used in transforms processed with floating-point buffers. |
| `M_SATURATION` | Clips (saturates) the results of a reverse FFT according to the destination buffer's data type.  [`M_SATURATION`](../../Reference/im/MimTransform.md) can only be used with a reverse FFT ([`M_FFT`](../../Reference/im/MimTransform.md)+[`M_REVERSE`](../../Reference/im/MimTransform.md)) operation which uses floating-point source buffers and integer destination buffers. |

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify that the magnitude of the forward FFT is scaled logarithmically.

| Value | Description |
| --- | --- |
| `M_LOG_SCALE` | Scales logarithmically the magnitude of the forward FFT to be in the range of 0-255. This value is used to scale the spectrum into a displayable range.  [`M_LOG_SCALE`](../../Reference/im/MimTransform.md) can only be used in forward transforms processed with floating-point buffers. **[Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2]** |

### Combination Constants — For specifying the transform characteristics

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to specify the transform characteristics.

> **Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

| Value | Description |
| --- | --- |
| `M_FAST_PHASE` | Performs faster phase computation.  For a forward polar transformation, it uses a faster computation of the phase results but slightly less accurate. For a reverse polar transformation, it has no effect. |
| `M_NORMALIZE_PHASE` | Rescales the phase results.  For a forward polar transformation, it rescales the phase results to be in the range of 0.0-1.0. For a reverse polar transformation, [`SrcImageRBufId`](../../Reference/im/MimTransform.md) must contain the phase values in the range of 0.0-1.0.  This value cannot be used in combination with [`M_NORMALIZE_PHASE_255`](../../Reference/im/MimTransform.md). |
| `M_NORMALIZE_PHASE_255` | Rescales the phase results.  For a forward polar conversion, it rescales the phase results to be in the range of 0-255. For a reverse polar transformation, [`SrcImageIBufId`](../../Reference/im/MimTransform.md) must contain the phase values in the range of 0-255.  This value cannot be used in combination with [`M_NORMALIZE_PHASE`](../../Reference/im/MimTransform.md). |
| `M_SQUARE_MAGNITUDE` | Specifies that the magnitude values are returned or used squared.  For a forward polar transformation, it gives the square of the magnitude results (that is, `_R_ <sup>2</sup>+_I_ <sup>2</sup>`). For a reverse transformation, [`SrcImageRBufId`](../../Reference/im/MimTransform.md) must contain the magnitude values squared. |
