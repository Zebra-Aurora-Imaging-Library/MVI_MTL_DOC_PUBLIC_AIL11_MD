---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Mosaicing
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Mosaicing"
---

# Mosaicing and super-resolution

Once you have performed the registration of images, you can use the results to create a mosaic. Mosaicing combines images to form one larger image or to create an image with improved resolution (super-resolution). To combine the images, mosaicing uses the transformations calculated during the registration. To perform mosaicing, use [`MregTransformImage`](../../Reference/reg/MregTransformImage.md).

You can create mosaics from different series of images using the results of one correlation-stitching registration calculation. This is useful, for example, if you have a camera that is taking a series of images and the camera's position is moved between each snapshot. If you take another series of images and the camera's position for each individual image is the same as it was for the previous series of images, then you can perform the registration calculation once (for the first series), and re-use the results, stored in the registration result buffer, to compose a mosaic with the subsequent series of images. Note that any slight change between the original camera position and the ones used to take a subsequent series of images will result in a mosaic in which the images are not optimally aligned.

You can also omit certain images from a series of images when producing a mosaic. For example, you might want to add each image to a mosaic as soon as it is grabbed. You can do this by calling [`MregTransformImage`](../../Reference/reg/MregTransformImage.md), omitting all the images in the input array except for the grabbed one, and passing the same destination image (the one containing the partial mosaic) upon each call. To omit an image from the input array, replace the identifier of the image by **M_NULL**.

There also exist various correlation-stitching registration result buffer settings that affect the composition of your mosaic; you can control their settings using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_GENERAL`](../../Reference/reg/MregControl.md). These are discussed in the following subsections.

## Positioning and scaling your mosaic in the destination image buffer

You can control the orientation, position, and scale of your mosaic in the destination image buffer using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md), [`M_MOSAIC_OFFSET_X`](../../Reference/reg/MregControl.md), [`M_MOSAIC_OFFSET_Y`](../../Reference/reg/MregControl.md), and [`M_MOSAIC_SCALE`](../../Reference/reg/MregControl.md) respectively.

You specify the coordinate system relative to which your mosaic will be oriented when it is composed, using [`M_MOSAIC_STATIC_INDEX`](../../Reference/reg/MregControl.md). To compose the mosaic relative to a specific image, specify the image's index; the origin of the image's pixel coordinate system will be placed at the center of the top-left pixel of the destination image (assuming no offsets were specified). In addition, this image will be placed upright in the mosaic, and the other images will be positioned relative to its coordinate system. The other possible choice is to compose the mosaic relative to the global pixel coordinate system, in which case the origin of the global pixel coordinate system will appear at the center of the top-left of the destination image buffer (assuming no offsets were specified). Consequently, the images in the mosaic will be oriented according to their calculated positions. The default is the coordinate system of the image associated with the first registration result element.

*[Image: MosaicStaticIndex.png]*

Once you have specified the coordinate system relative to which the mosaic will be composed, you can set the X- and Y-offset between the origin of this coordinate system and the center of the top-left pixel of the destination image buffer, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_OFFSET_X`](../../Reference/reg/MregControl.md) and [`M_MOSAIC_OFFSET_Y`](../../Reference/reg/MregControl.md). This offset allows you to control the location of the mosaic in the destination image buffer. You can compose the mosaic so that the left-most point of the mosaic appears at the complete left of the destination image buffer; to do so, set [`M_MOSAIC_OFFSET_X`](../../Reference/reg/MregControl.md) to [`M_ALIGN_LEFT`](../../Reference/reg/MregControl.md). Similarly, to place the top-most point of the mosaic at the complete top of the destination image, set [`M_MOSAIC_OFFSET_Y`](../../Reference/reg/MregControl.md) to [`M_ALIGN_TOP`](../../Reference/reg/MregControl.md). The X- and Y-offsets are illustrated below.

*[Image: MosaicOffsets.png]*

You can also apply a scale factor to produce an enlarged mosaic, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_SCALE`](../../Reference/reg/MregControl.md). A value greater than 1.0 produces an enlarged mosaic, while a value less than 1.0 produces a reduced one.

*[Image: MosaicScale.png]*

## Mosaic composition in the overlapping regions

Although images have been aligned during the registration process to optimize the match in the overlapping regions, the pixels in these overlapping regions are not always superimposed perfectly. Furthermore, the contrast and pixel intensity of one image is not always the same as in the image it overlaps. Therefore, to create a mosaic, you must specify how the Registration module handles the overlapping regions of the images, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_COMPOSITION`](../../Reference/reg/MregControl.md). You can choose to:

- Use the pixels of the image associated with the registration result element with the lowest index ([`M_FIRST_IMAGE`](../../Reference/reg/MregControl.md)).
- Use the pixels of the image associated with the registration result element with the highest index ([`M_LAST_IMAGE`](../../Reference/reg/MregControl.md)). This is the default.
- Use the average value of the images' pixels in the overlapping region ([`M_AVERAGE_IMAGE`](../../Reference/reg/MregControl.md)).
- Fuse the images by progressively blending overlapping pixels ([`M_FUSION_IMAGE`](../../Reference/reg/MregControl.md)). That is, overlapping pixel values are modified (blended) to form a transitional portion. Blending is based on the distance between each pixel and the edges of the images in the mosaic.
- Use the pixels of all the images to create a new image with enhanced resolution ([`M_SUPER_RESOLUTION`](../../Reference/reg/MregControl.md)).

The following example illustrates the types of mosaic compositions that you can specify to create a larger image:

*[Image: MosaicComposition.png]*

The following section explains how to combine images of the same area to increase their resolution.

## Mosaic composition using super-resolution

Super-resolution is a process where multiple source images are used to create a new image with enhanced resolution. Details that are difficult to see in the original source images are extracted and can be seen in the mosaic when it is enlarged. To compose a super-resolution mosaic, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MOSAIC_COMPOSITION`](../../Reference/reg/MregControl.md) set to [`M_SUPER_RESOLUTION`](../../Reference/reg/MregControl.md). Set [`M_MOSAIC_SCALE`](../../Reference/reg/MregControl.md) to a value greater than one to enlarge the mosaic image.

The following shows the comparison of an image resized using bilinear interpolation and the result of a super-resolution mosaic composition. Four source images are taken to create the mosaic and the result is scaled up three times with [`M_MOSAIC_SCALE`](../../Reference/reg/MregControl.md).

*[Image: reg_super_resolution.png]*

The source images need to be very accurately aligned before the mosaic composition can take place. To ensure that the images are aligned with subpixel accuracy, call [`MregControl`](../../Reference/reg/MregControl.md)with [`M_ACCURACY`](../../Reference/reg/MregControl.md) set to [`M_HIGH`](../../Reference/reg/MregControl.md) before using [`MregTransformImage`](../../Reference/reg/MregTransformImage.md).

During super-resolution calculation, the Registration module tries to remove the effect of blurring caused by your image acquisition setup. For optimal results, you should set [`M_SR_PSF_TYPE`](../../Reference/reg/MregControl.md) to the point spread function (PSF) that best models how a point of light is blurred by the lens and CCD of your acquisition setup. An [`M_CIRCULAR`](../../Reference/reg/MregControl.md) PSF assumes that the setup blurs each point of light as a uniform circle. An [`M_GAUSSIAN`](../../Reference/reg/MregControl.md) PSF assumes the setup blurs each point of light according to a radially symmetric Gaussian function. An [`M_SQUARE`](../../Reference/reg/MregControl.md) PSF assumes the setup blurs each point of light according to a symmetric square function. You must specify a radius for each of these PSFs using [`M_SR_PSF_RADIUS`](../../Reference/reg/MregControl.md); for an [`M_GAUSSIAN`](../../Reference/reg/MregControl.md) PSF, the radius corresponds to the standard deviation of the Gaussian function, and for an [`M_SQUARE`](../../Reference/reg/MregControl.md)PSF, the radius corresponds to half the length of a side of the square. A correctly estimated PSF radius can enhance the super-resolution mosaic further, while a radius which is estimated too large can create large artifacts.

*[Image: reg_psf.png]*

If your source images have noise, super-resolution mosaicing will enhance the noise in the final image. To avoid the amplification of noise, you can specify a smoothness value. However, a smoothing value which is too high can blur the image.

*[Image: reg_smoothness.png]*
