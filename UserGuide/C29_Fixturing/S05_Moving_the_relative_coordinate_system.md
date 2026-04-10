---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Moving_the_relative_coordinate_system
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Moving the relative coordinate system"
---

# Moving the relative coordinate system

There are five ways to fixture the relative coordinate system to an object:

- Move it to the position and angle of a specified occurrence of an Aurora Imaging Library Pattern Matching or Model Finder model.
- Move it to the position and angle of a specified Metrology reference frame feature.
- Move it to a position and angle that are with respect to a relative coordinate system.
- Move it to a position and angle that are with respect to the absolute coordinate system.
- Move it to a position and angle using one of the above techniques, and then copy its position and angle to other images that require the same relative coordinate system displacement.

## Moving using a model

The easiest way to move the relative coordinate system for fixturing is to use an Aurora Imaging Library Pattern Matching or Model Finder model that is at a fixed positional and angular offset from the object to analyze or process. In this case, search for the model. Then, to move the relative coordinate system to the reference location of a found occurrence, call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md), the Model Finder/Pattern Matching result buffer, and the index of the occurrence.

For example, to move the relative coordinate system of an image to the reference location of the first occurrence of a pattern matching model:

> **Code example:** [userguide.fixturing_in_ail.moving_the_relative_coordinate_system01](userguide.fixturing_in_ail.moving_the_relative_coordinate_system01)

You should ensure that the Pattern Matching or Model Finder model used to establish the location has good, unique features so that there is no ambiguity over where to move the relative coordinate system.

*[Image: fixture_example1.png]*

By default, when using this technique, the relative coordinate system is placed at the reference location of the specified model occurrence. You can, however, set the relative coordinate system at an offset from this location. See [Fixturing offset](S06_Fixturing_offset.md).

For more information about defining a Pattern Matching model, see [Pattern matching](../C07_Pattern_matching/ChapterInformation.md). For more information about defining a Model Finder model, see [Geometric Model Finder](../C08_Geometric_Model_Finder/ChapterInformation.md).

## Moving using a metrology reference frame

You can specify the new location for the relative coordinate system using the position and angle of a Metrology reference frame. To do so, call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md), the identifier of a Metrology result buffer, and the index of the reference frame feature to use. Aurora Imaging Library will move the relative coordinate system to the location, offset, and angle specified by the constructed Metrology reference frame.

*[Image: MetResultFixturing.png]*

The constructed frames used in metrology cannot be used directly in other modules, and so moving the relative coordinate system to the location specified by the constructed frame could be useful to, for example, process the same image using a different module after having processed it with Metrology.

> **Code example:** [userguide.fixturing_in_ail.moving_the_relative_coordinate_system05](userguide.fixturing_in_ail.moving_the_relative_coordinate_system05)

By default, when using this technique, the relative coordinate system is placed at the reference location of the specified Metrology global or local frame result. You can, however, set the relative coordinate system at an offset from this location. See [Fixturing offset](S06_Fixturing_offset.md).

## Moving using a position and angle expressed in a specified relative coordinate system

You can have Aurora Imaging Library establish the new location for the relative coordinate system based on an explicitly specified position and angle that are expressed in a specified relative coordinate system. In this case, the position and angle would correspond to a calculated reference location for the object to analyze or process.

To move the relative coordinate system using this technique, call [`McalFixture`](../../Reference/cal/McalFixture.md) with all of the following:

- [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md).
- The position and angle expressed in a relative coordinate system.
- If different from the target's current relative coordinate system, specify the calibrated image/camera calibration context with the relative coordinate system of the position and angle. Otherwise, use [`M_DEFAULT`](../../Reference/cal/McalFixture.md) (for [`CalOrLocSourceId`](../../Reference/cal/McalFixture.md)).

Internally, [`McalFixture`](../../Reference/cal/McalFixture.md) converts the specified position and angle to be with respect to the absolute coordinate system. It then uses the converted position and angle to place the new relative coordinate system.

The following animation illustrates the above mentioned process.

*[Image: fixture_example_two]*

The animation above shows the position and angle of the relative coordinate system before and after calling [`McalFixture`](../../Reference/cal/McalFixture.md). When you call [`McalFixture`](../../Reference/cal/McalFixture.md) with the position (X2, Y2) and angle Theta2 expressed in the current relative coordinate system, the position (X3, Y3) and angle Theta3 for the new relative coordinate system are internally calculated in the absolute coordinate system. Finally, the relative coordinate system is moved to the new position and angle in the absolute coordinate system.

> **Code example:** [userguide.fixturing_in_ail.moving_the_relative_coordinate_system02](userguide.fixturing_in_ail.moving_the_relative_coordinate_system02)

By default, the relative coordinate system is placed at the calculated position and angle. You can, however, set the relative coordinate system at an offset from the calculated position and angle. See [Fixturing offset](S06_Fixturing_offset.md).

## Moving using a position and angle that are with respect to the absolute coordinate system

If you know the position and angle for the new relative coordinate system with respect to the absolute coordinate system, you can set the new relative coordinate system directly, using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) or [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

For example, if in the previous example, you knew the position (X3, Y3) and angle Theta3 in the absolute coordinate system, you could call [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) as follows:

> **Code example:** [userguide.fixturing_in_ail.moving_the_relative_coordinate_system03](userguide.fixturing_in_ail.moving_the_relative_coordinate_system03)

## Moving using a point and direction point that are with respect to the relative coordinate system

You can specify the location of the new relative coordinate system using two points. The first point defines the position, and the vector going from the first point to the second point defines the angle and direction of the X-axis.

To move the relative coordinate system using this technique, call [`McalFixture`](../../Reference/cal/McalFixture.md) with all of the following:

- [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md).
- [`M_POINT_AND_DIRECTION_POINT`](../../Reference/cal/McalFixture.md).
- The first and second points expressed in the relative coordinate system.
  > **Note:** Note that you can only pass X and Y-coordinates for the points. So, essentially you are moving the relative coordinate system along its current XY-plane.

The following animation illustrates how to move the relative coordinate system using two points in the current relative coordinate system.

*[Image: fixture_point_direction_point]*

## Moving the relative coordinate system in 3D

Typically when using [`McalFixture`](../../Reference/cal/McalFixture.md), you can only move the relative coordinate system along its XY-plane (Z = 0) and you can only rotate the relative coordinate system around the Z-axis or an axis parallel to it (the direction and angle of the Z-axis remains unchanged).

The animation below shows a 3D view on how the relative coordinate system is moved using [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_POINT_AND_DIRECTION_POINT`](../../Reference/cal/McalFixture.md).

*[Image: fixture_point_direction_point_ThreeD]*

This rule does not apply if you are using [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LASER_3DMAP`](../../Reference/cal/McalFixture.md) or [`M_RESULT_POINT_CLOUD_3DMAP`](../../Reference/cal/McalFixture.md); these allow you to move the relative coordinate system in 3D. If you need to move the relative coordinate system in 3D and can't use [`McalFixture`](../../Reference/cal/McalFixture.md), you can use [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md).

## Copying the new relative coordinate system to other images

When working with multiple images of the same scene taken from different view points, you might want to move the relative coordinate system in one image, and then copy the position and angle of the new relative coordinate system to the other images. To perform the copy, iteratively call [`McalFixture`](../../Reference/cal/McalFixture.md) with all of the following:

1. Call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md) as the [`Operation`](../../Reference/cal/McalFixture.md) and [`M_SAME_AS_SOURCE`](../../Reference/cal/McalFixture.md) as the [`LocationType`](../../Reference/cal/McalFixture.md).
2. Specify the source calibrated object to be copied in [`CalOrLocSourceId`](../../Reference/cal/McalFixture.md).
3. Specify the destination of the copy operation in [`DstCalibratedAilObjectId`](../../Reference/cal/McalFixture.md).

This operation does not copy the absolute coordinate system information. It assumes that the absolute coordinate systems of the different images are the same, although the mapping between the world and pixel coordinate systems might be different.
