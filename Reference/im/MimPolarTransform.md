---
doctype: Reference
module: im
function: MimPolarTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimPolarTransform"
---

# MimPolarTransform

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
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Yes |

> Perform a polar-to-rectangular or rectangular-to-polar transformation on an image or generate LUTs used to perform the transformation.

## Syntax

```c
void MimPolarTransform(
    AIL_ID       SrcImageOrDstXLutBufId,  //in
    AIL_ID       DstImageOrYLutBufId,     //out
    AIL_DOUBLE   CenterPosX,              //in
    AIL_DOUBLE   CenterPosY,              //in
    AIL_DOUBLE   StartRadius,             //in
    AIL_DOUBLE   EndRadius,               //in
    AIL_DOUBLE   StartAngle,              //in
    AIL_DOUBLE   EndAngle,                //in
    AIL_INT64    OperationMode,           //in
    AIL_INT64    InterpolationMode,       //in
    AIL_DOUBLE * DstSizeXPtr,             //out
    AIL_DOUBLE * DstSizeYPtr              //out
)
```

## Description

This function performs a rectangular-to-polar or polar-to-rectangular transformation. Alternatively, this function can generate the X- and Y-coordinate LUTs required to perform the transformation using[`MimWarp`](../../Reference/im/MimWarp.md).

For a rectangular-to-polar transformation, the zone of interest to convert is based on a circle centered at [`CenterPosX`](../../Reference/im/MimPolarTransform.md)and [`CenterPosY`](../../Reference/im/MimPolarTransform.md), with the part to convert having the specified start and end radius ([`StartRadius`](../../Reference/im/MimPolarTransform.md)and[`EndRadius`](../../Reference/im/MimPolarTransform.md)) and specified start and end angle ([`StartAngle`](../../Reference/im/MimPolarTransform.md)and[`EndAngle`](../../Reference/im/MimPolarTransform.md)), measured with respect to the X-axis of the image.

*[Image: mimpolartransform_dia1.png]*

The result will be mapped to the destination buffer as follows:

*[Image: mimpolartransform_dia2.png]*

The increment in angle is determined by the length, in pixels, of the outside arc, calculated as follows:

(endangle - startangle) / arclength.

A polar-to-rectangular transform performs the reverse of the transform described above. It takes a polar buffer and maps it to a rectangular buffer. Use the[`CenterPosX`](../../Reference/im/MimPolarTransform.md), [`CenterPosY`](../../Reference/im/MimPolarTransform.md), [`StartAngle`](../../Reference/im/MimPolarTransform.md), [`EndAngle`](../../Reference/im/MimPolarTransform.md), [`StartRadius`](../../Reference/im/MimPolarTransform.md), [`EndRadius`](../../Reference/im/MimPolarTransform.md) parameters to specify where in the destination buffer the contents of the polar buffer will be mapped.

To determine the required size of the destination image or LUT buffer, call the function with [`DstImageOrYLutBufId`](../../Reference/im/MimPolarTransform.md) set to [`M_NULL`](../../Reference/im/MimPolarTransform.md); in this case, [`DstSizeXPtr`](../../Reference/im/MimPolarTransform.md) and [`DstSizeYPtr`](../../Reference/im/MimPolarTransform.md) will return the required X- and Y- size of the buffer, respectively. Note that [`DstSizeXPtr`](../../Reference/im/MimPolarTransform.md) and [`DstSizeYPtr`](../../Reference/im/MimPolarTransform.md) will return _AIL_DOUBLE_ values; to allocate an appropriate buffer using these values, you should use a ceiling function, such as `ceil()`, to obtain the X- and Y- sizes as _integer (AIL_INT)_ values.

## Parameters

### `SrcImageOrDstXLutBufId` *(in, AIL_ID)*

Specifies the identifier of the source image buffer or the destination X-coordinate LUT buffer.

### `DstImageOrYLutBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or the Y-coordinate LUT buffer.

### `CenterPosX` *(in, AIL_DOUBLE)*

Specifies the X-coordinate of the circle's center in the rectangular image.

### `CenterPosY` *(in, AIL_DOUBLE)*

Specifies the Y-coordinate of the circle's center in the rectangular image.

### `StartRadius` *(in, AIL_DOUBLE)*

Specifies the start radius of the zone of interest in the circle, in pixels.

### `EndRadius` *(in, AIL_DOUBLE)*

Specifies the end radius of the zone of interest in the circle, in pixels.

### `StartAngle` *(in, AIL_DOUBLE)*

Specifies the start angle of the zone of interest. The scan starts at this angle.

*For specifying the starting angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 720.0` | Specifies the start angle.

If the start angle ([`StartAngle`](../../Reference/im/MimPolarTransform.md)) is less than the end angle ([`EndAngle`](../../Reference/im/MimPolarTransform.md)), the direction of the scan will be counter clockwise. If the start angle is greater than the end angle, the direction of the scan will be clockwise. There is no maximum span limit between [`StartAngle`](../../Reference/im/MimPolarTransform.md) and [`EndAngle`](../../Reference/im/MimPolarTransform.md). |

### `EndAngle` *(in, AIL_DOUBLE)*

Specifies the end angle of the zone of interest. The scan ends at this angle.

*For specifying the ending angle*

| Value | Description |
| --- | --- |
| `-360.0 <= Value <= 720.0` | Specifies the end angle.

If the end angle ([`EndAngle`](../../Reference/im/MimPolarTransform.md)) is greater than the start angle ([`StartAngle`](../../Reference/im/MimPolarTransform.md)), the direction of the scan will be counter clockwise. If the end angle is less than the start angle, the direction of the scan will be clockwise. There is no maximum span limit between [`StartAngle`](../../Reference/im/MimPolarTransform.md) and [`EndAngle`](../../Reference/im/MimPolarTransform.md). |

### `OperationMode` *(in, AIL_INT64)*

Specifies the transform to perform.

### `InterpolationMode` *(in, AIL_INT64)*

Specifies the interpolation mode and overscan used when transforming the source image. When generating the LUTs to perform the transformation, this parameter is ignored and should be set to [`M_DEFAULT`](../../Reference/im/MimPolarTransform.md).

*For specifying the interpolation mode used in the conversion*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default interpolation mode and overscan. When transforming the source image, the default is the same as [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimPolarTransform.md) + [`M_OVERSCAN_ENABLE`](../../Reference/im/MimPolarTransform.md).

When only generating the LUTs to perform the transformation, this is the only possible value. |
| `M_BICUBIC` | Specifies bicubic interpolation. The new value is determined by taking a weighted average of the 16 values (4x4) that surround the source point. Note that the sum of the weights used for bicubic interpolation might be greater than one. If this occurs and the result reflects an overflow or underflow, the result is saturated according to the depth and data type of the destination buffer. |
| `M_BILINEAR` | Specifies bilinear interpolation. The new value is determined by taking a weighted average of the 4 values (2x2) that surround the source point. |
| `M_NEAREST_NEIGHBOR` | Specifies nearest neighbor interpolation. The new value is that of the pixel closest to the source point. |

*For specifying how to determine the value of a destination pixel when its associated point falls outside the source buffer*

| Value | Description |
| --- | --- |
| `M_OVERSCAN_CLEAR` | Sets the destination pixel to 0. |
| `M_OVERSCAN_DISABLE` | Leaves the destination pixel as is. |
| `M_OVERSCAN_ENABLE` *(default)* | Uses pixels from the source buffer's ancestor buffer. If the source buffer is not a child buffer or if the point falls outside the ancestor buffer, leave the destination pixel as is. |
| `M_OVERSCAN_FAST` | Specifies that Aurora Imaging Library will automatically select the overscan that optimizes speed, according to the specified operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.

Note that when using [`M_OVERSCAN_FAST`](../../Reference/im/MimWarp.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |

### `DstSizeXPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which the required width of the destination buffer is written. If you don't need this information, set this parameter to `M_NULL`.

### `DstSizeYPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which the required height of the destination buffer is written. If you don't need this information, set this parameter to `M_NULL`.

## Parameter Associations

### For selecting the Operation Mode

---

### `M_POLAR_TO_RECTANGULAR`

Performs a polar-to-rectangular transformation.

---

### `M_POLAR_TO_RECTANGULAR_LUT`

Generates X- and Y-coordinate LUTs for use with [`MimWarp`](../../Reference/im/MimWarp.md)to perform the specified polar-to-rectangular transformation.

---

### `M_RECTANGULAR_TO_POLAR`

Performs a rectangular-to-polar transformation.

---

### `M_RECTANGULAR_TO_POLAR_LUT`

Generates X- and Y-coordinate LUTs for use with [`MimWarp`](../../Reference/im/MimWarp.md)to perform the specified rectangular-to-polar transformation.

### Combination Constants — For generating LUTs

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the number of fractional bits in the coordinates of the source point.

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Specifies the number of fractional bits for the source point (`_x_ <sub>s</sub>`, `_y_ <sub>s</sub>`). To do so, add the define [`M_FIXED_POINT + n`](../../Reference/im/MimPolarTransform.md) to [`M_POLAR_TO_RECTANGULAR_LUT`](../../Reference/im/MimPolarTransform.md) or [`M_RECTANGULAR_TO_POLAR_LUT`](../../Reference/im/MimPolarTransform.md)where `_n_ >= 0`.  If nothing is added to [`M_POLAR_TO_RECTANGULAR_LUT`](../../Reference/im/MimPolarTransform.md) or [`M_RECTANGULAR_TO_POLAR_LUT`](../../Reference/im/MimPolarTransform.md), it is assumed that there are no fractional bits in the coordinates of the source point. |
