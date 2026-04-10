---
doctype: Reference
module: buf
function: MbufConvert3d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufConvert3d"
---

# MbufConvert3d

> Converts 3D data to a format that is 3D-processable and/or 3D-displayable.

## Syntax

```c
void MbufConvert3d(
    AIL_ID    SrcContainerOrImageBufId,  //in
    AIL_ID    DstContainerOrImageBufId,  //out
    AIL_ID    ExternalYArrayBufId,       //in
    AIL_INT64 DstOptions,                //in
    AIL_INT64 ControlFlag                //in
)
```

## Description

This function converts the 3D data in the specified source container or fully-corrected depth map image buffer, and stores it in the specified destination container or image buffer. By default, if the destination is a container, the source data is converted to a point cloud format that is 3D-processable and/or 3D-displayable (depending on whether the destination container has the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)or [`M_PROC`](../../Reference/buf/MbufAllocContainer.md) attribute). You can set the [`DstOptions`](../../Reference/buf/MbufConvert3d.md) parameter to [`M_DEPTH_MAP_CONTAINER`](../../Reference/buf/MbufConvert3d.md) to convert the source data to a 3D-processable depth map format. If the destination is an image buffer and the range component of the source container stores a depth map, the source is converted to a fully corrected depth map.

> **Note:** You can only use an image buffer as a destination if the source is a container with a single range component, and the component has its[`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md) set to [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md).

If the source and destination are both containers, all components of the source container that do not affect whether the container is 3D-processable or 3D-displayable will be copied to the destination.

In some cases, if you have container that stores incomplete or ambiguous 3D data, you can still convert it by specifying the [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md)flag. When [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md)is specified, Aurora Imaging Library will try to convert the data by making assumptions about the missing or ambiguous information. For example, if the source container has a range component with the[`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md) set to [`M_UNCALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md), it will instead be treated as though it were[`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md). Typically, compensation should only be used for display purposes, because it can result in incorrectly calibrated 3D data in the destination container or image buffer.

If the source container has a range or disparity component with an [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquire.md) setting that specifies an external Y array, you must either set [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md)to the Aurora Imaging Library identifier of a buffer with an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute, or use [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md). Typically, this is only used when the 3D sensor, used to generate the range or disparity component, stores the position of a rotary/linear encoder to identify the position of the conveyor belt when scanning each line. In most cases, these 3D sensors transmit this information as GenICam chunk data, in which case you will need to manually extract the values immediately after the 3D data is grabbed. You can retrieve this data individually for each line using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) with the feature name "ChunkScanLineSelector" and[`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) with the feature name "ChunkEncoderValue".

> **Note:** To convert the rotary encoder position values retrieved from the 3D sensor to calibrated Y-coordinates, you will need to set [`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md)of the range or disparity component to a factor based on the displacement of the conveyor belt for each increment of the encoder. Your 3D sensor might be configured to set this value automatically during acquisition.

## Parameters

### `SrcContainerOrImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source container or fully corrected depth map image buffer to convert.

*For specifying the source container or fully corrected depth map image buffer*

| Value | Description |
| --- | --- |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of a fully corrected depth map image buffer.

The image buffer must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must store a fully-corrected depth map (that is, if you call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md), the function returns [`M_TRUE`](../../Reference/cal/McalInquire.md)).

You can mark invalid data in the image buffer by associating the buffer with a region of interest (ROI) using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). By default, each pixel outside the ROI will be given a confidence value of 0 in the destination container. If the destination is a 3D-processable depth map container, you can use [`M_PROPAGATE_REGION`](../../Reference/buf/MbufConvert3d.md) to copy the ROI to the [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component of the container instead. The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)). |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of a container to convert.

To be convertible, the source container must have exactly one component with either the component type [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md) or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufAllocComponent.md); the container can have other components, but it must not have more than one range or disparity component (if necessary, you can use a child container, created using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), to meet this restriction). If the container has a range component, it must have[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufControlContainer.md) set to[`M_CARTESIAN`](../../Reference/buf/MbufControlContainer.md). The range or disparity component must also meet certain requirements, depending upon its [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquire.md)setting:

| 3D representation | Requirements |
| --- | --- |
|  |
| [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControlContainer.md) | A 3-band range component. |
| [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md) | A 3-band range component with an [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md) of 1. |
| [`M_CALIBRATED_XZ_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | A 3-band range component, with Y values provided by passing an Aurora Imaging Library array buffer to [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md). The Y-axis band of the range component will be ignored. |
| [`M_CALIBRATED_XZ_UNIFORM_Y`](../../Reference/buf/MbufControl.md) | A 3-band range component, with Y values identified by multiplying the row index of each point by the range component's [`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md) setting. The Y-axis band of the range component will be ignored. |
| [`M_CALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component. The [`ControlFlag`](../../Reference/buf/MbufConvert3d.md) parameter must be set to [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md). |
| [`M_CALIBRATED_Z_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component, with Y values provided by passing an Aurora Imaging Library array buffer to [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md). The [`ControlFlag`](../../Reference/buf/MbufConvert3d.md) parameter must be set to [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md). |
| [`M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component, with X values identified by multiplying the column index of each point by the range component's [`M_3D_SCALE_X`](../../Reference/buf/MbufControlContainer.md) setting and Y values provided by passing an Aurora Imaging Library array buffer to [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md). |
| [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component, with X and Y values identified by multiplying the column and row index of each point by the range component's [`M_3D_SCALE_X`](../../Reference/buf/MbufControlContainer.md)and[`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md) settings, respectively. |
| [`M_UNCALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component. The [`ControlFlag`](../../Reference/buf/MbufConvert3d.md) parameter must be set to [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md). |
| [`M_DISPARITY`](../../Reference/buf/MbufControlContainer.md) | A 1-band disparity component. > **Note:** Typically, this setting should only be used with disparity components that have been generated using area scan stereoscopic cameras. |
| [`M_DISPARITY_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | A 1-band disparity component, with Y values provided by passing an Aurora Imaging Library array buffer to [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md). > **Note:** Typically, this setting should only be used with disparity components that have been generated using line scan stereoscopic cameras. |
| [`M_DISPARITY_UNIFORM_Y`](../../Reference/buf/MbufControlContainer.md) | A 1-band disparity component, with Y values identified by multiplying the row index of each pixel by the disparity component's [`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md) setting. > **Note:** Typically, this setting should only be used with disparity components that have been generated using line scan stereoscopic cameras. |
| [`M_PROJECTED_Z`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component, with X and Y values identified using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the column and row index for each corresponding pixel or the A and B coordinate maps provided by the camera. > **Note:** Typically, this setting should only be used with projective maps generated using area scan cameras. |
| [`M_PROJECTED_Z_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md) | A 1-band range component, with Y values provided by passing an Aurora Imaging Library array buffer to [`ExternalYArrayBufId`](../../Reference/buf/MbufConvert3d.md), and with X values identified using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and the column index for each corresponding pixel or the A coordinate map provided by the camera. > **Note:** Typically, this setting should only be used with projective maps generated using line scan cameras. |

This function uses the 3D settings of the range or disparity component during conversion. For example, all Z-coordinates in the source container's range or disparity component will be offset by the value of [`M_3D_OFFSET_Z`](../../Reference/buf/MbufControlContainer.md)in the destination container or depth map image buffer. In the destination container, the range component will then have the[`M_3D_OFFSET_Z`](../../Reference/buf/MbufControlContainer.md) setting reset to the default value of 0.

If your 3D sensor transmits these 3D settings, they will be set automatically during acquisition. If not, you must modify the 3D settings of components directly using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md) with settings from the table [`Component3DSettings`](../../Reference/buf/MbufControlContainer.md).

> **Note:** Note that the [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufControlContainer.md) setting of the range or disparity component will be copied to the range component of the destination, but will not be used during conversion.

The source container can have an [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component that stores an ROI. By default, each point outside the ROI will be set to the invalid value in the destination image buffer or given a confidence value of 0 in the destination container. You can use [`M_PROPAGATE_REGION`](../../Reference/buf/MbufConvert3d.md) to copy the ROI to an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI in the destination image buffer or the [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component of the destination container instead. |

### `DstContainerOrImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination container or image buffer in which to store the converted data.

*For specifying the destination container or image buffer*

| Value | Description |
| --- | --- |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of a 1-band, 8-bit, 16-bit, or 32-bit unsigned image buffer. In this case, the source must be a container with a range component that stores a depth map. The depth map in the source container will be converted to a fully corrected depth map and stored in the destination image buffer. The image buffer must have been allocated with the same[`SizeX`](../../Reference/buf/MbufAlloc2d.md)and [`SizeY`](../../Reference/buf/MbufAlloc2d.md)settings as the range component of the source container.

This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error.

> **Note:** You can only use an image buffer as a destination if the source is a container with a single range component and the component has its[`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md) setting set to [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md). If [`ControlFlag`](../../Reference/buf/MbufConvert3d.md)is set to[`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md), [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquire.md)can also be set to any 3D representation setting that can be interpreted as a depth map. |
| `Container identifier` | Specifies the Aurora Imaging Library identifier of a container that is not a child container, and has an [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)and/or[`M_PROC`](../../Reference/buf/MbufAllocContainer.md) attribute.

To make the destination container 3D-processable and/or 3D-displayable, existing components in the destination container with the following component types might be freed, reallocated, or filled with new data:

- [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md).
- [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufAllocComponent.md).

> **Note:** If [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)reallocates existing components in the destination container, their Aurora Imaging Library identifiers will change. You can determine the current Aurora Imaging Library identifier of a component with a specific component type, using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md). |

### `ExternalYArrayBufId` *(in, AIL_ID)*

Specifies the identifier of a 1-band, 1D [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer containing the Y-axis value for each row of coordinates stored in the range or disparity component of the source container. The array buffer must store one value per row in the range or disparity component of the source container.

### `DstOptions` *(in, AIL_INT64)*

Specifies which operations to apply to the data during conversion.

*For specifying operations to apply to the data*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to convert the data without performing extra operations. |
| `M_DEPTH_MAP_CONTAINER` | Specifies to set the destination container to be a 3D-processable depth map container. |
| `M_NO_REMAP_DEPTH_MAP` | Specifies to mark all pixels with the maximum value in the source depth map as invalid during conversion. This can significantly speed up the [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) operation if pixels with the maximum value are not important. For example, if the source and destination depth maps are both 8-bit buffers, and the source has valid pixels at the maximum value (255), the pixels will be treated as invalid in the destination.

> **Note:** Note that when the source's depth is greater than that of the destination (for example, a 16-bit source and an 8-bit destination), this setting is ignored and a remap will occur. |
| `M_REMOVE_NON_FINITE` | Specifies to mark all non-finite 3D data in the source container as invalid during conversion. Non-finite 3D data is that which has a value of infinity, negative infinity, or NaN in the range or disparity component. This setting is only available when the source is a container.

If the destination is a container, all non-finite points are given a confidence value of 0. If the destination is an image buffer, all non-finite points are set to the maximum possible finite value, which marks them as missing or invalid. |

*For specifying whether to propagate the ROI*

| Value | Description |
| --- | --- |
| `M_PROPAGATE_REGION` | Specifies to propagate the ROI of the source into an [`M_RASTER`](../../Reference/buf/MbufInquire.md) ROI in the destination depth map image buffer or [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufAllocComponent.md) component in the destination depth map container.

> **Note:** Note that [`M_PROPAGATE_REGION`](../../Reference/buf/MbufConvert3d.md) is ignored if there is no ROI in the source. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to generate an error when there is missing or ambiguous information in the source, or to compensate by making assumptions about the missing information.

*For specifying the control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to generate an error if conversion requires information that is missing or ambiguous in the source. |
| `M_COMPENSATE` | Specifies to compensate for missing or ambiguous information in the source. The following assumptions are used when compensating:

- The coordinates in the source depth map or point cloud are uniformly spaced for any missing axes. For example, a range component with [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquire.md) set to[`M_CALIBRATED_Z_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md)will be treated as though it were set to [`M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y`](../../Reference/buf/MbufControlContainer.md).
- If the source is a container with no confidence information, all coordinates in the source are considered valid.
- If the source is a container, all coordinates in the source are considered natively calibrated.
- If [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)of the range component is larger than 1, then the point cloud in the container is considered organized.
- Any components in the source container that have the same component type are not copied.
- Any confidence, intensity, reflectance, mesh, or normals component in the source container that is not correctly formatted should be ignored. |
