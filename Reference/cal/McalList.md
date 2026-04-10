---
doctype: Reference
module: cal
function: McalList
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalList"
---

# McalList

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

> Calibrate your camera setup using a list of coordinates.

## Syntax

```c
void McalList(
    AIL_ID             CalibrationId,        //out
    const AIL_DOUBLE * PixCoordXArrayPtr,    //in
    const AIL_DOUBLE * PixCoordYArrayPtr,    //in
    const AIL_DOUBLE * WorldCoordXArrayPtr,  //in
    const AIL_DOUBLE * WorldCoordYArrayPtr,  //in
    const AIL_DOUBLE * WorldCoordZArrayPtr,  //in
    AIL_INT            NumPoint,             //in
    AIL_INT64          Operation,            //in
    AIL_INT64          ControlFlag           //in
)
```

## Description

This function uses a specified list of calibration points to calibrate your camera setup. Calibration points are pixel coordinates and their associated real-world coordinates. The mapping is stored with the specified camera calibration context. The specified camera calibration context determines the camera calibration mode used.

Unlike when using [`McalGrid`](../../Reference/cal/McalGrid.md), [`McalList`](../../Reference/cal/McalList.md) does not use a default orientation for the Y-axis of the absolute coordinate system. It establishes the orientation based on the specified calibration points. It arbitrarily selects three of the specified points (for example, point A, B, and C). It then establishes the direction that a line between point A and B would rotate to meet a line between point A and C while using the smallest angle of rotation. It finally selects the Y-axis orientation for the absolute coordinate system so that the rotation direction in the pixel coordinate system and in the absolute coordinate system are the same. You can use [`M_Y_AXIS_DIRECTION`](../../Reference/cal/McalInquire.md) to ensure that the orientation of the Y-axis was set correctly. For more information, see [Calibrating using calibration points from a list](../../UserGuide/C28_Calibration/S12_Calibrating_using_calibration_points_from_a_list.md).

Typically, you specify the list of calibration points in the absolute coordinate system. However, for 3D-based camera calibration modes ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)), it is possible to use a list of calibration points that are not in the Z=0 plane of the absolute coordinate system. In this case, it is possible to describe the points in the relative coordinate system instead, using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) set to [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalControl.md). You can then use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) to move the relative coordinate system. Both of these calls must be made before calling [`McalList`](../../Reference/cal/McalList.md).

When working in [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must perform at least three calls to [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) before performing a full camera calibration. Before each call, you must change the position and orientation of the tool holding the camera with respect to the robot base. More specifically, you must rotate the tool along at least two non-parallel axes, retrieve the tool's new position with respect to the robot base from the robot encoder, and using this information, set the position of the tool coordinate system with respect to the robot base coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses. Using [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times before performing a full camera calibration greatly improves the accuracy of your camera calibration.

When working in [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must perform at least three calls to [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) before performing a full camera calibration. For each call, you must provide a list of calibration points taken from a different point of view than a previous call to this function with [`M_ACCUMULATE`](../../Reference/cal/McalList.md). The pose provided in the last call will be used to automatically set the position of the absolute and camera coordinate systems. When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses. Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) more than three times greatly improves the accuracy of the camera calibration.

When working in [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, you must perform at least three calls to [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) before performing a full camera calibration. For each call, you must follow the steps described in the previous two paragraphs. When you are done accumulating data, you must call this function with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md) and no image to perform the full camera calibration. Once you perform a full camera calibration, you can no longer accumulate camera calibration poses. Calling this function with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) more than three times greatly improves the accuracy of the camera calibration.

> **Note:** Note that for an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) calibration context, which require multiple calls to[`McalList`](../../Reference/cal/McalList.md), you can inquire about any particular call using [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md). For example, you can inquire about the pixel coordinates and associated real-world coordinates of each calibration point of one particular call using [`McalInquireSingle`](../../Reference/cal/McalInquireSingle.md).

For 3D-based camera calibration modes, [`McalList`](../../Reference/cal/McalList.md) performs two types of calculations. It calculates the camera's internal attributes, and it calculates the orientation and distance between the camera and camera calibration plane. Initially, the latter is used to set the camera coordinate system. If you move the camera or the grid (not both), you can have [`McalList`](../../Reference/cal/McalList.md) recalculate only the orientation and distance between them. In this case, depending on what you moved, you can specify that [`McalList`](../../Reference/cal/McalList.md) displace either the camera coordinate system or the relative coordinate system, using [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) or [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalList.md), respectively. Note that [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalList.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode. When using [`M_DISPLACE_RELATIVE_COORD`](../../Reference/cal/McalList.md), the camera calibration plane is considered to be the XY-plane of the relative coordinate system, regardless of how [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) is set.

When working with [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), it is mandatory to set the image coordinates of the principal point prior to calibrating using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md), and [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md). For [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibrations, [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md) and [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md) are determined during calibration. If the aspect ratio of the CCD element is different than 1, specify it using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_CCD_ASPECT_RATIO`](../../Reference/cal/McalControl.md) before calling [`McalList`](../../Reference/cal/McalList.md).

Also, for most 3D-based camera calibration modes, a successful call to [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md) creates a rigid link between the tool coordinate system and the head (camera or reference/calibration object) coordinate system. This link ensures that moving either the tool or the head coordinate system will affect both. This link can be broken using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md).

> **Note:** Note that the internal attributes calculated for the camera are not those of the physical camera, but those of the ideal pinhole-camera used to model the physical camera.

After calling [`McalList`](../../Reference/cal/McalList.md), you can inquire about the success of the camera calibration using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_STATUS`](../../Reference/cal/McalInquire.md). Also, you can inquire about the pixel coordinates and associated real-world coordinates of each calibration point using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_CALIBRATION_IMAGE_POINTS_X`](../../Reference/cal/McalInquire.md), [`M_CALIBRATION_IMAGE_POINTS_Y`](../../Reference/cal/McalInquire.md), [`M_CALIBRATION_WORLD_POINTS_X`](../../Reference/cal/McalInquire.md) and [`M_CALIBRATION_WORLD_POINTS_Y`](../../Reference/cal/McalInquire.md).

For more information on performing camera calibration, see [Steps to performing a camera calibration](../../UserGuide/C28_Calibration/S02_Steps_to_performing_a_calibration.md).

## Parameters

### `CalibrationId` *(out, AIL_ID)*

Specifies the identifier of the camera calibration context.

### `PixCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-pixel coordinates.

### `PixCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-pixel coordinates.

### `WorldCoordXArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the X-world coordinates in the camera calibration-plane coordinate system.

### `WorldCoordYArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Y-world coordinates in the camera calibration-plane coordinate system.

### `WorldCoordZArrayPtr` *(in, *AIL_DOUBLE)*

Specifies the address of the array containing the Z-world coordinates.

### `NumPoint` *(in, AIL_INT)*

Specifies the number of coordinates in the supplied arrays.

### `Operation` *(in, AIL_INT64)*

Specifies the camera calibration operation to perform. This parameter must be set to one of the following values:

*For specifying the camera calibration operation*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCUMULATE` | Specifies to receive only the position of the calibration points and stores them in the camera calibration context.

For [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), at least three calls to [`McalList`](../../Reference/cal/McalList.md) should be made with this operation before performing the full camera calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md). Providing more than three calls can greatly improve the accuracy of the camera calibration.

> **Note:** This operation is only supported for [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), and [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration. |
| `M_DISPLACE_CAMERA_COORD` | Calculates only the position and orientation between the camera and the camera calibration plane, and displaces the camera coordinate system accordingly. Note that besides the camera coordinate system, this also displaces the tool coordinate system (if still linked with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md)); no other coordinate system is affected. This camera calibration operation is only supported for 3D-based camera calibration contexts that are fully calibrated with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md).

If the camera is moved to a new position but its internal attributes are already known by a previous full camera calibration, you can use this operation to allow for a faster camera calibration.

> **Note:** Note that [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalList.md) is not supported for an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration. |
| `M_DISPLACE_RELATIVE_COORD` | Calculates only the relative coordinate system position in space. This camera calibration operation is only supported for 3D-based camera calibration contexts that are fully calibrated with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md).

You can use this operation to move the relative coordinate system only and remain calibrated. This operation keeps the camera fixed with respect to the absolute coordinate system, and its intrinsic attributes unmodified. You can then obtain the position and orientation of the relative coordinate system with respect to any other coordinate system using [`McalGetCoordinateSystem`](../../Reference/cal/McalGetCoordinateSystem.md), and calculate the unknown position of an object in space.

When using this operation, the camera calibration plane is considered to be the relative coordinate system regardless of the [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) setting. |
| `M_FULL_CALIBRATION` *(default)* | Performs a full camera calibration. For 3D-based camera calibration contexts, this operation calculates the camera's internal attributes and the difference in position and orientation between the camera and the camera calibration plane. It then sets the camera coordinate system accordingly.

> **Note:** When working in [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode, the camera's optical axis should be at least 30 degrees away from the axis perpendicular to the camera calibration plane (also known as, angle of incidence). Otherwise, the camera calibration might fail. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion. This parameter must be set to `M_DEFAULT`.
