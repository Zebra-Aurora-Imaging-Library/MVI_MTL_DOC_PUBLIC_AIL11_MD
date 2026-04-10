---
doctype: Reference
module: 3dim
function: M3dimProject
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimProject"
---

# M3dimProject

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

> Project a point cloud or 3D geometry onto the XY (Z=0) plane to generate a fully corrected depth map.

## Syntax

```c
void M3dimProject(
    AIL_ID    SrcContainerBufOrGeometry3dgeoId,  //in
    AIL_ID    DepthMapImageBufId,                //out
    AIL_ID    IntensityMapImageBufId,            //out
    AIL_INT64 ProjectionMode,                    //in
    AIL_INT64 OverlapMode,                       //in
    AIL_INT64 Options,                           //in
    AIL_INT64 ControlFlag                        //in
)
```

## Description

This function projects a point cloud or a 3D geometry onto the XY (Z=0) plane, generating a fully corrected depth map. A depth map pixel's gray level indicates depth. [`M3dimProject`](../../Reference/3dim/M3dimProject.md) can also generate an intensity map image from the source container, if an appropriate component is available ([`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) and/or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md)). An intensity map pixel's gray level indicates the luminosity of the original scene at the location represented by the corresponding pixel in the depth map.

[`M3dimProject`](../../Reference/3dim/M3dimProject.md) projects the points of the point cloud on the XY (Z=0) plane of its working coordinate system. To change the resulting depth map, use [`M3dimRotate`](../../Reference/3dim/M3dimRotate.md) or [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) to rotate or transform (respectively) the 3D points before calling [`M3dimProject`](../../Reference/3dim/M3dimProject.md).

To define the region of the point cloud or 3D geometry to project, the destination buffer's size and camera calibration information are used to establish a bounding box. You can use [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) to establish the required destination depth map buffer size in X and Y, and [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md) to calibrate the buffer. Note that [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) recommends a size such that most points map to their own pixel with the greatest ratio of valid pixels to invalid pixels.

If you want multiple points to map to the same pixel, you can use [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md) to establish the required destination depth map buffer size in X and Y ([`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_NUMBER_OF_CELLS_...`](../../Reference/3dim/M3dimGetResult.md)), and, if the destination is a depth map image buffer, [`McalUniform`](../../Reference/cal/McalUniform.md) to manually calibrate the image buffer. If the destination is a depth map container, use [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md) to specify the required offsets and scales to produce natively calibrated coordinates. Use [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md) with [`M_START_POINT_...`](../../Reference/3dim/M3dimGetResult.md) and [`M_CELL_SIZE_...`](../../Reference/3dim/M3dimGetResult.md) to specify the translation and scale of the transformation between pixel and world units. For more information, see [Steps to create a depth map](../../UserGuide/C35_3D_Image_processing/S15_Generating_a_fully_corrected_depth_map.md).

If you want to limit which points are considered for the projection, prior to calling [`M3dimProject`](../../Reference/3dim/M3dimProject.md), you can crop or mask the point cloud (using [`M3dimCrop`](../../Reference/3dim/M3dimCrop.md) or changing the data in the source container's confidence component). For more information, see [Regions of interest](../../UserGuide/C02_Building_an_application/S12_Working_with_3D.md).

Note that, when generating an intensity map, the source container's reflectance or intensity component provides the color or intensity for pixels in the destination image. If both components exist, only the reflectance component is considered. You cannot generate an intensity map when projecting a 3D geometry.

## Parameters

### `SrcContainerBufOrGeometry3dgeoId` *(in, AIL_ID)*

Specifies a source container or 3D geometry object.

*For specifying the source container or 3D geometry object identifier*

| Value | Description |
| --- | --- |
| `Source 3D geometry object identifier` | Specifies the identifier of a 3D geometry object, except that of a box geometry object. The 3D geometry object must have been previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with[`M_GEOMETRY`](../../Reference/3dgeo/M3dgeoAlloc.md), and must have been successfully defined. Supported 3D geometries include cylinder, line, plane, point, and sphere. |
| `Source container identifier` | Specifies the identifier of the source container.

The container must be 3D-processable (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md)). The container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md). |

### `DepthMapImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination depth map container or depth map image buffer, previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) or [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). To establish the required destination depth map buffer size in X and Y, use [`M3dimCalculateMapSize`](../../Reference/3dim/M3dimCalculateMapSize.md) or [`M3dimLattice`](../../Reference/3dim/M3dimLattice.md).

*For specifying the depth map*

| Value | Description |
| --- | --- |
| `Destination depth map container identifier` | Specifies the identifier of the destination depth map container.

The destination container must have been previously allocated using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md) with [`M_PROC`](../../Reference/buf/MbufAllocContainer.md), must store data in a 3D-processable depth map format (that is, if you call [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md), the function returns [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md)), and must not be a child container. See [Steps to create a depth map](../../UserGuide/C35_3D_Image_processing/S15_Generating_a_fully_corrected_depth_map.md) to set up such a container. |
| `Destination depth map image buffer identifier` | Specifies the identifier of the destination depth map image buffer.

The destination image buffer must be fully corrected (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)). This means that the buffer must be associated with a calibration context and have a constant pixel size in X and Y; it must also have a valid Z-scale. To establish the required calibration, use [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md), or use [`McalUniform`](../../Reference/cal/McalUniform.md) to calibrate the buffer and [`McalControl`](../../Reference/cal/McalControl.md) with [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) to set the Z-scale. Note that, depending on the Z-scale and offset of the destination image buffer, a point's Z-value can place it outside the buffer's range of possible values. By default, the point is ignored, but saturation to either 0 or the buffer's maximum value (minus 1) is possible with the [`Options`](../../Reference/3dim/M3dimProject.md) parameter set to [`M_SATURATION`](../../Reference/3dim/M3dimProject.md).

The destination image buffer must be a 1-band, 8-bit, 16-bit or 32-bit unsigned buffer. |

### `IntensityMapImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination intensity map image buffer. The image buffer must have the same dimensions as the buffer specified for [`DepthMapImageBufId`](../../Reference/3dim/M3dimProject.md). In addition, the image buffer must be an 8-bit or 16-bit unsigned, 1-band or 3-band buffer. The number of bands must be equal to that of the component [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md). If the container has neither [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md) nor [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md) components, or if projecting a 3D geometry, this parameter must be set to `M_NULL`.

### `ProjectionMode` *(in, AIL_INT64)*

Specifies the projection mode.

*For specifying the projection mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MESH_BASED` | Specifies to consider the source container's mesh component when extracting 3D points for projection into the depth map. For a meshed point cloud, each triangular face has 3 vertices, and a vertex's Z-value determines the gray level of its corresponding pixel in the depth map. The remaining pixels (in the depth map) are assigned interpolated gray levels, since they correspond to the triangular faces between the vertices (in the point cloud's mesh). The container must have an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component before using the [`M_MESH_BASED`](../../Reference/3dim/M3dimProject.md) mode. |
| `M_POINT_BASED` *(default)* | Specifies to project the 3D points directly into the depth map. |

### `OverlapMode` *(in, AIL_INT64)*

Specifies the overlap mode, which determines a pixel's gray level when multiple points correspond to the same destination depth map pixel.

*For specifying the overlap mode*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AVERAGE` | Specifies to use the average gray level of all the 3D points projected on a single depth map pixel. |
| `M_MAX_INTENSITY` | Specifies to use the gray level of the 3D point with the highest intensity. The source container must have an intensity or reflectance component.

> **Note:** Note that [`M_MAX_INTENSITY`](../../Reference/3dim/M3dimProject.md) is not available when the source object is a 3D geometry. |
| `M_MAX_Z` *(default)* | Specifies to use the gray level of the 3D point with the largest Z-value, of all the 3D points projected on a single depth map pixel. Note that this value can be bright or dark, depending on whether the Z-scale ([`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md)) is positive or negative. If you calibrated the buffer using [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md), the sign of the Z-scale depends on whether [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md) with [`ZSign`](../../Reference/3dim/M3dimCalibrateDepthMap.md) was set to [`M_POSITIVE`](../../Reference/3dim/M3dimCalibrateDepthMap.md) or [`M_NEGATIVE`](../../Reference/3dim/M3dimCalibrateDepthMap.md). |
| `M_MIN_Z` | Specifies to use the gray level of the 3D point with the smallest Z-value, of all the 3D points projected on a single depth map pixel. Note that this value can be dark or bright, depending on whether the Z-scale ([`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md)) is positive or negative. If you calibrated the buffer using [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md), the sign of the Z-scale depends on whether [`M3dimCalibrateDepthMap`](../../Reference/3dim/M3dimCalibrateDepthMap.md) with [`ZSign`](../../Reference/3dim/M3dimCalibrateDepthMap.md) was set to [`M_POSITIVE`](../../Reference/3dim/M3dimCalibrateDepthMap.md) or [`M_NEGATIVE`](../../Reference/3dim/M3dimCalibrateDepthMap.md). |
| `M_OVERWRITE` | Specifies to use the last gray level of all the 3D points projected on a single depth map pixel. This mode continually overwrites the previous gray value for the pixel. [`M_OVERWRITE`](../../Reference/3dim/M3dimProject.md) can be marginally faster than the other overlap modes. |

### `Options` *(in, AIL_INT64)*

Specifies additional options.

*For specifying the option*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no saturation; points falling outside the depth map image buffer's grayscale range are ignored. |
| `M_ACCUMULATE` | Specifies not to clear the previous contents of the depth map image buffer prior to projection. |
| `M_SATURATION` | Specifies saturation. Note that saturation can occur, depending on the Z-scale and offset of the destination depth map buffer. If, for a given pixel of the destination buffer, the corresponding gray level of the 3D geometry or point of the point cloud falls outside the buffer's grayscale range, the pixel will be saturated to either 0 or the buffer's maximum value, minus 1 (the maximum value is reserved to represent missing data). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
