---
doctype: UserGuide
part: "3D processing and analysis"
chapter: Common_tasks_with_3D_data
section: Item_picking_robot
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / Common_tasks_with_3D_data / Item picking robot"
---

# Item picking robot

If you need a robot to pick up objects, you must establish the translation and rotation needed to reach the objects.

## Establishing the relationship between the camera and the robot

To pass positional results to the robot, the coordinates should be in the robot's coordinate system. To do so, you need to perform hand-eye calibration. This involves establishing the transformation between the working coordinate system and the robot coordinate system, and then applying this transformation to the point cloud before analysis or to the positional results after analysis. Use [`McalCalculateHandEye`](../../Reference/cal/McalCalculateHandEye.md) to calculate the required transformations between coordinate systems in a robotics setup. For more information, see [Hand-eye calibration](../C42_Grabbing_from_3D_sensors/S09_Using_a_different_working_coordinate_system.md).

## Determining the pose of the object(s)

You can use many of the 3D modules to determine the pose of an object. To decide which module to use, refer to [Retrieving the orientation and center point or position of a 3D object](S03_Retrieving_the_orientation_and_center_point.md). For a discussion of other considerations when working with 3D data, see [Implementation considerations](S02_Implementation_considerations.md).

Your robot will require the location at which to pick up the object. This involves retrieving the matrix that defines the required transformation and then getting the transformation in the format required by the robot. The following subsections describe this procedure.

## Retrieving the matrix

You can obtain the translation and rotation needed to move the robot to the object in the form of a transformation matrix, which, when applied, transforms coordinates appropriately. Typically, for a robot picker application, you must obtain the inverse of a fixturing matrix. When not inverted, the fixturing matrix provides the transformation values that can move the working coordinate system to the object, whereas the inverse gives values for moving the robot to the object's found location in the point cloud.

In most cases, obtaining the inverse fixturing matrix is a two-step process. First, use an [`M...CopyResult`](../../Reference/3dmet/M3dmetCopyResult.md) function to obtain a fixturing matrix (for instance, [`M3dmodCopyResult`](../../Reference/3dmod/M3dmodCopyResult.md) with [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md).) Then, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with [`M_INVERSE`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to reverse the direction of the transformation.

> **Note:** Note that for surface results from a 3D model finder operation, [`M_OCCURRENCE_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md) is equivalent to the inverse of [`M_FIXTURING_MATRIX`](../../Reference/3dmod/M3dmodCopyResult.md).

Note that if you want to pick up objects that are rotated along a certain axis, you might need to add an additional transformation, depending on how the fixturing matrix is defined. For example, the fixturing matrix of a cylinder model occurrence defines the transformation that aligns the positive Z-axis with the occurrence's central axis. If you want to pick up cylinders that are also rotated along the X- or Y-axis, set up a transformation matrix that contains the rotation to the required axis (for example, [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with [`M_ROTATION_...`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md)). Then, use [`M3dgeoMatrixSetTransform`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) with [`M_COMPOSE_TWO_MATRICES`](../../Reference/3dgeo/M3dgeoMatrixSetTransform.md) to add it.

Also note that you should compose the resulting matrix with the matrix that defines the transformation between the working coordinate system and the robot coordinate system (if not applied to the point cloud before analysis).

## Getting the transformation in the format required by the robot

Once you have a matrix that represents the required transformations, retrieve the translations and rotations to pass to the robot controller. These are displacements in X, Y, and Z and rotation angles in degrees, respectively. Use [`M3dgeoMatrixGetTransform`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) with [`M_TRANSLATION`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) and [`M_ROTATION_...`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) to get the required values from the transformation matrix.

A robot rotates by performing a sequence of rotations about the axes of the robot coordinate system (also known as Roll-Pitch-Yaw rotation). When using [`M3dgeoMatrixGetTransform`](../../Reference/3dgeo/M3dgeoMatrixGetTransform.md) to retrieve the required set of rotation angles, you must specify the correct transformation type that corresponds to the order in which the robot will perform them, since the rotations are successive. You can determine the required rotation order by checking the convention used by your robot's manufacturer.

The following table shows the notation used for each rotational axis by the robot controllers of various robot manufacturers.

| Robot type | Robot controller representation |
| --- | --- |
| ABB | The orientation is converted by the robot controller to a quaternion. |
| DENSO | Roll | Pitch | Yaw |
| Epson | W | V | U |
| Fanuc | W | P | R |
| KUKA | C | B | A |
| Staubli | Rx | Ry | Rz |

Once you have retrieved the translation and/or rotation values from the transformation matrix, you can then pass the values to the robot (using [`McomSendPosition`](../../Reference/com/McomSendPosition.md)).
