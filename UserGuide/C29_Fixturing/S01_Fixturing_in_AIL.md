---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: Fixturing_in_AIL
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / Fixturing in AIL"
---

# Fixturing overview

Some production methods use mechanical fixtures or robots to present a part to be inspected. In those cases, the vision system always sees the part in the same position and orientation. However, in many cases, the objects are not physically fixtured and move in the field of view; in these cases, Aurora Imaging Library allows you to fixture the objects in software.

In Aurora Imaging Library, fixturing is implemented using the relative coordinate system of an image. For each instance of an object, you move the relative coordinate system of the image to a fixed positional and angular offset from the object, based upon some previous analysis. If you specify settings for the processing and analysis of the object relative to this coordinate system, you won't need to re-adjust these settings for each object. Since the distance and orientation between the relative coordinate system and the target object are kept the same, the coordinates of the target object are the same. This also means that you also avoid transforming results obtained from different instances of an object for comparison purposes.

The relative coordinate system is a coordinate system defined in real-world units in the absolute (world) coordinate system of an image. You define both the absolute and relative coordinate systems using the Aurora Imaging Library Camera Calibration module. The absolute coordinate system is established when calibrating the camera setup. If you don't really need to work in real-world units except for fixturing purposes, you can associate the default uniform camera calibration context with your image; this sets the absolute coordinate system to have the same origin and orientation as the pixel coordinate system and sets the world units equal to 1 pixel.

Fixturing is a useful precursor, for example, to using the Aurora Imaging Library Measurement module because the box search region must be placed very precisely around the object characteristics to analyze. If the object is not at a fixed location and fixturing is not used, you would need to explicitly move the box search region at different locations. With fixturing, the box search region placement settings can be left untouched; the box will be moved automatically according to the movement of the relative coordinate system.

The following animation illustrates the general process of fixturing, where the relative coordinate system is fixtured to each instance of the nail, before placing the Aurora Imaging Library Measurement module's search region with respect to the relative coordinate system and then performing the analysis.

*[Image: fixture_nail_model.png]*

*[Image: fixture_intro]*

Choosing the new location (position and angle) for the relative coordinate system is crucial for fixturing. For flexibility, the new location can be specified as a combination of a calculated reference location for the object and a preestablished offset to apply to this reference location. This is useful if the reference location is not at an ideal location for the relative coordinate system.

When fixturing, you can also restrict processing and analysis to a region of the object, using a region of interest in the image buffer. If the region of interest is defined with respect to the relative coordinate system, it will automatically be moved when the relative coordinate system moves.
