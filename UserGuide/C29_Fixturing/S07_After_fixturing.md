---
doctype: UserGuide
part: "2D related information"
chapter: Fixturing
section: After_fixturing
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / fixturing / After fixturing"
---

# After fixturing

Fixturing is useful if, after moving the relative coordinate system, you perform and/or retrieve results from analysis operations in world units. It is also useful if you perform processing/analysis operations restricted to an image buffer's region of interest defined in world units. Essentially, you need to work in world units for the units to be with respect to the relative coordinate system. For a list of the modules that support input settings and returned results in world units, and those that support regions of interest defined in world units, see [Working with real-world units](../C28_Calibration/S09_Working_with_realworld_units.md).

If you can't work in world units because the Aurora Imaging Library module does not support it or you are using a third-party tool, it might still be useful to fixture the relative coordinate system to an object. In this case, you could use the relative coordinate system to transform the image so that the object has the same coordinates in the pixel coordinate system as it has in a training image.

Note that if you don't need to work in real-world units, you can associate the default uniform camera calibration context with your image; this sets the absolute world coordinate system to have the same origin and orientation as the pixel coordinate system, and sets world units equal to 1 pixel.

## Inputting values in real-world units

The modules that support settings in world units require that you set the working unit to [`M_WORLD`](../../Reference/blob/MblobControl.md) and you use a calibrated image, before they interpret their settings as being in world units. Otherwise, by default, they interpret their settings as being in pixel units, even if a module natively works in world units. The function and constant to set the working unit depends on the module; for a list of these, see [Input settings in world units](../C28_Calibration/S09_Working_with_realworld_units.md).

## Results in world units

When you use a calibrated image, the modules that support returning results in world units typically do so by default. In this case, to have results returned in pixel units, you must set the output units to pixels; to do so, call the module's control function with [`M_RESULT_OUTPUT_UNITS`](../../Reference/blob/MblobControl.md) set to [`M_PIXEL`](../../Reference/blob/MblobControl.md). For more information, see [Getting results in world units](../C28_Calibration/S09_Working_with_realworld_units.md).

## Restricting processing/analysis to a region of interest defined in world units

If supported by the function, you can restrict processing and analysis to an image buffer's region of interest; define the region of interest for the image buffer using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). If the region of interest is defined in world units, it will move according to the relative coordinate system of the image. So, if you fixture the relative coordinate system to an object, the region of interest will move accordingly, allowing you to restrict processing/analysis to the required object characteristics.

## Transforming your images after fixturing

Even if you can't continue working in world units, fixturing can simplify your application when analyzing/processing objects that don't have a fixed location. In these cases, you can transform grabbed images so that the target object is at the same location as in a training image. This allows you to keep the same analysis setting for all instances of an object without taking into account the new location of the object. This also allows you to make comparisons between expected results and obtained results without conversion.

To transform grabbed images so that the target object is at the same location as in a training image and then perform required analysis/processing on the object, do the following:

1. Allocate an image buffer to store the transformed version of the grabbed image; it will be used as the working image buffer. Allocate it to have the same size as the training image.
2. Copy the camera calibration information of the training image to the working image, using [`McalAssociate`](../../Reference/cal/McalAssociate.md).
3. For each instance of the object in each grabbed image:
   1. Fixture the relative coordinate system to the target object in the grabbed image.
   2. Create a transformed version of the grabbed image and save it in the working image buffer, using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_USE_DESTINATION_CALIBRATION`](../../Reference/cal/McalTransformImage.md). This transforms the grabbed image so that its relative coordinate system is placed at the same location as the current relative coordinate system of the working image buffer.
      *[Image: cal_using_dest_mode_when_transforming.png]*
   3. Perform the required processing/analysis operations on the working image buffer.

For example, to transform a grabbed image so that an object has the same size and position as in a training image:

> **Code example:** [userguide.fixturing_in_ail.after_fixturing01](userguide.fixturing_in_ail.after_fixturing01)
