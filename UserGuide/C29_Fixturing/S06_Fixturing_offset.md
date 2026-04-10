---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Fixturing_offset
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Fixturing offset"
---

# Fixturing offset

When fixturing, you might need to offset the relative coordinate system from the default reference location that you can automatically establish from analysis/processing. There are different ways to apply a fixturing offset depending on the reason that you want to apply an offset:

- Fixturing offset for convenience.
- Fixturing offset to decouple the reference location from the object to analyze.

## Fixturing offset for convenience

If placing the relative coordinate system at the default reference location is not convenient, you can have Aurora Imaging Library automatically apply a required offset to the default reference location. This can be useful, for example, if the default reference location would cause you to deal with negative coordinates during analysis. In this case, there are two ways to automatically apply a fixturing offset.

- If using a model occurrence to displace the relative coordinate system, you can offset the reference location of the model to the required location prior to searching for occurrences of it.
- If using [`McalFixture`](../../Reference/cal/McalFixture.md) to move the relative coordinate system to a specific location, regardless of how you pass the reference location, you can use a fixturing offset object.

### Moving the reference location prior to searching

If using a model occurrence to displace the relative coordinate system and you need to apply an offset from the occurrence's center, you can move the reference location of the model so that the returned location of each occurrence is at the required offset from the occurrence.

To move the reference location of a pattern matching model, use [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE...`](../../Reference/pat/MpatControl.md). To move the reference location of a Model Finder model, use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_REFERENCE_X`](../../Reference/mod/MmodControl.md), [`M_REFERENCE_Y`](../../Reference/mod/MmodControl.md), and [`M_REFERENCE_ANGLE`](../../Reference/mod/MmodControl.md).

*[Image: fixture_nail_measurement.png]*

The following code snippet shows how to move the reference location of a model and then place the relative coordinate system at the reference location of a model occurrence:

> **Code example:** [userguide.fixturing_in_ail.fixturing_offset01](userguide.fixturing_in_ail.fixturing_offset01)

Note that moving the relative coordinate system using this technique can result in the returned locations being far from the actual occurrences of the model. This might not be ideal when using the locations not only to place the relative coordinate system, but also to, for example, draw the locations of the occurrences.

### Moving the relative coordinate system to a logical offset from a reference location

You can also add a fixturing offset to the reference location using a fixturing offset object. Besides being useful if you are not using a model occurrence, it can also be useful if using a model occurrence. In the latter case, a fixturing offset object offers the advantage of keeping the model's reference location at its original location, while specifying an offset for the relative coordinate system. A fixturing offset object is an Aurora Imaging Library object that stores a positional and angular offset.

You can use the following steps to allocate and set up a fixturing offset object:

1. Allocate a fixturing offset object using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_FIXTURING_OFFSET`](../../Reference/cal/McalAlloc.md).
2. In a training image, ensure that the relative coordinate system is at a required location in the image; if it isn't, move it using [`McalRelativeOrigin`](../../Reference/cal/McalRelativeOrigin.md) or [`McalSetCoordinateSystem`](../../Reference/cal/McalSetCoordinateSystem.md). Typically, you would place the relative coordinate system at the required offset from a location that you can automatically establish in the training image (the reference location).
3. Establish a location in the training image that has the required positional and angular offset from the relative coordinate system. This is typically the reference location in the training image.
4. Call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md), the fixturing offset object, the training image, and the established location. Aurora Imaging Library will learn the positional and angular offset that the established location has from the relative coordinate system, and will save it in the fixturing offset object.
5. At runtime, call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md) and the fixturing offset object.

The following animation offers a visual representation of the above steps.

*[Image: fixture_fixturing_offset_two_placing_rel_coord_sys]*

In the example above, we want to obtain the exact location and angle of the bar code with respect to the board because we want to check if the location and angle are correct compared to the schematics.

The following code snippet illustrates how to move the relative coordinate system to a preestablished offset from the reference location of a model occurrence, without moving the model's default reference location:

> **Code example:** [userguide.fixturing_in_ail.fixturing_offset02](userguide.fixturing_in_ail.fixturing_offset02)

## Fixturing offset to decouple the reference location from the object to analyze

You can also use a fixturing offset object to decouple the reference location from the location of the object to analyze. This might be useful, for example, if you are testing which is the best model to use to establish the reference location. In this case, when setting up the fixturing offset object, you can keep the relative coordinate system fixed at an arbitrary location from the object to analyze, while you try setting up the fixturing offset object on the different reference locations. By keeping the relative coordinate system fixed, you don't have to keep updating the region in which the object to analyze is located with respect to the new relative coordinate system; you just have to learn the offset from the new reference location. While setting up the fixturing offset object, leave the relative coordinate system at its current position or place it at any arbitrary location. This assumes that the actual location of the relative coordinate system is not crucial to the analysis process as long as it is at a fixed location from the object to analyze (for example, you want to fixture the location from which to read a bar code but you don't need to compare the location of the bar code against specifications).

For this case, you can use the following steps to allocate and set up a fixturing offset object:

1. Allocate a fixturing offset object using [`McalAlloc`](../../Reference/cal/McalAlloc.md) with [`M_FIXTURING_OFFSET`](../../Reference/cal/McalAlloc.md).
2. Without moving the relative coordinate system, establish a location in the training image. Typically, this is a location that you can automatically establish in the training image (the reference location). This can be, for example, the reference location of a model or a pattern, or the center of gravity of a blob.
3. Call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md), the fixturing offset object, the training image, and the established location. Aurora Imaging Library will learn the positional and angular offset that the established location has from the relative coordinate system, and saves it in the fixturing offset object.
4. At runtime, call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_MOVE_RELATIVE`](../../Reference/cal/McalFixture.md) and the fixturing offset object.

The following animation offers a visual representation of the above steps.

*[Image: fixture_fixturing_offset_one]*

In the example above, we want to read the bar code but we don't care about its precise location on the board; we just want to be able to find the bar code and read it, regardless of its angle or location with respect to the board.

The following code snippet illustrates how to move the relative coordinate system to a preestablished offset from the reference location of a model occurrence, without moving the model's default reference location or the relative coordinate system during design-time:

> **Code example:** [userguide.fixturing_in_ail.fixturing_offset04](userguide.fixturing_in_ail.fixturing_offset04)

## Establishing the positional and angular offset for the fixturing offset object

As mentioned above, to establish the positional and angular offset for the fixturing offset object, you must call [`McalFixture`](../../Reference/cal/McalFixture.md) with [`M_LEARN_OFFSET`](../../Reference/cal/McalFixture.md), the fixturing offset object in the training image, and the established reference location that has the required positional and angular offset from the relative coordinate system. There are several ways of specifying the reference location such as:

- You can define a Model Finder or Pattern Matching model in the training image, and then pass the Model Finder/Pattern Matching context and model index to [`McalFixture`](../../Reference/cal/McalFixture.md). Aurora Imaging Library will extract the offset of the reference location of the model from the relative coordinate system of the training image.
- You can search for a Model Finder or Pattern Matching model in the training image, and then pass the Model Finder/Pattern Matching result buffer and model occurrence index to [`McalFixture`](../../Reference/cal/McalFixture.md). Aurora Imaging Library will extract the offset of the model occurrence from the relative coordinate system of the training image.
- You can establish the location of the Metrology reference frame in the training image and then pass the identifier of a Metrology result buffer and the index of a Metrology reference frame feature to [`McalFixture`](../../Reference/cal/McalFixture.md). Aurora Imaging Library will extract the offset and angle of the reference frame feature from the relative coordinate system of the training image.
- You can pass the training image and a preestablished position and angle in this image to [`McalFixture`](../../Reference/cal/McalFixture.md). Aurora Imaging Library will extract the offset of the preestablished position and angle from the relative coordinate system of the training image.

> **Code example:** [userguide.fixturing_in_ail.fixturing_offset03](userguide.fixturing_in_ail.fixturing_offset03)

To ensure that the appropriate offset has been established, you can draw a line from the current origin of the relative coordinate system to the position that must have been passed to establish the offset. To do so, use [`McalDraw`](../../Reference/cal/McalDraw.md) with the fixturing offset object and [`M_DRAW_FIXTURING_OFFSET`](../../Reference/cal/McalDraw.md). This also draws a small arrow at this position, which illustrates the angle that must have been passed.
