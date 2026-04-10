---
doctype: Reference
module: 3dmet
function: M3dmetVolume
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetVolume"
---

# M3dmetVolume

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

> Computes the volume of a point cloud's mesh or of a depth map, delimited optionally by a specified reference object.

## Syntax

```c
AIL_DOUBLE M3dmetVolume(
    AIL_ID       SrcContainerOrImageBufId,    //in
    AIL_ID       Reference3dgeoOrImageBufId,  //in
    AIL_INT64    Options,                     //in
    AIL_INT64    ControlFlag,                 //in
    AIL_DOUBLE * VolumePtr,                   //out
    AIL_INT64 *  StatusPtr                    //out
)
```

## Description

This function calculates the volume of a point cloud's mesh or of a depth map, delimited optionally by a specified reference object.

If the source Aurora Imaging Library object is not sufficient to define a closed volume, you must specify a reference object. This function extends the boundaries of the source Aurora Imaging Library object (and for a mesh, its holes) onto the reference Aurora Imaging Library object to define the closed 3D shape whose volume to calculate. How the boundaries extend onto the reference Aurora Imaging Library object depends on the type of the source Aurora Imaging Library object. If the boundaries of a mesh are extended, they will be extended perpendicular to the reference Aurora Imaging Library object. If the boundaries of a depth map are extended, they will be extended in the Z-direction until they meet the reference Aurora Imaging Library object.

Even if not required, you can specify a reference object to limit the volume to calculate. You can use the [`Options`](../../Reference/3dmet/M3dmetVolume.md) parameter to specify how the position of the reference Aurora Imaging Library object, relative to enclosed shape, defines the volume calculation. For example, if you use a 3D plane geometry as the reference object of a mesh, you can specify whether to calculate the volume of the mesh above or below the plane.

## Parameters

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the point cloud or depth map for which to calculate the volume.

*For specifying the point cloud or depth map*

| Value | Description |
| --- | --- |
| `Depth map container identifier` | Specifies the identifier of the container, containing a 3D-processable depth map, to use as the source Aurora Imaging Library object.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

Any invalid points in the depth map are treated like holes, but you can use [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md) to fill them before using this function.

The function will compute the volume of the enclosed shape defined by the depth map, a reference object, and planes defined by the extension of the edges of the depth map to the reference Aurora Imaging Library object in the Z-direction. |
| `Depth map image buffer identifier` | Specifies the identifier of the depth map image buffer to use as the source Aurora Imaging Library object. The image buffer must be a 1-band, 8-bit, 16-bit or 32-bit unsigned buffer and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

This image buffer can have a region of interest (ROI) associated with it, in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). Only points inside the ROI will be used in the volume calculation.

Any invalid points in the depth map are treated like holes, but you can use [`M3dimFillGaps`](../../Reference/3dim/M3dimFillGaps.md) to fill them before using this function.

The function will compute the volume of the enclosed shape defined by the depth map, a reference object, and planes defined by the extension of the edges of the depth map to the reference Aurora Imaging Library object in the Z-direction. |
| `Meshed point cloud container identifier` | Specifies the identifier of the container, containing a 3D-processable meshed point cloud, to use as the source Aurora Imaging Library object.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)).

The container must contain a mesh component. If the mesh does not contain any holes, the function computes the volume of the mesh, optionally delimited by the reference object. If the mesh contains holes, the function will compute the volume of the enclosed shape defined by the mesh, the reference object, and the planes defined by the extension of the edges of the mesh to the reference object at a perpendicular angle.

For more detailed information on how the volume is calculated for meshes with holes, see [How the volume of meshes with holes are calculated](../../UserGuide/C39_3D_Metrology/S06_Calculate_volume.md). |

### `Reference3dgeoOrImageBufId` *(in, AIL_ID)*

Specifies the reference Aurora Imaging Library object to use to define a closed 3D shape whose volume to calculate.

*For specifying the reference Aurora Imaging Library object*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to perform the default behavior.

If the source Aurora Imaging Library object is a point cloud container, the default behavior is the same as [`M_NULL`](../../Reference/3dmet/M3dmetVolume.md).

If the source Aurora Imaging Library object is a depth map, the default behavior is the same as [`M_XY_PLANE`](../../Reference/3dmet/M3dmetVolume.md). |
| `M_NULL` | Specifies to not use a reference Aurora Imaging Library object for defining the volume to calculate.

[`M_NULL`](../../Reference/3dmet/M3dmetVolume.md) can only be specified when calculating the volume of meshes without any holes. |
| `M_XY_PLANE` | Specifies to use the XY (Z=0) plane as the reference Aurora Imaging Library object.

If the source Aurora Imaging Library object is a point cloud container, holes in the mesh are allowed, and the boundaries of the holes in the mesh are extended to the XY-plane at an angle perpendicular to the plane.

If the source Aurora Imaging Library object is a depth map, the boundaries of the holes in the depth map are extended in the Z-direction to the XY-plane. |
| `3D plane geometry object identifier` | Specifies the identifier of the 3D plane geometry object to use as the reference Aurora Imaging Library object. The 3D plane geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.

If the source Aurora Imaging Library object is a point cloud container, holes in the mesh are allowed, and the boundaries of the holes in the mesh are extended to the reference plane at an angle perpendicular to the plane.

If the source Aurora Imaging Library object is a depth map, the boundaries of the holes in the depth map are extended in the Z-direction to the reference plane. |
| `Depth map container identifier` | Specifies the identifier of the depth map container to use as the reference Aurora Imaging Library object. This is only usable with a depth map as the source Aurora Imaging Library object.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

When both source and reference Aurora Imaging Library objects are depth maps, their calibration information can vary in Z, but not in X and Y. That is, the depth map specified with [`SrcContainerOrImageBufId`](../../Reference/3dmet/M3dmetVolume.md) must have equivalent calibration information for X and Y as the depth map specified with [`Reference3dgeoOrImageBufId`](../../Reference/3dmet/M3dmetVolume.md). The operation is performed point-to-point; therefore, each respective pixel must represent the same area in the real world in X and Y. |
| `Depth map image buffer identifier` | Specifies the identifier of the depth map image buffer to use as the reference Aurora Imaging Library object. This is only usable with a depth map as the source Aurora Imaging Library object.

This image buffer can have a region of interest (ROI) associated with it, in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)).

When both source and reference Aurora Imaging Library objects are depth maps, their calibration information can vary in Z, but not in X and Y. That is, the depth map specified with [`SrcContainerOrImageBufId`](../../Reference/3dmet/M3dmetVolume.md) must have equivalent calibration information for X and Y as the depth map specified with [`Reference3dgeoOrImageBufId`](../../Reference/3dmet/M3dmetVolume.md). The operation is performed point-to-point; therefore, each respective pixel must represent the same area in the real world in X and Y. |

### `Options` *(in, AIL_INT64)*

Specifies how to calculate the total volume, given the position of the source Aurora Imaging Library object relative to the reference object. If you are calculating the volume of a point cloud without any reference Aurora Imaging Library object, set this parameter to [`M_DEFAULT`](../../Reference/3dmet/M3dmetVolume.md).

*For specifying the condition*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABOVE` | Specifies that the total volume is equal only to the volume above the reference Aurora Imaging Library object. Any volume below the reference Aurora Imaging Library object is ignored. |
| `M_DIFFERENCE` | Specifies that the total volume is calculated as the volume above the reference Aurora Imaging Library object, minus the volume below the reference Aurora Imaging Library object. |
| `M_TOTAL` *(default)* | Specifies that the total volume is calculated as the sum of all volumes between the source and reference Aurora Imaging Library objects, whether above or below the reference Aurora Imaging Library object. |
| `M_UNDER` | Specifies that the total volume is equal only to the volume under the reference Aurora Imaging Library object. Any volume above the reference Aurora Imaging Library object is ignored. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `VolumePtr` *(out, *AIL_DOUBLE)*

Specifies the address of the variable in which to write the calculated volume. Since this function also returns the volume, this parameter can be set to `M_NULL`.

### `StatusPtr` *(out, *AIL_INT64)*

Specifies the address of the variable in which to write the status of the volume calculation.

*For retrieving the status of the volume operation*

| Value | Description |
| --- | --- |
| `M_FAIL_GAPS` | Specifies that the volume was not successfully computed, since the source Aurora Imaging Library object was a mesh with gaps, and there was no reference Aurora Imaging Library object. |
| `M_FAIL_INVALID_MESH` | Specifies that the volume was not successfully computed, since the mesh was not usable. |
| `M_SUCCESS` | Specifies that the volume was successfully computed. |
| `M_WARNING_GAPS` | Specifies that the volume was successfully computed, but there were gaps in the specified source depth map. The gaps were ignored in the volume calculation. |

## Return Value

**Type:** `AIL_DOUBLE`

The returned value is the computed volume, if successful. If the volume calculation fails, `M_NULL` is returned.
