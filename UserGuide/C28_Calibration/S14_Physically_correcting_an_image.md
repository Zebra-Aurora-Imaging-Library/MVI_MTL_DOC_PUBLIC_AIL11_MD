---
doctype: UserGuide
part: "2D related information"
chapter: Calibration
section: Physically_correcting_an_image
module_tag: cal
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / calibration / Physically correcting an image"
---

# Physically correcting an image

A calibrated image still appears distorted because it has not been physically corrected. You can use [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) to physically correct and remove distortions from your images. An image that has no distortions and appears exactly as it would if looking straight down onto it is known as a corrected image. When a corrected image is used within an Aurora Imaging Library module, pixels have the same, uniform size in every direction, and results can be returned in real-world units. As is expected, the image features in the corrected image will have the same world coordinates as they had in the source image, despite the fact their pixel coordinates have changed.

For example, after calibrating the source image below, the world coordinates of the bottom-left circle are (5,6). After correcting your image, the world coordinates of the bottom-left circle in the corrected image are also (5,6), even though it is evident that the pixel coordinates are different for the bottom-left circle in the source and corrected images.

*[Image: cal_correct.png]*

When working in Tsai-based, Zhang-based, or a robotics camera calibration mode, it is possible to physically correct lens distortions of an image (that is, barrel distortion and pin cushion distortion) without removing other types of distortions. To correct lens distortions only, use [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_CORRECT_LENS_DISTORTION_ONLY`](../../Reference/cal/McalTransformImage.md).

*[Image: cal_lens_distortion.png]*

Note that images are physically corrected using a geometric warping.

## Generating lookup tables to correct images

Instead of generating a corrected image, it is possible to generate lookup tables (LUTs), using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) with [`M_EXTRACT_LUT_X`](../../Reference/cal/McalTransformImage.md) and [`M_EXTRACT_LUT_Y`](../../Reference/cal/McalTransformImage.md), and then use these LUTs to correct images using [`MimWarp`](../../Reference/im/MimWarp.md). It is recommended to use this method when you are using hardware that yields faster processing with LUT inputs.

## Scale and position of the corrected image

When calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md), you can specify the fill mode. This essentially allows you to specify how the transformed image should be positioned and scaled in the destination image.

There are three types of fill mode: [`M_FIT`](../../Reference/cal/McalTransformImage.md), [`M_CLIP`](../../Reference/cal/McalTransformImage.md), and [`M_USE_DESTINATION_CALIBRATION`](../../Reference/cal/McalTransformImage.md). An [`M_FIT`](../../Reference/cal/McalTransformImage.md) fill mode scales and positions the transformed image such that every source pixel maps to a valid destination pixel. An [`M_CLIP`](../../Reference/cal/McalTransformImage.md) fill mode scales and positions the transformed image so that every destination pixel maps to a valid source pixel. Whereas, an [`M_USE_DESTINATION_CALIBRATION`](../../Reference/cal/McalTransformImage.md) fill mode scales and positions the transformed image using the camera calibration information of the destination image.

*[Image: calImageFillMode.png]*

When using an [`M_USE_DESTINATION_CALIBRATION`](../../Reference/cal/McalTransformImage.md) fill mode, the source image is positioned such that its relative coordinate system is placed at the same location as the current relative coordinate system of the destination image. In addition, the source image is scaled to have the same pixel-to-world mapping as the destination image.

To use this fill mode, the destination image must be a calibrated image with a uniform pixel size. To apply a calibration to the destination image that just scales and offsets the world coordinate system from the pixel coordinate system, use [`McalUniform`](../../Reference/cal/McalUniform.md) on the destination buffer prior to calling [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).

Note that this fill mode only affects how the image is positioned and scaled in the destination image; the source image is always transformed according to the calibration setting of the [`CalibrationId`](../../Reference/cal/McalTransformImage.md) parameter.

This fill mode cannot be used with [`M_EXTRACT_LUT_X`](../../Reference/cal/McalTransformImage.md) or [`M_EXTRACT_LUT_Y`](../../Reference/cal/McalTransformImage.md) or performing an [`M_CORRECT_LENS_DISTORTION_ONLY`](../../Reference/cal/McalTransformImage.md) operation.

If you want to preserve the average pixel size of the source image when using an [`M_FIT`](../../Reference/cal/McalTransformImage.md) or [`M_CLIP`](../../Reference/cal/McalTransformImage.md) fill mode, you need to allocate the destination image buffer with an appropriate width and height. If you are performing an [`M_FIT`](../../Reference/cal/McalTransformImage.md) type operation, you can use [`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_TRANSFORM_FIT_SIZE_X_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) and [`M_TRANSFORM_FIT_SIZE_Y_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) to determine the width and height of your destination image buffer, in pixels, that will best preserve the source image's average pixel size. Likewise, if you are performing an [`M_CLIP`](../../Reference/cal/McalTransformImage.md) type operation, you can use[`McalInquire`](../../Reference/cal/McalInquire.md) with [`M_TRANSFORM_CLIP_SIZE_X_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) and [`M_TRANSFORM_CLIP_SIZE_Y_PRESERVE_PIXEL_SIZE`](../../Reference/cal/McalInquire.md) to determine the width and height of your destination image buffer, in pixels, that will best preserve the source image's average pixel size.

## Transformation cache

By default, a cache is used to physically correct an image. The first time a camera calibration context is used to transform an image, this cache fills up with information relevant to the transformation. On subsequent transformations with the camera calibration context, the information in the cache can significantly accelerate the transform. However, if you need to save memory, you can disable this cache, using [`McalControl`](../../Reference/cal/McalControl.md) with [`M_TRANSFORM_CACHE`](../../Reference/cal/McalControl.md). The cache consists of two 32-bit buffers that have the same size as the destination buffer of [`McalTransformImage`](../../Reference/cal/McalTransformImage.md).

The information in the cache is flushed whenever you change the size of the source or destination buffers of [`McalTransformImage`](../../Reference/cal/McalTransformImage.md), the angle of the relative coordinate system in the camera calibration context, or the size and position of the pixels in the corrected image.
