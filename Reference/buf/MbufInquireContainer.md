---
doctype: Reference
module: buf
function: MbufInquireContainer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufInquireContainer"
---

# MbufInquireContainer

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

> Inquire about an Aurora Imaging Library container or component setting.

## Syntax

```c
AIL_INT MbufInquireContainer(
    AIL_ID    ContainerBufId,  //in
    AIL_INT64 Component,       //in
    AIL_INT64 InquireType,     //in
    void *    UserVarPtr       //out
)
```

## Description

This function inquires about a specified setting of an Aurora Imaging Library container or component.

## Parameters

### `ContainerBufId` *(in, AIL_ID)*

Specifies the identifier of the source container. The container must have been previously allocated on the required system using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md).

### `Component` *(in, AIL_INT64)*

Specifies the component to inquire.

*For specifying to inquire the container itself*

| Value | Description |
| --- | --- |
| `M_CONTAINER` | Specifies to inquire a setting of the container. |

*For specifying the component to inquire by a unique identifier*

| Value | Description |
| --- | --- |
| `M_COMPONENT_BY_ID` | Specifies to inquire the component, in the container, which has the specified buffer identifier. An error will be generated if the container does not have a component with this buffer identifier. |
| `M_COMPONENT_BY_INDEX` | Specifies to inquire the component, in the container, at the specified index. An error will be generated if the container does not have a component at this index. |

*For specifying the component to inquire by component type*

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquireContainer.md)component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions.A confidence component is associated with a range or disparity component in the same container when there are no other range, disparity, or confidence components in the container. If there is more than one range, disparity, and/or confidence component in a container (for example, because a single grab into the container transmitted multiple range and confidence components), you will need to either create a child container which contains only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), or free the extra components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 254.You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in transmitted components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. Only depth map containers can have a matrix component.A matrix component is associated with a range component in the same container when there is only one range component and no other matrix components in that container. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md)component of the container.A mesh component is associated with a range component in the same container when there are no other range or mesh components in that container. A point cloud container which has a mesh component is referred to as a meshed point cloud container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the component stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md)component of the container.A normals component is associated with a range component in the same container when there are no other range or normals components in that container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. This can be either a 1-band component that stores a depth map, or a 3-band buffer that stores coordinates of 3D points. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light spectrum used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each row indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component stores information of an unknown type. |

### `InquireType` *(in, AIL_INT64)*

Specifies the setting to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to return the value of the inquired setting. Since the [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about a container

To inquire the container itself, specify [`M_CONTAINER`](../../Reference/buf/MbufInquireContainer.md) and one of the following constants.

---

### `M_3D_CONVERTIBLE`

Inquires whether the container can be converted to a 3D-processable and/or natively 3D displayable container using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), and whether compensation is required to convert the data.  > **Note:** Note that irrespective of the returned value for this inquire type, components with invalid values will result in undefined behavior. This includes non-finite values for valid points (infinity, negative infinity, or NaN), invalid normals, and invalid vertices or edges in mesh triangles. Depending on the type of invalid values, you can use[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with [`M_REMOVE_NON_FINITE`](../../Reference/buf/MbufConvert3d.md) to remove non-finite values in the range and disparity components. Components with invalid values must be fixed before passing the container to a 3D processing function.

| Value | Description |
| --- | --- |
| `M_CONVERTIBLE` | Specifies that the container stores 3D data that can be converted using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). |
| `M_CONVERTIBLE_WITH_COMPENSATION` | Specifies that the container stores 3D data that can be converted using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md)with[`M_COMPENSATE`](../../Reference/buf/MbufConvert3d.md). |
| `M_NOT_CONVERTIBLE` | Specifies that the container can be converted us. |

---

### `M_3D_DISPLAYABLE`

Inquires whether the container can be used with 3D displays.  Any container that is 3D-convertible is also 3D-displayable. A conversion will occur each time the container is modified, unless you convert the container to a format that is natively 3D-displayable.  To be natively 3D-displayable, a container must have the attribute [`M_DISP`](../../Reference/buf/MbufAllocContainer.md)and must have the following components:  - A single range component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md). The component's[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufInquireContainer.md). - A single confidence component, which is a 1-band, 8-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md) and with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. - Optionally, either a single intensity or single reflectance component, which are 1-band or 3-band, 8- or 16-bit unsigned data image buffers with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single mesh component, which is a 1-band, 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single normals component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that irrespective of the returned value for this inquire type, components with invalid values will result in undefined behavior. This includes non-finite values for valid points (infinity, negative infinity, or NaN), invalid normals, and invalid vertices or edges in mesh triangles. Depending on the type of invalid values, you can use[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with [`M_REMOVE_NON_FINITE`](../../Reference/buf/MbufConvert3d.md) to remove non-finite values in the range and disparity components. Components with invalid values must be fixed before passing the container to a 3D processing function.

| Value | Description |
| --- | --- |
| `M_DISPLAYABLE` | Specifies that the container can be used with 3D displays. |
| `M_DISPLAYABLE_WITH_CONVERSION` | Specifies that the container can be used with 3D displays, with automatic conversion. |
| `M_NOT_DISPLAYABLE` | Specifies that the container cannot be used with 3D displays. |

---

### `M_3D_PROCESSABLE`

Inquires whether the container can be used for 3D processing.  To be 3D-processable, a container must have the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)and must have the following components:  - A single range component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md). The component's[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufInquireContainer.md) or [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufInquireContainer.md). - A single confidence component, which is a 1-band, 8-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md) and with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. - Optionally, either a single intensity or single reflectance component, which are 1-band or 3-band, 8- or 16-bit unsigned data image buffers with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single mesh component, which is a 1-band, 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single normals component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that irrespective of the returned value for this inquire type, components with invalid values will result in undefined behavior. This includes non-finite values for valid points (infinity, negative infinity, or NaN), invalid normals, and invalid vertices or edges in mesh triangles. Depending on the type of invalid values, you can use[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with [`M_REMOVE_NON_FINITE`](../../Reference/buf/MbufConvert3d.md) to remove non-finite values in the range and disparity components. Components with invalid values must be fixed before passing the container to a 3D processing function.

| Value | Description |
| --- | --- |
| `M_NOT_PROCESSABLE` |  |
| `M_PROCESSABLE` |  |

---

### `M_3D_PROCESSABLE_DEPTH_MAP`

Inquires whether the container is a 3D-processable depth map container.  > **Note:** Note that if a function has an optional functionality that is available for point cloud containers but not depth map image buffers, it will not be available for depth map containers.  To store data in a 3D-processable depth map format, a container must have the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)and must have the following components:  - A single range component, which is a 1-band, 8-bit, 16-bit, or 32-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md). The component's[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md), [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CALIBRATED_Z_UNIFORM_XY`](../../Reference/buf/MbufInquireContainer.md), [`M_3D_SCALE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_SCALE_Y`](../../Reference/buf/MbufInquireContainer.md) must be positive, and [`M_3D_SHEAR_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_SHEAR_Z`](../../Reference/buf/MbufInquireContainer.md) must be zero. If [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufInquireContainer.md) is set to [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md), [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufInquireContainer.md) must be the maximum value of the buffer. - If [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufInquireContainer.md) is set to [`M_FALSE`](../../Reference/buf/MbufInquireContainer.md), a single confidence component, which is a 1-band, 8-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md) and with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. - Optionally, a single region component, which is a 1-band, 8-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_REGION_AIL`](../../Reference/buf/MbufInquireContainer.md) and with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. - Optionally, a single matrix component, which is a 1-band, 32-bit float 4x4 [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md) set to [`M_COMPONENT_MATRIX_AIL`](../../Reference/buf/MbufInquireContainer.md). - Optionally, either a single intensity or single reflectance component, which are 1-band or 3-band, 8- or 16-bit unsigned data image buffers with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single mesh component, which is a 1-band, 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single normals component, which is a 3-band, 32-bit float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that irrespective of the returned value for this inquire type, components with invalid values will result in undefined behavior. This includes non-finite values for valid points (infinity, negative infinity, or NaN), invalid normals, and invalid vertices or edges in mesh triangles. Depending on the type of invalid values, you can use[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with [`M_REMOVE_NON_FINITE`](../../Reference/buf/MbufConvert3d.md) to remove non-finite values in the range and disparity components. Components with invalid values must be fixed before passing the container to a 3D processing function.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the container cannot store data in a 3D-processable depth map format. |
| `M_TRUE` | Specifies that the container can store data in a 3D-processable depth map format. |

---

### `M_3D_PROCESSABLE_MESHED`

To be 3D-processable, a container must have the attribute [`M_PROC`](../../Reference/buf/MbufAllocContainer.md)and must have the following components:  - A single range component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md). The component's[`M_3D_COORDINATE_SYSTEM_TYPE`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md) and [`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md)must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufInquireContainer.md) or [`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufInquireContainer.md). - A single confidence component, which is a 1-band, 8-bit unsigned data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_CONFIDENCE`](../../Reference/buf/MbufInquireContainer.md) and with the same dimensions ([`M_SIZE_X`](../../Reference/buf/MbufInquireContainer.md) and [`M_SIZE_Y`](../../Reference/buf/MbufInquireContainer.md)) as the range component. - Optionally, either a single intensity or single reflectance component, which are 1-band or 3-band, 8- or 16-bit unsigned data image buffers with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_INTENSITY`](../../Reference/buf/MbufInquireContainer.md) or [`M_COMPONENT_REFLECTANCE`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single mesh component, which is a 1-band, 32-bit unsigned 2D [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufInquireContainer.md). - Optionally, a single normals component, which is a 3-band, float data image buffer with the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md)set to[`M_COMPONENT_NORMALS_AIL`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that irrespective of the returned value for this inquire type, components with invalid values will result in undefined behavior. This includes non-finite values for valid points (infinity, negative infinity, or NaN), invalid normals, and invalid vertices or edges in mesh triangles. Depending on the type of invalid values, you can use[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with [`M_REMOVE_NON_FINITE`](../../Reference/buf/MbufConvert3d.md) to remove non-finite values in the range and disparity components. Components with invalid values must be fixed before passing the container to a 3D processing function.

| Value | Description |
| --- | --- |
| `M_FALSE` |  |
| `M_TRUE` |  |

---

### `M_ANCESTOR_ID`

Inquires the Aurora Imaging Library identifier of the ancestor container. Only child containers have an ancestor container. The ancestor container is the container from which the specified container ultimately originated. It is the root container; it does not have a parent container (it is not a child container of another container).  If the specified container is not a child container, the identifier of the container itself is returned as the ancestor container.  To establish the parent container of the specified container, use [`M_PARENT_ID`](../../Reference/buf/MbufInquireContainer.md) instead.

---

### `M_COMPONENT_GROUP_ID_LIST`

Inquires the list of unique group IDs of components in the container.

---

### `M_COMPONENT_REGION_ID_LIST`

Inquires the list of unique region IDs of components in the container.

---

### `M_COMPONENT_SOURCE_ID_LIST`

Inquires the list of unique source IDs of components in the container.

---

### `M_COMPONENT_TYPE_LIST`

Inquires the list of unique component types of components in the container.

---

### `M_LAYOUT_MODIFICATION_COUNT`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the current value of the modification counter of the specified container. When the container is allocated, the layout modification counter is initialized to a default value. The layout modification counter is incremented each time a component is added or removed from the container.  This feature is useful for optimization. For example, you can avoid repeating certain computations (for example, analysis computations) if you know that the image buffer has not been modified. In this case, inquire the count before the first computation in the sequence of computations, and then inquire it again before repeating the same sequence. If no modifications have been made to the image buffer, you can avoid repeating the sequence unnecessarily.  > **Note:** Note that[`Component`](../../Reference/buf/MbufInquireContainer.md)must be set to[`M_CONTAINER`](../../Reference/buf/MbufInquireContainer.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the container has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_OWNER_SYSTEM_TYPE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the type of system on which the container was allocated.

| Value | Description |
| --- | --- |
| *(see [`M_OWNER_SYSTEM_TYPE`](Reference/buf/MbufInquire.md))* |  |

---

### `M_PARENT_ID`

Inquires the Aurora Imaging Library identifier of the parent container. Only child container have a parent container. The parent container is the container from which the specified container ([`ContainerBufId`](../../Reference/buf/MbufInquireContainer.md)) was defined. The container can itself have a parent container. If the specified container has no parent container, the identifier of the specified container is returned.  To establish the ultimate ancestor container of the specified container, use [`M_ANCESTOR_ID`](../../Reference/buf/MbufInquireContainer.md) instead.

---

### `M_SYSTEM_LOCATION`

Inquires whether the specified container is allocated on a system on the local computer or the remote computer.

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that the container is allocated on a local system. |
| `M_REMOTE` | Specifies that the container is allocated on a remote system. |

### For inquiring about which components are in a container

---

### `M_COMPONENT_COUNT`

Inquires how many components are in the container. If [`Component`](../../Reference/buf/MbufInquireContainer.md)is set to[`M_CONTAINER`](../../Reference/buf/MbufInquireContainer.md), the returned value will be the number of all components in the container. Otherwise, the returned value will be the number of components in the container which meet the specified criteria.

---

### `M_COMPONENT_LIST`

Inquires the Aurora Imaging Library identifiers of components in the container. If [`Component`](../../Reference/buf/MbufInquireContainer.md)is set to[`M_CONTAINER`](../../Reference/buf/MbufInquireContainer.md), the returned array will contain the Aurora Imaging Library identifiers of all components in the container. Otherwise, the returned array will contain the Aurora Imaging Library identifiers of all components in the container which meet the specified criteria.

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### For inquiring about settings that apply to both containers and components

To inquire a container or component, specify one of the following constants.

---

---

---

---

---

### `M_EXTENDED_ATTRIBUTE`

Inquires the attributes of the container or component.  To retrieve only the data storage format of the buffer, use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_DATA_FORMAT`](../../Reference/buf/MbufInquireContainer.md). To retrieve the detailed storage format of the specified buffer, use [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that you cannot set the [`UserVarPtr`](../../Reference/buf/MbufInquireContainer.md) parameter to [`M_NULL`](../../Reference/buf/MbufInquireContainer.md) when [`InquireType`](../../Reference/buf/MbufInquireContainer.md) is set to [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquireContainer.md). In addition, a valid _AIL_INT64_ pointer must be passed to the function; otherwise, an error will occur.

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_DIB` | Forces the buffer to be a DIB buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory. |
| `M_ON_BOARD` | Forces the buffer in on-board memory. |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank. |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible. |
| `M_PAGED` | Forces the buffer in pageable memory. |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar standard. |
| `M_YUV12` | Specifies YUV12 planar standard. |
| `M_YUV24` | Specifies YUV24 planar standard. |
| `M_BGR24` | Specifies 24-bit color depth (BGR) packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth (BGR) packed pixels (BGRXBGRX). |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 packed or planar (4:2:2) standard. |
| `M_CONTAINER` | Specifies a container. |

---

### `M_EXTENDED_ATTRIBUTE_NAME`

---

---

---

---

### `M_MODIFICATION_HOOK`

Inquires the status of the modification hook, which runs a user-defined function upon an event. These user-defined functions are initially hooked to the buffer modification event using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| *(see [`M_MODIFICATION_HOOK`](Reference/buf/MbufInquire.md))* |  |

### For inquiring about settings that apply only to components

---

---

### `M_COMPONENT_ID`

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no component in the container matches the specified criterion. |
| `M_MULTIPLE_IDS` | Specifies that more than one component in the container matches the specified criterion. |
| `Component identifier` | Specifies the Aurora Imaging Library identifier of the component. |

---

### `M_COMPONENT_INVALID`

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the information in the buffer has not been marked invalid by the camera. |
| `M_TRUE` | Specifies that the information in the buffer has been marked invalid by the camera. |

---

---

---

---

---

---

---

---

### `M_COMPONENT_TYPE`

Inquires the component type of the buffer, used when the buffer is a component of a container. The component type specifies what kind of information is stored in the component.

| Value | Description |
| --- | --- |
| *(see [`M_COMPONENT_TYPE`](Reference/buf/MbufInquire.md))* |  |

---

### `M_COMPONENT_TYPE_NAME`

Inquires the component type of the buffer in a human-readable format.

---

### `M_DATA_FORMAT`

Inquires the data storage format of the buffer. This is the color or monochrome storage format of the buffer and includes whether it is in packed or planar format. Note that non-color buffers are stored in planar format and lack a color space.  To retrieve the entire list of attributes, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquireContainer.md) or [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquireContainer.md).

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |

---

### `M_DATA_TYPE`

Inquires the buffer data type.

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the buffer uses the float data type. |
| `M_SIGNED` | Specifies that the buffer uses the signed data type. |
| `M_UNSIGNED` | Specifies that the buffer uses the unsigned data type. |

---

---

---

---

---

---

---

---

---

### `M_PITCH`

Inquires the number of pixels between the beginnings of any two adjacent lines of the buffer data.

| Value | Description |
| --- | --- |
| `Value` | Specifies the pitch, in pixels. |

---

### `M_PITCH_BYTE`

Inquires the number of bytes between the beginnings of any two adjacent lines of the buffer data.

| Value | Description |
| --- | --- |
| `Value` | Specifies the pitch, in bytes. |

---

### `M_SIZE_BAND`

Inquires the number of buffer bands.

| Value | Description |
| --- | --- |
| `1 <= Value <= 1024` | Specifies the number of bands. |

---

### `M_SIZE_BIT`

Inquires the depth per band.

| Value | Description |
| --- | --- |
| `Value` | Specifies the depth per band, in bits. |

---

### `M_SIZE_X`

Inquires the width of the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the buffer, in pixels. |

---

### `M_SIZE_Y`

Inquires the height of the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the buffer, in pixels. |

---

### `M_TYPE`

Inquires the buffer data type and depth. Depth is returned in bits.

| Value | Description |
| --- | --- |
| `M_FLOAT + Depth value` | Specifies the data depth and that the data type is floating-point. |
| `M_SIGNED + Depth value` | Specifies the data depth and that the data type is signed. |
| `M_UNSIGNED + Depth value` | Specifies the data depth and that the data type is unsigned. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — Returns the location of the buffer

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the location of the buffer.

### Combination Constants — Returns whether the buffer was allocated in paged or non-paged memory

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer was allocated in paged or non-paged memory.

### Combination Constants — Returns the intended purpose of the image buffer

> *Essential.*

> **Usage:** You must add one or more of the following values to the above-mentioned values to determine the intended purpose of the buffer.

### Combination Constants — Returns the storage format and location specifier

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the storage format and location specifier.

You might have set this value, or it could have been automatically selected by Aurora Imaging Library.

### Combination Constants — Returns the compression type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the compression type.

### Combination Constants — Returns whether the buffer was allocated with an overscan region

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer was allocated with an overscan region.

### Combination Constants — Returns whether the buffer is FPGA accessible

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer is FPGA accessible.

### Combination Constants — Returns the format in which image buffers were stored

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the format in which image buffers were stored.

### Combination Constants — Returns the packed or planar color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the packed or planar color buffer format.

### Combination Constants — Returns the packed color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the packed color buffer format.

### Combination Constants — Returns the planar color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the planar color buffer format.

### Combination Constants — Returns the memory bank used

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the memory bank in which the buffer was allocated.

| Value | Description |
| --- | --- |
| `M_MEMORY_BANK_n` | Inquires the buffer allocated in the specified memory bank. |

### Combination Constants — For specifying a location in a specific type of memory

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set a location in a specific type of memory.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

### For inquiring about 3D settings that apply only to range and disparity components that are image buffers

For components with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)attribute and with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquireContainer.md) set to [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquireContainer.md) or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquireContainer.md), [`InquireType`](../../Reference/buf/MbufInquireContainer.md) can also be set to one of the values below.  These settings are used only when the component is the range or disparity component of a container that is used as a source with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Additionally, a container cannot be 3D-processable or 3D-displayable unless it has a single range component with all of these settings at their default values (except for [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufInquireContainer.md) which can be any value and[`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquireContainer.md), which must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufInquireContainer.md)or[`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufInquireContainer.md)).

---

### `M_3D_ASPECT_RATIO`

Inquires by how much the focal length in the Y-direction stored in the buffer will be scaled in the destination point cloud to correct for non-square pixels in 3D reconstruction modes that rely on projective geometry.

| Value | Description |
| --- | --- |
| *(see [`M_3D_ASPECT_RATIO`](Reference/buf/MbufControlContainer.md))* |  |

---

### `M_3D_COORDINATE_SYSTEM_TYPE`

Inquires which type of coordinate system to use to interpret the coordinates stored in the bands of the buffer if it is a range component.  > **Note:** Note that Aurora Imaging Library does not support any functionality with coordinates stored using a coordinate system type other than [`M_CARTESIAN`](../../Reference/buf/MbufInquireContainer.md). If a buffer stores information defined using another type of coordinate system, you will need to manually convert that information to cartesian coordinates before the buffer can be used with any Aurora Imaging Library processing function. This conversion cannot be done using[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_COORDINATE_SYSTEM_TYPE`](Reference/buf/MbufControlContainer.md))* |  |

---

### `M_3D_DISPARITY_BASELINE`

Inquires the stereo baseline value of the stereoscopic camera used to generate the data in the buffer. This is the physical distance between the lenses of the camera.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true physical distance between the lenses of your camera.

| Value | Description |
| --- | --- |
| *(see [`M_3D_DISPARITY_BASELINE`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_DISTANCE_UNIT`

Inquires the unit to use when the buffer is part of a container and stores natively calibrated distance data.

| Value | Description |
| --- | --- |
| *(see [`M_3D_DISTANCE_UNIT`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_FOCAL_LENGTH`

Inquires the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true focal length of your camera.

| Value | Description |
| --- | --- |
| *(see [`M_3D_FOCAL_LENGTH`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_INVALID_DATA_FLAG`

Inquires whether the buffer uses a specific value to indicate invalid data. For 3-band buffers that store coordinates, the invalid data flag should be stored in the Z-axis band. Specify the value that indicates invalid data using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufInquireContainer.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_INVALID_DATA_FLAG`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_INVALID_DATA_VALUE`

Inquires the value used to indicate missing data when [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufInquireContainer.md) is set to [`M_TRUE`](../../Reference/buf/MbufInquireContainer.md).  > **Note:** Note that this value is not used or respected by any Aurora Imaging Library functions except for[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when the buffer is a component of the source container.

| Value | Description |
| --- | --- |
| *(see [`M_3D_INVALID_DATA_VALUE`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_OFFSET_X`

Inquires by how much the X-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_OFFSET_X`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_OFFSET_Y`

Inquires by how much the Y-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_OFFSET_Y`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_OFFSET_Z`

Inquires by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_OFFSET_Z`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_PRINCIPAL_POINT_X`

Inquires the X-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| *(see [`M_3D_PRINCIPAL_POINT_X`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_PRINCIPAL_POINT_Y`

Inquires the Y-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| *(see [`M_3D_PRINCIPAL_POINT_Y`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_REPRESENTATION`

Inquires how 3D data is stored in the buffer; this information is used when the buffer is a range or disparity component of a container. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_REPRESENTATION`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_SCALE_X`

Inquires by how much the X-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_SCALE_X`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_SCALE_Y`

Inquires by how much the Y-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_SCALE_Y`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_SCALE_Z`

Inquires by how much the Z-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_SCALE_Z`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_SHEAR_X`

Inquires by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_SHEAR_X`](Reference/buf/MbufInquire.md))* |  |

---

### `M_3D_SHEAR_Z`

Inquires by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| *(see [`M_3D_SHEAR_Z`](Reference/buf/MbufInquire.md))* |  |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information. If the requested information cannot be retrieved, `M_ERROR` is returned.

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> While an image buffer with an [`M_KERNEL`](../../Reference/buf/MbufAlloc1d.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc1d.md) attribute are available under Aurora Imaging Library Lite, these attributes are not required for the image buffer to be available to other Aurora Imaging Library Lite functions.

To convert the returned attributes, select the **Benchmarks and Utilities** item in the tree structure of the Aurora Imaging Configurator utility. Then, select the **Buffer format** item. On the **Value type** pane, paste the returned attributes in the text box provided. Then, click on the **Value lookup** button. The results of the translation are presented below the **Value lookup** button.
