---
doctype: Reference
module: 3dim
function: M3dimCalculateMapSize
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimCalculateMapSize"
---

# M3dimCalculateMapSize

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

> Estimate a recommended depth map image buffer size from a source container's point cloud.

## Syntax

```c
void M3dimCalculateMapSize(
    AIL_ID    CalculateMapSizeContext3dimId,  //in
    AIL_ID    SrcContainerBufId,              //in
    AIL_ID    AABox3dgeoId,                   //in
    AIL_INT64 ControlFlag,                    //in
    AIL_INT * DepthMapSizeXPtr,               //out
    AIL_INT * DepthMapSizeYPtr                //out
)
```

## Description

This function estimates a recommended depth map image buffer size in X and Y, using the dimensions of the source container's point cloud. You can optionally limit the calculation to consider only points inside a specified axis-aligned 3D box geometry.

The recommended size is useful for allocating a depth map image buffer (using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md)) before calling [`M3dimProject`](../../Reference/3dim/M3dimProject.md).

Using [`M3dimControl`](../../Reference/3dim/M3dimControl.md), you can set the depth map's required pixel aspect ratio. You can also set a fixed size along 1 dimension, and the function will calculate the necessary size of the other dimension (for either pixel size or image buffer size). Default settings let [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) do all calculations.

## Parameters

### `CalculateMapSizeContext3dimId` *(in, AIL_ID)*

Specifies a calculate map size 3D image processing context.

*For specifying the calculate map size 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default calculate map size 3D image processing context of the current Aurora Imaging Library application.

> **Note:** The operation will use default values for all calculate map size context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)). |
| `Calculate map size 3D image processing context identifier` | Specifies the identifier of a calculate map size 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_CALCULATE_MAP_SIZE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a calculate map size context is specified, the function applies the calculate map size control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source container containing a 3D-processable point cloud. The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

### `AABox3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the axis-aligned 3D box geometry object to use to limit the calculation to points within the box. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DepthMapSizeXPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the estimated depth map image buffer size in X.

### `DepthMapSizeYPtr` *(out, *AIL_INT)*

Specifies the address of the variable in which to write the estimated depth map image buffer size in Y.
