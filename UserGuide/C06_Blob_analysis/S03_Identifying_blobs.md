---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Identifying_blobs
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Identifying blobs"
---

# Identifying blobs

The Aurora Imaging Library blob analysis capabilities allow you to identify and extract features of connected regions of pixels (commonly known as blobs) within an image. Aurora Imaging Library requires a user-specified blob identifier image to determine which pixels belong to which blob in the original image. Blob features involving overall shape are extracted directly from the identifier image. Features that use the actual pixel values of the blob also require the original image. For example:

*[Image: boltbina-2.png]*

The Aurora Imaging Library Blob Analysis module typically considers touching foreground pixels in the blob identifier image to be part of the same blob. Consequently, what is easily identifiable by the human eye as several distinct but touching blobs can be interpreted by Aurora Imaging Library as a single blob. In addition, any part of a blob that is in the background pixel state, because of lighting or reflection, is considered as background during analysis.

To reduce preprocessing, the blob identifier image should be acquired under the best possible circumstances. This means ensuring that blobs do not overlap and, if possible, don't touch. It also means ensuring the best possible lighting and using a background with a gray level that is very distinct from the gray level of the blobs. If noise is a problem, you might also need to filter the image after acquisition (for example, using a median filter, applying a convolution with [`M_SMOOTH`](../../Reference/im/MimConvolve.md), or using [`MimFilterAdaptive`](../../Reference/im/MimFilterAdaptive.md)).

## Segmenting the blob image

Once the best possible image is acquired and most noise is filtered out, you must separate the different blobs from the background. Segmentation can be done in two ways:

- Binarize the image, using [`MimBinarize`](../../Reference/im/MimBinarize.md) or [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md), so that background pixels are represented as zero values and blob pixels are represented as another value.
- Clip all background pixels to zero, while retaining the original values of blob pixels, using [`MimClip`](../../Reference/im/MimClip.md). This has the advantage of not needing a separate buffer to hold the binary image, but you will not see the result of the segmentation as clearly. [`MimBinarize`](../../Reference/im/MimBinarize.md) or [`MimBinarizeAdaptive`](../../Reference/im/MimBinarizeAdaptive.md) are usually better.

If simple segmentation is not possible due to poor lighting or blobs with the same gray level as parts of the background, you must develop a segmentation algorithm appropriate to your particular image.

## Preprocessing

Producing the blob identifier image frequently creates some spurious blobs or holes (for example, due to noise or lighting). Such noise blobs make it harder to interpret blob analysis results. If you have many noise blobs, you should probably preprocess the image before using it as an identifier. An opening operation (for non-zero valued blobs or holes) or a closing operation (for zero valued blobs or holes) will remove most noise without significantly affecting real features.

If blobs are touching, you might try eroding the image a few times to break them apart.

Preprocessing the blob identifier image might affect the accuracy of calculations because of the slight change in blob shape. If this is a problem, perform the calculations on all the blobs, including those that are actually introduced by noise, then use the results to filter out the noise. Note, however, that this might increase the memory required and the calculation time.
