---
doctype: UserGuide
part: "3D related information"
chapter: 3D_Containers
section: Preparing_a_container_for_display_or_processing
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / 3D_Containers / Preparing a container for display or processing"
---

# Preparing a container for display or processing

Your 3D sensor might transmit 3D information in a format that is not natively displayable or processable by Aurora Imaging Library. For example, Aurora Imaging Library can only process containers that have a range component with a particular structure, but your 3D sensor might be a stereoscopic camera that transmits a disparity component. You can use [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) to convert grabbed 3D data to a 3D-processable point cloud or depth map container and/or a 3D-displayable container. This function also applies some modifications to the coordinates transmitted by your 3D sensor, such as scaling or offsetting all coordinates by a specified value to convert the 3D data to natively calibrated coordinates.

Typically, if your 3D sensor is compliant (transmits data suitable for grabbing into a container), you do not need to modify grabbed data before making it a 3D-processable point cloud or depth map container and/or a 3D-displayable container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

If your 3D sensor transmits several sets of 3D components, you might need to create a child container that contains only a single set of 3D components before converting the data (for more information, see [Child containers](S05_Child_containers.md)). If your 3D sensor does not transmit required 3D settings for the range or disparity component, you must set those before converting the data.

## 3D settings for the range or disparity component

When you pass a 3D container as a source to[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), 3D settings in the range or disparity component are applied to the 3D data. For example, the X-coordinates of all points in the destination point cloud container are offset by the value specified by the[`M_3D_OFFSET_X`](../../Reference/buf/MbufControlContainer.md)setting. Additionally, some settings (such as [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)) must be set to convert the data. The 3D settings are listed in[`Component3DSettings`](../../Reference/buf/MbufControlContainer.md).

If the[`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)setting of your range or disparity component indicates a uniform distribution of X and/or Y values (for example, [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md)), you should specify [`M_3D_SCALE_X`](../../Reference/buf/MbufInquireContainer.md) and/or[`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md)to produce natively calibrated coordinates.

> **Note:** Scale settings are not exclusively used with uniform 3D representations; coordinates are always scaled using these settings during conversion, regardless of how those coordinates are stored.

If you are grabbing 3D data from a 3D profile sensor or linescan device that is not perpendicular to the conveyor belt, you can compensate for the angle using the [`M_3D_SHEAR_X`](../../Reference/buf/MbufInquireContainer.md)and [`M_3D_SHEAR_Z`](../../Reference/buf/MbufInquireContainer.md)settings. For example, if your conveyor belt slopes downward relative to your 3D sensor, you can specify an [`M_3D_SHEAR_Z`](../../Reference/buf/MbufInquireContainer.md)to offset the Z values in each slice from those in the previous slice by a set amount. Note that the resulting point cloud will be slanted; for more information, see [Pitch](../C42_Grabbing_from_3D_sensors/S07_Steps_to_calibrate_3D_profile_sensor.md).

For a disparity component, the disparity-specific 3D settings must be set (using [`MbufControlContainer`](../../Reference/buf/MbufControlContainer.md)with[`M_3D_DISPARITY_...`](../../Reference/buf/MbufControlContainer.md)). Refer to your camera manual for the correct values.

> **Note:** Note that [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)does not perform unit conversions; the source value for[`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufControlContainer.md) is always propagated to the destination without changes to the underlying data.

## Compensating for missing or ambiguous information

In some cases, you might grab 3D data that is missing required information. For example, you might grab a range component with [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)set to[`M_CALIBRATED_Z`](../../Reference/buf/MbufControlContainer.md). This 3D representation means that the underlying data has no specified X or Y-coordinates. By default, you cannot convert this container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)because the container is missing information. You can set the [`ControlFlag`](../../Reference/buf/MbufConvert3d.md)parameter to[`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md)flag to specify that you want Aurora Imaging Library to convert the data by making assumptions. In the example above, the range component will be treated as though it has the 3D representation [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md).

You can use [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md) to compensate for the following ambiguities in the source container:

- The [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)of the range or disparity component indicates that the coordinates for at least one axis are unspecified.
- There is no confidence information. Confidence information can be stored in a confidence component, or in the Z-axis values of the range or disparity component when [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControlContainer.md)is set to [`M_TRUE`](../../Reference/buf/MbufControlContainer.md).
- There is more than one confidence, intensity/reflectance, mesh, or normals component.
  > **Note:** Note that you cannot compensate for having a container with more than one range or disparity component. You can free the extraneous components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md), or allocate a child container to filter out the extraneous components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md).
- At least one component is incorrectly formatted. To learn the correct formatting for each component, see [Layout of data in a component](../C42_Grabbing_from_3D_sensors/S05_Working_with_noncompliant_cameras.md).

## Validating and fixing a container

For successful processing and analysis of a point cloud container, some of its components must meet specific conditions. You can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) to validate whether a container satisfies certain conditions and optionally fix them if they are not met. If you pass a destination container to the function, you can obtain a modified copy of the source container that satisfies the specified conditions.

Available conditions to verify and fix include:

- That each edge in a mesh is shared between at most two triangular faces, and that no triangular face is degenerate ([`M_MESH_VALID_NEIGHBORS`](../../Reference/3dim/M3dimFix.md)).
- That each point in a mesh is valid ([`M_MESH_VALID_POINTS`](../../Reference/3dim/M3dimFix.md)); valid points have a non-zero confidence score.
- That each point with a non-zero confidence has a normal vector value of exactly one or zero ([`M_NORMALS_NORMALIZED`](../../Reference/3dim/M3dimFix.md)).
- That each point has a finite normal vector ([`M_NORMALS_FINITE`](../../Reference/3dim/M3dimFix.md)).
- That each point in a range component is finite ([`M_RANGE_FINITE`](../../Reference/3dim/M3dimFix.md)).
- That each point with a non-zero confidence is unreplicated ([`M_UNREPLICATED_POINTS`](../../Reference/3dim/M3dimFix.md)).

> **Note:** Note that, depending on your 3D sensor software, infinite values are sometimes assigned to invalid points. For example, an infinite value might be assigned to an invalid point's Z-coordinate in its range component, or to its normal vector in the normals component. Such values can cause errors with other 3D image processing functions. You can use [`M3dimFix`](../../Reference/3dim/M3dimFix.md) to replace infinite values with the value used to indicate missing data ([`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControl.md)).

Note that you can combine values to validate multiple conditions in a single call to [`M3dimFix`](../../Reference/3dim/M3dimFix.md), or you can specify to verify all conditions ([`M_ALL`](../../Reference/3dim/M3dimFix.md)).

> **Note:** Any specified condition is available only if the related component is present in the source container.

## Requirements for a 3D-processable point cloud container

For a point cloud container to be 3D-processable, it must have the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)in addition to one (at most) of each of the following components:

| Component type | Required | Bands | Data type | Other Settings |
| --- | --- | --- | --- | --- |
| [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md) | Yes | 3 | Floating point | [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)set to[`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControlContainer.md) or [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md). All 3D settings (from the table [`Component3DSettings`](../../Reference/buf/MbufControlContainer.md)) set to their default, except for [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufControlContainer.md) which can have any setting. |
| [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufControlContainer.md) | Yes | 1 | 8-bit unsigned | The same[`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range component. |
| [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufControlContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufControlContainer.md) (not both) | No | 1 or 3 | 8-bit unsigned or 16-bit unsigned | The same[`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range component. |
| [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 3 | Floating point | The same[`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)as the range component. |
| [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 1 | 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) | If the range component has [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)set to[`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControlContainer.md),[`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)must be 6. If the range component has [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md)set to[`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md),[`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)must be 3 or 6. |

> **Note:** By default, any components with a component type not included in the table above will be propagated, but not used, by 3D processing functions and [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Some functions (such as [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md)) allow you to apply the operation to all components, including custom components, if you specify [`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dim/M3dimMerge.md). The components must have the same dimensions as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md); otherwise, they will be copied to the destination container unmodified.

You can determine whether a point cloud container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE`](../../Reference/buf/MbufInquireContainer.md).

## Requirements for a 3D-processable depth map container

For a depth map container to be 3D-processable, it must have the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)in addition to one (at most) of each of the following components:

| Component type | Required | Bands | Data type | Other Settings |
| --- | --- | --- | --- | --- |
| [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md) | Yes | 1 | 8-bit unsigned, 16-bit unsigned, or 32-bit unsigned | [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md) set to[`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufControlContainer.md) and [`M_3D_SCALE_X`](../../Reference/buf/MbufControlContainer.md) and [`M_3D_SCALE_Y`](../../Reference/buf/MbufControlContainer.md) must be positive. [`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufControlContainer.md), [`M_3D_SHEAR_X`](../../Reference/buf/MbufControlContainer.md), and [`M_3D_SHEAR_Z`](../../Reference/buf/MbufControlContainer.md) are set to their respective default value. |
| [`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufControlContainer.md) | Depends | 1 | 8-bit unsigned | If [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControlContainer.md) is set to [`M_TRUE`](../../Reference/buf/MbufControlContainer.md), the depth map container cannot contain a confidence component. [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControlContainer.md) must be set to the maximum value of the buffer. If [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControlContainer.md) is set to [`M_FALSE`](../../Reference/buf/MbufControlContainer.md), a single confidence component with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component is required. |
| [`M_COMPONENT_MATRIX_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 1 | 32-bit float 4x4 [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) | N/A |
| [`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufControlContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufControlContainer.md) (not both) | No | 1 or 3 | 8-bit unsigned or 16-bit unsigned | The same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. |
| [`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 3 | 32-bit float | The same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. |
| [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 1 | 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) | [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)must be 6. |
| [`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufControlContainer.md) | No | 1 | 8-bit unsigned | The same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. |

> **Note:** By default, any components with a component type not included in the table above will be propagated, but not used, by 3D processing functions and [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Some functions (such as [`M3dimMerge`](../../Reference/3dim/M3dimMerge.md)) allow you to apply the operation to all components, including custom components, if you specify [`M_APPLY_TO_ALL_COMPONENTS`](../../Reference/3dim/M3dimMerge.md). The components must have the same dimensions as [`M_COMPONENT_RANGE`](../../Reference/buf/MbufAllocComponent.md); otherwise, they will be copied to the destination container unmodified.

You can determine whether a depth map container is 3D-processable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_PROCESSABLE_DEPTH_MAP`](../../Reference/buf/MbufInquireContainer.md).

## Requirements for a 3D-displayable container

Any container with the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md) attribute that can be passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) (with [`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md)) is 3D-displayable. A conversion will occur each time the container is modified, unless you convert the container to a format that is natively 3D-displayable. Automatic conversion is primarily useful for displaying 3D data as it is being grabbed.

The requirements for a container to be natively 3D-displayable are the same as the requirements for a point cloud container to be 3D-processable, except that the [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)attribute is required.

You can determine whether a container is 3D-displayable using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_3D_DISPLAYABLE`](../../Reference/buf/MbufInquireContainer.md).
