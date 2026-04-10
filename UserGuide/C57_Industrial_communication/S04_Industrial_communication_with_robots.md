---
doctype: UserGuide
part: "Communication"
chapter: Industrial_communication
section: Industrial_communication_with_robots
module_tag: com
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IndCom / Industrial communication with robots"
---

# Industrial communication with robots

To communicate with an industrial robot controller, you must allocate an Industrial Communication context using [`McomAlloc`](../../Reference/com/McomAlloc.md)and specify the communication protocol of the robot controller. A robot controller will typically support multiple robot arm models. The following table outlines the robot controller brands and models supported and how to allocate an Industrial Communication context to communicate with the robot controller.

| Robot brand | Supported robot controller models | Allocate context using[`McomAlloc`](../../Reference/com/McomAlloc.md) with |
| --- | --- | --- |
| ABB | IRC5 | [`M_COM_PROTOCOL_ABB`](../../Reference/com/McomAlloc.md) |
| DENSO | RC8 | [`M_COM_PROTOCOL_DENSO`](../../Reference/com/McomAlloc.md) |
| Epson | RC420+, RC520+ | [`M_COM_PROTOCOL_EPSON`](../../Reference/com/McomAlloc.md) |
| Fanuc | LRMate200iC, LRMate200iD | [`M_COM_PROTOCOL_FANUC`](../../Reference/com/McomAlloc.md) |
| KUKA | KR C2 | [`M_COM_PROTOCOL_KUKA`](../../Reference/com/McomAlloc.md) |
| Staubli | CS8, CS8C HP, CS9 | [`M_COM_PROTOCOL_STAUBLI`](../../Reference/com/McomAlloc.md) |

Before an Aurora Imaging Library application can exchange information with a robot, you need to install the appropriate robot-side API files located in the installed Robots config directory (for example, _C:\Program Files\Aurora Imaging Library\11\Library\Config\Robots\_ on your robot controller. The robot-side API files provide the required functionality necessary to write specialized code to interface the robot controller with Aurora Imaging Library. The commands are specific to the brand of robot controller that you have. For full documentation on how to install the robot-side API files on your robot controller, and an explanation of the Zebra commands to include in the robot-side code, see _Aurora Imaging Library's Robot-side Communication API_. This documentation, along with the installation files and demo code, are all found in the Aurora Imaging Library folder on your computer (for example, _C:\Program Files\Aurora Imaging Library\&lt;version>\Library\Config\Robots_).

## Calibration considerations

For an Aurora Imaging Library application and a robot controller to properly exchange position information, a mapping between the pixel and robot coordinate systems (robot base coordinate system) must be made. To do so, you must perform a camera calibration. Robotic applications typically use one of two methods to provide camera calibration mappings. One method directly links the robot's coordinate system to the pixel coordinate system; the other method indirectly links the robot's coordinate system to the pixel coordinate system by linking each to the absolute coordinate system.

*[Image: Calibration_Introduction.png]*

Legend:

| Marker | Description |
| --- | --- |
| A | Origin of the absolute coordinate system |
| B | Camera field of view |
| C | Origin of robot base coordinate system |
| D | Pixel coordinate system |

### Calibration by directly mapping the robot coordinate system to the pixel coordinate system

When there is little distortion in your images, it is typically most efficient to link the pixel coordinate system directly to the robot coordinate system. When using this method, you pass new positions to the robot controller directly in the robot coordinate system. You don't need to change the reference frame of the robot controller. In this case, you must know the position of your calibration points in the robot's coordinate system (and in the pixel coordinate system) and calibrate using [`McalList`](../../Reference/cal/McalList.md).

*[Image: Calibration_Method1.png]*

Legend:

| Marker | Description |
| --- | --- |
| A | Origin of the absolute coordinate system |
| B | Camera field of view |
| C | Origin of robot base coordinate system |
| D | Pixel coordinate system |

A minimum of three to seven calibration points are required, depending on the camera calibration mode. Passing more calibration points than the minimum allows the camera calibration to compensate for non-linear robot travel across the work area (the robot moves to a specified point with repeatability, but that point might not be the exact point you wanted it to move to). Use the origin and axes of the robot coordinate system as the origin and axes of the absolute coordinate system. This means that when you specify your calibration points, you do so with respect to the robot coordinate system.

The calibration points do not need to come from a camera calibration grid. You can use any set of known positions, as found on a reference work piece or jig for example. For best accuracy, include locations from the outer limits of the work area. For example, if the object or part is in a box, include the corners of the box as some of the calibration points.

The following are the steps required to perform a camera calibration that directly links the robot and pixel coordinate systems:

1. Establish a minimum of three to seven calibration points, depending on the camera calibration mode.
2. Use the robot controller to move the robot to each point and note the coordinates that the robot controller returns for each point.
3. Allocate a camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md).
4. Use [`McalList`](../../Reference/cal/McalList.md) to map each of the calibration points in pixel coordinates to robot coordinates. For more information about camera calibration using a list of coordinates, see [Calibrating using calibration points from a list](../C28_Calibration/S12_Calibrating_using_calibration_points_from_a_list.md). The direction of the Y-axis and the Z-axis are automatically established from the specified calibration points.

In instances where the camera is directly attached to the end point (tool) of the robot arm, you can use a 3D camera calibration mode ([`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)) to relate the [pixel coordinate system](../C28_Calibration/S04_Coordinate_systems.md) to the [robot base coordinate system](../C28_Calibration/S04_Coordinate_systems.md) without the latter being equivalent to the absolute coordinate system. When using these 3D camera calibrations, you can pass positional and rotational data to the robot controller also in the [tool coordinate system](../C28_Calibration/S04_Coordinate_systems.md); however, there must be a rigid link between the camera and the tool.

*[Image: robot.png]*

Legend:

| Marker | Description |
| --- | --- |
| A | Origin of the absolute coordinate system. |
| B | Camera field of view from which image is captured |
| C | Origin of the relative coordinate system |
| D | Origin of the tool coordinate system |
| E | Origin of the camera coordinate system |
| F | Origin of robot base coordinate system |
| G | Pixel coordinate system |

For more information on calibrating using [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) and [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), see [Robotics mode](../C28_Calibration/S07_3D_Calibration_modes.md).

### Calibration by indirectly mapping the robot and pixel coordinate systems to the world coordinate system

When there is large distortion in your images, it is typically most efficient to map the pixel coordinate system to the robot coordinate system indirectly. When using this method, you pass new positions to the robot controller with respect to some other position, and you must displace the robot's reference frame to this new position.

*[Image: Calibration_Method2.png]*

Legend:

| Marker | Description |
| --- | --- |
| A | Origin of the absolute coordinate system |
| B | Camera field of view |
| C | Origin of robot base coordinate system |
| D | Pixel coordinate system |

When using this method, the camera and robot maintain independent mappings of the working area. You specify the location of the origin and axes of the absolute coordinate system both in the pixel coordinate system using Aurora Imaging Library and in the robot coordinate system using the robot controller's software that defines its reference frame. This method is quick and easy to implement, but relies on the assumption that Aurora Imaging Library and the robot controller return accurate coordinates in the working area.

Use [`McalList`](../../Reference/cal/McalList.md) to calibrate using a list of at least three to seven calibration points, depending on the camera calibration mode. You can also use[`McalGrid`](../../Reference/cal/McalGrid.md) to calibrate using a grid. You should calibrate using a grid if there are distortions in your image.

The following are the steps suggested to perform a camera calibration that indirectly links the robot and pixel coordinate systems using a grid:

1. Grab an image of the camera calibration grid. This image must be representative of the work area to be calibrated.
2. Allocate a camera calibration context using [`McalAlloc`](../../Reference/cal/McalAlloc.md).
3. Calibrate the camera using [`McalGrid`](../../Reference/cal/McalGrid.md). You can use [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_Y_AXIS_COUNTER_CLOCKWISE`](../../Reference/cal/McalGrid.md) so that the Y-axis and the Z-axis are pointing up (unlike in the image above). For more information on how to calibrate using a grid, see [Calibrating using calibration points from a grid](../C28_Calibration/S10_Calibrating_using_calibration_points_from_a_grid.md).
4. Move the robot arm to the dot selected to be the origin of the absolute coordinate system.
5. Inquire the position of this origin point in the robot's coordinate system using the robot controller.
6. Move the robot to the center of the next dot in the X-direction and record its position. Repeat for the adjacent dot in the Y-direction.
7. Use the coordinate system definition tool of the robot controller to specify the new reference frame for the robot.

As was previously mentioned, this method is the fastest way to calibrate when large distortions in your image play a factor, because a very large number of calibration points can be included. It is assumed that the robot can very accurately hit the center of any position on the grid based on its row and column position, and therefore this camera calibration method does not require you to collect and record the robot position for all the points.

## Communicating with a robot

When communicating with a robot controller, your Aurora Imaging Library application should typically wait for the robot controller to request a new position (for example, the location of the next item to pick up), using [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md). When a request is received, your Aurora Imaging Library application should grab an image of the item and perform the required analysis to establish the item's position. It should then send the new position to the robot controller using [`McomSendPosition`](../../Reference/com/McomSendPosition.md). A position for a robot controller consists of six values: X-position, Y-position, Z-position, rotation around X, rotation around Y, and rotation around Z.

*[Image: RobotFlowchartSample.png]*

For more advanced applications involving multiple object-part types in the camera's field of view, the [`McomWaitPositionRequest`](../../Reference/com/McomWaitPositionRequest.md) function can receive from the robot-side application a user-defined number ([`ObjectSpecifierPtr`](../../Reference/com/McomWaitPositionRequest.md)) that identifies the part or object that the Aurora Imaging Library application should locate. Similarly, for applications where multiple occurrences of objects might be present in the field of view, your Aurora Imaging Library application should send the robot-side application the user-defined number ([`ObjectSpecifier`](../../Reference/com/McomSendPosition.md)) that identifies the object for which it is sending the position.

## Orientation conventions

The different rotations about positional axes are represented differently by the robot controllers of various robot manufacturers. The following table shows the notation used for each rotational axis by the robot controller when it displays positional information.

| Robot type | Robot controller representation |
| --- | --- |
| ABB | The orientation is converted by the robot controller to a quaternion. |
| DENSO | Roll | Pitch | Yaw |
| Epson | W | V | U |
| Fanuc | W | P | R |
| KUKA | C | B | A |
| Staubli | Rx | Ry | Rz |
