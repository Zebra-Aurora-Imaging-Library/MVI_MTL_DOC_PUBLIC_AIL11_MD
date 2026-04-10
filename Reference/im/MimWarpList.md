---
doctype: Reference
module: im
function: MimWarpList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / im / MimWarpList"
---

# MimWarpList

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

> Applies a warping matrix to a list of coordinates.

## Syntax

```c
void MimWarpList(
    AIL_ID             WarpParamBufId,     //in
    AIL_INT64          TransformType,      //in
    AIL_INT            NumPoints,          //in
    const AIL_DOUBLE * SrcCoordXArrayPtr,  //in
    const AIL_DOUBLE * SrcCoordYArrayPtr,  //in
    AIL_DOUBLE *       DstCoordXArrayPtr,  //out
    AIL_DOUBLE *       DstCoordYArrayPtr,  //out
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function transforms a list of coordinates (points) using the specified warping matrix and stores the resulting coordinates in the destination arrays ([`M_REVERSE`](../../Reference/im/MimWarpList.md)). Alternatively, this function can transform the coordinates using the inverse of the specified matrix ([`M_FORWARD`](../../Reference/im/MimWarpList.md)).

When this function transforms the coordinates using the warping matrix as is ([`M_REVERSE`](../../Reference/im/MimWarpList.md)), it is referred to as a reverse warping transformation because this is how [`MimWarp`](../../Reference/im/MimWarp.md)associates each pixel position of its destination buffer, (_x<sub>d</sub>, y<sub>d</sub> _), with a specific point in its source buffer, (_x<sub>s</sub>, y<sub>s</sub> _). Therefore, you can use [`MimWarpList`](../../Reference/im/MimWarpList.md)to establish which points in a source image [`MimWarp`](../../Reference/im/MimWarp.md)associates with destination pixels when using the same matrix. When this function transforms the coordinates using the inverse of the specified warping matrix ([`M_FORWARD`](../../Reference/im/MimWarpList.md)), it is referred to as a forward warping transformation; you can use a forward warping transformation, for example, to see if a specific pixel in the source image of [`MimWarp`](../../Reference/im/MimWarp.md)is actually mapped to a destination buffer pixel.

You can only specify the coefficients for a 3x3 or 3x2 matrix; this allows you to perform a first-order polynomial warping or a perspective polynomial warping.

Perform a first-order polynomial warping by passing a 3x3 matrix, where the third row has the coefficients [0,0,1], or by passing a 3x2 matrix, the last row is assumed to be [0,0,1]. Perform a perspective polynomial warping by passing a 3x3 matrix, where the third row does not have the coefficients [0,0,1]. The matrix is applied as follows:

`_x_ <sub>d</sub> = (a<sub>0</sub> _x_ <sub>s</sub> + a<sub>1</sub> _y_ <sub>s</sub> + a<sub>2</sub>)/(c<sub>0</sub> _x_ <sub>s</sub> + c<sub>1</sub> _y_ <sub>s</sub> + c<sub>2</sub>)`

`_y_ <sub>d</sub> = (b<sub>0</sub> _x_ <sub>s</sub> + b<sub>1</sub> _y_ <sub>s</sub> + b<sub>2</sub>)/(c<sub>0</sub> _x_ <sub>s</sub> + c<sub>1</sub> _y_ <sub>s</sub> + c<sub>2</sub>)`

If a 3x2 matrix, or a 3x3 matrix whose third row has the coefficients of [0,0,1] is specified (for a first-order polynomial warping operation), the denominator resolves to 1, effectively leaving only the numerator. The coefficients can be automatically generated using [`MgenWarpParameter`](../../Reference/gen/MgenWarpParameter.md), [`MimResize`](../../Reference/im/MimResize.md), [`MimRotate`](../../Reference/im/MimRotate.md), or [`MimTranslate`](../../Reference/im/MimTranslate.md), or can be user-established.

Note that [`MimWarpList`](../../Reference/im/MimWarpList.md)will internally invert the coefficients matrix (_a<sub>0</sub>...a<sub>2</sub>, b<sub>0</sub>...b<sub>2</sub>, c<sub>0</sub>...c<sub>2</sub> _) for a forward transfomation ([`M_FORWARD`](../../Reference/im/MimWarpList.md)).

## Parameters

### `WarpParamBufId` *(in, AIL_ID)*

Specifies the buffer containing the warp matrix coefficients.

### `TransformType` *(in, AIL_INT64)*

Specifies the transformation type to perform the warping.

*For specifying the transformation type.*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FORWARD` | Specifies to perform a forward transformation. A forward transformation uses the inverse of the specified warping matrix to transform the source X- and Y-coordinates. |
| `M_REVERSE` *(default)* | Specifies to perform a reverse transformation. A reverse transformation uses the specified warping matrix as is to transform the source X- and Y-coordinates. |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the arrays.

### `SrcCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source X-coordinates.

### `SrcCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array that contains the source Y-coordinates.

### `DstCoordXArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the destination X-coordinates.

### `DstCoordYArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to store the destination Y-coordinates.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
