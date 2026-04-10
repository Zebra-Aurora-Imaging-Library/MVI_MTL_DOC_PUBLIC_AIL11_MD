---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Steps_to_performing_blob_analysis
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Steps to performing blob analysis"
---

# Steps to performing blob analysis

The following steps provide a basic methodology for using the Aurora Imaging Library Blob Analysis module:

1. Allocate a context, using [`MblobAlloc`](../../Reference/blob/MblobAlloc.md). This context is used to specify the features that should be calculated. By default, few features are enabled for calculation.
2. Allocate a buffer for blob analysis results, using [`MblobAllocResult`](../../Reference/blob/MblobAllocResult.md).
3. If necessary, adjust global blob analysis settings to fit your application, using successive calls to [`MblobControl`](../../Reference/blob/MblobControl.md).
4. Grab or load an image that was captured under the best possible conditions to minimize the amount of preprocessing required.
5. If necessary, reduce the amount of noise in the image. Noise makes the next step more difficult.
6. Segment the image so that blobs are separated from the background and from each other. Typically, this involves binarizing the image so that the background is in one state (zero or non-zero) and the blob pixels are in the other state. This image is known as the blob identifier image. If you plan to perform grayscale calculations, you will need the original grayscale image as well.
7. If necessary, preprocess the blob identifier image. If there are too many noise particles, calculation time will be increased. An opening operation (for non-zero blobs) or a closing operation (for zero blobs) will remove most of the noise particles without significantly affecting real blobs. You might also need to separate touching blobs at this stage (or they will be counted as a single blob).
8. Calculate the required features and analyze the results. This involves the following:
   1. Enable the features that should be calculated, using successive calls to [`MblobControl`](../../Reference/blob/MblobControl.md).
   2. Select the feature(s) to use as a sorting key(s) for results, using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_SORTn`](../../Reference/blob/MblobControl.md). The calculated results are sorted according to the sorting key selection.
   3. Calculate results for the features enabled for calculation, using [`MblobCalculate`](../../Reference/blob/MblobCalculate.md). For this function, you will have to specify the blob identifier image that will be used to identify the blobs and calculate binary features, and (optionally) the grayscale image that will be used to calculate grayscale features. Instead of passing the blob identifier image separately, you can pass a grayscale image buffer or 3D-processable depth map container with an ROI that acts as the blob identifier image.
   4. If necessary, exclude or delete blobs that do not meet a required criteria, using successive calls to [`MblobSelect`](../../Reference/blob/MblobSelect.md). Results for the excluded or deleted blobs will not be returned when calling [`MblobGetResult`](../../Reference/blob/MblobGetResult.md). Excluded blobs will be ignored in future calculations, while deleted blobs will be removed from the blob analysis result buffer altogether and will not be re-included in future calculations.
   5. Get the number of blobs currently included, using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md)with [`M_NUMBER`](../../Reference/blob/MblobGetResult.md), and then retrieve other required results from the blob result buffer, using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md). You can also use [`MblobGetLabel`](../../Reference/blob/MblobGetLabel.md) to obtain the label value of a specific blob at a specified position. Note that trying to retrieve a result that is not available generates an error.
   6. If necessary, preprocess the blob identifier image. If there are too many noise particles, calculation time will be increased. An opening operation (for non-zero blobs) or a closing operation (for zero blobs) will remove most of the noise particles without significantly affecting real blobs. You might also need to separate touching blobs at this stage (or they will be counted as a single blob).
   You can repeat this step until you obtain all required results for the blobs of interest. Note, the process of excluding or deleting unwanted blobs and then calculating more features is the preferred method if you have many unwanted blobs. If this is not the case, it is often faster to calculate all the required features for all the blobs with a single call to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), and then exclude or delete unwanted blob results afterwards.
9. Free all your allocated objects, using [`MblobFree`](../../Reference/blob/MblobFree.md), unless [`M_UNIQUE_ID`](../../Reference/blob/MblobAlloc.md) was specified during allocation.
