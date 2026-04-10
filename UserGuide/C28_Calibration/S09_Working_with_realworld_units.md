---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Working_with_realworld_units
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Working with realworld units"
---

# Working with real-world units

One of the main advantages of calibration is the ability to work in real-world units, so that locations in your images correspond to real-world locations.

The following table lists the modules that support input and output units in world units, and those that support regions of interest defined in world units, assuming that the image used is associated with a camera calibration context:

| Module [^ExceptionsID2] | World native processing | World input units | World output units | Region of interest |
| --- | --- | --- | --- | --- |
| M3dmap | Y | Y | Y | N |
| M3dmeas | Y | Y | Y | N |
| Magm | N | N | N | N |
| Mbead | N | Y | Y | N |
| Mblob | N | Y[^ExceptionsID] | Y | Y |
| Mclass | N | N | N | Y |
| Mcode | N | Y | Y | Y |
| Mcol | N | N | N/A | Y |
| Mdlocr | N | N | Y | Y |
| Mdmr | N | N | Y | Y |
| Medge | Y | N | Y | N |
| Mgra | N/A | Y | N/A | N |
| Mim | N | N | Y | N |
| Mmeas | N | Y | Y | Y |
| Mmet | Y | Y | Y | N |
| Mmod | Y | N | Y | N |
| Mocr | N | N | Y | Y |
| Mpat | N | N | Y | Y |
| Mreg | N | N | N | N |
| Mstr | N | N | Y | Y |

[^ExceptionsID2]: For the 3D modules that work with point cloud containers and/or 3D geometry objects, all 3D positional information (coordinates) are considered real-world (all source data is treated as if it is natively calibrated).

[^ExceptionsID]: Some exceptions.

## Getting results in world units

If an image is calibrated, most results obtained from it will be returned in world units by default. When a result is returned in world units, it is returned with respect to the relative coordinate system. If a result can be returned in either world or pixel units, it will be stated in the function description. You can use a module's [`M...Control`](../../Reference/3dmap/M3dmapControl.md) function with [`M_RESULT_OUTPUT_UNITS`](../../Reference/mod/MmodControl.md) to specify that results should be returned in pixel units instead. Note that if you set this control type to [`M_WORLD`](../../Reference/blob/MblobControl.md) but your results are not obtained from a calibrated image, the module's corresponding [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md) function will generate an error when you try to retrieve results.

You can use [`McalTransformCoordinate`](../../Reference/cal/McalTransformCoordinate.md), [`McalTransformCoordinateList`](../../Reference/cal/McalTransformCoordinateList.md), or [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) to convert coordinates from their pixel to their world values (or vice versa). The latter two functions allow you to convert an entire list of coordinates at once for better efficiency. [`McalTransformCoordinate3dList`](../../Reference/cal/McalTransformCoordinate3dList.md) also allows you to convert a coordinate in the relative coordinate system (or any other world coordinate system) to its corresponding value in any other world coordinate system.

You can use [`McalTransformResult`](../../Reference/cal/McalTransformResult.md) to convert a specific non-positional result (a length, angle, or area) from its pixel to its world value (or vice versa). Note, however, that this function uses the average pixel size to perform the conversion; results will be more accurate if you first correct the image.

You can use [`McalTransformResultAtPosition`](../../Reference/cal/McalTransformResultAtPosition.md) to convert an angle from its pixel value to its world value or vice versa at a specified position. In the presence of distortion, some results are meaningless when converted from real-world to pixel units unless considered at a specific position in the image. For example, if a Model Finder model appears warped in a target image, but the camera calibration context of the image compensates for this during the model search, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system, unless converted at a specific position in the target image. Since [`McalTransformResultAtPosition`](../../Reference/cal/McalTransformResultAtPosition.md) operates on a position, it is not affected by distortion, and can be applied to uncorrected images.

## Input settings in world units

The modules that support settings in world units require that you set the working unit to [`M_WORLD`](../../Reference/blob/MblobControl.md) and you use a calibrated image, before they interpret their settings as being in world units. Otherwise, by default, they interpret their settings as being in pixel units, even if a module natively works in world units.

The function and constant to set the input units depend on the module (see the table below). Some modules have multiple constants to set the input units, so that you can specify some settings in world units and others in pixel units. Note that if you set these constants to [`M_WORLD`](../../Reference/blob/MblobControl.md) but you don't perform the module's operation on a calibrated image (for example, [`M...Calculate`](../../Reference/blob/MblobCalculate.md), [`M...Read`](../../Reference/code/McodeRead.md), or [`M...Find`](../../Reference/mod/MmodFind.md)), the operation will generate an error.

| How to set the working units | Settings that must respect the specified working units |
| --- | --- |
| 3D Reconstruction | The 3D Reconstruction module has some settings that are always expected in pixels and some that are always expected in world units. | Depends on setting. |
| Bead | [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TEMPLATE_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) | Template points and settings (position and dimension). |
| [`MbeadControl`](../../Reference/bead/MbeadControl.md) with [`M_TRAINING_BOX_INPUT_UNITS`](../../Reference/bead/MbeadControl.md) | Search box settings in the training phase. |
| Blob | [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md) | Condition low and high values passed to [`MblobSelect`](../../Reference/blob/MblobSelect.md). |
| Code | [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeControl.md) | [`M_SCANLINE_STEP`](../../Reference/code/McodeControl.md) and [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md). |
| [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) | [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeControl.md). |
| [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md) | [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). |
| [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) | [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md). |
| [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md) | [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md). |
| Graphics | [`MgraControlList`](../../Reference/gra/MgraControlList.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControlList.md) | Position and dimension settings when changing a graphic in a 2D graphics list. |
| [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) | Position and dimension settings passed to [`Mgra...`](../../Reference/gra/MgraAlloc.md) functions. |
| Measurement | [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_SEARCH_REGION_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) | Position and dimension of the search region. |
| [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_MARKER_REFERENCE_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) | Marker reference position. |
| [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md) with [`M_POINT_INPUT_UNITS`](../../Reference/meas/MmeasSetMarker.md) | Position, spacing, and angle of a point marker. |
| Metrology | The Metrology module accepts input values in world units if the image is calibrated, and pixel units otherwise. When using a calibrated image, you cannot specify pixel units. | Position and dimension settings. |

[^ExceptionsID3]: For the 3D modules that work with point cloud containers and/or 3D geometry objects, all 3D positional information (coordinates) are considered real-world (all source data is treated as if it is natively calibrated).

## Angle convention in Aurora Imaging Library

When working in pixel units, you define angles with respect to the pixel coordinate system. In this case, positive angles are always interpreted to be counterclockwise with respect to the initial line.

When working in world units, you define angles with respect to the relative coordinate system. In this case, the direction of rotation of the angles depends on the direction of the Y-axis.

*[Image: AngleConvention_Pixel_World.png]*

Note that since the Y-axis of an image is oriented downwards by default, the angles in Aurora Imaging Library are inverted from mathematical convention. In certain cases, you might want to convert angles from Aurora Imaging Library convention to mathematical convention: the negative value of an angle in Aurora Imaging Library convention is the positive value of the same angle in mathematical convention. For example, 45° in Aurora Imaging Library convention is the same as -45° in mathematical convention.

The inverted nature of angles in Aurora Imaging Library leads to complications when dealing with trigonometry, specifically the sine function. If you use the sine function with an input angle in the Aurora Imaging Library convention, the result of the function will be in Aurora Imaging Library convention, inverted from mathematical convention. It is for this reason that, when rotating a point (x, y) around the origin, the 2D rotation matrix that Aurora Imaging Library uses is:

*[Image: Rotation_matrix.png]*

Note that this matrix has already taken into account the inversion of sine functions: the `-sin` and `sin`functions are switched from their conventional positions in a standard 2D rotation matrix. Therefore, you don't need to worry about the inverted angles when working with this 2D rotation matrix within Aurora Imaging Library.
