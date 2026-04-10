---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Image_processing
section: Removing_nonuniform_lighting_from_grabbed_images
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / image / Removing nonuniform lighting from grabbed images"
---

# Removing uneven lighting from grabbed images

The quality of processing results can be degraded if the source images have been taken under non-uniform lighting. Non-uniform lighting can be caused by many different scenarios. It is detectable by taking a line profile (for example, along the diagonal) in an image of a uniform gray object in the imaging setup. To remove uneven lighting, you can use [`MimFlatField`](../../Reference/im/MimFlatField.md).

To use [`MimFlatField`](../../Reference/im/MimFlatField.md), you must set up a flat-field image processing context, using [`MimControl`](../../Reference/im/MimControl.md). The flat-field image processing context must contain a flat image, a gain constant, and optionally two dark images. [`MimFlatField`](../../Reference/im/MimFlatField.md) applies the flat-field image processing context to the source image using the following modified formula.

*[Image: IM-FlatFieldEquation02.png]*

The flat image provides an example of the uneven lighting without any objects in the foreground, while the gain constant is required to normalize (or scale) the result, typically back to the full dynamic range of the destination image. Note that the gain factor can be automatically generated using [`M_GAIN_CONST`](../../Reference/im/MimControl.md)set to [`M_AUTOMATIC`](../../Reference/im/MimControl.md). In this case, the gain factor is calculated based on the average of the denominator (that is, `Avg(_AvgFlatImg_ - _AvgDarkImg02_)`).

The two dark images can be replaced by constants (for example, to remove these two images, replace them with a zero in the calculation).

As an example of using images, this formula combines the various images of the processing context in the following way:

*[Image: MimFlatField_uneven_lighting.png]*

> **Note:** Note that you can also use [`MimFlatField`](../../Reference/im/MimFlatField.md) to remove CCD artifacts from grabbed images. For more information, refer to [Removing CCD artifacts from grabbed images](../C05_Specialized_image_processing/S05_Removing_artifacts_from_grabbed_images.md).

## How to remove non-uniform lighting from grabbed images

Perform the following steps to remove non-uniform lighting from your grabbed images:

1. Allocate a flat-field image processing context, using [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_FLAT_FIELD_CONTEXT`](../../Reference/im/MimAlloc.md).
2. If necessary, using the same exposure time as was used for the source image, grab multiple images taken with the lens cap placed on the lens. Average the grabbed images by performing a mean operation (using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)when [`MimControl`](../../Reference/im/MimControl.md) with [`M_STAT_MEAN`](../../Reference/im/MimControl.md)is set to [`M_ENABLE`](../../Reference/im/MimControl.md)). Add this image to the flat-field image processing context, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_IMAGE`](../../Reference/im/MimControl.md). This averaged image will be used as_AvgDarkImg01_for the calculation.
   > **Note:** Note that, in most cases, if you provide an offset image (_AvgDarkImg02_), you should provide a dark image (_AvgDarkImg01_).
   If a dark image is not necessary, set [`MimControl`](../../Reference/im/MimControl.md) with [`M_DARK_CONST`](../../Reference/im/MimControl.md) to 0.
3. Using an exposure time set so that no pixel is saturated, grab multiple images of the same area as in your source image, except place a uniform gray object in the field of view (such as, grabbing an image of a blank gray piece of paper). Average the grabbed images by performing a mean operation (using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)when [`MimControl`](../../Reference/im/MimControl.md) with [`M_STAT_MEAN`](../../Reference/im/MimControl.md)is set to [`M_ENABLE`](../../Reference/im/MimControl.md)). Add this image to the flat-field image processing context, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FLAT_IMAGE`](../../Reference/im/MimControl.md). This averaged image will be used as_AvgFlatImg_for the calculation.
4. If necessary, using the same exposure time as for the flat image, grab multiple images taken with the lens cap placed on the lens. Average the grabbed images by performing a mean operation (using [`MimStatCalculate`](../../Reference/im/MimStatCalculate.md)when [`MimControl`](../../Reference/im/MimControl.md) with [`M_STAT_MEAN`](../../Reference/im/MimControl.md)is set to [`M_ENABLE`](../../Reference/im/MimControl.md)). Add this image to the flat-field image processing context, using [`MimControl`](../../Reference/im/MimControl.md) with[`M_OFFSET_IMAGE`](../../Reference/im/MimControl.md). This averaged image will be used as _AvgDarkImg02_for the calculation.
   > **Note:** Note that, in most cases, if you provide a dark image (_AvgDarkImg01_), you should provide an offset image (_AvgDarkImg02_).
   If a dark image is not necessary, set [`MimControl`](../../Reference/im/MimControl.md) with [`M_OFFSET_CONST`](../../Reference/im/MimControl.md) to 0.
5. Specify the gain factor to apply, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_GAIN_CONST`](../../Reference/im/MimControl.md). To automatically calculate an appropriate gain factor to scale the results, typically back to the full dynamic range of the destination image, use [`M_GAIN_CONST`](../../Reference/im/MimControl.md) set to [`M_AUTOMATIC`](../../Reference/im/MimControl.md). Alternatively, you can set [`M_GAIN_CONST`](../../Reference/im/MimControl.md) to any value greater than 0.
   The automatically calculated gain factor might not always result in the best gain factor to use for your image; this is typically because the range of pixel values in the _AvgFlatImg_image is too great. In this case, inquire the automatically calculated gain factor, using [`MimInquire`](../../Reference/im/MimInquire.md) with [`M_EFFECTIVE_GAIN_CONST`](../../Reference/im/MimInquire.md). If darkening light pixels (removing saturation) from the image, set [`M_GAIN_CONST`](../../Reference/im/MimControl.md) to a value lower than the automatically calculated gain factor. Whereas, if lightening dark pixels, set [`M_GAIN_CONST`](../../Reference/im/MimControl.md) to a value higher than the automatically calculated gain factor.
6. Preprocess the context by calling [`MimFlatField`](../../Reference/im/MimFlatField.md) with [`M_PREPROCESS`](../../Reference/im/MimFlatField.md). Both the source and/or destination image buffers can be set to [`M_NULL`](../../Reference/im/MimFlatField.md). If, however, the source or destination image is provided, it should be a typical source or destination image, respectively, and it will be used in the preprocess operation to better optimize future calls. If the preprocess operation is not done explicitly, it will be done when [`MimFlatField`](../../Reference/im/MimFlatField.md) is first called.
7. Call [`MimFlatField`](../../Reference/im/MimFlatField.md) with the configured flat-field image processing context and your source image. The non-uniform lighting in the image should be improved. Note that you might have to perform the operation several times before determining the best value for the gain.
