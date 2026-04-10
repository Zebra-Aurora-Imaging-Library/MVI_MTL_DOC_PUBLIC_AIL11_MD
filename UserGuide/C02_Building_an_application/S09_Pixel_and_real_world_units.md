---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Pixel_and_real_world_units
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Pixel and real world units"
---

# Pixel and real-world units

In Aurora Imaging Library, there are two types of units that you can use for setting information and retrieving results: **pixel units** and **real-world units** (for example, inches or centimeters). Real-world units are called world units for simplicity. Pixel units are directly related to the image and are always available; however, to work in world units, you must perform **camera calibration** to establish the relationship between the pixel coordinate system and the real world.

> **Note:** Note that Aurora Imaging Library Lite does not support world units.

## Pixel units and the pixel coordinate system

When working in pixel units, you are using the pixel coordinate system. The center of the top-left pixel of the image corresponds to the origin (0,0). The X-axis follows the first row of pixels, and the Y-axis follows the first column of pixels. The following image shows the pixel coordinate system; the dots indicate the center of the pixels.

*[Image: cal_pixel_coordinate_system.png]*

For Aurora Imaging Library modules that support subpixel accuracy, it is important to keep in mind that since the center of the pixel is used as its reference position, the top-left corner of the first pixel is considered (-0.5, -0.5), and the bottom-right corner of the last pixel is (sizeX - 0.5, sizeY - 0.5).

When working in pixel units, angles are interpreted and measured in degrees with respect to the pixel coordinate system; positive angles are always interpreted to be in a counter-clockwise direction from the initial side.

*[Image: misc_pixel_angle_convention.png]*

Note that since the Y-axis of an image is oriented downwards, the angles in Aurora Imaging Library are inverted from mathematical convention. In certain cases, you might want to convert angles from Aurora Imaging Library convention to mathematical convention: the negative value of an angle in Aurora Imaging Library convention is the true value of the same angle in mathematical convention. For example, 45° in Aurora Imaging Library convention is the same as -45° in mathematical convention.

For more information on the pixel coordinate system, using subpixel accuracy, and angle conventions in Aurora Imaging Library, see [Pixel conventions and subpixel accuracy](../C23_Data_buffers/S15_Pixel_conventions.md).

## World units and world coordinate systems

In Aurora Imaging Library, you can set and retrieve positional information in real-world units (for example, inches or centimeters). To do so, you must specify the relationship between the pixel coordinate system and the real world through camera calibration. This involves choosing a fixed location (that is, a position and orientation) in the real world from which to make this mapping, and the distance that will represent one unit. Together, these define the **absolute real-world coordinate system** (or for simplicity, the **absolute coordinate system**). You can then map pixels to the location that they represent in the real world with reference to the absolute coordinate system.

After calibrating, you should further specify a reference in the absolute coordinate system, known as the **relative coordinate system**, from which you want to specify settings and retrieve results. Unlike the absolute coordinate system, which is immoveable, you can move the relative coordinate system to any required location. This allows you to specify settings and retrieve information with respect to any required location.

*[Image: cal_pixel_abs_rel_coordsys.png]*

Moving the relative coordinate system can be useful, for example, if an object appears at a number of different positions in your image, and you want to apply the same measurement or inspection process to every instance of the object. You can fix the relative coordinate system with respect to each object in succession, and apply your process to every instance of the object. This process is known as fixturing; for more information, see [Fixturing an object with the relative coordinate system](S10_Child_buffers_regions_of_interest_and_fixturing.md).

If you then need to transform results with respect to a location in the real world other than the absolute coordinate system (for example, the camera), Aurora Imaging Library allows you to define other coordinate systems in the real world and transform locations between them. For more information, see [Coordinate systems](../C28_Calibration/S04_Coordinate_systems.md).

Most Aurora Imaging Library modules support real-world units and can take settings and return results with respect to the relative coordinate system. For a complete list of Aurora Imaging Library modules that support this functionality, see [Working with real-world units](../C28_Calibration/S09_Working_with_realworld_units.md).

### Camera calibration

Camera calibration relates pixels to real locations and distances. For example, you can create the following relationship between a pixel and its size and location in the real world.

*[Image: cal_uniform_mapping.png]*

Uniform camera calibration is the simplest technique that you can use to calibrate your camera setup. To use this technique, each pixel in the image should represent the same sized area in the real world, your camera should be placed directly above the subject in the image, and lens distortion should be negligible. To perform a uniform camera calibration, use [`McalUniform`](../../Reference/cal/McalUniform.md). This mapping can be any combination of scaling, rotation, and translation.

The following illustrations show the position of the pixel and absolute coordinate systems after three example uniform camera calibrations. Note that the pixel coordinate system is defined with respect to the absolute coordinate system (not vice versa) because the absolute coordinate system is fixed in the world.

*[Image: cal_uniform_calibration.png]*

To handle more complex setups and various types of distortion, Aurora Imaging Library supports a variety of different camera calibration modes, including 3D-based camera calibration modes. For more information on camera calibration, see [Camera calibration modes](../C28_Calibration/S05_Calibration_modes_overview.md).
