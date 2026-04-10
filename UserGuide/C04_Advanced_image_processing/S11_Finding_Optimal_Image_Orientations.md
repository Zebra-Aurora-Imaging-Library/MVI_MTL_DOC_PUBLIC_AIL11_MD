---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Finding_Optimal_Image_Orientations
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Finding Optimal Image Orientations"
---

# Finding dominant image orientations

An image's orientation can be affected by a wide variety of circumstances such as misalignment of the camera, robot movements, or objects landing in various positions on a conveyor belt. Using the consistent spatial patterns in an image, Aurora Imaging Library can find the dominant orientations (best viewing angles). Finding the dominant orientations of your image, and then rotating the image accordingly, can greatly improve the robustness of your applications, since some modules have limited angle support, such as the Aurora Imaging Library Code and OCR modules. You can find the dominant orientations of your images using [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md).

You can find multiple orientations to check that the most dominant orientation of the image, as calculated by Aurora Imaging Library, is the best orientation for your particular application. *[Image: FindOrientationAlignedText.png]*

The above images demonstrate one possible use of the find orientation operation. On the left is an image of some rotated text with an arrow in the direction of the most dominant orientation drawn onto the image. The image on the right shows the rotated image with the text at the right orientation.

The orientations are stored in a find orientation image processing result buffer, which you must allocate using [`MimAllocResult`](../../Reference/im/MimAllocResult.md) with [`M_FIND_ORIENTATION_LIST`](../../Reference/im/MimAllocResult.md).

## Customizing your find orientation image processing context

The find orientation operation requires that your image has dimensions (X-size and Y-size) that are a power of two. If the image buffer's dimensions do not meet this requirement, [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) determines the orientation of a clipped or resized version of the source image, but does not alter the original source image. If you set [`MimControl`](../../Reference/im/MimControl.md) with [`M_MODE`](../../Reference/im/MimControl.md) to [`M_CLIP_CENTER`](../../Reference/im/MimControl.md), the find orientation operation will find the best orientations of the largest centered portion of the image with dimensions that are a power of two. Otherwise, the function will perform the operation on either a subsampled version of the image (set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_RESIZE_DOWN`](../../Reference/im/MimControl.md)) or a zoomed version of the image (set using [`MimControl`](../../Reference/im/MimControl.md) with [`M_RESIZE_UP`](../../Reference/im/MimControl.md)). Using [`MimControl`](../../Reference/im/MimControl.md) with [`M_INTERPOLATION_MODE`](../../Reference/im/MimControl.md), you can specify how to interpolate during the resizing. If speed and precision are an issue, you can try using the find orientation operation on a smaller section of the image. Larger images have more information, and will take longer to process. For more information on resizing, see [Basic geometric transforms](../C03_Image_processing/S15_Basic_geometrical_transform.md).

The [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) function indirectly relies on straight edges to find the orientations; so your image should ideally have objects that have well-defined, linear edges. For example, images of bar codes and text typically have better edges than images of rounded objects (for example, gaskets and washers). If your image does not have well-defined edges due to focus or resolution problems, performing a sharpening operation on the image can resolve the problem in some cases.

The find orientation operation looks for consistent spatial patterns, and can filter out unwanted frequencies for a more robust operation. The operation excludes frequencies as a percentage of the maximum possible frequency in the image, which is dictated by the size of your image. Larger images will have higher maximum possible frequencies. The default setting is to disregard frequencies with a value below 5% of the maximum possible frequency in the image; however, if the default settings picks up artifacts (for example, shadows), and as such, are not optimally configured for your application, you can set the low frequency cutoff value using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FREQUENCY_CUTOFF_RATIO_LOW`](../../Reference/im/MimControl.md). Additionally, the high frequencies in the image usually delineate edges, which are useful for the operation, but very high frequencies might be noise, and therefore detrimental to the precision of the operation. Having a well-tuned high frequency cutoff can reduce noise in your image and can enhance the accuracy of the find orientation operation. The default setting is to disregard frequencies with a value above 95% of the maximum possible frequency in the image. To set this high frequency cutoff as a percentage of the maximum frequency in your image, use [`MimControl`](../../Reference/im/MimControl.md) with [`M_FREQUENCY_CUTOFF_RATIO_HIGH`](../../Reference/im/MimControl.md). The majority of applications will return meaningful results with the default values, and they should only be changed for advanced applications. If your image does not contain prominent, sharp edges (for example, an image of a cell with faint membranes), [`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) might return better results with both frequency cutoffs lowered.

Like frequencies that are too high or too low, the borders of the image can potentially pose a problem and obfuscate the results. You can use [`M_BORDER_ATTENUATION`](../../Reference/im/MimControl.md) to specify whether Aurora Imaging Library can ignore the borders of the image when performing the find orientation operation.

## Using the find orientation image processing results

[`MimFindOrientation`](../../Reference/im/MimFindOrientation.md) will find the N most dominant image orientations of your image, where N is the number of entries with which you have allocated your find orientation image processing result buffer. You can either draw the found orientations for a visual representation, or retrieve the image orientation results for further use. The former can help you determine which orientation is the best for your specific application.

To get a visual representation of the orientations, you can draw arrows pointing in the directions of the orientations using [`MimDraw`](../../Reference/im/MimDraw.md) with [`M_DRAW_IMAGE_ORIENTATION`](../../Reference/im/MimDraw.md). You can draw one or multiple orientations at a time; each arrow intersects the center of the image. The arrows are drawn at the angle of the orientation that they represent, and their size is proportional to their score; the longest arrow has the highest score. The image below shows the arrows drawn by [`MimDraw`](../../Reference/im/MimDraw.md)with the scores (retrieved using [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md)) labeled for convenience. *[Image: FindOrientationDraw.png]*

The dominant orientations are angle values that you can use to rotate your image to achieve the required orientation. Use [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) with [`M_ANGLE`](../../Reference/im/MimGetResult1d.md) to retrieve the angle values of each orientation. For information on how to rotate your image using the angles found using the find orientation operation, see [Basic geometric transforms](../C03_Image_processing/S15_Basic_geometrical_transform.md).

The results are automatically sorted and stored in descending order, with the best image orientation always in the first position, at index 0. You can use [`MimGetResult1d`](../../Reference/im/MimGetResult1d.md) with [`M_SCORE`](../../Reference/im/MimGetResult1d.md) to establish the score of each orientation. The results are scored as a percentage, and are relative to each other. For example, the first orientation always has a score of 100, and Aurora Imaging Library considers it to be the best orientation based on the given information. Other orientations will have lower scores.

## An example

The _FindImageOrientation.cpp_ example illustrates a find orientation operation used on a variety of different images. To run this and other examples, use Aurora Imaging Example Launcher.
