---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Customizing_your_correlation_stitching_registration_settings
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Customizing your correlation stitching registration settings"
---

# Customizing your registration settings

Once you have set the rough location of your images, you can customize how to perform the registration operation, using [`MregControl`](../../Reference/reg/MregControl.md). Some of the following settings will affect the registration process in general, whereas others will affect the registration of a specific image.

## Selecting the transformation type

You can control the type of transformation that the registration operation can use to optimize the match in the overlapping region, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TRANSFORMATION_TYPE`](../../Reference/reg/MregControl.md). This is a global setting and it will affect the registration of all your images. The Registration module can optimize the match using a translation, a translation with rotation, a translation with rotation and scale, or a perspective transformation ([`M_TRANSLATION`](../../Reference/reg/MregControl.md), [`M_TRANSLATION_ROTATION`](../../Reference/reg/MregControl.md), [`M_TRANSLATION_ROTATION_SCALE`](../../Reference/reg/MregControl.md), and [`M_PERSPECTIVE`](../../Reference/reg/MregControl.md) respectively). Choosing between the types is not always obvious and will depend on the way the images were taken, as well as the precision used when setting the rough location of the images.

For example, if the camera that captured the images moved only from side to side or up and down, then a translation transformation should be sufficient to optimize the alignment. Alternatively, if the camera made more complex movements when capturing the series of images, such as rotating, tilting, zooming, or panning, a perspective transformation should produce a more accurate alignment.

The optimal transformation depends on how you specified the rough location of your images. Taking the latter example, although the camera made complex movements, if you precisely specified how each image maps into its reference image's pixel coordinate system, you might only need a translation to perfect the alignment.

Note that if you are not sure which transformation type to select, select the perspective transformation since it is more general. However, if you are sure a translation will result in an optimal alignment, it is faster to use a translation transformation than it is to use the more general perspective transformation.

The following example shows a pair of images, first placed according to their rough locations and then according to their positions calculated using [`MregCalculate`](../../Reference/reg/MregCalculate.md), in which a perspective warping transformation type was permitted. Notice how the distorted quadrilateral was successfully warped so that it overlaps with the corresponding square in its reference image.

*[Image: RegistrationWarping.png]*

## Specifying the maximum allowable displacement

You can set the maximum displacement that the transformation can shift the pixels in the images during registration to optimize the match in the overlapping regions. To do so, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_LOCATION_DELTA`](../../Reference/reg/MregControl.md) and specify the maximum displacement as a percentage of the biggest dimension in the images to align. The default setting is 5%. This is a global setting and it will affect the registration of all your images.

In general, the more precise you are in setting the rough location of your images (using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md)), the faster your registration calculation can be. Being more precise when setting the location of your images allows you to set a smaller value for the maximum displacement, which results in a faster calculation. A more general location means that you must specify a larger maximum displacement; consequently, the algorithm will have to search a much broader area of the images to find an optimal match, making the calculation longer.

## Skipping the optimization step

When the precise location of one or more images is already known, you can bypass the optimization step of the registration calculation. If you choose to do so, for each of these images, the transformation specified in [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) represents the optimal transformation. When you call [`MregCalculate`](../../Reference/reg/MregCalculate.md), the specified transformation will be converted so that it maps the image into the global pixel coordinate system instead of into its reference image's pixel coordinate system.

There are two ways of bypassing the optimization step of the registration of an image (image X). They differ in how other images using image X as their reference will be registered.

- You can disable the [`MregControl`](../../Reference/reg/MregControl.md)   [`M_OPTIMIZE_LOCATION`](../../Reference/reg/MregControl.md) control type of image X's registration element. If another image (image A) is to be registered with respect to image X, the optimization step of image A's registration will still be performed.
- You can replace image X in the image array by **M_NULL**. Since the registration calculation does not have access to image X, no overlapping region will exist between it and any other images. Therefore, if image A is to be registered with respect to image X, the optimization step of image A's registration will not be performed (even if [`M_OPTIMIZE_LOCATION`](../../Reference/reg/MregControl.md) is enabled). In this case, image A's optimal transformation is the one set with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md).

The following illustration gives an example of the two ways of bypassing the optimization step during the registration of an image, and their different effects on other images.

*[Image: BypassOptimization.png]*

## Specifying the minimum overlap between images

To increase the speed and robustness of the registration, you can specify the minimum overlap between each image and its reference image, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_MIN_OVERLAP`](../../Reference/reg/MregControl.md). This is a global setting and affects the registration of all images. Express the overlap as a percentage of the smallest of the two images.

*[Image: MinimumOverlap.png]*

[`MregSetLocation`](../../Reference/reg/MregSetLocation.md) will not generate an error if the specified rough location does not comply with the minimum overlap setting. However, when you call [`MregCalculate`](../../Reference/reg/MregCalculate.md), the optimization step of the registration calculation is not performed for this particular image. In addition, you will get [`M_FAILURE`](../../Reference/reg/MregGetResult.md) if you retrieve the calculation result using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_RESULT`](../../Reference/reg/MregGetResult.md), for either the overall registration process or the registration of the individual image.

## Accuracy

You can control the accuracy of the registration calculation using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_ACCURACY`](../../Reference/reg/MregControl.md). This is a global setting and it will affect the registration of all your images. Registration accuracy can be set to one of the following: [`M_LOW`](../../Reference/reg/MregControl.md) or [`M_HIGH`](../../Reference/reg/MregControl.md). The default setting is [`M_HIGH`](../../Reference/reg/MregControl.md). When you set the accuracy to high, the registration calculation is performed with subpixel accuracy. To perform the calculation with pixel accuracy, set [`M_ACCURACY`](../../Reference/reg/MregControl.md) to [`M_LOW`](../../Reference/reg/MregControl.md).

## Setting the origin of an image's pixel coordinate system

In certain cases, it can be useful to individually choose the origin of an image's pixel coordinate system instead of using the image's default coordinate system (the center of the image's top-left pixel). To do so, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_REFERENCE_X`](../../Reference/reg/MregControl.md) and [`M_REFERENCE_Y`](../../Reference/reg/MregControl.md) to set the coordinates of the origin to any point inside or outside of the image. An example of when this is useful is when the image is stored in a child buffer, and you know its exact location within the parent buffer. Another example is when images are taken with respect to a moving point, such as the corner of a moving table. In this case, the origin of your images' pixel coordinate systems can be set to that point.

It is very important to be aware of the origin of each image's pixel coordinate system, especially when trying to set the location of your images with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md). It is also crucial to know the origin of an image's pixel coordinate system when performing other operations in the Registration module, such as composing a mosaic or transforming coordinates from a source coordinate system to a destination coordinate system.
