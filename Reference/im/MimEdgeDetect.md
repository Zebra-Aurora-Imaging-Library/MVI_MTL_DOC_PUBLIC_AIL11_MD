---
doctype: Reference
module: im
function: MimEdgeDetect
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimEdgeDetect"
---

# MimEdgeDetect

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

> Perform a specific edge detection operation and produce a gradient intensity and/or gradient angle image.

## Syntax

```c
void MimEdgeDetect(
    AIL_ID    SrcImageBufId,           //in
    AIL_ID    DstIntensityImageBufId,  //out
    AIL_ID    DstAngleImageBufId,      //out
    AIL_ID    KernelId,                //in
    AIL_INT64 ControlFlag,             //in
    AIL_INT   Threshold                //in
)
```

## Description

This function performs an edge detection operation on the specified source image, using the specified filter. It produces a gradient intensity image and/or a gradient angle image in the specified image buffer(s). If one of the destination images is not required, specify [`M_NULL`](../../Reference/im/MimEdgeDetect.md) as its image buffer identifier.

For an unsigned destination buffer, the angle is returned from 0° to 360° (counter-clockwise) and mapped to the entire range of the destination buffer. For example, for an 8-bit unsigned buffer, the angles of 0°, 90°, 180°, and 270° map to 0, 64, 128, and 192, respectively. For a signed destination buffer, mapping is done in both the positive and negative range of the buffer and represents angle values from -180° to 180°. For example, for an 8-bit signed buffer, the angles of -180°, -90°, 0°, and 90° map to -128, -64, 0 and 64, respectively.

This function can also be performed on a floating-point buffer. However, the results are not mapped and are returned in the range of 0° to 360°. The degree of precision is equal to the precision of the floating-point buffer for regular computation or to +/- 0.4° for fast computation. The value for undefined results is the maximum positive value of the floating-point buffer.

You can perform the operation using a combination of fast and full gradient and angle computations.

| Type of computation | Description of computation |
| --- | --- |
| Full gradient computation | Gradient = sqrt(GradientX*GradientX + GradientY*GradientY) |
| Fast gradient computation | Gradient = (abs(GradientX) + abs(GradientY))/2 |
| Full angle computation | Angle = arctan(-GradientY/GradientX) |
| Fast angle computation | The fast angle approximation is based on the same equation as the full angle computation; however, the value is determined from a LUT mapping. |

## Parameters

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source of the operation. This parameter must be given an image buffer identifier.

### `DstIntensityImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer for the resulting gradient intensity image. This parameter must be given an image buffer identifier or `M_NULL`. Saturation is performed on this buffer.

### `DstAngleImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer for the resulting gradient angle image. This parameter must be given an image buffer identifier or `M_NULL`.

### `KernelId` *(in, AIL_ID)*

Specifies the filter to use. This parameter must be set to the value below.

*For specifying the filter to use*

| Value | Description |
| --- | --- |
| `M_SOBEL` | Specifies to use the predefined 3x3 edge detection filters.

The following kernel is convolved with the image to produce GradientX, which is an intermediate image used in the gradient or angle computation. For each pixel, GradientX contains directional derivative values calculated from horizontal (X-direction) intensity changes.

*[Image: mimedgedetect_x.png]*

The following kernel is convolved with the image to produce GradientY, which, like GradientX, is an intermediate image used in the gradient or angle computation. For each pixel, GradientY contains directional derivative values calculated from vertical (Y-direction) intensity changes.

*[Image: mimedgedetect_y.png]* |

*For specifying the overscan setting*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_DISABLE` | Specifies to disable overscanning for this operation. You should disable overscanning to accelerate the operation for applications in which the overscan data is not important in the resulting buffer. |
| `M_OVERSCAN_ENABLE` *(default)* | Specifies to enable overscanning for this operation. The overscan uses pixels from the ancestor of the source buffer. If the source buffer is not a child buffer, or if the point falls outside the ancestor buffer, a mirror type overscan is used. A mirror overscan specifies that the overscan pixels will be a mirror copy of the source buffer's border pixels. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies how to perform the operation.

*For specifying how to perform the operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FAST_ANGLE + M_REGULAR_GRADIENT` | Specifies a full gradient computation and a fast angle approximation. |
| `M_FAST_EDGE_DETECT` *(default)* | Specifies a fast gradient computation and a fast angle approximation. The gradient is computed as an approximation, using the average of the absolute value of the two directional components (X and Y). |
| `M_REGULAR_ANGLE + M_FAST_GRADIENT` | Specifies a fast gradient computation and a full angle computation. |
| `M_REGULAR_EDGE_DETECT` | Specifies a full gradient computation and a full angle computation. The gradient is computed as the square root of the sum of the square of each directional component (X and Y). This calculation is slower but more precise. |

### `Threshold` *(in, AIL_INT)*

Specifies the threshold value for calculating the gradient angle. For a gradient intensity value lower than the threshold value, the angle is not computed (not considered a significant edge) and the resulting angle for that pixel is undetermined. To perform the full operation, set this parameter to zero or `M_NULL`.
