---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Calibration_and_one_or_many_fixtures
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Calibration and one or many fixtures"
---

# Calibration and one or many fixtures

In Aurora Imaging Library, fixturing is implemented using the relative coordinate system of an image. For each instance of an object, you move the relative coordinate system of the image to a fixed positional and angular offset from the object, based upon some previous analysis. The relative coordinate system is a coordinate system defined in real-world units in the absolute (world) coordinate system of an image. You define both the absolute and relative coordinate systems using the Aurora Imaging Library Camera Calibration module. The absolute coordinate system is established when calibrating the camera setup. If you don't want to calibrate your camera set up, you can still use fixturing, using the default uniform calibration context.

Regardless if you use the default calibration context or perform a full camera calibration, you can also set up multiple relative coordinate systems for multiple fixtures using child buffers.

## If you don't really need to work in real world units except for fixturing purposes

If you don't really need to work in real-world units except for fixturing purposes, you can associate the default uniform camera calibration context with your image. This sets the absolute coordinate system to have the same origin and orientation as the pixel coordinate system and sets the world units equal to 1 pixel. For more information on calibrating and defining more meaningful pixel-to-world mappings, see [Camera calibration modes](../C28_Calibration/S05_Calibration_modes_overview.md).

The following code snippet shows how to associate a uniform camera calibration context with an image of a set of nails, so that you can use the relative coordinate system for fixturing. The example then fixtures the relative coordinate system to each instance of the nail, before placing the Aurora Imaging Library Measurement module's search region with respect to the relative coordinate system and then performing the analysis as illustrated in the following animation.

*[Image: fixture_intro]*

> **Code example:** [userguide.building_an_application.child_buffers_regions_of_interest_and_fixturing01](userguide.building_an_application.child_buffers_regions_of_interest_and_fixturing01)

## Multiple relative coordinate systems

You can have more than one relative coordinate system in an image by creating child buffers. A child buffer can have the same size as the its parent buffer or a portion of it. Since each child buffer receives a copy of the camera calibration information stored in the parent, they initially have a copy of the parent's relative coordinate system that takes into account the different origin in the pixel coordinate system.

*[Image: cal_fixture_relative_coord_parent.png]*

You can displace the relative coordinate system of the child buffer; this does not affect the relative coordinate system of the parent. This allows you to analyze different aspects of an image with respect to different relative coordinate systems, while preserving the location of the relative coordinate system of the parent buffer.

In addition, if you change the relative coordinate system of the parent, the relative coordinate system of each of the child buffers will also be displaced. There exists a rigid link between the position and orientation of the parent and child buffers' relative coordinate systems. Consequently, if you have multiple child buffers, their relative coordinate system will be changed to maintain the same pose that was previously established with respect to the parent coordinate system.

The following animation illustrates the relationship between the relative coordinate systems of the parent and its children.

*[Image: cal_fixture_displace_relative_coord_parent]*

Having multiple relative coordinate systems allows you to perform operations with respect to different reference points. If you have an image that contains a complex object with multiple features that you want to analyze, you can fixture the relative coordinate system of the parent to the object, and then fixture each child buffer's relative coordinate system to one of the object's features. This ensures that when you locate the object in general and move the relative coordinate system of the parents buffer, the relative coordinate system of the child buffers are relocated to the correct position, due to the rigid link between the coordinate systems. This allows the complex features of the object to be easily analyzed when the parent's relative coordinate system is fixtured to a new instance of the object.

*[Image: cal_fixture_child_buf_example.png]*
