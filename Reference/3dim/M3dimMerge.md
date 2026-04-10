---
doctype: Reference
module: 3dim
function: M3dimMerge
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimMerge"
---

# M3dimMerge

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

> Merge multiple point clouds or depth maps into a single point cloud or depth map.

## Syntax

```c
void M3dimMerge(
    const AIL_ID * SrcContainerBufIdArrayPtr,  //in
    AIL_ID         DstContainerBufId,          //out
    AIL_INT        NumContainers,              //in
    AIL_ID         Context3dimId,              //in
    AIL_INT64      ControlFlag                 //in
)
```

## Description

This function merges all the specified point clouds or depth maps into a single point cloud or depth map. Merged point clouds will either be unorganized or organized in a 3D-processable point cloud container, whereas merged depth maps will always be organized in a 3D-processable depth map container.

By default, each depth map will be stitched in fast mode ([`M_FAST`](../../Reference/3dim/M3dimControl.md)) to the first depth map provided in the array. You can specify to stitch to a different depth map, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_STATIC_INDEX`](../../Reference/3dim/M3dimControl.md). When [`M_STITCH_MODE`](../../Reference/3dim/M3dimControl.md) is set to [`M_FAST`](../../Reference/3dim/M3dimControl.md), the depth maps are combined quickly but without interpolation, risking an inaccurate stitch. If the depth map pixels are not perfectly aligned with the chosen depth map (for example, the world X-coordinate is 1 for the first pixel in the depth map that you specify with [`M_STATIC_INDEX`](../../Reference/3dim/M3dimControl.md), but the world X-coordinate is 3.2 for the first pixel in a depth map to be stitched, and they both have a pixel width of 1 world unit), the stitch will not be accurate in fast mode because its pixels will be forced to shift to align with the pixels of the chosen depth map in the destination. Also, there will be no gray level correction to represent the new position. For example, the intensity value of the destination pixel at (3,0) in the world will be the intensity value of the source pixel at (3.2,0) in the world. You can set [`M_STITCH_MODE`](../../Reference/3dim/M3dimControl.md) to [`M_PRECISE`](../../Reference/3dim/M3dimControl.md) so that the destination depth map pixels are assigned interpolated gray levels. In the above example, Aurora Imaging Library will attempt to calculate an intensity value for the destination pixel at (3,0) in the world by taking a weighted average of the four source pixels nearest that point. Note that stitching depth maps in precise mode is slower, but more accurate.

If 2 depth maps overlap in X and/or Y, the pixel value in the resulting depth map will be the average value of the overlapping pixels, by default. You can specify to use the minimum or maximum value instead, using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_OVERLAP_MODE`](../../Reference/3dim/M3dimControl.md).

Optionally, to reduce the number of points in the resulting point cloud, you can specify a subsample 3D image processing context, to apply a subsampling operation to the merged point cloud.

Note that if you specify a stitch or subsample 3D image processing context, the stitch or subsample control settings specified using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) are applied.

> **Note:** Note that after the merge operation, a resulting point cloud is unorganized, unless a stitch context is specified or the optional subsample context specifies an organized resulting point cloud (that is, [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) is set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md)). If the resulting merged point cloud is unorganized, you can still create an organized point cloud from the data. To do so, pass the unorganized point cloud to [`M3dimSample`](../../Reference/3dim/M3dimSample.md) along with a subsample 3D image processing context configured using [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) set to [`M_SUBSAMPLE_GRID`](../../Reference/3dim/M3dimControl.md) and [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md) set to [`M_ORGANIZED`](../../Reference/3dim/M3dimControl.md). Note that in this case, you cannot set [`M_SUBSAMPLE_MODE`](../../Reference/3dim/M3dimControl.md) to [`M_SUBSAMPLE_DECIMATE`](../../Reference/3dim/M3dimControl.md), since this mode is supported only when the source point cloud is organized.

## Parameters

### `SrcContainerBufIdArrayPtr` *(in, *AIL_ID)*

Specifies the address of the array that holds the identifiers of the point cloud containers, depth map containers, or depth map image buffers to merge. You can set some entries in the array to [`M_NULL`](../../Reference/3dim/M3dimMerge.md) to exclude them from the merge operation.

*For specifying the array of containers or image buffers*

| Value | Description |
| --- | --- |
| `Array of depth map container IDs` | Specifies an array containing the identifiers of depth map containers.

The depth map containers must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)). |
| `Array of depth map image buffer IDs` | Specifies an array containing the identifiers of depth map image buffers.

The image buffers must be 8-bit, 16-bit, or 32-bit unsigned, 1-band buffers, and must contain a fully corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). |
| `Array of point cloud container IDs` | Specifies an array containing the identifiers of point cloud containers.

The point cloud containers must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). |

### `DstContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container in which to store the resulting point cloud or depth map. The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), and must not be a child container.

### `NumContainers` *(in, AIL_INT)*

Specifies the size of the array passed to [`SrcContainerBufIdArrayPtr`](../../Reference/3dim/M3dimMerge.md).

### `Context3dimId` *(in, AIL_ID)*

Specifies the stitch or subsample 3D image processing context identifier.

*For specifying the stitch or subsample 3D image processing context identifier*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter.

Note that the resulting merged point cloud will be unorganized.

> **Note:** This value is only available when merging point clouds. |
| `M_STITCH_CONTEXT_DEFAULT` | Specifies a predefined stitch 3D image processing context with the default settings.

Note that the resulting merged point cloud or depth map will be organized.

> **Note:** This value is only available when merging organized point clouds or depth maps. |
| `Stitch 3D image processing context identifier` | Specifies the identifier of a stitch 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_STITCH_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

Note that [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_STATIC_INDEX`](../../Reference/3dim/M3dimInquire.md) must be set to a value less than the [`NumContainers`](../../Reference/3dim/M3dimMerge.md) parameter.

Note that the resulting merged point cloud or depth map will be organized.

> **Note:** This value is only available when merging organized point clouds or depth maps. |
| `Subsample 3D image processing context identifier` | Specifies the identifier of a subsample 3D image processing context, previously allocated using [`M3dimAlloc`](../../Reference/3dim/M3dimAlloc.md) with [`M_SUBSAMPLE_CONTEXT`](../../Reference/3dim/M3dimAlloc.md).

Note that the organization of the resulting merged point clouds depends on the value of [`M3dimControl`](../../Reference/3dim/M3dimControl.md) with [`M_ORGANIZATION_TYPE`](../../Reference/3dim/M3dimControl.md).

> **Note:** This value is only available when merging point clouds. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies which components will be affected by the operation. Note that when merging depth map image buffers, this parameter is reserved for future expansion and must be set to [`M_DEFAULT`](../../Reference/3dim/M3dimMerge.md).

*For specifying whether to merge points from all components*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the operation will only affect the core components of a 3D-processable point cloud or depth map container. These core components are listed in the description of [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md) or [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md).

All other components are copied to the destination container unmodified. |
| `M_APPLY_TO_ALL_COMPONENTS` | Specifies that the operation will affect the core components of a 3D-processable point cloud container, as well as any other component (including custom components) that has the same dimension as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md). These components must not have a region of interest (ROI) associated with them. For example, using a component that is an image buffer with an ROI will cause an error.

> **Note:** Note that if a custom component has an associated calibration context, the component's information is merged, but its calibration context is discarded.

To avoid ambiguity, within each source point cloud container, each component must be unique such that its data type and number of bands are not identical to any other component of the same type in the container.

[`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dim/M3dimMerge.md) does not support 1-bit buffers.

If the resulting operation yields an empty container with 0 valid points, the components with the same dimensions are not created and will not exist in the destination container.

> **Note:** This value is only available when merging point clouds. |
