---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Steps_to_performing_correlation_stitching_registration
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Steps to performing correlation stitching registration"
---

# Steps to performing a correlation-stitching registration

The following steps provide a basic methodology for using the Registration module to perform a correlation-stitching registration operation.

1. Allocate a registration context, using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_STITCHING`](../../Reference/reg/MregAlloc.md).
2. Allocate a registration result buffer to hold the results of the registration operation, using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_STITCHING_RESULT`](../../Reference/reg/MregAllocResult.md).
3. Verify that the number of registration elements is sufficient for your application (by default, it is 256); you can inquire the number using [`MregInquire`](../../Reference/reg/MregInquire.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md). A registration element contains the information required to register an image; therefore, the context must contain at least the same number of registration elements as images that need to be registered. To change the number of registration elements, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md).
4. Optionally, specify the rough location of the images, using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md) with each of the images' registration element.
5. Specify the global registration settings, using [`MregControl`](../../Reference/reg/MregControl.md).
6. Specify the registration settings for the individual registration elements, using [`MregControl`](../../Reference/reg/MregControl.md).
7. Perform the registration, using [`MregCalculate`](../../Reference/reg/MregCalculate.md).
8. Retrieve the required results from the result buffer, using [`MregGetResult`](../../Reference/reg/MregGetResult.md).
9. If necessary, save your registration context or your result buffer, using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md).
10. If necessary, use the registration results to perform mosaicing, using [`MregTransformImage`](../../Reference/reg/MregTransformImage.md).
11. If necessary, use the correlation-stitching registration results to convert positions between two of the following coordinate systems: the global pixel coordinate system, any registered image's pixel coordinate system, or a mosaic's coordinate system. This is done using [`MregTransformCoordinate`](../../Reference/reg/MregTransformCoordinate.md) or [`MregTransformCoordinateList`](../../Reference/reg/MregTransformCoordinateList.md).
12. Free all your allocated objects, using [`MregFree`](../../Reference/reg/MregFree.md), unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

Using the correlation-stitching registration results, you can create a mosaic. For more information, refer to [Mosaicing and super-resolution](S09_Mosaicing.md).
