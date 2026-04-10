---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Blob_analysis_overview
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Blob analysis overview"
---

# Blob analysis overview

Blob analysis allows you to identify connected regions of pixels within an image, then calculate selected features of those regions. The regions are commonly known as blobs. Blobs are areas of touching pixels that are in the same logical pixel state. This pixel state is called the foreground state, while the alternate state is called the background state. Typically, the background has the value zero and the foreground is everything else (although some control is generally provided to reverse the sense).

In many applications, we are interested only in blobs whose features satisfy certain criteria. Since computation is time-consuming, blob analysis is often performed as an elimination process whereby only blobs of interest are considered in further analysis. The steps involved in feature extraction are:

1. Analyze an image and exclude or delete blobs that don't meet determined criteria.
2. Analyze the remaining blobs to extract further features and determine their criteria.

Repeat these steps, as necessary, until you have all the blob measurement results you need.

Reducing the raw data to just a few feature measurements generally produces more comprehensible and useful results.

## Aurora Imaging Library and blob analysis

The Aurora Imaging Library package includes a Blob Analysis module that can extract a wide assortment of blob features, such as the blob area, perimeter, maximum diameter at a given angle (Feret diameter), minimum bounding box, and compactness.

Aurora Imaging Library uses a user-specified blob identifier image to discriminate between blobs and the background. Controls are provided to allow you to specify how this identifier image is interpreted (which pixels are part of which blob). Blobs are considered to consist of either zero or non-zero pixels, depending on the foreground control setting. The non-zero pixels can either have any value or must be set to the maximum value of the buffer (for example, 0xff for an 8-bit image), depending on the identifier type (grayscale or binary). In addition, Aurora Imaging Library provides controls to take into account such blob image information as the pixel aspect ratio.

For binary feature extractions, such as those that pertain to the overall shape of the blob, the blob identifier image is used for both identification and computation. For grayscale extractions (for example, the mean pixel value in a blob), you must also provide a grayscale image whose pixel values will be used in computation. Instead of providing a separate blob identifier image, you can provide a grayscale image buffer or 3D-processable depth map container with an ROI that acts as the blob identifier image.

Aurora Imaging Library also allows you to combine computation results and merge them together into one result. It is also sometimes desirable to merge blobs with specific characteristics. Aurora Imaging Library supports this feature as well.

The module provides some support for camera calibration. Although most results are calculated in pixels, if the blob identifier image has been associated with a camera calibration context, positional results can be returned in calibrated real-world units. If a grayscale image has also been provided, it must have the same camera calibration context for the results to be calibrated.

With Aurora Imaging Library, you can perform blob analysis on 1-bit, 8-bit or 16-bit unsigned buffers.
