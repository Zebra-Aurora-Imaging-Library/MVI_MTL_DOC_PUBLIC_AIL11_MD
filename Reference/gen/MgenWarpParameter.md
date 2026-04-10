---
doctype: Reference
module: gen
function: MgenWarpParameter
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / gen / MgenWarpParameter"
---

# MgenWarpParameter

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

> Generate coefficients or LUTs used to warp an image.

## Syntax

```c
void MgenWarpParameter(
    AIL_ID     SrcArrayBufId,        //in
    AIL_ID     DstXLutOrArrayBufId,  //out
    AIL_ID     DstYLutBufId,         //out
    AIL_INT    OperationMode,        //in
    AIL_INT    Transform,            //in
    AIL_DOUBLE Val1,                 //in
    AIL_DOUBLE Val2                  //in
)
```

## Description

This function generates coefficients or LUTs to be used by [`MimWarp`](../../Reference/im/MimWarp.md) to perform first-order polynomial warpings or perspective polynomial warpings. Specifically, this function can generate:

- Coefficients for a first-order polynomial warping ([`M_WARP_POLYNOMIAL`](../../Reference/gen/MgenWarpParameter.md)).
- Coefficients or LUTs for a perspective polynomial warping that maps an arbitrary quadrilateral onto a rectangle ([`M_WARP_4_CORNER`](../../Reference/gen/MgenWarpParameter.md)) or that maps a rectangle onto an arbitrary quadrilateral ([`M_WARP_4_CORNER_REVERSE`](../../Reference/gen/MgenWarpParameter.md)).
- LUTs, based on the input of a matrix of coefficients, for a first-order polynomial warping or for a perspective polynomial warping ([`M_WARP_LUT`](../../Reference/gen/MgenWarpParameter.md)).

Note that when generating LUTs for an [`M_WARP_4_CORNER`](../../Reference/gen/MgenWarpParameter.md) or an [`M_WARP_4_CORNER_REVERSE`](../../Reference/gen/MgenWarpParameter.md) perspective polynomial warping, the coefficients that were internally generated and used cannot be retrieved. To generate LUTs while saving the coefficients, you must call [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md) twice: once to generate the coefficients needed to produce the LUTs, and the second time to actually generate the LUTs with [`M_WARP_LUT`](../../Reference/gen/MgenWarpParameter.md).

For a first-order polynomial warping, this function can generate coefficients or LUTs for a rotation, a scaling, a shearing, or a translation. To combine coefficients (for example, to generate coefficients that will perform both a rotation and a translation), you need to make separate calls to this function, using the previous output buffer of type [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) as your current input buffer. If required, after the appropriate coefficients are generated, you can pass the coefficient buffer to [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md) with [`M_WARP_LUT`](../../Reference/gen/MgenWarpParameter.md) to generate the corresponding LUTs.

Coefficients are calculated such that each pixel position of the destination buffer, `(_x_ <sub>d</sub>,_y_ <sub>d</sub>)` is associated with a specific point in the source buffer, `(_x_ <sub>s</sub>,_y_ <sub>s</sub>)`, according to the following equation:

*[Image: mgenwarpparameter_eq1.png]*

Where _x<sub>s</sub> _ and _y<sub>s</sub> _ are defined as follows:

*[Image: MgenWarpParameter_Eq2.png]*

*[Image: MgenWarpParameter_Eq2(2).png]*

Note that in the case of a first-order polynomial warping, the _c<sub>0</sub>, c<sub>1</sub>, c<sub>2</sub> _ coefficients are 0, 0, and 1, respectively. The equation then reduces to:

*[Image: MgenWarpParameter_Eq3.png]*

*[Image: MgenWarpParameter_Eq3(2).png]*

## Parameters

### `SrcArrayBufId` *(in, AIL_ID)*

Specifies the input buffer from which to generate coefficients or LUT entries. If an input buffer is not required to perform the coefficient generation, set this parameter to `M_NULL`.

### `DstXLutOrArrayBufId` *(out, AIL_ID)*

Specifies the buffer in which to place generated coefficients or X-LUT entries.

### `DstYLutBufId` *(out, AIL_ID)*

Specifies the buffer in which to place Y-LUT entries. If you are not generating LUTs, set this parameter to `M_NULL`.

### `OperationMode` *(in, AIL_INT)*

Specifies the mode of operation.

### `Transform` *(in, AIL_INT)*

Specifies the type of first-order polynomial warping for which to generate coefficients. If you are not generating coefficients for a first-order polynomial warping ([`M_WARP_POLYNOMIAL`](../../Reference/gen/MgenWarpParameter.md)), set the this parameter to `M_DEFAULT`.

*For specifying the type of first-order polynomial warping*

| Value | Description |
| --- | --- |
| `M_ROTATE` | Generates coefficients for a counter-clockwise rotation around (0,0) by [`Val1`](../../Reference/gen/MgenWarpParameter.md)°. |
| `M_SCALE` | Generates coefficients for an image scaling, by a factor of [`Val1`](../../Reference/gen/MgenWarpParameter.md) in the X-direction and by a factor of [`Val2`](../../Reference/gen/MgenWarpParameter.md) in the Y-direction. |
| `M_SHEAR_X` | Generates coefficients or for a shearing in the X-direction, by a factor of [`Val1`](../../Reference/gen/MgenWarpParameter.md). |
| `M_SHEAR_Y` | Generates coefficients for a shearing in the Y-direction, by a factor of [`Val1`](../../Reference/gen/MgenWarpParameter.md). |
| `M_TRANSLATE` | Generates coefficients for a translation by [`Val1`](../../Reference/gen/MgenWarpParameter.md) pixels in the X-direction and by [`Val2`](../../Reference/gen/MgenWarpParameter.md) pixels in the Y-direction. |

### `Val1` *(in, AIL_DOUBLE)*

Specifies the first transform constant. If this parameter is not being used, set it to `M_NULL`.

### `Val2` *(in, AIL_DOUBLE)*

Specifies the second transform constant. If this parameter is not being used, set it to `M_NULL`.

## Parameter Associations

### For selecting the operation mode

---

### `M_WARP_4_CORNER`

Generates coefficients or LUT values for a perspective polynomial warping which, when passed to [`MimWarp`](../../Reference/im/MimWarp.md), maps an arbitrary quadrilateral onto a rectangle.  *[Image: persp_warp_4_corner.png]*

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the Aurora Imaging Library array buffer identifier. The buffer must be a 3x3 floating-point buffer allocated with an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute. |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library LUT buffer identifier. The buffer must be a signed 16- or 32-bit integer buffer or a 32-bit floating-point buffer with an [`M_LUT`](../../Reference/buf/MbufAlloc1d.md) attribute and the same X-size as the destination image buffer that will be passed to [`MimWarp`](../../Reference/im/MimWarp.md). |
| `M_NULL` | Specifies that the coefficients will be generated. |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library LUT buffer identifier. The buffer must be a signed 16- or 32-bit integer buffer or a 32-bit floating-point buffer, which has an [`M_LUT`](../../Reference/buf/MbufAlloc1d.md) attribute and the same Y-size as the destination image buffer that will be passed to [`MimWarp`](../../Reference/im/MimWarp.md). |

---

### `M_WARP_4_CORNER_REVERSE`

Generates coefficients or LUT values for a perspective polynomial warping which, when passed to [`MimWarp`](../../Reference/im/MimWarp.md), maps a rectangle onto an arbitrary quadrilateral.  *[Image: persp_warp_4_corner_reverse.png]*

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the Aurora Imaging Library array buffer identifier. The buffer must be a 3x3 floating-point buffer allocated with an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute. |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library LUT buffer identifier. The buffer must be a signed 16- or 32-bit integer buffer or a 32-bit floating-point buffer with an [`M_LUT`](../../Reference/buf/MbufAlloc1d.md) attribute and the same X-size as the destination image buffer that will be passed to [`MimWarp`](../../Reference/im/MimWarp.md). |
| `M_NULL` | Specifies that the coefficients will be generated. |
| `LUT buffer identifier` | Specifies a signed 16- or 32-bit integer buffer or a 32-bit floating-point buffer, which has an [`M_LUT`](../../Reference/buf/MbufAlloc1d.md) attribute and the same Y-size as the destination image buffer that will be passed to [`MimWarp`](../../Reference/im/MimWarp.md). |

---

### `M_WARP_LUT`

Generates LUTs that can be passed to [`MimWarp`](../../Reference/im/MimWarp.md) to perform a warping that is equivalent to the warping that would be performed if the coefficients specified in the [`SrcArrayBufId`](../../Reference/gen/MgenWarpParameter.md) parameter were used instead.

---

### `M_WARP_POLYNOMIAL`

Generates coefficients that can be passed to [`MimWarp`](../../Reference/im/MimWarp.md) to perform a first-order polynomial warping.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that this is the first call to [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md). |
| `Array buffer identifier` | Specifies an Aurora Imaging Library array buffer identifier. The buffer must be a 3x3 floating-point buffer with an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute. |

### Combination Constants — For the output LUT buffers

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the number of fractional bits for the source address.

| Value | Description |
| --- | --- |
| `M_FIXED_POINT + n` | Specifies the number of fractional bits for the source address `(_x_ <sub>s</sub>`,`_y_ <sub>s</sub>)`. The default value is 0.  Note that when using this combination constant with [`M_WARP_4_CORNER`](../../Reference/gen/MgenWarpParameter.md) or [`M_WARP_4_CORNER_REVERSE`](../../Reference/gen/MgenWarpParameter.md), the destination buffers ([`DstXLutOrArrayBufId`](../../Reference/gen/MgenWarpParameter.md) and [`DstYLutBufId`](../../Reference/gen/MgenWarpParameter.md)) must be LUT buffers (similar to when using [`M_WARP_LUT`](../../Reference/gen/MgenWarpParameter.md)). |
