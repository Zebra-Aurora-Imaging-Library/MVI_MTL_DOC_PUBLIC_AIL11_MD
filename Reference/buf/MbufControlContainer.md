---
doctype: Reference
module: buf
function: MbufControlContainer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufControlContainer"
---

# MbufControlContainer

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

> Control a setting of the specified container, or one of its components.

## Syntax

```c
void MbufControlContainer(
    AIL_ID     ContainerBufId,  //out
    AIL_INT64  Component,       //in
    AIL_INT64  ControlType,     //in
    AIL_DOUBLE ControlValue     //in
)
```

## Description

This function allows you to control a setting of the specified container, or one of its components.

## Parameters

### `ContainerBufId` *(out, AIL_ID)*

Specifies the identifier of the container to control, or of which to control a component.

### `Component` *(in, AIL_INT64)*

Specifies what to control; either the container itself, or a component of the container.

*For specifying to control the container itself*

| Value | Description |
| --- | --- |
| `M_CONTAINER` | Specifies to control a setting of the container. |

*For specifying which component to control by a unique criterion*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies to control the component, in the container, which has the specified buffer identifier. An error will be generated if the container does not have a component with this buffer identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies to control the component, in the container, at the specified index. An error will be generated if the container does not have a component at this index. |

*For specifying the component to control by component type*

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControlContainer.md)component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions.A confidence component is associated with a range or disparity component in the same container when there are no other range, disparity, or confidence components in the container. If there is more than one range, disparity, and/or confidence component in a container (for example, because a single grab into the container transmitted multiple range and confidence components), you will need to either create a child container which contains only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), or free the extra components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254.You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in transmitted components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. Only depth map containers can have a matrix component.A matrix component is associated with a range component in the same container when there is only one range component and no other matrix components in that container. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md)component of the container.A mesh component is associated with a range component in the same container when there are no other range or mesh components in that container. A point cloud container which has a mesh component is referred to as a meshed point cloud container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md)component of the container.A normals component is associated with a range component in the same container when there are no other range or normals components in that container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. This can be either a 1-band component that stores a depth map, or a 3-band buffer that stores coordinates of 3D points. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light spectrum used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each row indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value needed for the setting.

## Parameter Associations

### For specifying settings for a container or a component that is an image buffer

The following [`ControlType`](../../Reference/buf/MbufControlContainer.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControlContainer.md) parameter settings are used to control settings of containers and image buffer components.

---

---

---

### For specifying settings for any component

The following [`ControlType`](../../Reference/buf/MbufControlContainer.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControlContainer.md) parameter settings are used to control all components.

---

### For specifying settings useful with components that store 3D data

For components with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)attribute and with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufControlContainer.md) set to [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControlContainer.md) or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControlContainer.md), [`ControlType`](../../Reference/buf/MbufControlContainer.md) and [`ControlValue`](../../Reference/buf/MbufControlContainer.md) can also be set to one of the values below.  These component settings are useful only when the container is passed as a source to[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Additionally, a container cannot be 3D-processable or 3D-displayable unless it has a single range component with all of these settings at their default values (except for [`M_3D_REPRESENTATION`](../../Reference/buf/MbufControlContainer.md), which must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControlContainer.md)or[`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControlContainer.md)).

---

### `M_3D_ASPECT_RATIO`

Sets by how much the focal length in the Y-direction stored in the buffer will be scaled in the destination point cloud to correct for non-square pixels in 3D reconstruction modes that rely on projective geometry.  This is useful when the focal length parameters in the camera intrinsic matrix are not equal (in the X- and Y- directions).

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies by how much the focal length the Y-direction will be scaled. |

---

### `M_3D_COORDINATE_SYSTEM_TYPE`

Sets which type of coordinate system to use to interpret the coordinates stored in the bands of the buffer if it is a range component.  > **Note:** Note that Aurora Imaging Library does not support any functionality with coordinates stored using a coordinate system type other than [`M_CARTESIAN`](../../Reference/buf/MbufControlContainer.md). If a buffer stores information defined using another type of coordinate system, you will need to manually convert that information to cartesian coordinates before the buffer can be used with any Aurora Imaging Library processing function. This conversion cannot be done using[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CARTESIAN` *(default)* | Specifies that the buffer stores right-handed cartesian coordinates.If the buffer is a 1-band buffer, its values will be interpreted as Z-coordinates (equivalent to the pixels of a depth map).If the buffer is a 3-band buffer, the bands store the following: |
| `M_CYLINDRICAL` | Specifies that the buffer stores cylindrical coordinates (theta-Y-rho).In a cylindrical coordinate system, points are defined relative to a reference axis and reference angle perpendicular to that axis. The Y-coordinate is the distance from the origin along the reference axis, theta (θ) is the polar angle of the point relative to a reference angle, and rho is the distance from that axis along the line described by theta and perpendicular to the reference axis (the radius).If the buffer is a 1-band buffer, its values store rho, the distance from a chosen reference axis.If the buffer is a 3-band buffer, the bands store the following: |
| `M_SPHERICAL` | Specifies that the buffer stores spherical coordinates (theta-phi-rho).In a spherical coordinate system, points are defined relative to a reference axis and a reference angle perpendicular to that axis. Rho is the distance from the origin (the radius), theta (θ) is the elevation angle of the point relative to reference axis, and phi is the azimuthal angle relative to the reference angle.If the buffer is a 1-band buffer, its values store rho, the distance from a chosen reference axis.If the buffer is a 3-band buffer, the bands store the following: |
| `M_UNKNOWN` | Specifies that the coordinate system type is unknown. |

---

### `M_3D_DISPARITY_BASELINE`

Sets the stereo baseline value of the stereoscopic camera used to generate the data in the buffer. This is the physical distance between the lenses of the camera.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true physical distance between the lenses of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the stereo baseline value, expressed in meters. |

---

### `M_3D_DISTANCE_UNIT`

Sets the unit to use when the buffer is part of a container and stores natively calibrated distance data.

| Value | Description |
| --- | --- |
| `M_INCH` | Specifies that the distance data is provided in inches. |
| `M_MILLIMETER` | Specifies that the distance data is provided in millimeters. |
| `M_PIXEL` | Specifies that the distance data is provided in pixels. This setting is only useful for buffers that have the component type [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControlContainer.md)and store a disparity map. |
| `M_UNKNOWN` *(default)* | Specifies that the distance unit is unknown. |

---

### `M_3D_FOCAL_LENGTH`

Sets the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true focal length of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer, expressed in pixels. For example, if the buffer has 1000 pixels per millimeter of a single image sensor (after calibration), and the focal length of the lenses in millimeters is 35, the focal length in pixels is 35000. |

---

### `M_3D_INVALID_DATA_FLAG`

Sets whether the buffer uses a specific value to indicate invalid data. For 3-band buffers that store coordinates, the invalid data flag should be stored in the Z-axis band. Specify the value that indicates invalid data using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControlContainer.md).

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the buffer does not use a special value to indicate invalid data. |
| `M_TRUE` | Specifies that the buffer uses a special value to indicate invalid data. |

---

### `M_3D_INVALID_DATA_VALUE`

Sets the value used to indicate missing data when [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControlContainer.md) is set to [`M_TRUE`](../../Reference/buf/MbufControlContainer.md).  > **Note:** Note that this value is not used or respected by any Aurora Imaging Library functions except for[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when the buffer is a component of the source container.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value used to indicate missing data. |

---

### `M_3D_OFFSET_X`

Sets by how much the X-coordinates stored in the buffer will be offset in the destination point cloud when the container is passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Y`

Sets by how much the Y-coordinates stored in the buffer will be offset in the destination point cloud when the container is passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Y-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Z`

Sets by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud when the container is passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset. |

---

### `M_3D_PRINCIPAL_POINT_X`

Sets the X-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies X-position of the principal point, expressed in pixels. |

---

### `M_3D_PRINCIPAL_POINT_Y`

Sets the Y-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies Y-position of the principal point, expressed in pixels. |

---

### `M_3D_REPRESENTATION`

Sets how 3D data is stored in the buffer; this information is used when the buffer is a range or disparity component of a container. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CALIBRATED_XYZ` | Specifies that the component stores organized and natively calibrated X, Y, and Z-coordinates.This is the default value for 3-band buffers with [`M_SIZE_Y`](../../Reference/buf/MbufInquire.md) greater than 1.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedABC_Grid. |
| `M_CALIBRATED_XYZ_UNORGANIZED` | Specifies that the component stores unorganized and natively calibrated X, Y, and Z-coordinates.This is the default value for 3-band buffers with [`M_SIZE_Y`](../../Reference/buf/MbufInquire.md) of 1.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedABC_PointCloud. |
| `M_CALIBRATED_XZ_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates stored in a separate array buffer.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedAC_Linescan. |
| `M_CALIBRATED_XZ_UNIFORM_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates identified by the row index.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedAC. |
| `M_CALIBRATED_Z` | Specifies that the component stores organized and natively calibrated Z-coordinates, without X and Y-coordinates.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedC. |
| `M_CALIBRATED_Z_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates without X-coordinates; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container.This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedC_Linescan. |
| `M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates, with the X-coordinates identified by column index; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container.This corresponds to the GenICam Scan3dOutputMode feature set to RectifiedC_Linescan. |
| `M_CALIBRATED_Z_UNIFORM_XY` | Specifies that the component stores organized and natively calibrated Z-coordinates, with X and Y-coordinates identified by column and row index respectively. The manufacturer of your camera might refer to a range component with this setting as a depth map.This is the default value for 1-band buffers.This corresponds to the GenICam Scan3dOutputMode feature set to RectifiedC. |
| `M_DISPARITY` | Specifies that the component stores a disparity map with perspective distortion along the Y-axis. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point in the resulting point cloud will be generated using the row index of the corresponding pixel and compensating for perspective.This corresponds to the GenICam Scan3dOutputMode feature set to DisparityC.Typically, this setting should only be used with disparity components that have been generated using area scan cameras. |
| `M_DISPARITY_EXTERNAL_Y` | Specifies that the component stores a disparity map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. When used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point in the resulting point cloud will be taken from the specified buffer.This corresponds to the GenICam Scan3dOutputMode feature set to DisparityC_Linescan.Typically, this setting should only be used with disparity components that have been generated using line scan cameras. |
| `M_DISPARITY_UNIFORM_Y` | Specifies that the component stores a disparity map, with Y-values identified by row index. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinates of each point in the resulting point cloud will be generated using the row index of the corresponding pixel. Since the Y-values are uniform, no perspective correction is applied. This partially corresponds to the GenICam Scan3dOutputMode feature set to DisparityC_Linescan, except that the Y-values are not stored in a separate buffer.Typically, this setting should only be used with disparity components that have been generated using line scan cameras. |
| `M_PROJECTED_Z` | Specifies that the component stores a projective map. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the X- and Y-coordinates of each point will be generated using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the column and row index for each corresponding pixel or the A and B coordinate maps provided by the camera.Providing the A and B coordinate maps ([`M_COMPONENT_COORDINATE_MAP_A_AIL`](../../Reference/buf/MbufControlContainer.md), and [`M_COMPONENT_COORDINATE_MAP_B_AIL`](../../Reference/buf/MbufControlContainer.md), respectively) will result in a more accurate representation of the camera's lens model (properties of the camera). If the coordinate maps are not provided when used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the column and row index for each corresponding pixel will be used to generate the X- and Y-coordinates.This corresponds to the GenICam Scan3dOutputMode feature set to ProjectedC.Typically, this setting should only be used with projective maps generated using area scan cameras. |
| `M_PROJECTED_Z_EXTERNAL_Y` | Specifies that the component stores a projective map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. When used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point will be taken from the specified buffer. In this case, only the X-coordinates of each point will be generated using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the column index for each corresponding pixel or the A coordinate map provided by the camera.Providing the A coordinate map ([`M_COMPONENT_COORDINATE_MAP_A_AIL`](../../Reference/buf/MbufControlContainer.md)) will result in a more accurate representation of the camera's lens model (a property of the camera). If the coordinate map is not provided when used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the column index for each corresponding pixel will be used to generate the X-coordinates.This corresponds to the GenICam Scan3dOutputMode feature set to ProjectedC_Linescan.Typically, this setting should only be used with projective maps generated using line scan cameras. |
| `M_UNCALIBRATED_Z` | Specifies that the component stores organized and uncalibrated Z-coordinates, without X and Y-coordinates.This corresponds to the GenICam Scan3dOutputMode feature set to UncalibratedC. |

---

### `M_3D_SCALE_X`

Sets by how much the X-coordinates stored in the buffer will be scaled in the destination point cloud when the container is passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Y`

Sets by how much the Y-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Y-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Z`

Sets by how much the Z-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Z-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SHEAR_X`

Sets by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row. |

---

### `M_3D_SHEAR_Z`

Sets by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row. |
