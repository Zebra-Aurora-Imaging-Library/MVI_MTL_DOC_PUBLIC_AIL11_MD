---
doctype: Reference
module: gen
function: MgenGabor
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / gen / MgenGabor"
---

# MgenGabor

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

> Generate a Log-Gabor filter and its Hilbert transform.

## Syntax

```c
void MgenGabor(
    AIL_ID     ContextId,            //in
    AIL_ID     GaborDestId,          //out
    AIL_ID     HilbertDestId,        //out
    AIL_DOUBLE Param1,               //in
    AIL_DOUBLE Param2,               //in
    AIL_DOUBLE Param3,               //in
    AIL_DOUBLE Param4,               //in
    AIL_DOUBLE Param5,               //in
    AIL_INT    Normalization,        //in
    AIL_INT    DCComponentPosition,  //in
    AIL_INT    ControlFlag           //in
)
```

## Description

This function generates a Log-Gabor filter and its Hilbert transformation using the following equation:

*[Image: MgenGaborEquation.png]*

This function writes the Log-Gabor filter and its Hilbert transformation to the specified image buffers in the frequency domain.

The Log-Gabor filter is useful for filtering out specific frequencies at a specified angle in images.

To apply the Log-Gabor filter to an image, the image must be in the frequency domain. You can perform a forward Fourier transform on an image, using [`MimTransform`](../../Reference/im/MimTransform.md) with [`M_FFT`](../../Reference/im/MimTransform.md) and [`M_FORWARD`](../../Reference/im/MimTransform.md). You can apply the Log-Gabor filter to the transformed source image, using [`MimArith`](../../Reference/im/MimArith.md). Then, you can reverse the Fourier transform, using [`MimTransform`](../../Reference/im/MimTransform.md) with [`M_FFT`](../../Reference/im/MimTransform.md) and [`M_REVERSE`](../../Reference/im/MimTransform.md), and display the filtered image.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the type of context.

### `GaborDestId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to write the Log-Gabor filter.

*For specifying the image buffer in which to write the Log-Gabor filter*

| Value | Description |
| --- | --- |
| `Image buffer identifier` | Specifies the image buffer in which to write the Log-Gabor filter. The image buffer must be a 1-band, 32-bit processable float buffer. |

### `HilbertDestId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer in which to write the Hilbert transform of the Log-Gabor filter.

*For specifying the image buffer in which to write the Hilbert transform*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to not store the Hilbert transform in an image buffer. |
| `Image buffer identifier` | Specifies the image buffer in which to write the Hilbert transform. The image buffer must be a 1-band, 32-bit processable float buffer. |

### `Param1` *(in, AIL_DOUBLE)*

Specifies the center orientation (_θ<sub>0</sub> _) of the Gabor filter.

### `Param2` *(in, AIL_DOUBLE)*

Specifies the center frequency (_f<sub>0</sub> _) of the Log-Gabor filter.

### `Param3` *(in, AIL_DOUBLE)*

Specifies the width parameter for the frequency (_σ<sub>r</sub> _) of the Log-Gabor filter.

### `Param4` *(in, AIL_DOUBLE)*

Specifies the width parameter for the orientation (_σ<sub>a</sub> _) of the Log-Gabor filter.

### `Param5` *(in, AIL_DOUBLE)*

Reserved for future use and must be set to `M_NULL`.

### `Normalization` *(in, AIL_INT)*

Specifies the normalization factor applied to all values of the filters.

*For specifying the Normalization factor*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not use a normalization factor. |
| `M_NORMALIZE` | Specifies to use a normalization factor. The normalization factor is defined as the [`SizeX`](../../Reference/buf/MbufAlloc2d.md) multiplied by the [`SizeY`](../../Reference/buf/MbufAlloc2d.md) of the image buffer specified by the [`GaborDestId`](../../Reference/gen/MgenGabor.md) parameter. |

### `DCComponentPosition` *(in, AIL_INT)*

Specifies the position of the DC Component.

*For specifying the position of the DC component*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CENTER` | Specifies that the DC component is located at the center. |
| `M_TOP_LEFT` *(default)* | Specifies that the DC component is located at the top left corner. |

### `ControlFlag` *(in, AIL_INT)*

Reserved for future use and must be set to `M_DEFAULT`.

## Parameter Associations

### For selecting the Gabor filter

---

### `M_LOG_GABOR_FREQUENTIAL_CONTEXT_DEFAULT`

Specifies a predefined Log-Gabor frequential filter context.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 180.0` | Specifies the center angle. |
| `0.0 < Value <= 0.75` | Specifies the center frequency. |
| `Value > 0.0` | Specifies the width parameter for the frequency. |
| `Value > 0.0` | Specifies the width parameter for the orientation. |
