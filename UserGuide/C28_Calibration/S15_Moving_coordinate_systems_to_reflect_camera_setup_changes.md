---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Moving_coordinate_systems_to_reflect_camera_setup_changes
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Moving coordinate systems to reflect camera setup changes"
---

# Moving coordinate systems to reflect camera setup changes

Once your camera setup is calibrated, you might need to reposition the world coordinate systems, established during calibration, for different reasons. For example, you might need to reposition the tool and the camera coordinate system because the tool holding your camera or grid might have moved, or you might need to reposition the relative coordinate system to measure, in real-world units, an object located at a different position in an image (fixturing).

There are two general ways to reposition a coordinate system:

- Specify an exact transformation using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) for 3D-based camera calibration contexts ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)) or using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) or [`McalControl`](../../Reference/cal/McalControl.md) for other camera calibration contexts.
- Automatically recalculate the camera coordinate system's new position using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) for 3D-based camera calibration contexts ([`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)).

All results are returned with respect to the relative coordinate system, so you must ensure it is placed at an appropriate location for your measurements. If you are using 3D-based camera calibration modes, the location of the relative coordinate system along the Z-axis of the absolute coordinate system also affects measurements. For more information, see [Moving the relative coordinate system to account for height](../C34_3D_analysis_using_planar_views_of_an_object/S02_Moving_the_relative_coordinate_system_to_account_for_height.md).

## Types of transformation

Aurora Imaging Library can reposition a coordinate system (target) with respect to another coordinate system (reference), using two main types of transformations: translation and rotation.

### Translation

A translation of a coordinate system is equivalent to the displacement of all points along the reference coordinate axes and is specified using a translation vector. For camera calibration contexts using a perspective-transformation mode or linear-interpolation mode (two-dimensional camera calibration modes), you can move the relative coordinate system using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md). For 3D-based camera calibration contexts, you can apply a translation along the X, Y, and Z-axes using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_TRANSLATION`](../../Reference/cal/McalSetCoordinateSystem.md).

### Rotation

A rotation of a coordinate system is equivalent to the movement of all points in a circular motion around some axis in a reference coordinate system.

To rotate the relative coordinate system of a camera calibration context that uses a two-dimensional camera calibration mode, use [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) with the [`AngularOffset`](../../Reference/cal/McalRelativeOrigin.md) parameter set to the rotation angle, in degrees.

To rotate a coordinate system of a 3D-based camera calibration context, use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). Rotations in three-dimensional space can be specified in multiple ways using this function:

- **Axis and angle rotation**. Expresses the rotation of the target coordinate system as a rotation of all its points by a certain angle of rotation ( *[Image: theta.png]*
  
  ), around an arbitrary axis of rotation defined by a unit vector in the reference coordinate system.
  *[Image: cal_axis_angle_rotation.png]*
  The animation below illustrates rotation using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with[`M_ROTATION_AXIS_ANGLE`](../../Reference/cal/McalSetCoordinateSystem.md) and [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md).
  *[Image: cal_rotation_axis_angle]*
- **Quaternion rotation**. Expresses the rotation of the target coordinate system as a quaternion from the angle of rotation ( *[Image: theta.png]*
  
  ) and the normalized axis of rotation ( *[Image: cal_vector_v.png]*
  
  ), as defined in the following equation.
  *[Image: cal_quaternion1.png]*
- **Matrix rotation**. Expresses the rotation of the target coordinate system as a 3x3 square matrix transformation.
  *[Image: cal_rotation_matrix.png]*
- **X-Y-Z rotation**. Expresses the rotation of the target coordinate system as a rotation of all points along the three axes of the reference coordinate system, with all possible X-, Y- and Z-component combinations. It is also known as Roll-Pitch-Yaw rotation.
  The animation below illustrates the rotation using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with[`M_ROTATION_XYZ`](../../Reference/cal/McalSetCoordinateSystem.md) and [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md).
  *[Image: cal_rotation_xyz]*
- **Homogeneous matrix rotation**. Expresses the rotation of the target coordinate system as a homogeneous matrix transformation. Homogeneous matrices can express both a translation and a rotation of a coordinate system. It is a 4x4 matrix made up of a 3x3 rotation matrix and a translation vector. If the rotation matrix is an identity matrix, only a translation takes effect.
  *[Image: cal_homogeneous_matrix.png]*

## Transforming coordinate systems

For two-dimensional camera calibration modes, you can only transform the relative and tool coordinate systems.

- To define, move, or rotate the relative coordinate system, use [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md).
- To specify a new position for the tool coordinate system, you can use [`McalControl`](../../Reference/cal/McalControl.md) with [`M_TOOL_POSITION_X`](../../Reference/cal/McalControl.md), [`M_TOOL_POSITION_Y`](../../Reference/cal/McalControl.md), [`M_TOOL_POSITION_Z`](../../Reference/cal/McalControl.md), and [`M_TOOL_ROTATION_Z`](../../Reference/cal/McalControl.md).

For three-dimensional camera calibration modes, use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) to transform a three-dimensional coordinate system in terms of another. This function can apply the transformation of a specified target coordinate system in two distinct ways:

- From the origin and orientation of the reference coordinate system, using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) with [`M_ASSIGN`](../../Reference/cal/McalSetCoordinateSystem.md). This method is useful to explicitly define the origin of the relative coordinate system or the tool coordinate system when they have not been previously defined. It is also useful when, for example, all movements of the target coordinate system are only known from the origin of the reference coordinate system.
  *[Image: cal_CS_rotation_assign.png]*
- From its current position in a reference coordinate system using [`M_COMPOSE_WITH_CURRENT`](../../Reference/cal/McalSetCoordinateSystem.md). The transformation is still defined in terms of the reference coordinate system. Essentially, the specified transformation is composed with the current position and orientation of the target coordinate system. This method is useful, for example, to translate the tool coordinate system, when you know the precise movement of a robotic arm in the absolute coordinate system.
  *[Image: cal_CS_transformation_compose.png]*

> **Note:** Note that the pixel coordinate system cannot be used with [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

The following code snippet shows how to use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md) to update your camera calibration context after rotating a robotic arm by 30 degrees. In this example, the robotic arm is the tool holding the camera; this means that the tool coordinate system should be rotated. Note that this will also rotate the camera if [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md) is set to [`M_ENABLE`](../../Reference/cal/McalControl.md), and it is not an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md).

> **Code example:** [userguide.calibration.rotating_a_camera_attached_to_robotic_hand](userguide.calibration.rotating_a_camera_attached_to_robotic_hand)

## Moving the camera after calibrating with a 3D-based camera calibration context

After calibrating your 3D-based camera calibration context, you can move your camera to a new location and automatically reposition the camera coordinate system, assuming that you are not calibrating an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md). To automatically reposition the coordinate system, you can use [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md). This determines and displaces the camera coordinate system to its new location. Note that besides the camera coordinate system, this also displaces the tool coordinate system (if still linked -discussed later); no other coordinate system is affected. The operation requires a minimum of 4 calibration points and only calculates the position and orientation between the camera and camera calibration plane. It does not recompute the corrections for non-linear distortions; this allows for faster processing.

When determining the new location of the camera coordinate system, [`McalGrid`](../../Reference/cal/McalGrid.md) and [`McalList`](../../Reference/cal/McalList.md) compute this position based on the provided calibration points. Depending on the feature extraction used to extract the calibration points, there could be extracted points which are outliers that incorrectly skew results. If you have some knowledge about how many calibration points could be outliers, you can specify this number using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LOCALIZATION_NB_OUTLIERS_MAX`](../../Reference/cal/McalControl.md). The computation will then attempt to compute a solution using a subset of points. This creates an increased number of possible combinations of points that could form a solution and can lengthen the computation time. However, by considering a subset of points, you can increase the likelihood of finding an accurate solution when outliers are present.

An iterative method is used to determine which subset of points best determines the new location of the coordinate system when the possibility of outliers has been specified. By specifying a maximum number of iterations using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LOCALIZATION_NB_ITERATIONS_MAX`](../../Reference/cal/McalControl.md), you can adjust the robustness of the algorithm. By reducing the value of [`M_LOCALIZATION_NB_ITERATIONS_MAX`](../../Reference/cal/McalControl.md), you can speed up the computation but possibly have a less accurate result.

> **Note:** [`M_DISPLACE_CAMERA_COORD`](../../Reference/cal/McalGrid.md) is only supported for 3D-based camera calibration contexts that are calibrated using [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md). In addition, if a rigid link exists between the camera coordinate system and the tool coordinate system, both the camera coordinate system and tool coordinate systems are displaced. The other coordinate systems are left as is.

## Special considerations concerning the tool and camera coordinate systems

Camera calibration contexts allocated in three-dimensional modes automatically define the camera coordinate system after a successful call to [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) with [`M_FULL_CALIBRATION`](../../Reference/cal/McalGrid.md), and creates a rigid link with the tool coordinate system such that moving one will move both (unless it is an [`M_STATIONARY_CAMERA`](../../Reference/cal/McalAlloc.md) type of [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md)). To move the camera coordinate system by assigning the tool coordinate system to known positions in another coordinate system, it is preferable to specify the actual real-world starting position and orientation of the tool coordinate system before calibrating (before calling [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md), rather than leaving them both at the same position). If you have a camera mounted on the last joint of a robotic arm, you can set the arm's position as the origin of the tool coordinate system. After a successful [`M_TSAI_BASED`](../../Reference/cal/McalAlloc.md), [`M_ZHANG_BASED`](../../Reference/cal/McalAlloc.md), [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md), or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration, you can move the tool coordinate system according to the movements of the arm and be assured that the camera coordinate system will move accordingly.

You can disable the link using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_LINK_TOOL_AND_HEAD`](../../Reference/cal/McalControl.md) set to [`M_DISABLE`](../../Reference/cal/McalControl.md). In this case, any change applied to the camera position will affect only the camera coordinate system and not the tool coordinate system and vice versa.

When working in a robotics mode, you can specify the position and orientation of the robot tool, returned by a robot controller software, to the camera calibration context using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). When the camera and robot tool are linked together, the camera is automatically positioned and oriented accordingly. The images grabbed by the camera at the new position and orientation are therefore correctly calibrated. The following code snippet shows how to provide data to the camera calibration context using [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md):

> **Code example:** [userguide.calibration.changing_tool_coordinate_system](userguide.calibration.changing_tool_coordinate_system)
