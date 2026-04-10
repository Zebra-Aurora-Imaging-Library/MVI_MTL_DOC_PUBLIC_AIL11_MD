---
doctype: Reference
module: 3dim
function: M3dimProfile
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / 3dim / M3dimProfile"
---

# M3dimProfile

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

> Takes the profile of a depth map, 3D geometry, mesh, or point cloud, along a specified slicing plane.

## Syntax

```c
void M3dimProfile(
    AIL_ID     SrcAilObjectId,       //in
    AIL_ID     ProfileResult3dimId,  //out
    AIL_INT64  ProfileType,          //in
    AIL_INT64  Param1,               //in
    AIL_DOUBLE Param2,               //in
    AIL_DOUBLE Param3,               //in
    AIL_DOUBLE Param4,               //in
    AIL_DOUBLE Param5,               //in
    AIL_INT64  ControlFlag           //in
)
```

## Description

This function takes the profile of a depth map, 3D geometry, mesh, or point cloud, along a specified slicing plane. The specified result buffer is filled with coordinates that make up the profile.

For a depth map profile, pixels are sampled along a specified line and corresponding points are extracted; if the specified line crosses a valid depth map pixel, the corresponding point is included in the profile. The specified line effectively defines a slicing plane for the depth map, since the pixels' intensity values indicate depth (Z-values). You can use [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) to consider multiple source points for each profile point so that the thickness of the line is taken into consideration. Note that [`M3dimProfileEx`](../../Reference/3dim/M3dimProfileEx.md) always extracts a uniform depth map profile, where each point is at an equal distance away from the next along the profile line defined by the start and end points.

For a 3D geometry or mesh profile, 3D points are sampled and extracted along an intersection of the specified slicing plane and the geometry or mesh.

For a point cloud profile, 3D points are sampled and extracted along a specified slicing plane, which is divided into a grid of cells. For each cell, if points exist within the specified cutoff distance (perpendicular to the plane), the point closest to the plane is selected for the profile.

> **Note:** Note that the profile contains valid points only. No profile points are extracted from gaps in the depth map or invalid points in the source container.

You can retrieve results using [`M3dimGetResult`](../../Reference/3dim/M3dimGetResult.md). To draw the profile, use [`M3dimDraw3d`](../../Reference/3dim/M3dimDraw3d.md). Alternatively, you can use [`MgraDots`](../../Reference/gra/MgraDots.md) or [`M3dgraDots`](../../Reference/3dgra/M3dgraDots.md) to draw the profile points.

## Parameters

### `SrcAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the source point cloud container, depth map container, 3D geometry object, or depth map image buffer.

### `ProfileResult3dimId` *(out, AIL_ID)*

Specifies the identifier of the profile 3D image processing result buffer. The result buffer must have been allocated using [`M3dimAllocResult`](../../Reference/3dim/M3dimAllocResult.md) with [`M_PROFILE_RESULT`](../../Reference/3dim/M3dimAllocResult.md).

### `ProfileType` *(in, AIL_INT64)*

Specifies the type of profile to generate.

### `Param1` *(in, AIL_INT64)*

Specifies the first parameter. Depends on [`ProfileType`](../../Reference/3dim/M3dimProfile.md).

### `Param2` *(in, AIL_DOUBLE)*

Specifies the second parameter. Depends on [`ProfileType`](../../Reference/3dim/M3dimProfile.md).

### `Param3` *(in, AIL_DOUBLE)*

Specifies the third parameter. Depends on [`ProfileType`](../../Reference/3dim/M3dimProfile.md).

### `Param4` *(in, AIL_DOUBLE)*

Specifies the fourth parameter. Depends on [`ProfileType`](../../Reference/3dim/M3dimProfile.md).

### `Param5` *(in, AIL_DOUBLE)*

Specifies the fifth parameter. Depends on [`ProfileType`](../../Reference/3dim/M3dimProfile.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

## Parameter Associations

### For specifying the profile type

Set unused parameters to [`M_DEFAULT`](../../Reference/3dim/M3dimProfile.md).

---

### `M_PROFILE_DEPTH_MAP`

Specifies to take the profile of a depth map. Points are extracted along the specified line. If the line crosses a valid depth map pixel, the corresponding point is included in the profile. Note that invalid pixels (gaps) are ignored.  The source object ([`SrcAilObjectId`](../../Reference/3dim/M3dimProfile.md)) must be a depth map container or depth map image buffer.

| Value | Description |
| --- | --- |
| `M_PIXEL` | Specifies to interpret the values in pixel units. |
| `M_WORLD` | Specifies to interpret the values in world units. |

---

### `M_PROFILE_GEOMETRY`

Specifies to take the profile of a 3D geometry. 3D points are extracted at the intersection of the specified slicing plane and the geometry.  > **Note:** Note that the source object ([`SrcAilObjectId`](../../Reference/3dim/M3dimProfile.md)) must be a 3D geometry object, previously allocated using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) and successfully defined as a box, cylinder, plane, or sphere.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no limit along X. Note that, when [`M_DEFAULT`](../../Reference/3dim/M3dimProfile.md) is specified, an included point's X-coordinate can have any value, positive or negative.  > **Note:** This option is not available when taking the profile of a plane. |
| `Value > 0` | Specifies the length along the X-axis beyond which points are not included for the profile. This means that included points are those whose X-coordinate is between 0 and the specified [`Param3`](../../Reference/3dim/M3dimProfile.md) value. |
| `M_DEFAULT` | Specifies either [`M_ANGULAR`](../../Reference/3dim/M3dimProfile.md) for 3D geometries of type [`M_CYLINDER`](../../Reference/3dgeo/M3dgeoInquire.md) or [`M_SPHERE`](../../Reference/3dgeo/M3dgeoInquire.md), or [`M_EUCLIDEAN`](../../Reference/3dim/M3dimProfile.md) for other 3D geometry object types. |
| `M_ANGULAR` | Specifies that the sampling distance is an angular distance, in degrees. |
| `M_EUCLIDEAN` | Specifies that the sampling distance is a Euclidean distance. |

---

### `M_PROFILE_MESH`

Specifies to take the profile of a mesh. 3D points are extracted at the intersection of the specified slicing plane and the mesh.  > **Note:** Note that the source object ([`SrcAilObjectId`](../../Reference/3dim/M3dimProfile.md)) must be a container with an [`M_COMPONENT_MESH_AIL`](../../Reference/buf/MbufAllocComponent.md) component.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no limit along X. Note that, when [`M_DEFAULT`](../../Reference/3dim/M3dimProfile.md) is specified, an included point's X-coordinate can have any value, positive or negative. |
| `Value > 0` | Specifies the length along the X-axis beyond which points are not included for the profile. This means that included points are those whose X-coordinate is between 0 and the specified [`Param3`](../../Reference/3dim/M3dimProfile.md) value. |

---

### `M_PROFILE_POINT_CLOUD`

Specifies to take the profile of a point cloud. 3D points are extracted along the specified slicing plane, and within a specified distance from the plane. Invalid points in the point cloud are ignored.  The source object ([`SrcAilObjectId`](../../Reference/3dim/M3dimProfile.md)) must be a container.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no limit along X. Note that, when [`M_DEFAULT`](../../Reference/3dim/M3dimProfile.md) is specified, an included point's X-coordinate can have any value, positive or negative. |
| `Value > 0` | Specifies the length along the X-axis beyond which points are not included for the profile. This means that included points are those whose X-coordinate is between 0 and the specified [`Param5`](../../Reference/3dim/M3dimProfile.md) value. |
