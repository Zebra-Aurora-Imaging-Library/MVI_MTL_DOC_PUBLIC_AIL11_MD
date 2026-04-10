---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Correlation_stitching_registration
section: Correlation_stitching_registration_process
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / CorrelationStitchingRegistration / Correlation stitching registration process"
---

# Correlation-stitching registration process

For correlation-stitching registration, [`MregCalculate`](../../Reference/reg/MregCalculate.md) takes a series of images and their rough locations in another image's pixel coordinate system or the global pixel coordinate system, and applies the following three step process to determine the transformations that optimally position each image in the global pixel coordinate system.

1. Each image is first transformed into its rough location in the pixel coordinate system of its reference image (specified using [`MregSetLocation`](../../Reference/reg/MregSetLocation.md)). This initial rough alignment of each image with its reference image creates a region in which both images overlap, which is necessary for the correlation-stitching registration process to be successful.
2. For each image, the correlation-stitching registration operation then finds the transformation that optimizes the match in the overlapping region between the image and its reference image; this is referred to as the optimization step. You can control the type of transformation that the algorithm can use to reposition the image from its rough location (for example, a translation or a perspective warping), using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_TRANSFORMATION_TYPE`](../../Reference/reg/MregControl.md). For more information, see [Selecting the transformation type](S07_Customizing_your_correlation_stitching_registration_settings.md).
3. Once the optimal transformations that place each image in its reference image's pixel coordinate system are found, each transformation is converted so that it maps the image into the global pixel coordinate system instead.

Note that for the image whose reference coordinate system is the global pixel coordinate system, no correlation-stitching registration is performed and the optimal transformation is the same as the one set with [`MregSetLocation`](../../Reference/reg/MregSetLocation.md).

Correlation-stitching registration uses normalized grayscale correlation to optimize the match in the overlapping regions. Subsections within the overlapping regions are chosen and a pixel by pixel normalized grayscale correlation is calculated on each of the subsections. The [`MregCalculate`](../../Reference/reg/MregCalculate.md) function then finds the best possible correlation between the subsections of the overlapping regions and computes the transformations required to obtain this alignment.

By applying the correlation algorithm on subsections within an overlapping region instead of on the whole overlapping region, the algorithm is more resistant to local changes in contrast and intensity within the images. For a better understanding of the normalized grayscale correlation algorithm, see [Pattern matching algorithm (for advanced users)](../C07_Pattern_matching/S10_Pattern_matching_algorithm_for_advanced_users.md).
