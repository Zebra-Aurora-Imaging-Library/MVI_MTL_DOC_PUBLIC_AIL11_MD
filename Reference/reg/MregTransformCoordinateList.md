---
doctype: Reference
module: reg
function: MregTransformCoordinateList
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / reg / MregTransformCoordinateList"
---

# MregTransformCoordinateList

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

> Convert a list of coordinates between two of the following coordinate systems: the global pixel coordinate system, any registered image's pixel coordinate system, and the mosaic's coordinate system.

## Syntax

```c
void MregTransformCoordinateList(
    AIL_ID             RegResultId,        //in
    AIL_INT            Source,             //in
    AIL_INT            Destination,        //in
    AIL_INT            NumPoints,          //in
    const AIL_DOUBLE * SrcCoordXArrayPtr,  //in
    const AIL_DOUBLE * SrcCoordYArrayPtr,  //in
    AIL_DOUBLE *       DstCoordXArrayPtr,  //out
    AIL_DOUBLE *       DstCoordYArrayPtr,  //out
    AIL_INT64          ControlFlag         //in
)
```

## Description

Convert a list of coordinates between two of the following coordinate systems: the global pixel coordinate system, any registered image's pixel coordinate system, and the mosaic's coordinate system.

## Parameters

### `RegResultId` *(in, AIL_ID)*

Specifies the registration result buffer that contains the information that will be used to transform the coordinates. The registration result buffer must have been previously allocated on the required system using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) or restored from a file using [`MregRestore`](../../Reference/reg/MregRestore.md).

### `Source` *(in, AIL_INT)*

Specifies the source coordinate system. This parameter must be set to one of the following values:

*For the source coordinate system*

| Value | Description |
| --- | --- |
| `M_MOSAIC` | Specifies to use the coordinate system relative to which the mosaic will be composed. This coordinate system was specified using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md). |
| `M_REGISTRATION_GLOBAL` | Specifies the global pixel coordinate system. |
| `Value` | Specifies the index of the registration result element associated with the image whose coordinate system to use. |

### `Destination` *(in, AIL_INT)*

Specifies the destination coordinate system. This parameter must be set to one of the following values:

*For the destination coordinate system*

| Value | Description |
| --- | --- |
| `M_MOSAIC` | Specifies to use the coordinate system relative to which the mosaic will be composed. This coordinate system was specified using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md). |
| `M_REGISTRATION_GLOBAL` | Specifies the global pixel coordinate system. |
| `Value` | Specifies the index of the registration result element associated with the image whose coordinate system to use. |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points in the coordinate list.

### `SrcCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array of the X-coordinates in the source coordinate system.

### `SrcCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array of the Y-coordinates in the source coordinate system.

### `DstCoordXArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address in which to write the resulting X-coordinate. This coordinate is given in the coordinate system that is specified in the [`Destination`](../../Reference/reg/MregTransformCoordinate.md) parameter.

### `DstCoordYArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address in which to write the resulting Y-coordinate. This coordinate is given in the coordinate system that is specified in the [`Destination`](../../Reference/reg/MregTransformCoordinate.md) parameter.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
