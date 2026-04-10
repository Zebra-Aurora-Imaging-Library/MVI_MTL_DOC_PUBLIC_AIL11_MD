---
doctype: Reference
module: 3dim
function: M3dimProjectEx
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimProjectEx"
---

# M3dimProjectEx

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

> Project a point cloud or 3D geometry onto the XY (Z=0) plane to generate a fully corrected depth map according to the settings of the project 3D image processing context.

## Syntax

```c
void M3dimProjectEx(
    AIL_ID    ProjectContext3dimId,              //in
    AIL_ID    SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID    AABox3dgeoId,                      //in
    AIL_ID    DstDepthMapBufId,                  //in
    AIL_INT64 Options,                           //in
    AIL_INT64 ControlFlag                        //in
)
```

## Description

This function projects a point cloud or a 3D geometry onto the XY (Z=0) plane, generating a fully corrected depth map. A depth map pixel's gray level indicates depth. You can use [`M3dimControl`](../../Reference/3dim/M3dimControl.md) to specify settings, such as the pixel aspect ratio and projection mode.

This function can be used instead of [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md), [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md), and [`M3dimProject`](../../Reference/3dim/M3dimProject.md). If you are projecting into a container, [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) can optionally allocate an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component or change the dimensions of a previously existing [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component and assign the 3D properties required to produce natively calibrated coordinates before projecting. If you are projecting into an image buffer, [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) can be used instead of [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md) and [`M3dimProject`](../../Reference/3dim/M3dimProject.md) to calibrate the destination image buffer and then project.

You can optionally specify a region of the point cloud to project using an axis-aligned 3D box geometry and [`M_FIT_BOX`](../../Reference/3dim/M3dimProjectEx.md) or [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md).

Note that [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) can also project intensity values from the source container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) component, if one exists. Intensity values are projected into the destination container's [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) component (respectively), which is created if not previously existing in the destination container. If both components exist in the source container, only the reflectance component is considered.

[`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md) projects the points of the point cloud on the XY (Z=0) plane of its working coordinate system. To change the resulting depth map, use [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md) or [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to rotate or transform (respectively) the 3D points before calling [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md).

If you want to limit which points are considered for the projection, prior to calling [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md), you can crop or mask the point cloud (using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) or changing the data in the source container's confidence component). For more information, see [Regions of interest](../../UserGuide/C02_Building_an_application/S12_Working_with_3D.md).

## Parameters

### `ProjectContext3dimId` *(in, AIL_ID)*

Specifies a project 3D image processing context.

*For specifying the project 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default project 3D image processing context of the current Aurora Imaging Library application.

> **Note:** The operation will use default values for all project context control types ([`M3dimControl`](../../Reference/3dim/M3dimControl.md)). |
| `Project 3D image processing context identifier` | Specifies the identifier of a project 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_PROJECT_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

> **Note:** If a project context is specified, the function applies the project control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md). |

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies a source container or 3D geometry object to project.

*For specifying the source container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Source 3D geometry object identifier` | Specifies the identifier of a 3D geometry object, except that of a box geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include cylinder, line, plane, point, and sphere. |
| `Source container identifier` | Specifies the identifier of the source container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `AABox3dgeoId` *(in, AIL_ID)*

Specifies the identifier of the axis-aligned 3D box geometry object to use to limit the calculation to points within the box. For [`M_FIT_BOX`](../../Reference/3dim/M3dimProjectEx.md) and [`M_USE_BOX_WORLD_SIZE`](../../Reference/3dim/M3dimProjectEx.md), the box is also used to define the calibration region. The 3D box geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and defined as a box.

### `DstDepthMapBufId` *(in, AIL_ID)*

Specifies the identifier of the destination image buffer or container in which to store the depth map.

*For specifying the depth map*

| Value | Description |
| --- | --- |
| `Destination container identifier` | Specifies the identifier of the destination container.

The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

If you pass [`M_USE_DESTINATION`](../../Reference/3dim/M3dimProjectEx.md) to the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter, the container must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)). If you pass [`M_FIT_BOX`](../../Reference/3dim/M3dimProjectEx.md) or [`M_FIT_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md) to the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter, an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component must exist in the container. |
| `Destination image buffer identifier` | Specifies the identifier of the destination image buffer.

The destination image buffer must be a 1-band, 8-bit, 16-bit or 32-bit unsigned image buffer.

If you pass [`M_USE_DESTINATION`](../../Reference/3dim/M3dimProjectEx.md) to the [`Options`](../../Reference/3dim/M3dimProjectEx.md) parameter, the image buffer must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). |

### `Options` *(in, AIL_INT64)*

Specifies additional options for the destination.

*For specifying the option*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically choose the option.

If the source is a point cloud and the destination is a container, [`M_AUTO`](../../Reference/3dim/M3dimProjectEx.md) is the same as [`M_USE_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md). If the source is a point cloud and the destination is an image buffer, or if the source is a finite geometry, [`M_AUTO`](../../Reference/3dim/M3dimProjectEx.md) is the same as [`M_FIT_SRC_DATA`](../../Reference/3dim/M3dimProjectEx.md). If the source is an infinite geometry and an axis-aligned box is provided, [`M_AUTO`](../../Reference/3dim/M3dimProjectEx.md) is the same as [`M_FIT_BOX`](../../Reference/3dim/M3dimProjectEx.md). If the source is an infinite geometry and no axis-aligned box is provided, [`M_AUTO`](../../Reference/3dim/M3dimProjectEx.md) is the same as [`M_USE_DESTINATION`](../../Reference/3dim/M3dimProjectEx.md). |
| `M_FIT_BOX` | Specifies to reuse the X-size and Y-size of the destination image buffer or container's range component and to choose the scales and offsets so that the axis-aligned box fits inside the destination depth map.

This option is not available when the [`AABox3dgeoId`](../../Reference/3dim/M3dimProjectEx.md) parameter is set to `M_NULL` or the destination is a container that does not have an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_FIT_SRC_DATA` | Specifies to reuse the X-size and Y-size of the destination image buffer or container's range component and to choose the scales and offsets so that the source data fits inside the destination depth map.

Note that if an axis-aligned box is provided, only points within the box are considered.

This option is not available when the source is an infinite geometry or the destination is a container that does not have an [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) component. |
| `M_USE_BOX_WORLD_SIZE` | Specifies to use the axis-aligned box to determine the X-size, Y-size, scales, and offsets of the destination container's range component.

Note that the points within the box are used to determine the destination pixel sizes.

This option is not available when the [`AABox3dgeoId`](../../Reference/3dim/M3dimProjectEx.md) parameter is set to `M_NULL`, the source is a geometry, or the destination is an image buffer. |
| `M_USE_DESTINATION` | Specifies to reuse the X-size, Y-size, and calibration information of the destination image buffer or 3D properties of the destination container's range component.

This option is only available when the destination is a fully corrected depth map image buffer or a 3D-processable depth map container. |
| `M_USE_SRC_DATA` | Specifies to use the source data to determine the X-size, Y-size, scales, and offsets of the destination container's range component.

Note that if an axis-aligned box is provided, only points within the box are considered.

This option is not available when the source is a geometry or the destination is an image buffer. |

*For accumulating data*

| Value | Description |
| --- | --- |
| `M_ACCUMULATE` | Specifies not to clear the previous contents of the destination depth map prior to projection. This is useful to project additional data. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
