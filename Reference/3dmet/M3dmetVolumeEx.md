---
doctype: Reference
module: 3dmet
function: M3dmetVolumeEx
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmet / M3dmetVolumeEx"
---

# M3dmetVolumeEx

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

> Computes the volume of a point cloud's mesh or of a depth map, delimited optionally by a specified reference object, and stores the result in a result buffer.

## Syntax

```c
void M3dmetVolumeEx(
    AIL_ID    Volume3dmetContextId,        //in
    AIL_ID    SrcContainerOrImageBufId,    //in
    AIL_ID    Reference3dgeoOrImageBufId,  //in
    AIL_ID    Calculate3dmetResultId,      //out
    AIL_INT64 ControlFlag                  //in
)
```

## Description

This function calculates the volume of a point cloud's mesh or of a depth map, delimited optionally by a specified reference object, and stores the result in a result buffer. To retrieve the result and related calculation information, use [`M3dmetGetResult`](../../Reference/3dmet/M3dmetGetResult.md). If you only need the calculated volume, you can use [`M3dmetVolume`](../../Reference/3dmet/M3dmetVolume.md) instead of [`M3dmetVolumeEx`](../../Reference/3dmet/M3dmetVolumeEx.md).

You can specify a predefined context for calculating the volume.

If the source Aurora Imaging Library object is not sufficient to define a closed volume, you must specify a reference object. This function extends the boundaries of the source Aurora Imaging Library object (and for a mesh, its holes) onto the reference Aurora Imaging Library object to define the closed 3D shape whose volume to calculate. How the boundaries extend onto the reference Aurora Imaging Library object depends on the type of the source Aurora Imaging Library object. If the boundaries of a mesh are extended, they will be extended perpendicular to the reference Aurora Imaging Library object. If the boundaries of a depth map are extended, they will be extended in the Z-direction until they meet the reference Aurora Imaging Library object.

Even if not required, you can specify a reference object that limits the volume to calculate. Use [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md) with [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) to specify how the position of the reference Aurora Imaging Library object, relative to the enclosed shape, defines the volume calculation. For example, if you use a 3D plane geometry as the reference object of a mesh, you can specify whether to calculate the volume of the mesh above or below the plane.

## Parameters

### `Volume3dmetContextId` *(in, AIL_ID)*

Specifies the context for volume calculation.

*For specifying the volume 3D metrology context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default volume 3D metrology context of the current Aurora Imaging Library application.

> **Note:** The volume operation will use default values for all volume control types listed in [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |
| `M_VOLUME_CONTEXT_ABOVE` | Specifies a predefined volume 3D metrology context with all volume context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_ABOVE`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to specify that the total volume is equal only to the volume above the reference Aurora Imaging Library object. Any volume below the reference Aurora Imaging Library object is ignored. |
| `M_VOLUME_CONTEXT_DIFFERENCE` | Specifies a predefined volume 3D metrology context with all volume context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_DIFFERENCE`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the total volume as the volume above the reference Aurora Imaging Library object, minus the volume below the reference Aurora Imaging Library object. |
| `M_VOLUME_CONTEXT_TOTAL` | Specifies a predefined volume 3D metrology context with all volume context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_TOTAL`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to calculate the total volume as the sum of all volumes between the source and reference Aurora Imaging Library objects, whether above or below the reference Aurora Imaging Library object. |
| `M_VOLUME_CONTEXT_UNDER` | Specifies a predefined volume 3D metrology context with all volume context control types ([`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md)) set to their default, except [`M_VOLUME_MODE`](../../Reference/3dmet/M3dmetControl.md) which is set to [`M_UNDER`](../../Reference/3dmet/M3dmetControl.md). Use this predefined context to specify that the total volume is equal only to the volume under the reference Aurora Imaging Library object. Any volume above the reference Aurora Imaging Library object is ignored. |
| `Volume 3D metrology context identifier` | Specifies the identifier of a volume 3D metrology context, previously allocated using [`M3dmetAlloc`](../../Reference/3dmet/M3dmetAlloc.md) with [`M_VOLUME_CONTEXT`](../../Reference/3dmet/M3dmetAlloc.md).

> **Note:** If a previously allocated context is specified, the function applies the volume control settings specified using [`M3dmetControl`](../../Reference/3dmet/M3dmetControl.md). |

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

If the source Aurora Imaging Library object is a point cloud container, the default behavior is the same as [`M_NULL`](../../Reference/3dmet/M3dmetVolumeEx.md).

If the source Aurora Imaging Library object is a depth map, the default behavior is the same as [`M_XY_PLANE`](../../Reference/3dmet/M3dmetVolumeEx.md). |
| `M_NULL` | Specifies to not use a reference Aurora Imaging Library object for defining the volume to calculate.

[`M_NULL`](../../Reference/3dmet/M3dmetVolumeEx.md) can only be specified when calculating the volume of meshes without any holes. |
| `M_XY_PLANE` | Specifies to use the XY (Z=0) plane as the reference Aurora Imaging Library object.

If the source Aurora Imaging Library object is a point cloud container, holes in the mesh are allowed, and the boundaries of the holes in the mesh are extended to the XY-plane at an angle perpendicular to the plane.

If the source Aurora Imaging Library object is a depth map, the boundaries of the holes in the depth map are extended in the Z-direction to the XY-plane. |
| `3D plane geometry object identifier` | Specifies the identifier of the 3D plane geometry object to use as the reference Aurora Imaging Library object. The 3D plane geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a plane.

If the source Aurora Imaging Library object is a point cloud container, holes in the mesh are allowed, and the boundaries of the holes in the mesh are extended to the reference plane at an angle perpendicular to the plane.

If the source Aurora Imaging Library object is a depth map, the boundaries of the holes in the depth map are extended in the Z-direction to the reference plane. |
| `Depth map container identifier` | Specifies the identifier of the depth map container to use as the reference Aurora Imaging Library object. This is only usable with a depth map as the source Aurora Imaging Library object.

The container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)).

When both source and reference Aurora Imaging Library objects are depth maps, their calibration information can vary in Z, but not in X and Y. That is, the depth map specified with [`SrcContainerOrImageBufId`](../../Reference/3dmet/M3dmetVolumeEx.md) must have equivalent calibration information for X and Y as the depth map specified with [`Reference3dgeoOrImageBufId`](../../Reference/3dmet/M3dmetVolumeEx.md). The operation is performed point-to-point; therefore, each respective pixel must represent the same area in the real world in X and Y. |
| `Depth map image buffer identifier` | Specifies the identifier of the depth map image buffer to use as the reference Aurora Imaging Library object. This is only usable with a depth map as the source Aurora Imaging Library object.

This image buffer can have a region of interest (ROI) associated with it, in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)).

When both source and reference Aurora Imaging Library objects are depth maps, their calibration information can vary in Z, but not in X and Y. That is, the depth map specified with [`SrcContainerOrImageBufId`](../../Reference/3dmet/M3dmetVolumeEx.md) must have equivalent calibration information for X and Y as the depth map specified with [`Reference3dgeoOrImageBufId`](../../Reference/3dmet/M3dmetVolumeEx.md). The operation is performed point-to-point; therefore, each respective pixel must represent the same area in the real world in X and Y. |

### `Calculate3dmetResultId` *(out, AIL_ID)*

Specifies the identifier of the calculate 3D metrology result buffer in which to store the results of the volume operation.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
