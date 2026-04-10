---
doctype: Reference
module: cal
function: McalTransformCoordinate3dList
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / cal / McalTransformCoordinate3dList"
---

# McalTransformCoordinate3dList

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

> Convert a list of coordinates, including 3D coordinates, between two coordinate systems.

## Syntax

```c
void McalTransformCoordinate3dList(
    AIL_ID             CalibrationOrImageId,  //in
    AIL_INT64          SrcCoordinateSystem,   //in
    AIL_INT64          DstCoordinateSystem,   //in
    AIL_INT            NumPoints,             //in
    const AIL_DOUBLE * SrcCoordXArrayPtr,     //in
    const AIL_DOUBLE * SrcCoordYArrayPtr,     //in
    const AIL_DOUBLE * SrcCoordZArrayPtr,     //in
    AIL_DOUBLE *       DstCoordXArrayPtr,     //out
    AIL_DOUBLE *       DstCoordYArrayPtr,     //out
    AIL_DOUBLE *       DstCoordZArrayPtr,     //out
    AIL_INT64          ModeFlag               //in
)
```

## Description

This function converts a list of coordinates from a source coordinate system to a destination coordinate system, where coordinates of unit direction vectors and of depth maps are also accepted as inputs or outputs.

Using this function, you can convert between the following:

- Pixel coordinate system and world coordinate system.
- World coordinate system and any other world coordinate system.
- Fully corrected depth maps and world coordinate system.
- Pixel coordinate system and unit direction vectors in any world coordinate system.

> **Note:** Note that conversion between unit direction vectors and the pixel coordinate system is supported, but conversion of direction vectors to depth maps or world coordinate systems is not. Also, if you pass a corrected image to [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md), and you specify that the source points are expressed in the pixel coordinate system ([`M_PIXEL_COORDINATE_SYSTEM`](../../Reference/cal/McalTransformCoordinate3dList.md)), the function assumes that the source points are expressed in the corrected pixel coordinate system.

To convert a point from the pixel coordinate system to its corresponding point in the specified world coordinate system, this function creates a line (simulating a light ray) from the effective pinhole of the camera through the specified point in the image plane, and then to the specified relative coordinate system ([`DstCoordinateSystem`](../../Reference/cal/McalTransformCoordinate3dList.md)). Since all points along a given light ray project onto the same point in the image plane, every point along the ray is theoretically a solution. Therefore, to produce a unique solution, the point of intersection between the line and the XY-plane (Z=0) of the relative coordinate system is transformed to the specified world coordinate system. Then, the corresponding world point is returned. Alternatively, [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) can return the X, Y, and Z-components of the unit direction vector corresponding to the direction of the calculated light ray as it intersects the source point (using [`M_UNIT_DIRECTION_VECTOR`](../../Reference/cal/McalTransformCoordinate3dList.md)).

If the image plane is not parallel to the XY-plane (Z=0) of the relative coordinate system, due to the camera setup or a displacement of the relative coordinate system, not every point in the image plane will have a valid real-world equivalent. Three types of intersections can occur when transforming from the image plane to the world plane. If the specified point in the image plane corresponds to a point in front of the camera in the relative coordinate system, its corresponding position in the destination world coordinate system is returned. However, if the line traced through the specified point in the image plane does not intersect the XY-plane of the relative coordinate system, **M_INVALID_POINT** is returned. If the projected line's intersection with the XY-plane of the relative coordinate system is behind the camera, the mathematically computed value of this point of intersection is returned. Because this point of intersection is located behind the camera, its location in the relative coordinate system is considered as incorrect.

However, using [`M_NO_POINTS_BEHIND_CAMERA`](../../Reference/cal/McalTransformCoordinate.md) with [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) returns **M_INVALID_POINT** if the returned point is behind the camera.

To convert a point specified in a world coordinate system to its corresponding point in the pixel coordinate system, the function transforms the specified point to the relative coordinate system and a line is formed (simulating a light ray) from the point to the effective pinhole of the camera. The function then returns the point of intersection of this line with the image plane. If the point given is behind the camera, the function will return the mathematically computed value. However, using[`M_NO_POINTS_BEHIND_CAMERA`](../../Reference/cal/McalTransformCoordinate3dList.md) with [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) will return **M_INVALID_POINT** instead of the mathematically computed value.

This function also supports the conversion of coordinates from a depth map to a specified world coordinate system and vice versa. To convert from a depth map to a world coordinate system, you must provide the depth map's X and Y coordinates and, optionally, the intensity value of the pixel. You can choose to omit the intensity value, in which case the function will obtain the value from the specified location in the depth map. If you choose to omit the intensity values, set [`SrcCoordZArrayPtr`](../../Reference/cal/McalTransformCoordinate3dList.md) to [`M_NULL`](../../Reference/cal/McalTransformCoordinate3dList.md). The intensity value of the depth map point is linearly mapped to a corresponding Z-coordinate for the world coordinate system. Note, source pixel coordinates that are outside the depth map image will generate **M_INVALID_POINT**.

If the intensity of a depth map pixel is equal to the buffer's maximum intensity value, it is interpreted as missing data and cannot be converted. **M_INVALID_POINT** will be returned. Note, that this applies whether you choose to omit the intensity values or not.

To convert a point from a specified world coordinate system to a depth map, you must provide the X-, Y-, and Z-coordinates of the point. The function converts the X-, Y-, and Z-coordinates of the world coordinate system to X, Y and intensity values, respectively. The Z-coordinate of the world coordinate system is linearly mapped to a corresponding intensity value where the maximum Z-coordinate in the list is given the highest possible depth value and the minimum Z-coordinate is give the lowest possible depth value.

## Parameters

### `CalibrationOrImageId` *(in, AIL_ID)*

Specifies the identifier of a camera calibration context, 3D reconstruction context, calibrated image, corrected image, or 3d reconstruction result buffer of type [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md). When an identifier of an image buffer is specified, the transformation uses the camera calibration information associated with this image. The image buffer must be a 1- or 3-band buffer.

### `SrcCoordinateSystem` *(in, AIL_INT64)*

Specifies the coordinate system of the source coordinates. This parameter must be set to one of the following values:

*For specifying the type of the source coordinate system*

| Value | Description |
| --- | --- |
| `M_ABSOLUTE_COORDINATE_SYSTEM` | Specifies an implicit and fixed coordinate system from which all other world coordinate systems are defined.

> **Note:** Note that for a 3D reconstruction context, you can only convert absolute coordinates to laser coordinates, and vice versa. For an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer identifier, you can only convert absolute coordinates to relative coordinates, and vice versa. |
| `M_CAMERA_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin corresponds to the effective pinhole of the camera and whose Z-axis points in the direction that the camera is facing. This coordinate system is only defined after a successful camera calibration in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode. Alternatively, you can define this coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md). |
| `M_LASER_LINE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is on the first laser line at the point closest to the origin of the absolute coordinate system.

> **Note:** Note that for a 3D reconstruction context, you can only convert laser coordinates to absolute coordinates, and vice versa. |
| `M_PIXEL_COORDINATE_SYSTEM` | Specifies the pixel coordinate system of the image passed to [`CalibrationOrImageId`](../../Reference/cal/McalTransformCoordinate3dList.md) (corrected or uncorrected); if a camera calibration context is passed instead, the pixel coordinate system is of any image that has the same pixel-to-world mapping as the specified camera calibration context. The origin of this coordinate system, (0, 0), is at the center of the image's top-left pixel. This is a 2D coordinate system. |
| `M_RELATIVE_COORDINATE_SYSTEM` | Specifies the relative coordinate system. The XY-plane (Z=0) of the relative coordinate system defines the world plane in which results are measured. By default, it is positioned at the absolute coordinate system.

> **Note:** Note that for an [`M_POINT_CLOUD_RESULT`](../../Reference/3dmap/M3dmapAllocResult.md) 3D reconstruction result buffer, you can only convert relative coordinates to absolute coordinates, and vice versa. |
| `M_ROBOT_BASE_COORDINATE_SYSTEM` | Specifies the coordinate system whose origin is positioned at the base of the robot holding the camera. This coordinate system is only available for an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration context, calibrated image, or corrected image. |
| `M_TOOL_COORDINATE_SYSTEM` | Specifies the coordinate system used to position the camera. This coordinate system is not necessarily associated with the real camera position. |

### `DstCoordinateSystem` *(in, AIL_INT64)*

Specifies the coordinate system in which to return the coordinates. This parameter must be set to one of the following values:

*For specifying the type of the destination coordinate system*

| Value | Description |
| --- | --- |
| *(see `CoordSystems`)* |  |

### `NumPoints` *(in, AIL_INT)*

Specifies the number of points to convert.

### `SrcCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-coordinates of the source points. These coordinates must be expressed in the source coordinate system.

### `SrcCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-coordinates of the source points. These coordinates must be expressed in the source coordinate system.

### `SrcCoordZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Z-coordinates of the source points. These coordinates must be expressed in the source coordinate system.

### `DstCoordXArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to return the X-coordinates of the resulting points . These coordinates are expressed in the destination coordinate system.

### `DstCoordYArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to return the Y-coordinates of the resulting points . These coordinates are expressed in the destination coordinate system.

### `DstCoordZArrayPtr` *(out, *AIL_DOUBLE)*

Specifies the address of the array in which to return the Z-coordinates of the resulting points . These coordinates are expressed in the destination coordinate system.

### `ModeFlag` *(in, AIL_INT64)*

Specifies the mode of transformation and how to deal with invalid points.

*For specifying the mode of transformation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default mode of transformation.

When converting world points to points in the pixel coordinate system, this function makes a line (simulating a light ray) from the effective pinhole of the camera to the point in the world coordinate system, and then returns the intersection of this line with the image plane; there is no ambiguity.

When converting world points from one world coordinate system to another, a matrix multiplication is used. |
| `M_DEPTH_MAP` | Specifies that coordinates expressed in the pixel coordinate system, whether source or destination coordinates, are interpreted as depth map coordinates. The X- and Y-coordinates describe the pixels in the image, while the Z-coordinate parameter describes the intensity (gray level) of the pixels.

To convert from a depth map using [`M_DEPTH_MAP`](../../Reference/cal/McalTransformCoordinate3dList.md),[`CalibrationOrImageId`](../../Reference/cal/McalTransformCoordinate3dList.md) must be a 1-band, 8-bit, 16-bit, or 32-bit unsigned buffer and must contain a fully corrected depth map. The 3D Image Processing module is capable of generating these fully corrected depth maps.

> **Note:** This value cannot be used if both [`SrcCoordinateSystem`](../../Reference/cal/McalTransformCoordinate3dList.md) and [`DstCoordinateSystem`](../../Reference/cal/McalTransformCoordinate3dList.md) are world coordinate systems. |
| `M_UNIT_DIRECTION_VECTOR` | Specifies that points expressed in a world coordinate system, whether they are source or destination coordinates, are interpreted as unit direction vectors. Each vector originates at the camera's effective pinhole and points towards the specified point in the image plane.

This transformation mode is only available:

- After a successful camera calibration in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) mode.
- When converting points in the pixel coordinate system to unit direction vectors in any world coordinate system, and vice versa.

When [`SrcCoordinateSystem`](../../Reference/cal/McalTransformCoordinate3dList.md) is set to [`M_PIXEL_COORDINATE_SYSTEM`](../../Reference/cal/McalTransformCoordinate3dList.md), unit direction vectors are generated for each point specified in the image plane. When [`DstCoordinateSystem`](../../Reference/cal/McalTransformCoordinate3dList.md) is set to [`M_PIXEL_COORDINATE_SYSTEM`](../../Reference/cal/McalTransformCoordinate3dList.md), points in the image plane corresponding to the specified unit direction vectors are returned.

*[Image: cal_unit_direction_vector.png]* |

*For specifying when to return*

| Value | Description |
| --- | --- |
| `M_NO_EXTRAPOLATED_POINTS` | Specifies that if a pixel involved in the transformation is not inside the calibrated region, **M_INVALID_POINT**will be returned, instead of a coordinate resulting from the extrapolation. The calibrated region is defined as the image region covered by the camera calibration grid. This can be displayed by calling [`McalDraw`](../../Reference/cal/McalDraw.md) with [`M_DRAW_VALID_REGION`](../../Reference/cal/McalDraw.md).

> **Note:** This value only applies to piecewise linear camera calibrations ([`M_LINEAR_INTERPOLATION`](../../Reference/cal/McalAlloc.md)); if it is set and the image has any other type of camera calibration, it is ignored. |
| `M_NO_POINTS_BEHIND_CAMERA` | Specifies that **M_INVALID_POINT** is returned when a computed point is mathematically valid but physically impossible (behind the camera).

> **Note:** If this value does not apply to the specified transformation type, then it is ignored. |
