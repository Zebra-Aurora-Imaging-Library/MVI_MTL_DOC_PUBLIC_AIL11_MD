---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Rotation
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Rotation"
---

# Finding models when they are at an angle

There are two ways to search for a model that can appear at different angles using the [`MpatFind`](../../Reference/pat/MpatFind.md) function:

- Define and then search for a rotated model.
- Search for models taken from the same region in rotated images.
  *[Image: overscan.png]*

To implement the first type of search, define a model using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with [`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md) or [`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md). Then, enable the angular search using [`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) and set the angular range using [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md). When you call [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md), it will internally create different models by rotating the original model at the required angles, assigning "don't care" pixels to regions that do not have corresponding data in the original model.

You should implement this technique when the pixels surrounding the model follow no predictable pattern (for example, when searching for loose nuts and bolts lying on a conveyor).

To implement the second type of search, define a regular or automatic model using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_REGULAR_MODEL`](../../Reference/pat/MpatDefine.md)+[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) or [`MpatDefine`](../../Reference/pat/MpatDefine.md) with[`M_AUTO_MODEL`](../../Reference/pat/MpatDefine.md) +[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md), respectively; this will extract the model as well as circular overscan data from the model's source image (specifically, Aurora Imaging Library extracts the region enclosed by a circle which circumscribes the model). Then, enable angular search using [`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) and set the angular range using [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md). When you call [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md), it will extract different orientations of the model from the overscanned model. This technique should only be used when the model's distinct features lie in the center of the region, so that they are included in all models when rotated. Therefore, it is recommended that an[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) model be as square as possible: the longer the rectangle, the smaller the number of consistent central pixels in every model.

*[Image: OverscanData.png]*

As mentioned, a larger region than the one defined will be fetched from the model's source image. Therefore, the model must not be extracted from a region too close to the edge of the model's source image.

The pixels surrounding the model should be relevant to the positioning of the pattern (that is, the model should appear in the target image with the same overscan data). For example, taking a model from an integrated circuit board, you can expect that the model will always be found in the same area of the board, with the same background.

*[Image: Mpat-RotatedIntegratedCircuit.png]*

Both techniques find the position and match score of the model in a target image.

Finally, it should be noted that Aurora Imaging Library's implementation of [`MpatFind`](../../Reference/pat/MpatFind.md) with an [`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md) type model is significantly faster than that of a model without circular overscan when performing an angular search.

## Setting the angle of search

By default, the nominal angle of the search is 0°. However, using [`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SEARCH_ANGLE_MODE`](../../Reference/pat/MpatControl.md) enabled, you can then change the starting angle (using [`M_SEARCH_ANGLE`](../../Reference/pat/MpatControl.md)) and specify an angular range ([`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md)). You can also specify the required precision of the resulting angle and the interpolation mode used to rotate the model. These settings can influence the speed of the search significantly.

When an angular range has been specified using [`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md), [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) creates a model for every _x_ degrees within the range, where _x_ is determined by the specified rotational tolerance ([`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md)). Rotational tolerance defines the full range of degrees within which the pattern in the target image can be rotated from a model at a specific angle and still meet the acceptance level. The rotational tolerance can also be automatically determined according to the angular correlation of the model with versions of itself at specific angles when the model is preprocessed; to do so, set [`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md) to [`M_AUTO`](../../Reference/pat/MpatControl.md). Note that [`M_SEARCH_ANGLE_INTERPOLATION_MODE`](../../Reference/pat/MpatControl.md) sets the type of interpolation to use when rotating the model.

After the approximate location is found, Aurora Imaging Library fine-tunes the search, according to the specified angle accuracy ([`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md)). It searches within one rotational tolerance before and after the approximate location, at an angle refinement step determined by the specified angle accuracy. The greater the angle refinement step, the faster the search; however, your results will be less accurate. Note that you must set the angle accuracy (angle refinement step) to a value smaller than that of the rotation tolerance.

*[Image: UG-MPat-Rotation.png]*

If you don't know the best search angle accuracy to set, you can have Aurora Imaging Library automatically select the angle refinement step. To do so, set [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) to [`M_DISABLE`](../../Reference/pat/MpatControl.md). Then, enable angle refinement mode and set the acceptable minimum score for a rotated model, both by setting[`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_ROTATED_MODEL_MINIMUM_SCORE`](../../Reference/pat/MpatControl.md)to the minimum acceptable score. Angle refinement mode improves accuracy without unnecessarily sacrificing performance. Similar to automatically determining rotational tolerance, during preprocessing, angle refinement mode determines the full range of degrees within which a rotated version of the model can be rotated from the model at a specific angle and still achieve the specified score. The specified score must be set to a value higher than the acceptance level. The higher this score is set, the smaller the angle refinement step will be.

If you need a minimum accuracy, you can set [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md) to the required minimum accuracy and then enable angle refinement mode by setting the minimum score for a rotated model. The lowest value between the determined angle refinement step and the specified angle accuracy is the angle refinement step used when searching for the pattern in the target image.

You can set the method for scanning the specified angular range for multiple occurrences, or for one occurrence with circular overscan data, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_ANGLE_ACCURACY_MODE`](../../Reference/pat/MpatControl.md). By default, Aurora Imaging Library returns the closest occurrence to the approximate location found within the angular range, but you can set [`M_SEARCH_ANGLE_ACCURACY_MODE`](../../Reference/pat/MpatControl.md) to [`M_ALL`](../../Reference/pat/MpatControl.md) to scan through all possible occurrences found at the angle refinement steps and return the best one. Doing so might reduce performance, so you should only set [`M_SEARCH_ANGLE_ACCURACY_MODE`](../../Reference/pat/MpatControl.md) to [`M_ALL`](../../Reference/pat/MpatControl.md) when the target image has many potential false positive occurrences.<sub></sub>

When searching within a range of angles, you should use as narrow a range, as high a tolerance, and as large an angle accuracy (angle refinement step) as possible, since the operation can take a long time to perform.

## Determining the rotation tolerance of a model

Every model has its own particular rotation tolerance. This tolerance is dependent on the individual model characteristics and surrounding target image features. If you don't want Aurora Imaging Library to automatically calculate the rotational tolerance (using [`MpatControl`](../../Reference/pat/MpatControl.md) with[`M_SEARCH_ANGLE_TOLERANCE`](../../Reference/pat/MpatControl.md) set to [`M_AUTO`](../../Reference/pat/MpatControl.md)), you must specify the required precision of the resulting angle as a value from 0 to 180.0 degrees. To determine the rotation tolerance of a model:

1. Set the search angle of a model to the same angle as the sought for pattern in a sample target image. However, set the positive and negative delta values ([`M_SEARCH_ANGLE_DELTA...`](../../Reference/pat/MpatControl.md)) to zero since you want to test by how much a pattern in an image can be rotated and still correlate with a model at a specific angle.
2. Use the [`MimRotate`](../../Reference/im/MimRotate.md) function to rotate the image in very small, positive increments (for example, 0.5°), and perform a [`MpatFind`](../../Reference/pat/MpatFind.md) operation at every angle. Make sure that the image's center of rotation is the same as that of the model; otherwise, the resulting tolerance will not be accurate. Note, when rotating the image, always set the angle from the image's original position to avoid interpolating the image more than once. Check the results for the greatest angle that produces an acceptable score.
3. Repeat steps 1 and 2, rotating the image in negative increments.
4. Take the minimum of the absolute value of these angles. Double this angle and set it as the rotation tolerance for the angular search.
