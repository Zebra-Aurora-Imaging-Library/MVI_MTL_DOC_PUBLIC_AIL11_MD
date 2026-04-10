---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Dimensions
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Dimensions"
---

# Dimensions

Besides the area and perimeter, you might need to determine the dimensions of the blobs. Since blobs are not typically rectangular in shape, you will probably have to take the length (or diameter) of the blobs at various angles from the horizontal axis. This is actually one of the many definitions of the blob length, called the Feret diameter. Several Feret diameters are illustrated below. Note, the angle at which the Feret diameter is taken (relative to the horizontal axis) is specified as a subscript to the F.

*[Image: feretdia.png]*

With Aurora Imaging Library, you can calculate the Feret diameter at a specified angle by enabling it for calculation, using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_FERET_GENERAL`](../../Reference/blob/MblobControl.md) and then using [`M_FERET_GENERAL_ANGLE`](../../Reference/blob/MblobControl.md) to specify the angle; use [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_GENERAL`](../../Reference/blob/MblobGetResult.md) to retrieve the result after calculation ([`MblobCalculate`](../../Reference/blob/MblobCalculate.md)). To calculate the Feret diameter at 0° (horizontal Feret diameter) and at 90° (vertical Feret diameter), you could alternatively enable the [`M_BOX`](../../Reference/blob/MblobControl.md) group of features for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md); after calculation, you can retrieve, among other features, [`M_FERET_X`](../../Reference/blob/MblobGetResult.md) and [`M_FERET_Y`](../../Reference/blob/MblobGetResult.md), respectively, using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md).

To determine the minimum, maximum, and average Feret diameters of the blob, enable the [`M_FERETS`](../../Reference/blob/MblobControl.md) group of features for calculation, using [`MblobControl`](../../Reference/blob/MblobControl.md); then, after calculation, you can retrieve the results using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_MIN_DIAMETER`](../../Reference/blob/MblobGetResult.md), [`M_FERET_MAX_DIAMETER`](../../Reference/blob/MblobGetResult.md), and [`M_FERET_MEAN_DIAMETER`](../../Reference/blob/MblobGetResult.md), respectively. These diameters will be determined by testing the diameter of the blobs at several angles. Alternatively, you can set the range over which to calculate the Feret diameters (or other Feret-based features) using [`MblobControl`](../../Reference/blob/MblobControl.md) with[`M_FERET_ANGLE_SEARCH_START`](../../Reference/blob/MblobControl.md) and [`M_FERET_ANGLE_SEARCH_END`](../../Reference/blob/MblobControl.md).

You can use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md) to change the default number of angles; these angles will start at 0° and increase in increments of `180°/_n_`, where _n_ is the number of Feret diameters. Increasing the number of angles that are tested increases the accuracy of the results, but also increases processing time.

> **Note:** Note that, since memory usage increases with the number of Feret diameters, using a large number of Feret diameters in an image with many blobs might result in memory allocation errors.

When the Feret is computed at 0° or 90°, the Feret diameter is equal to the exact length of the blob in that direction. However, when the Feret is computed at another angle, the Feret diameter is always smaller than the width of the blob in that direction. This is because pixels are considered circular. The approximation is at its worse at 45°. The relative difference between the Feret approximation and the exact length decreases as the blobs get bigger. The difference between the approximation and the exact length is shown in the image below.

*[Image: blobferetexample.png]*

The maximum Feret diameter is not very sensitive to the number of angles; using 8 angles usually produces an accurate result. The minimum diameter, however, can be inaccurate for long thin blobs unless many angles are used.

The angles at which the minimum and maximum Feret diameters were found can be retrieved using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md) with [`M_FERET_MIN_ANGLE`](../../Reference/blob/MblobGetResult.md) and [`M_FERET_MAX_ANGLE`](../../Reference/blob/MblobGetResult.md), respectively.

In addition, you can retrieve the ratio of the maximum to minimum Feret diameter using the above function with [`M_FERET_ELONGATION`](../../Reference/blob/MblobGetResult.md).

Although the Feret diameters provide a good approximation of the blob size, these features are not very good for long, thin blobs. For these, the following features, which can be enabled for calculation with [`MblobControl`](../../Reference/blob/MblobControl.md), might provide better results:

- [`M_LENGTH`](../../Reference/blob/MblobControl.md): an extraction of the true length of a blob.
- [`M_BREADTH`](../../Reference/blob/MblobControl.md): an extraction of the true breadth of a blob.
- [`M_ELONGATION`](../../Reference/blob/MblobControl.md): the ratio of the length to the breadth.

These features are derived from the area and perimeter, using the assumption that the blob area is equal to the `[length x breadth]` and the perimeter is equal to `[2(length + breadth)]`. These relations only hold if the length and breadth are constant throughout a blob. However, long, thin blobs generally satisfy this assumption, even if they are not straight.

Note, since these features use only the area and perimeter, they are faster to calculate than Feret features.
