---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: Extending_your_depth_of_field
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / Extending your depth of field"
---

# Extending your depth of field

At a given focus distance, the entire image is not always in focus, resulting in blurry objects. Only objects within a tolerated distance from this focus distance are in focus. The tolerated distance is referred to as the depth of field, and it increases as the focus distance increases, assuming no other camera settings are changed. To emulate a larger depth of field, so that all objects of interest in the field of view are in focus, you can perform an extended depth of field (EDoF) registration operation using the Aurora Imaging Library Registration module. This operation takes multiple images of the same scene, taken at different focus distances, and combines the focused objects in all of the images to create an image with all the objects of interest in focus. This process is also known as focus stacking.

*[Image: ExtendedDepthOfFieldExample.png]*

Extended depth of field registration is useful for applications where normal camera focusing techniques cannot capture the entire image in focus, such as microscopy or macro photography.

The extended depth of field registration operation first preprocesses each input image. Considering the sharp portions in the input images, the operation computes the best combination for a fusion of the focused image sections. The above image shows two source images of the same scene, taken with different focus distances. The smiley face is in the foreground, and the lightning bolt is in the middle-ground. The source images each contain a portion of the scene in focus; after the extended depth of field registration operation, the smiley face and the lightning bolt are both in focus. Since the source images do not contain a focused version of the dark shape in the background, it cannot be resolved in the extended depth of field (EDoF) image.

## Steps to performing an extended depth of field registration

The following steps provide a basic methodology for performing an extended depth of field operation on multiple images:

1. Allocate an EDoF registration context, using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_EXTENDED_DEPTH_OF_FIELD`](../../Reference/reg/MregAlloc.md).
2. Allocate an EDoF registration result buffer to hold the results of the registration operation, using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_EXTENDED_DEPTH_OF_FIELD_RESULT`](../../Reference/reg/MregAllocResult.md).
3. If necessary, specify the control settings of your EDoF registration context using [`MregControl`](../../Reference/reg/MregControl.md).
4. Set up an array with the identifiers of image buffers that hold the input images. All the images should be of the same scene, taken with all the same camera settings except focus, and in buffers of the same size, type, and number of bands.
5. If necessary, allocate an image buffer to hold the EDoF image (alternatively, the EDoF image can be stored in the EDoF result buffer). The image buffer should be of the same size, type, and number of bands as the input images.
6. Perform the extended depth of field operation, using [`MregCalculate`](../../Reference/reg/MregCalculate.md). If you want to perform the registration with all the default settings, skip steps 1 and 3 and use the [`M_DEFAULT_EXTENDED_DEPTH_OF_FIELD_CONTEXT`](../../Reference/reg/MregCalculate.md) context.
7. If necessary, retrieve the required results from the result buffer using [`MregGetResult`](../../Reference/reg/MregGetResult.md).
8. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), you can draw the EDoF image from the result buffer into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_EDOF_IMAGE`](../../Reference/reg/MregDraw.md). You can use [`MregGetResult`](../../Reference/reg/MregGetResult.md) to establish the size and type of the image buffer required to draw the EDoF image.
9. If necessary, save your registration context, using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md).
10. You can clear the result buffer to perform additional operations using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_RESET`](../../Reference/reg/MregCalculate.md).
11. Free your allocated registration objects, using [`MregFree`](../../Reference/reg/MregFree.md), unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

The following animation demonstrates how extended depth of field is achieved.

*[Image: EDoF]*

This example presents a subset of the source images used to calculate the EDoF of a single object, and that of multiple objects in a scene. Each image is taken at a different focus distance, such that the focus is moved from the farthest point to the closest point in the scene. At each focus distance, a part of an object is in focus. In this case, the final extended depth of field image is calculated using[`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_DEFAULT_EXTENDED_DEPTH_OF_FIELD_CONTEXT`](../../Reference/reg/MregCalculate.md) and the source images.

> **Note:** Note that even when the animation indicates that the bottle's label is in focus, the entire label is not completely in focus because the image is taken at an angle and the label is on a slanted surface of the object. Several images taken at different focus distances are required to have the bottle's label completely in focus. It is illustrated as "in focus" only for display purposes.

## Customizing your extended depth of field context

You can customize an extended depth of field registration operation using [`MregControl`](../../Reference/reg/MregControl.md).

If the default value for the maximum radius of the circle of confusion in your input images is incorrect, adjust it using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_CIRCLE_OF_CONFUSION_RADIUS_MAX`](../../Reference/reg/MregControl.md). Also known as a blurring circle, a circle of confusion is the image representation of an out-of-focus point source of light. When the circle of confusion of a point source of light falls within a single pixel, the point source is considered in focus. A larger circle of confusion radius indicates that the point source is less in focus. The following image illustrates the circle of confusion of a point of an object as a source; in this case, the point is out of focus, since it occupies several pixels. Note that it is not drawn to scale.

*[Image: CircleOfConfusionDrawing.png]*

For Aurora Imaging Library to optimally perform an extended depth of field operation, it must know the maximum radius of the circle of confusion among all input images. Overestimating or underestimating this value will not produce optimal results.

In multiple images with varying focus distances, an out of focus object can appear to be at a different location than in an image where that object is in focus. If necessary, adjust the maximum translation distance between any two input images, in pixels, with the control type [`M_TRANSLATION_TOLERANCE`](../../Reference/reg/MregControl.md).

You can also reduce processing time by setting [`M_MODE`](../../Reference/reg/MregControl.md) to [`M_FAST`](../../Reference/reg/MregControl.md), but this can potentially reduce the accuracy of the operation.

Note that since the input images must be of the same scene, it is assumed that the images completely overlap. Consequently, you do not set rough locations with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md).

## Registration process

Using [`MregCalculate`](../../Reference/reg/MregCalculate.md), each image is analyzed for focus, and the best parts of the image are taken into account with a weighted contribution. An image with a totally out of focus portion will contribute less of that portion to the final EDoF image, while the image with the best focus for that area of the image will contribute the most. The registration algorithm finds these portions by preprocessing the images before the EDoF image is computed.

Depending on your needs, you can create the EDoF image right away or preprocess images using [`MregCalculate`](../../Reference/reg/MregCalculate.md) as they are being grabbed and then, once all the images have been grabbed and preprocessed, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) to just compute the EDoF image.

If all the images are available, you can pass them all at once to [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md) to create the extended depth of field (EDoF) image immediately. [`MregCalculate`](../../Reference/reg/MregCalculate.md) can save the EDoF image in an image buffer; alternatively, it can save it in a result buffer, along with other preprocessing results. In the latter case, use [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_EDOF_IMAGE`](../../Reference/reg/MregDraw.md) to access the EDoF image. Note that with [`M_COMPUTE`](../../Reference/reg/MregCalculate.md), [`MregCalculate`](../../Reference/reg/MregCalculate.md) clears the result buffer before writing to it.

If the images are not all available at once, you can preprocess them in stages by passing them as they become available to [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md) and passing the same result buffer. Using [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md) can reduce memory consumption when creating an extended depth of field image, since the source images don't have to be available all at once. When adding the last of the source images for an EDoF operation, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md). [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md) preprocesses these additional images, and then the function computes the EDoF image using this new information and all of the information previously processed and stored in the result buffer. If you want to clear the result buffer of previous results, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_RESET`](../../Reference/reg/MregCalculate.md).

The following diagram shows the ways to use [`MregCalculate`](../../Reference/reg/MregCalculate.md) to perform an extended depth of field registration operation.

*[Image: ExtendedDepthOfFieldFlowchart.png]*

## Retrieving and using extended depth of field results

After performing an extended depth of field registration operation, the EDoF image is stored either in a result buffer or an image buffer.

If [`MregCalculate`](../../Reference/reg/MregCalculate.md) used an image buffer as the destination, you can access the image immediately after the registration operation. You cannot access or draw other results of the operation (using [`MregGetResult`](../../Reference/reg/MregGetResult.md) or [`MregDraw`](../../Reference/reg/MregDraw.md)) because only the EDoF image is saved in the buffer.

If [`MregCalculate`](../../Reference/reg/MregCalculate.md) used a result buffer as the destination, the EDoF image is stored in the result buffer along with some results and some information about the source images. You can use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_STATUS`](../../Reference/reg/MregGetResult.md) to check if the EDoF image is computed. A return value of [`M_ACCUMULATE`](../../Reference/reg/MregGetResult.md) means the result buffer contains preprocessing information, but the final EDoF image is not available. If [`MregCalculate`](../../Reference/reg/MregCalculate.md) returns [`M_COMPLETE`](../../Reference/reg/MregGetResult.md), the preprocessing information and the EDoF image are available.

Many of the results that you can retrieve using [`MregGetResult`](../../Reference/reg/MregGetResult.md) depend on their availability within the result buffer. For these result types, you should add the combination constant [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md)when calling [`MregGetResult`](../../Reference/reg/MregGetResult.md). If you retrieve [`M_TRUE`](../../Reference/reg/MregGetResult.md), the information is available and can be retrieved using another call to [`MregGetResult`](../../Reference/reg/MregGetResult.md) without [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md). If the function returns [`M_FALSE`](../../Reference/reg/MregGetResult.md), the requested information is not available, and calling [`MregGetResult`](../../Reference/reg/MregGetResult.md) without [`M_AVAILABLE`](../../Reference/reg/MregGetResult.md) will cause an error.

If you want to access the EDoF image stored in the result buffer, you must allocate an image buffer, and then, fill the image buffer with the EDoF image, using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_EDOF_IMAGE`](../../Reference/reg/MregDraw.md). To allocate a buffer of the appropriate size, you can retrieve the size and characteristics of the EDoF image using [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_IMAGE_...`](../../Reference/reg/MregGetResult.md). If you call [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_EDOF_IMAGE`](../../Reference/reg/MregDraw.md) and specify a destination image buffer that is too small, the EDoF image will be clipped to fit into the buffer.

## Extended depth of field example

The extended depth of field example _ExtendedDepthOfField.cpp_ illustrates how the operation can create a single image of a scene with all the objects in focus. To run/view this and other examples, use Aurora Imaging Example Launcher.
