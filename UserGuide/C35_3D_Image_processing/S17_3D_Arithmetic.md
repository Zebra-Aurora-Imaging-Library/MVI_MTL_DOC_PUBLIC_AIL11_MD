---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Image_processing
section: 3D_Arithmetic
module_tag: 3dim
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Image_processing / 3D Arithmetic"
---

# Performing arithmetic operations on depth maps

You can perform arithmetic operations between depth maps and 3D geometries using [`M3dimArith`](../../Reference/3dim/M3dimArith.md), to produce a new depth map. Supported operations include addition, subtraction, and minimum/maximum operations. This could be useful to, for example, subtract a fitted plane from the depth map to find the regions which deviate from the fit. [`M3dimArith`](../../Reference/3dim/M3dimArith.md) can also be used to replace missing values in a depth map.

> **Note:** All image operands must be fully corrected depth maps for this operation. To determine if you have a fully corrected depth map, call [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_DEPTH_MAP`](../../Reference/cal/McalInquire.md) and ensure that it returns [`M_TRUE`](../../Reference/cal/McalInquire.md).

## Using arithmetic for defect detection

You can use arithmetic functions on depth maps for defect detection. With two aligned, fully calibrated depth maps of the same object, one depth map representing a perfect model of an object and the other representing a current instance of the object, you can subtract the intensities of the two to create a depth map that highlights where the current object differs from the perfect model, as in the following image.

*[Image: cal_arithmetic_operations_DIST_NN.png]*

To use arithmetic on depth maps for defect detection, you must start with two point clouds of the object, a perfect model of the object and the object currently being tested. Then, follow this procedure:

1. Find the transformation that would align the two point clouds, using the 3D Registration module.
2. Align the two point clouds by applying the transformation to the point cloud of the object currently being tested, using[`M3dregMerge`](../../Reference/3dreg/M3dregMerge.md) (pass [`M_NULL`](../../Reference/3dreg/M3dregMerge.md) for the point cloud of the perfect model).
3. Generate two depth maps, one for the perfect model and the other for the object currently being tested, using [`M3dimProject`](../../Reference/3dim/M3dimProject.md) or [`M3dimProjectEx`](../../Reference/3dim/M3dimProjectEx.md).
4. Subtract the two depth maps, using [`M3dimArith`](../../Reference/3dim/M3dimArith.md) with [`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md), [`M_DIST_NN_SIGNED()`](../../Reference/3dim/M3dimArith.md), [`M_SUB`](../../Reference/3dim/M3dimArith.md), or [`M_SUB_ABS`](../../Reference/3dim/M3dimArith.md), depending on your application and the depth maps.
   - If the depth maps should be nearly identical, except any defects, you can use [`M_SUB_ABS`](../../Reference/3dim/M3dimArith.md) or [`M_SUB`](../../Reference/3dim/M3dimArith.md) to determine where the defects are. These operations simply subtract the intensities of corresponding pixels in the two depth maps. These operations are fairly quick.
   - If the two aligned depth maps might have a small degree of difference that is not related to defects, such as shadows or incomplete alignment, you can use[`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md) or [`M_DIST_NN_SIGNED()`](../../Reference/3dim/M3dimArith.md) to determine where the defects are. These operations are more robust versions of [`M_SUB_ABS`](../../Reference/3dim/M3dimArith.md) and [`M_SUB`](../../Reference/3dim/M3dimArith.md) that compensate for noise or misalignment by considering the neighborhood of pixels around the corresponding pixels.
5. Use the resulting depth map to locate and possibly measure areas where the object being tested differs from the perfect model. These areas will likely have higher than average intensities.

## Subtraction considering neighborhood pixel distances

When looking for substantial differences between two depth maps (or geometry objects) that have a small degree of difference due to noise, simply subtracting the intensities of the pixels could give too many false differences. In this case, you can use [`M3dimArith`](../../Reference/3dim/M3dimArith.md) with [`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md) or [`M_DIST_NN_SIGNED()`](../../Reference/3dim/M3dimArith.md). For example, if the depth map differences are due to noise, such as incomplete alignment, small shadows, or other slight camera deviations, you can use these operations. These arithmetic operations are more robust to slight alignment differences or random deviations between the depth maps, but take longer to process. You can specify the size of the neighborhood used in the algorithm; typically, the bigger the neighborhood, the more robust the operation is, but the longer it takes to process.

Rather than subtracting the intensities of just the corresponding pixels in the two operands, as with [`M_SUB_ABS`](../../Reference/3dim/M3dimArith.md) and [`M_SUB`](../../Reference/3dim/M3dimArith.md), [`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md) and [`M_DIST_NN_SIGNED()`](../../Reference/3dim/M3dimArith.md)additionally considers the neighborhood of pixels around the corresponding pixels when determining the difference.

The [`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md) and [`M_DIST_NN_SIGNED()`](../../Reference/3dim/M3dimArith.md) operations use a three step process. The operations use the real-world coordinates of corresponding pixels in the depth map, with the pixel's intensity mapping to a Z-coordinate. The three step process to calculate the resulting intensity of each pixel is as follows:

1. The operation calculates the distance between a pixel in operand 1 (considered as a 3D point) and each of the neighborhood pixels of the corresponding pixel in operand 2, including the corresponding pixel itself. The shortest of these distances is considered.
   > **Note:** Note that the distance between two pixels is calculated by first converting the X- and Y-pixel coordinates and pixel intensity of each pixel into a 3D coordinate in world units. The distance between 2 points in 3D space is calculated using Euclidean geometry.
2. The operation calculates the 3D Euclidean distance between a pixel in operand 2 (considered as a 3D point) and each of the neighborhood pixels of the corresponding pixel in operand 1, including the corresponding pixel itself. The shortest of these distances is considered.
3. The operation uses the larger of the two minimum distances calculated in the previous two steps.
   > **Note:** Note that the resulting distance is converted back to an intensity using the Z-scale ([`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md)).

The animation below illustrates this algorithm using a neighborhood size of 3x3. Specify the size of the neighborhood using [`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md) with[`M_DIST_NN()`](../../Reference/3dim/M3dimArith.md). The larger the size of the neighborhood, the longer the process takes, but the more robust it tends to be. Neighborhoods that are too large might return meaningless results.

*[Image: 3dmap_arith_distNN_Animation]*

## Constraints on depth map operands and how to handle destination calibration

When two depth maps are the operands, [`M3dimArith`](../../Reference/3dim/M3dimArith.md) performs the selected operation point-to-point. Therefore, your settings with respect to X and Y must be equal. Specifically, both source depth maps must share the same values for the following:

- [`M_PIXEL_SIZE_X`](../../Reference/cal/McalInquire.md): the length, in world units, of one pixel in the corrected image, in the X-direction.
- [`M_PIXEL_SIZE_Y`](../../Reference/cal/McalInquire.md): the length, in world units, of one pixel in the corrected image, in the Y-direction.

Both source depth maps must also share the same world position for the top-left pixel in the corrected images. This means that an [`M_PIXEL_TO_WORLD`](../../Reference/cal/McalTransformCoordinate.md) transformation must be the same for the top-left pixel (0,0) of both images. Equivalently, both source depth maps must share the same values for the following:

- [`M_WORLD_POS_X`](../../Reference/cal/McalInquire.md) + ([`M_PIXEL_SIZE_X`](../../Reference/cal/McalInquire.md) * [`M_CALIBRATION_CHILD_OFFSET_X`](../../Reference/cal/McalInquire.md)): the X-coordinate of the center of the top-left pixel in the corrected image.
- [`M_WORLD_POS_Y`](../../Reference/cal/McalInquire.md) + ([`M_PIXEL_SIZE_Y`](../../Reference/cal/McalInquire.md) * [`M_CALIBRATION_CHILD_OFFSET_Y`](../../Reference/cal/McalInquire.md)): the Y-coordinate of the center of the top-left pixel in the corrected image.

Note, however, that the scale along Z ([`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md)) and the location of zero gray along Z ([`M_WORLD_POS_Z`](../../Reference/cal/McalInquire.md), also called the Z-offset) can differ, since the operation is performed using the Z-values. For example, [`M_ADD`](../../Reference/3dim/M3dimArith.md) adds together the world Z-coordinates of corresponding points.

After performing the operation, each world point for the destination image buffer is transformed back into a depth map pixel.

You can control what happens to the values of [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md) and [`M_WORLD_POS_Z`](../../Reference/cal/McalInquire.md) of the destination buffer, using the [`ControlFlag`](../../Reference/3dim/M3dimArith.md) parameter. The default setting is [`M_FIT_SCALES`](../../Reference/3dim/M3dimArith.md), which adjusts the Z-scale and Z-offset so that values can be represented in the destination image buffer without saturation. In determining these values, [`M3dimArith`](../../Reference/3dim/M3dimArith.md) considers all possible values that could result from the operation, and are thus dependent on the operator.

A simple example: let [6, 18] and [2, 10] be the range of representable world Z-values of the first and second operands, respectively. If the operation is [`M_SUB`](../../Reference/3dim/M3dimArith.md), the range of possible resulting values would be [6 - 10, 18 - 2] = [-4, 16]. Note that these are the extreme possible values, and not necessarily actual source values.

Note that both the [`M_MIN`](../../Reference/3dim/M3dimArith.md) and [`M_MAX`](../../Reference/3dim/M3dimArith.md) operations use the same range, which includes minimum and maximum extremes. This makes comparison between the two operations easier. So using the same ranges of the example operands above, the range of possible values for the destination image buffer (for either [`M_MIN`](../../Reference/3dim/M3dimArith.md) or [`M_MAX`](../../Reference/3dim/M3dimArith.md)) would be [min(6, 2), max(18, 10)] = [2, 18].

Once the destination range is computed, the values of [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalInquire.md) and [`M_WORLD_POS_Z`](../../Reference/cal/McalInquire.md) are chosen to span that range.

To constrain the destination Z-scale to that of the first source image, set [`ControlFlag`](../../Reference/3dim/M3dimArith.md) to [`M_SET_WORLD_OFFSET_Z`](../../Reference/3dim/M3dimArith.md). In this case, operation results are mapped to fit the Z-scale of the first source image and the Z-offset is automatically adjusted so that the destination range covers the center of the possible range of data. Saturation can occur with this setting.

If you set [`ControlFlag`](../../Reference/3dim/M3dimArith.md) to [`M_USE_DESTINATION_SCALES`](../../Reference/3dim/M3dimArith.md), you can explicitly set the values of [`M_GRAY_LEVEL_SIZE_Z`](../../Reference/cal/McalControl.md) and [`M_WORLD_POS_Z`](../../Reference/cal/McalControl.md) for the destination image buffer using [`McalControl`](../../Reference/cal/McalControl.md). In this case, [`M3dimArith`](../../Reference/3dim/M3dimArith.md) will use the provided destination calibration information when performing the arithmetic operation. The destination must be 3D-corrected before calling [`M3dimArith`](../../Reference/3dim/M3dimArith.md), and have the same scales and offsets in X and Y as the source image(s). Use [`McalUniform`](../../Reference/cal/McalUniform.md) and [`McalControl`](../../Reference/cal/McalControl.md) to manually set the calibration information of the destination image buffer. Alternatively, you can copy the camera calibration information from another image using [`McalAssociate`](../../Reference/cal/McalAssociate.md), or you can use [`M3dimConvertDepthMap`](../../Reference/3dim/M3dimConvertDepthMap.md) with [`M_CALIBRATE`](../../Reference/3dim/M3dimConvertDepthMap.md) to copy the calibration information from a depth map container. You can also use the calibration information of one of the source operands by setting [`ControlFlag`](../../Reference/3dim/M3dimArith.md) to [`M_USE_SOURCE1_SCALES`](../../Reference/3dim/M3dimArith.md) or [`M_USE_SOURCE2_SCALES`](../../Reference/3dim/M3dimArith.md).
