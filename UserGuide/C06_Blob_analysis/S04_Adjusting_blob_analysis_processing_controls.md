---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Adjusting_blob_analysis_processing_controls
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Adjusting blob analysis processing controls"
---

# Adjusting blob analysis processing controls

Before performing any blob analysis calculations, you should ensure the correct interpretation of the blob identifier image. Use [`MblobControl`](../../Reference/blob/MblobControl.md) to control how certain aspects of the blob identifier image are interpreted, for example:

- Which pixel values are considered to be in the foreground ([`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md)).
- Whether non-zero pixels can have any value or must be set to the maximum value of the buffer, for example, 0xff for an 8-bit buffer ([`M_IDENTIFIER_TYPE`](../../Reference/blob/MblobControl.md)).
- Whether two pixels touching at their corners are considered part of the same blob, by appropriately defining the image lattice ([`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md)).
- The pixel aspect ratio of the image ([`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md)).
- Whether to produce separate results for each blob or for groups of blobs ([`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md)).
- How many Feret angles are considered when calculating a Feret feature ([`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md)). Typically, the default value will be appropriate.
- Whether partially computed results should be returned if the processing operation is interrupted ([`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md)).

## Controlling the image lattice

Aurora Imaging Library represents images using a square lattice and considers adjacent pixels along the vertical or horizontal axis as touching. However, you can control whether two diagonally adjacent pixels are considered touching.

*[Image: lattice.png]*

Use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md) to specify how the blob identifier image lattice should be interpreted. For example, the following is considered one blob if the lattice is set to [`M_8_CONNECTED`](../../Reference/blob/MblobControl.md), but two blobs if set to [`M_4_CONNECTED`](../../Reference/blob/MblobControl.md).

*[Image: touching.png]*

## Pixel aspect ratio

When acquiring an image of a scene, each pixel represents some real distance both in width and in height. Ideally, this distance is the same in both directions, producing square pixels and allowing for accurate feature calculations. However, after digitization, it is quite common for a pixel to represent a different distance in each direction. In blob analysis, image distortions directly affect feature extractions. There are three ways of dealing with this problem:

- You can use the Aurora Imaging Library Camera Calibration module to calibrate and correct your images. This works both for uniform and non-linear distortion.
- You can use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md). This works only for uniform non-rotational distortion.
- You could use [`MimResize`](../../Reference/im/MimResize.md) to adjust the image to have a constant pixel aspect ratio.

If you want to use the Camera Calibration module, you can use [`McalUniform`](../../Reference/cal/McalUniform.md) for uniform distortion, or [`McalGrid`](../../Reference/cal/McalGrid.md) or [`McalList`](../../Reference/cal/McalList.md) for any distortion. In these cases, you should correct the image using [`McalTransformImage`](../../Reference/cal/McalTransformImage.md) before using the Blob module if you require precise results. This is required because the module performs calculations in the pixel coordinate system even if the target is calibrated; results can be returned in either the pixel or relative coordinate system. The advantage of using this technique is that your target image is physically corrected.

If your images only have uniform, non-rotational distortion, you can use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md) instead of calibration, to take into account the aspect ratio when calculating features. The ratio of a pixel's width to its height is called the pixel aspect ratio. For example, a pixel of equal width and height has a pixel aspect ratio of 1.0. The actual aspect ratio can be calculated using a simple procedure. Grab an image of a true circle or square and extract the [`M_FERET_X`](../../Reference/blob/MblobGetResult.md) and [`M_FERET_Y`](../../Reference/blob/MblobGetResult.md) features, which are enabled for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md) with the [`M_BOX`](../../Reference/blob/MblobControl.md) group of features. The relationship between these features represents the actual pixel aspect ratio to be used in calculations ([`M_FERET_Y`](../../Reference/blob/MblobGetResult.md) / [`M_FERET_X`](../../Reference/blob/MblobGetResult.md)).

Note, all results are affected by the [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md) control type, including those that are just positions within the image. For example, to mark [`M_BOX_X_MIN`](../../Reference/blob/MblobGetResult.md) on an image using a graphics function, such as [`MgraDots`](../../Reference/gra/MgraDots.md), you must take the aspect ratio to which [`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md) is set into account (in this case by dividing the returned result by the aspect ratio). However, when using [`MblobDraw`](../../Reference/blob/MblobDraw.md) to draw features onto an image, the pixel aspect ratio is taken into account.

*[Image: aspratio.png]*

## Setting the blob identification mode and calculations on blobs

Using [`MblobControl`](../../Reference/blob/MblobControl.md) with the blob identification mode setting ([`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md)), you can control how blobs in the blob identifier image are grouped during calculations:

- All blobs are measured individually ([`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md)).
- All blobs are grouped together ([`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md)).
- Blobs which have the same label, or touching blobs with distinct labels, are grouped together ([`M_LABELED`](../../Reference/blob/MblobControl.md)).
- Blobs which have the same label are grouped together and touching blobs with different labels are treated separately ([`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md)).

When using the Blob Analysis module, you usually want to make feature calculations on each blob. For example, if you want to find the area of each cell in a tissue image, set the blob identification mode to [`M_INDIVIDUAL`](../../Reference/blob/MblobControl.md).

Sometimes, however, you need calculations based on the entire image rather than individual blobs. For example, you might want to calculate the area of all the copper in a rock sample image. Aurora Imaging Library simplifies your task by allowing you to group all foreground pixels together by setting the blob identification mode to [`M_WHOLE_IMAGE`](../../Reference/blob/MblobControl.md). Blobs in an image are treated as one blob and features are calculated for this grouped blob. If the blob identifier image is already binarized (for example, pixel values for an 8-bit image are either 0 or 0xff), you can set [`M_IDENTIFIER_TYPE`](../../Reference/blob/MblobControl.md) to [`M_BINARY`](../../Reference/blob/MblobControl.md) to calculate features faster.

Blob identification mode [`M_LABELED`](../../Reference/blob/MblobControl.md) allows you to do joint calculations on blobs with the same label value, and touching blobs with different labels. When using labeled mode, each blob must have a uniform pixel value. That is, all pixels in a blob must have the same intensity value. If the blobs that you would like to group do not have the same label, you can use [`MblobSelect`](../../Reference/blob/MblobSelect.md) with [`M_MERGE`](../../Reference/blob/MblobSelect.md) to group blobs together and assign a common label to them.

*[Image: bloblb.png]*

To treat touching blobs that have different labels as distinct, use the blob identification mode [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md). With this mode, you can perform calculations on individual blobs even though they are touching, provided each blob has a different label.

When using [`M_LABELED`](../../Reference/blob/MblobControl.md) or [`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md), [`M_FOREGROUND_VALUE`](../../Reference/blob/MblobControl.md) cannot be set to [`M_ZERO`](../../Reference/blob/MblobControl.md), and the blob identifier image cannot be binary (1-bit).

[`M_LABELED`](../../Reference/blob/MblobControl.md) is not supported when using [`MblobMerge`](../../Reference/blob/MblobMerge.md), which merges results of two separate blob result buffers into a single result buffer.

[`M_LABELED_TOUCHING`](../../Reference/blob/MblobControl.md) is not supported with the following:

- [`MblobCalculate`](../../Reference/blob/MblobCalculate.md), when chain features are enabled ([`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_CHAINS`](../../Reference/blob/MblobControl.md) or [`M_ALL_FEATURES`](../../Reference/blob/MblobControl.md) enabled).
- [`MblobDraw`](../../Reference/blob/MblobDraw.md) with [`M_DRAW_BLOBS_CONTOUR`](../../Reference/blob/MblobDraw.md), [`M_DRAW_HOLES_CONTOUR`](../../Reference/blob/MblobDraw.md), or a combination of the two.
- [`MblobSelect`](../../Reference/blob/MblobSelect.md) with [`M_MERGE`](../../Reference/blob/MblobSelect.md).

## Returning partial results

Using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_RETURN_PARTIAL_RESULTS`](../../Reference/blob/MblobControl.md), you can control whether partial results of the blob calculation will be available if the computation is interrupted by one of the following conditions:

- The maximum number of blobs ([`M_MAX_BLOBS`](../../Reference/blob/MblobControl.md)) has been processed.
- The timeout limit ([`M_TIMEOUT`](../../Reference/blob/MblobControl.md)) is exceeded.

> **Note:** Note that when partial results are requested and a timeout occurs, some results might not be available for certain blobs.
