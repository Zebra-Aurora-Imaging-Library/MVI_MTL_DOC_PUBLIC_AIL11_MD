---
doctype: UserGuide
part: "3D related information"
chapter: Grabbing_from_3D_sensors
section: Using_a_different_working_coordinate_system
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D related information / Grabbing_from_3D_sensors / Using a different working coordinate system"
---

# Hand-eye calibration

[`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) calculates the transformations between coordinate systems in a robotics setup. The system of matrices typically takes the form of AX=ZB where A and B are known transformation matrices, and X and Z are unknown transformation matrices (X and Z do not refer to the X and Z-axes). The resulting matrices are useful to transform points/positional results so that they can be passed, for example, to a robot controller to move its arm to a specific position. To establish the unknown matrices, use a reference object (for example, a grid). You will need to provide the information from at least 3 different poses. For each pose, you must change the position and orientation of the tool holding the camera or grid with respect to the robot base. More specifically, you must rotate the tool along at least two non-parallel axes.

The two most common types of robotics setups that can be solved using [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) are:

- **Eye-in-hand calibration** (the camera attached to robotic arm).
- **Eye-to-hand calibration** (the camera stationary and robotic arm moves).

Note that [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) does not calibrate a camera; it only solves for the unknown transformation matrices. You must use [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md) with the resulting transformation matrices to apply the transformation. Therefore, this function is useful when your setup is using a precalibrated 2D camera or 3D sensor. In the case where you still need to calibrate your camera, you should use an [`M_3D_ROBOTICS`](../../Reference/cal/McalAlloc.md) or [`M_3D_ROBOTICS_ZHANG_BASED`](../../Reference/cal/McalAlloc.md) camera calibration mode to calibrate your setup instead of [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md). For more information on this type of calibration, see [Robotics mode](../C28_Calibration/S07_3D_Calibration_modes.md).

## Eye-in-hand calibration

Eye-in-hand calibration is used for camera setups where the camera is mounted on the last joint of a robot arm, while a calibration grid or reference object is stationary and in the camera's field of view.

*[Image: Eye_in_hand_calibration_2.png]*

Using [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to solve the system of matrices, the two known transformation matrices for the eye-in-hand type of robotics set up are:

- Matrix A which transform points from the tool coordinate system to the robot base coordinate system (typically provided by the robot software).
- Matrix B which transform points from the camera coordinate system to the reference object coordinate system.

The two unknown transformation matrices are:

- Matrix X which transforms points from the camera coordinate system to the tool coordinate system.
- Matrix Z which transforms points from the reference object coordinate system to the robot base coordinate system.

## Eye-to-hand calibration

Eye-to-hand calibration is used for a camera setup where the camera is stationary and the robot arm, with a calibration grid or reference object mounted on the last joint, is moving.

*[Image: Hand_on_eye_calibration_2.png]*

Using [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to solve the system of matrices, the two known transformation matrices for the eye-in-hand type of robotics set up are:

- Matrix A which transforms points from the robot base coordinate system to the tool coordinate system (typically provided by the robot software).
- Matrix B which transforms points from the camera coordinate system to the reference object coordinate system.

The two unknown transformation matrices are:

- Matrix X which transforms points from the camera coordinate system to the robot base coordinate system.
- Matrix Z which transforms points from the reference object coordinate system to the tool coordinate system.

## Transformation matrices

The following table summarizes what is said above. Note that the transformation matrices are not limited to the descriptions shown in the table below. There are many different types of systems of matrices, both in robotics and non-robotics applications, that take the form of AX=ZB where two transformation matrices are known and two are unknown. The transformation matrices A, B, X, and Z represent different coordinate systems based on the setup. You must provide both the A and B transformation matrices, where A is typically obtained using the robotic software.

| *[Image: z_TableSpacer20x20.png]* | Transformation matrix[^two] |
| --- | --- |
| **A** Known (Must be provided) | **X** Unknown (Calculated and returned) | **Z** Unknown (Calculated and returned) | **B** Known (Must be provided) |
| **Eye-in-hand calibration** Camera attached to robotic arm | Transform points from the tool coordinate system to the robot base coordinate system | Transforms points from the camera coordinate system to the tool coordinate system | Transforms points from the reference object coordinate system to the robot base coordinate system | Transform points from the camera coordinate system to the reference object coordinate system |
| **Eye-to-hand calibration** Camera stationary and robotic arm moves | Transforms points from the robot base coordinate system to the tool coordinate system | Transforms points from the camera coordinate system to the robot base coordinate system | Transforms points from the reference object coordinate system to the tool coordinate system | Transforms points from the camera coordinate system to the reference object coordinate system |

[^two]: In this table, the camera can be a 2D camera calibrated using a 3D camera calibration mode or a precalibrated 3D sensor.

> **Note:** Note that while the reference object is a calibration grid in the visualizations above, other types of reference objects such as spheres or calibration markers can be used in place of the calibration grid.

## Steps to perform hand-eye calibration

The following steps provide a basic methodology for calculating the transformations required to perform operations such as robotic hand-eye calibration:

1. Allocate a calculate hand-eye context, using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_CALCULATE_HAND_EYE_CONTEXT`](../../Reference/cal/McalAlloc.md).
2. Allocate a minimum of 8 transformation matrix objects using [`M3dgeoAlloc`](../../Reference/3dgeo/M3dgeoAlloc.md) with [`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md) and pass information about a minimum of 3 poses (each pose consists of an A matrix and a corresponding B matrix) to both [`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md) and [`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md), respectively. While you must pass a minimum of 3 poses, more poses improves the accuracy of the calculation. For each pose, you must change the position and orientation of the robot arm with respect to the robot base. More specifically, you must rotate the tool along at least two non-parallel axes. Two of the transformation matrices ([`M_TRANSFORMATION_MATRIX`](../../Reference/3dgeo/M3dgeoAlloc.md)) will be used to retrieve the resulting X and Z transformation matrices (using [`McalCopyResult`](../../Reference/cal/McalCopyResult.md)).
   > **Note:** The number of transformation matrix objects in the array passed to [`HandMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md) must be the same as[`EyeMatrix3dgeoIdArrayPtr`](../../Reference/cal/McalCalculateHandEye.md).
3. Allocate the result buffer using [`McalAllocResult`](../../Reference/cal/McalAllocResult.md) with [`M_CALCULATE_HAND_EYE_RESULT`](../../Reference/cal/McalAllocResult.md).
4. Call [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) and retrieve your X and Z transformation matrices using [`McalCopyResult`](../../Reference/cal/McalCopyResult.md) with [`M_MATRIX_X`](../../Reference/cal/McalCopyResult.md) and [`M_MATRIX_Z`](../../Reference/cal/McalCopyResult.md), respectively.
5. At runtime, using the same robotics setup:
   1. Grab a point cloud.
   2. Optionally, transform the point cloud so that points are represented in the robot coordinate system, using [`M3dimMatrixTransform`](../../Reference/3dim/M3dimMatrixTransform.md). For eye-in-hand calibration, use a composition of the A and X transformation matrices ([`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with [`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)), and for eye-to-hand calibration, only use the X transformation matrix.
   3. Perform the required analysis.
   4. If you didn't transform the point cloud in step b, transform positional results into the robot coordinate system using the above-mentioned transformations.
   5. Have the robot move the robot arm to the calculated position.
6. Free all your allocated objects, using [`McalFree`](../../Reference/cal/McalFree.md) and [`M3dgeoFree`](../../Reference/3dgeo/M3dgeoFree.md), unless [`M_UNIQUE_ID`](../../Reference/cal/McalAlloc.md) was specified during allocation.
