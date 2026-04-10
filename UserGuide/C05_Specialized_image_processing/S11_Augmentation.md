---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Specialized_image_processing
section: Augmentation
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Specialized-image / Augmentation"
---

# Augmentation

Augmentation allows you to create a plausible variation of an image. This is done by applying a specified ordered set of image processing operations with randomized settings within a specified range. For example, to plausibly mimic the different kinds of images a camera might capture of an object passing under it, [`MimAugment`](../../Reference/im/MimAugment.md) can randomly rotate, translate, and blur an image. This lets you generate the images that a camera can grab without the camera actually grabbing them.

*[Image: MimAugment.png]*

Augmentation can prove useful if you want to add a variety of modified images with which to perform test procedures, or if you want to increase the entries in your dataset when performing machine learning tasks, such as image classification or segmentation with the Aurora Imaging Library Classification module. Note, although you can easily integrate [`MimAugment`](../../Reference/im/MimAugment.md) operations from the Aurora Imaging Library Classification module, it is not typically necessary, as you can enable preset augmentation controls from the module itself ([`M_PRESET...`](../../Reference/class/MclassControl.md)). For more information, see[Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

## Steps to augment an image

The following steps provide a basic methodology for augmenting an image:

1. Allocate an image processing context for augmentation, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md).
2. Allocate an image processing buffer to hold augmentation results, using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_AUGMENTATION_RESULT`](../../Reference/im/MimAllocResult.md).
3. Enable the image processing operations that the augmentation can perform, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_..._OP`](../../Reference/im/MimControl.md). By default, all operations are disabled.
4. Optionally, modify the range of values for the image processing operations that the augmentation can perform, using [`MimControl`](../../Reference/im/MimControl.md). Aurora Imaging Library randomly chooses the values to use for the operations from within the specified range.
5. Optionally, set the priority (order) with which the augmentation can perform the image processing operations, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_PRIORITY`](../../Reference/im/MimControl.md).
6. Optionally, set the probability with which the augmentation can perform the image processing operations, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_PROBABILITY`](../../Reference/im/MimControl.md).
7. Perform the augmentation, using [`MimAugment`](../../Reference/im/MimAugment.md).
   This function performs the augmentation operation on a source image and places the results in either an image buffer or an augmentation result buffer, depending on the destination parameter setting. The resulting image buffer holds the modified version of the source image. The result buffer holds information about the modifications.
8. If you called [`MimAugment`](../../Reference/im/MimAugment.md) with an augmentation result buffer, retrieve the required results, using [`MimGetResult`](../../Reference/im/MimGetResult.md).
   Important results include the image processing operations performed, and the values used for them, for each call to [`MimAugment`](../../Reference/im/MimAugment.md). To save a report of such results, call [`MimStream`](../../Reference/im/MimStream.md) with the[`M_SAVE_REPORT`](../../Reference/im/MimStream.md) operation.
9. Optionally, draw the resulting image of the augmentation, using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_AUG_IMAGE`](../../Reference/im/MimDraw.md).
10. If necessary, save your augmentation context, using [`MimSave`](../../Reference/im/MimSave.md) or [`MimStream`](../../Reference/im/MimStream.md).
11. Free all your allocated objects, using [`MimFree`](../../Reference/im/MimFree.md), unless [`M_UNIQUE_ID`](../../Reference/im/MimAlloc.md) was specified during allocation.

The augmentation operation is usually run multiple times, since most augmentation related applications require multiple modified images. Typically, you should save the augmented images ([`MbufSave`](../../Reference/buf/MbufSave.md)). For more information about how to use the augmentation operation, see the _MimAugment.cpp_ example in Aurora Imaging Example Launcher.

Note, you can interactively configure and execute augmentation operations with Aurora Imaging CoPilot.

## Priority

By default, the image processing operations ([`M_AUG_..._OP`](../../Reference/im/MimControl.md)) that [`MimAugment`](../../Reference/im/MimAugment.md) can perform are prioritized (ordered) as follows:

1. Affine operations, such as [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md).
2. Structure operations, such as [`M_AUG_DILATION_OP`](../../Reference/im/MimControl.md).
3. Geometric operations, such as [`M_AUG_FLIP_OP`](../../Reference/im/MimControl.md).
4. Intensity operations, such as [`M_AUG_INTENSITY_MULTIPLY_OP`](../../Reference/im/MimControl.md).
5. Linear filter operations, such as [`M_AUG_SMOOTH_DERICHE_OP`](../../Reference/im/MimControl.md).
6. Noise operations, such as [`M_AUG_NOISE_GAUSSIAN_ADDITIVE_OP`](../../Reference/im/MimControl.md).

To modify this order, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_PRIORITY`](../../Reference/im/MimControl.md). Changing the order of the operations can alter their final effect; for example, smoothing an image before adding noise differs from adding noise before smoothing.

## Probability

Each time you call [`MimAugment`](../../Reference/im/MimAugment.md), Aurora Imaging Library randomly selects which enabled image processing operations to perform. By default, each operation has an equal chance of being randomly selected. To modify these chances, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_PROBABILITY`](../../Reference/im/MimControl.md).

Note, setting the probability to 100.0 ensures that Aurora Imaging Library performs the operation, while setting 0.0 ensures that Aurora Imaging Library will not perform the operation (this is the same as disabling the operation).

## Randomness and seeds

To establish the randomness required by the augmentation operation, Aurora Imaging Library uses an internal random number generator (RNG).

All the random values that [`MimAugment`](../../Reference/im/MimAugment.md) uses trace back to a seed; that is, the first initialization value of the internal random number generator. By default, Aurora Imaging Library randomly generates a new seed each time you call [`MimAugment`](../../Reference/im/MimAugment.md).

To specify an explicit initialization value for the internal random number generator, call [`MimControl`](../../Reference/im/MimControl.md) with [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md) set to [`M_RNG_INIT_VALUE`](../../Reference/im/MimControl.md), and then use [`M_AUG_RNG_INIT_VALUE`](../../Reference/im/MimControl.md) to specify the actual value. Using the same seed multiple times lets you rerun (repeat) the same augmentation process with the same randomized settings. This seed is saved with the augmentation context.

You can also specify the seed directly when calling [`MimAugment`](../../Reference/im/MimAugment.md), by using the [`SeedValue`](../../Reference/im/MimAugment.md) parameter. To do so, you must set [`M_AUG_SEED_MODE`](../../Reference/im/MimControl.md) to [`M_USER_DEFINED_SEED`](../../Reference/im/MimControl.md). This seed is not saved with the context.

Note that, you can get the seed that [`MimAugment`](../../Reference/im/MimAugment.md) uses (regardless of how the seed was established) with the [`M_AUG_SEED_USED`](../../Reference/im/MimGetResult.md) result, and then use that seed to repeat that exact augmentation operation.

## Random rotation angle

When you enable the rotation operation ([`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md)), a random rotation angle is generated to rotate the image. The random rotation angle can be represented with the following formula:

`_AngleMin_ &lt;= (_AngleRef_ + _i_ * _AngleStep_) ± (_AngleDelta_ / 2`) &lt; _AngleMax_, where:

- _AngleMin_ ([`M_AUG_ROTATION_OP_ANGLE_MIN`](../../Reference/im/MimControl.md)) represents the minimum rotation angle.
- _AngleRef_ ([`M_AUG_ROTATION_OP_ANGLE_REF`](../../Reference/im/MimControl.md)) represents the reference angle. All other angles for [`M_AUG_ROTATION_OP`](../../Reference/im/MimControl.md) will be calculated relative to this angle.
- _i_ is a randomly generated integer.
- _AngleStep_ ([`M_AUG_ROTATION_OP_ANGLE_STEP`](../../Reference/im/MimControl.md)) represents the step angle. The step angle is the distance, or gap, relative to the reference angle. The random integer _i_ determines the number of rotation steps from the reference angle to reach the angle range from which to randomly select a rotation angle.
- _AngleDelta_ ([`M_AUG_ROTATION_OP_ANGLE_DELTA`](../../Reference/im/MimControl.md)) represents the delta angle. The delta angle is the angular range, relative to (_AngleRef_ + _i_ * _AngleStep_).
- _AngleMax_ ([`M_AUG_ROTATION_OP_ANGLE_MAX`](../../Reference/im/MimControl.md)) represents the maximum rotation angle.

If the generated random rotation angle is outside the range of _AngleMin_ and _AngleMax_, another combination of _i_ and _RandomAngle_ will be generated. By default, _AngleMin_ ([`M_AUG_ROTATION_OP_ANGLE_MIN`](../../Reference/im/MimControl.md)) and _AngleMax_ ([`M_AUG_ROTATION_OP_ANGLE_MAX`](../../Reference/im/MimControl.md)) are set to 0 degrees and 360 degrees, respectively.

The following example illustrates how the random rotation angle is established. Note that since i is randomly selected, an angle can be selected from any of the angular ranges around each possible step (as long as the angle is between the specified minimum and maximum angle).

*[Image: MimAugment_rotation_angle.png]*
