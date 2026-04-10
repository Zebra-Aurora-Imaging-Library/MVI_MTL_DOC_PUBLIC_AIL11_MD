---
doctype: Reference
module: cal
function: McalAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalAlloc"
---

# McalAlloc

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

> Allocate a camera calibration context, a fixturing offset object, a calculate hand-eye context, or a 3D draw calibration context.

## Syntax

```c
AIL_ID McalAlloc(
    AIL_ID    SysId,            //in
    AIL_INT64 Mode,             //in
    AIL_INT64 ModeFlag,         //in
    AIL_ID *  CalibrationIdPtr  //out
)
```

## Description

This function allocates a camera calibration context, a fixturing offset object, a calculate hand-eye context, or a 3D draw calibration context.

When allocating a camera calibration context, use [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) to define the pixel-to-world mapping for the camera calibration context. For a uniform pixel-to-world mapping, instead of using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), use [`McalUniform`](../../Reference/cal/McalUniform.md). To associate the camera calibration context to an image buffer or digitizer, use [`McalAssociate`](../../Reference/cal/McalAssociate.md).

When the context/object is no longer required, release it using[`McalFree`](../../Reference/cal/McalFree.md)unless [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified, the smart identifier manages the context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the camera calibration context, fixturing offset object, calculate hand-eye context, or 3D draw calibration context. This parameter should be set to one of the following values:

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `Mode` *(in, AIL_INT64)*

Specifies the type of context/object to allocate, and when allocating a camera calibration context, specifies the camera calibration mode.

*To allocate a camera calibration context and the camera calibration mode that should be used*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_3D_ROBOTICS` | Specifies a camera calibration context for a 3D-based camera calibration that uses a Tsai-based camera model in robotics mode. This mode is a true 3D camera calibration technique that determines the distance and orientation between the modeled camera and the image plane. It also estimates the position and orientation of the robot base coordinate system with respect to the absolute coordinate system. This allows for results to be inquired with respect to any defined plane.

This calibration mode can be used for a camera setup where the camera is mounted on the last joint of a robot arm, or where the camera is stationary and the robot arm is moving. Use the [`ModeFlag`](../../Reference/cal/McalAlloc.md) parameter to specify your camera setup.

Tsai-based robotics mode requires at least three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) before performing a full calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md). Before each call, you must move the robot arm to a different position, retrieve the tool's new position with respect to the robot base from the robot encoder, and using this information, set the position of the tool coordinate system with respect to the robot base coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). Calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times can greatly improves the accuracy of the camera calibration. |
| `M_3D_ROBOTICS_ZHANG_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses a Zhang-based camera model in robotics mode.

This mode is similar to[`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) but makes use of an [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera model. With this mode, the camera's intrinsic parameters are determined separately from the position and orientation estimations of the robot system. This typically results in a more accurate camera calibration model compared to the [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) mode. This calibration mode can be used for a camera setup where the camera is mounted on the last joint of a robot arm, or where the camera is stationary and the robot arm is moving. Use the [`ModeFlag`](../../Reference/cal/McalAlloc.md) parameter to specify your camera setup.

Zhang-based robotics mode requires at least three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) before performing a full calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md). Before each call, you must move the robot arm to a different position, retrieve the tool's new position with respect to the robot base from the robot encoder, and using this information, set the position of the tool coordinate system with respect to the robot base coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). Calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times can greatly improves the accuracy of the camera calibration.

Before each call, you must also move the grid or camera such that the grid is not viewed at the same plane by the camera as a previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md). The pose provided in the last call to [`M_ACCUMULATE`](../../Reference/cal/McalList.md) will be used to automatically set the position of the absolute and camera coordinate systems. It is recommended to accumulate calibration data at various perceived depths, and at the edges of the working area of your application to accurately estimate the focal length and the radial distortion coefficients. |
| `M_LINEAR_INTERPOLATION` *(default)* | Specifies a camera calibration context for a 2D-based camera calibration that uses piecewise linear interpolation mode. This mode fits a piecewise linear interpolation function to the specified set of image coordinates and their real-world equivalents.

This mode can compensate for any type of distortion. It is very accurate for points located inside the working area. However, it is less accurate for points outside the working area. |
| `M_PERSPECTIVE_TRANSFORMATION` | Specifies a camera calibration context for a 2D-based camera calibration that uses perspective transformation mode. This mode best fits a global perspective transformation function to the set of image coordinates and their real-world equivalents.

This mode can compensate for rotation, translation, scale, and perspective distortions. For such distortions, the perspective transformation mode is accurate for points inside and outside the working area. This mode cannot compensate for non-linear distortions such as lens distortions. |
| `M_TSAI_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses Tsai-based mode. This mode is based on a technique developed by Roger Y. Tsai. It is a true 3D camera calibration technique that determines the distance and orientation between the modeled camera and the image plane. This allows for results to be inquired with respect to any defined plane.

To use this mode, the optical axis of your camera must be at least 30º away from the axis perpendicular to the camera calibration plane. If the optical axis is perpendicular to the camera calibration plane, you must perform two calibrations: the first calibration must be performed with the camera inclined with respect to the camera calibration plane, using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md); the second calibration must be performed after repositioning the camera, such that its optical axis is perpendicular to the camera calibration plane, using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalList.md). Instead of calibrating a second time, you can, alternatively, move the camera coordinate system using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) if the movement is known with great precision. |
| `M_UNIFORM_TRANSFORMATION` | Specifies a camera calibration context for a 2D-based camera calibration that uses uniform transformation mode.

This mode can only map a linear translation, rotation, and scaling between the pixel and world coordinate systems. |
| `M_ZHANG_BASED` | Specifies a camera calibration context for a 3D-based camera calibration that uses Zhang-based mode. This mode is based on a technique developed by Z. Zhang. It is a true 3D camera calibration technique that determines the distance and orientation between the modeled camera and the image plane. This allows for results to be inquired with respect to any defined plane.

To use this mode, it is required to make at least three calls to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md) before performing a full calibration with [`M_FULL_CALIBRATION`](../../Reference/cal/McalList.md). Calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalGrid.md) more than three times can greatly improves the accuracy of the camera calibration.

Before each call, move the grid or camera such that the grid is not viewed at the same plane by the camera as a previous call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_ACCUMULATE`](../../Reference/cal/McalList.md). The pose provided in the last call to [`M_ACCUMULATE`](../../Reference/cal/McalList.md) will be used to automatically set the position of the absolute and camera coordinate systems. It is recommended to accumulate calibration data at various perceived depths, and at the edges of the working area of your application to accurately estimate the focal length and the radial distortion coefficients. You can inquire the values of the second and fourth order radial distortion coefficients using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DISTORTION_RADIAL_1`](../../Reference/cal/McalInquire.md) and [`M_DISTORTION_RADIAL_2`](../../Reference/cal/McalInquire.md), respectively.

Note that the Zhang-based calibration mode calculates the image coordinates of the principal point, which can be inquired using [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_PRINCIPAL_POINT_X`](../../Reference/cal/McalControl.md) and [`M_PRINCIPAL_POINT_Y`](../../Reference/cal/McalControl.md). |

*For setting a fixturing offset*

| Value | Description |
| --- | --- |
| `M_FIXTURING_OFFSET` | Specifies to allocate a fixturing offset object, used to store a preestablished positional and angular offset of the relative coordinate system from the object to be processed or analyzed when fixturing.

To set up the fixturing offset object, use [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md). To use the fixturing offset object, call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md), the reference location, and the fixturing offset object.

> **Note:** Note that you cannot use a fixturing offset object to calibrate an image. |

*For allocating a 3D draw calibration context*

| Value | Description |
| --- | --- |
| `M_DRAW_3D_CONTEXT` | Specifies to allocate a 3D draw calibration context, for drawing 3D annotations using [`McalDraw3d`](../../Reference/cal/McalDraw3d.md). |

*For allocating a calculate hand-eye context*

| Value | Description |
| --- | --- |
| `M_CALCULATE_HAND_EYE_CONTEXT` | Specifies to allocate a calculate hand-eye context, for use with [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md), to calculate the transformations between elements of a robotics setup that uses a precalibrated camera/3D sensor (required for hand-eye calibration). |

### `ModeFlag` *(in, AIL_INT64)*

Specifies the mode in which to calibrate when using a robotics mode ([`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)). For all other contexts, set this parameter to [`M_DEFAULT`](../../Reference/cal/McalAlloc.md).

*For specifying the type of robot setup to use*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MOVING_CAMERA` *(default)* | Specifies a moving camera setup consisting of a camera/3D sensor mounted on the last joint of a robotic arm (eye-in-hand calibration).

When using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), the grid should be stationary. |
| `M_STATIONARY_CAMERA` | Specifies a stationary camera setup consisting of a stationary camera/3D sensor, and a moving robotic arm (eye-to-hand calibration).

When using [`McalGrid`](../../Reference/cal/McalGrid.md), the grid should be mounted on the robotic tool.

> **Note:** Note that specifying this camera setup changes the default value of [`M_CALIBRATION_PLANE`](../../Reference/cal/McalControl.md) to [`M_RELATIVE_COORDINATE_SYSTEM`](../../Reference/cal/McalControl.md). |

### `CalibrationIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the camera calibration context, fixturing offset object, calculate hand-eye context, or 3D draw calibration context identifier or specifies the data type that the function should use to return the identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated camera calibration context, fixturing
                              offset object, calculate hand-eye context, or 3D
                              draw calibration context; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated camera calibration context, fixturing
                              offset object, calculate hand-eye context, or 3D
                              draw calibration context; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CAL_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the camera calibration context, fixturing
                              offset object, calculate hand-eye context, or 3D
                              draw calibration context (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the 3D draw calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated 3D draw calibration context.

If allocation fails, [`M_NULL`](../../Reference/cal/McalAlloc.md) is written as the identifier. |
| `Address in which to write the calculate hand-eye context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated calculate hand-eye context.

If allocation fails, [`M_NULL`](../../Reference/cal/McalAlloc.md) is written as the identifier. |
| `Address in which to write the camera calibration context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated camera calibration context.

If allocation fails, [`M_NULL`](../../Reference/cal/McalAlloc.md) is written as the identifier. |
| `Address in which to write the fixturing offset object identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated fixturing offset object.

If allocation fails, [`M_NULL`](../../Reference/cal/McalAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the camera calibration context, fixturing offset object, calculate hand-eye context, or 3D draw calibration context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CAL_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified).

Since this mode requires that the image has perspective distortion, you should not use this mode when using a camera with a telecentric lens; telecentric lenses negate perspective effects.
