---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Registration_elements_for_correlation_stitching
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Registration elements for correlation stitching"
---

# Registration elements and images for correlation-stitching

The settings used to control the correlation-stitching registration calculation are specified in registration elements, located in the registration context. The results of the calculation are stored, per image in the registration result buffer. The result buffer can also store the settings for operations that use the correlation-stitching registration results, such as mosaicing.

Each image in the image array passed to [`MregCalculate`](../../Reference/reg/MregCalculate.md) is associated with the registration element and its registration image result whose index matches its own array index. For a correlation-stitching registration or mosaicing operation, each registration element controls how the registration of its associated image is performed. Each registration image result stores the results and settings for its associated image. This offers you the flexibility of being able to reuse registration settings and/or results with many different series of input images. For example, when composing a mosaic, images are combined to form one larger image according to the positions calculated during the registration. The freedom to reuse registration results with more than one series of input images allows you to perform the registration calculation once and quickly generate multiple mosaics.

The following diagram illustrates the relationship that exists between the registration elements, the input images, and the registration image results.

*[Image: RegistrationContext.png]*

It is important to know the index of the images in the input image array so that you can properly set up their corresponding registration elements. Also, you need to have at least as many registration elements as images. To verify that the number of registration elements is sufficient for your application, use [`MregInquire`](../../Reference/reg/MregInquire.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md). The default number is 256. To increase or decrease the number of registration elements, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md). Note that it is possible to have more registration elements than images; for information on this, see [Skipping the optimization step](S07_Customizing_your_correlation_stitching_registration_settings.md).

The following diagram illustrates how images and registration elements tie in with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md), [`MregTransformImage`](../../Reference/reg/MregTransformImage.md), [`MregTransformCoordinate`](../../Reference/reg/MregTransformCoordinate.md), [`MregTransformCoordinateList`](../../Reference/reg/MregTransformCoordinateList.md), and [`MregCalculate`](../../Reference/reg/MregCalculate.md) in the Registration module.

*[Image: RegistrationOverview.png]*

Note that there is no direct link in the diagram between the images and the [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) function because you set the rough location of your images using registration elements, and not the images themselves. However, knowledge of the relationships between your images is necessary and will be discussed in [Setting the rough location of your images](S06_Setting_the_rough_location_of_your_images.md). With the functions that do require input images ([`MregCalculate`](../../Reference/reg/MregCalculate.md) and [`MregTransformImage`](../../Reference/reg/MregTransformImage.md)), you can use more than one series of images with the same registration context or registration result buffer.
