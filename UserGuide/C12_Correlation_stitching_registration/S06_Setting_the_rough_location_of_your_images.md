---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Setting_the_rough_location_of_your_images
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Setting the rough location of your images"
---

# Setting the rough location of your images

Once you have specified an appropriate number of registration elements, you can specify the rough location of the images. To do so, call [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) for each input image and set its rough location by specifying the following:

- The registration element that will store the rough location information for the image.
- Relative to which coordinate system you are specifying the location.
- The transformation that specifies the rough location of the image in this reference coordinate system.

[`MregSetLocation`](../../Reference/reg/MregSetLocation.md) also allows you to copy this information from another registration element or from the results of a previous registration operation.

## Selecting an image's reference coordinate system

When specifying the rough location, one image must be positioned relative to the global pixel coordinate system; all other images must be positioned relative to another image's pixel coordinate system. This coordinate system is referred to as the image's reference coordinate system. By default, an image's reference coordinate system is that of the registration element's image whose index precedes the current registration element's index. For the image associated with registration element 0, the default reference coordinate system is the global pixel coordinate system. This is illustrated in the image below.

*[Image: DefaultRefImages.png]*

To set an image relative to another image's pixel coordinate system, you must specify the index of that image's registration element when calling [`MregSetLocation`](../../Reference/reg/MregSetLocation.md). Also, for the registration to be successful, it is necessary that each image and its reference image overlap. For information on the required minimum overlap between images, see [Specifying the minimum overlap between images](S07_Customizing_your_correlation_stitching_registration_settings.md).

For the image positioned relative to the global pixel coordinate system, the concept of overlapping does not apply. For this image, the transformation that roughly positions it in the global pixel coordinate system is also the optimal transformation. When you call [`MregCalculate`](../../Reference/reg/MregCalculate.md) this transformation is copied to the result buffer as the resulting transformation for the image.

You must not define the images' reference coordinate systems in a circular manner (see the diagram below).

*[Image: CircularRefCoord.png]*

Note that [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) will not return an error if the reference coordinate systems have been defined in a circular manner. An error will only be generated once you call [`MregCalculate`](../../Reference/reg/MregCalculate.md).

## Specifying the type of transformation and its settings

Once you have set each image's reference coordinate system, specify the transformation that will roughly map the image into this coordinate system, and thereby set the image's rough location. Specifying the rough location is optional, but improves performance. To set the rough location, use [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) to copy transformation settings, or specify a translation, translation with rotation, or perspective warping transformation.

When you specify a translation transformation (using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with [`M_POSITION_XY`](../../Reference/reg/MregSetLocation.md)), you must specify the X- and Y-offsets between any point in the current image's pixel coordinate system and the same point in its reference coordinate system. These offsets specify the rough location of the current image within the reference coordinate system. The following example illustrates these X- and Y-offsets.

*[Image: TranslationOffsets.png]*

In the case of an image that is rotated relative to its reference coordinate system you can set the rough location via a translation transformation and a rotation transformation (using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with [`M_POSITION_XY_ANGLE`](../../Reference/reg/MregSetLocation.md)).

The other type of transformation that you can specify is a perspective warping, using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with [`M_WARP_POLYNOMIAL`](../../Reference/reg/MregSetLocation.md), [`M_WARP_4_CORNER`](../../Reference/reg/MregSetLocation.md), or [`M_WARP_4_CORNER_REVERSE`](../../Reference/reg/MregSetLocation.md). If you specify a perspective warping, you can specify the mapping between the image's pixel coordinate system and the reference coordinate system in one of the following ways:

- Using a transformation matrix (when using a warping of type [`M_WARP_POLYNOMIAL`](../../Reference/reg/MregSetLocation.md)).
- Using a table containing 4 points in the source image and their corresponding points in the destination image (when using a warping of type [`M_WARP_4_CORNER`](../../Reference/reg/MregSetLocation.md) or [`M_WARP_4_CORNER_REVERSE`](../../Reference/reg/MregSetLocation.md)).

For more information on warping, see [Warping](../C04_Advanced_image_processing/S09_Warping.md).

Note that when you are setting the location of your images, the transformation used to position an image relative to its reference coordinate system can be different from one image to the next. For example, you can position one image using a translation and another image using a perspective warping.

## Copying the rough location

In some cases, you might have cameras set up at a fixed distance from one another to take sequential, partially overlapping images of a large object. The transformation information, as it relates to the previous image, is the same for each image. Rather than specifying this information for each image, you can copy this information from one registration element to another, using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with [`M_COPY_REG_CONTEXT`](../../Reference/reg/MregSetLocation.md) or [`M_COPY_REG_RESULT`](../../Reference/reg/MregSetLocation.md).

In some other cases, you might take several images of the same area to ensure that the registration results are extremely precise. To perform the registration you would probably want to select one of these images as the reference image and obtain registration results for one image in the selected reference image. Then, you would want to use the transformation results of the registration, to specify the rough location of another image relative to the same reference image. Rather than repeatedly getting results to set the rough locations, you can copy this transformation information directly from the result buffer to the registration element using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with [`M_COPY_REG_RESULT`](../../Reference/reg/MregSetLocation.md).

When copying the transformation information, you can also copy the reference index of the element from which you are copying by setting the [`Target`](../../Reference/reg/MregSetLocation.md) parameter to [`M_COPY`](../../Reference/reg/MregSetLocation.md). Alternatively, you can specify a different reference index.

The following image shows how you can use [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) to recursively optimize references by copying optimized transformations into new registration contexts:

*[Image: RegCopy.png]*

The following code snippet demonstrates how using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) simplifies copying the transformation information:

> **Code example:** [userguide.registration.setting_the_rough_location_of_your_images](userguide.registration.setting_the_rough_location_of_your_images)

## Precision of the rough location and its effect on speed

In general, the more precise the rough location of your images (specified with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md)), the faster your registration calculation will be. For example, you might know the precise location of your images. If this situation arises, it is possible to skip the optimization step of the registration calculation and only convert the transformations which specify the images' rough locations into the global pixel coordinate system (discussed later in this chapter). This is much faster than having to optimize each transformation in addition to performing the conversion. Alternatively, you might not know anything about your images aside from how they relate to each other. In this case, the rough locations will be very vague and the optimization step of the registration calculation could be relatively long.

Customize the optimization step of the registration calculation using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TRANSFORMATION_TYPE`](../../Reference/reg/MregControl.md). For example, once the rough location is specified, and if you know that rotation will be necessary to align the images correctly, then you must change the [`M_TRANSFORMATION_TYPE`](../../Reference/reg/MregControl.md) control type from its default value. For more information, see [Selecting the transformation type](S07_Customizing_your_correlation_stitching_registration_settings.md).
