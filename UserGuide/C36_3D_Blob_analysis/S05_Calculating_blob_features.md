---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Calculating_blob_features
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Calculating blob features"
---

# Calculating blob features

Once you have defined a blob segmentation, you can calculate a variety of different blob measurements or features, such as the bounding box, centroid, linearity, and planarity of each blob. You can also calculate moments and principal component analysis (PCA) results, as well as the distance to the nearest neighboring blob. Although the [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) function initiates the actual calculations, it is the specified context that determines which calculations are performed.

To calculate features of blobs, allocate a calculate 3D blob analysis context (using [`M3dblobAlloc`](../../Reference/3dblob/M3dblobAlloc.md) with [`M_CALCULATE_CONTEXT`](../../Reference/3dblob/M3dblobAlloc.md)). Aurora Imaging Library stores results in the same result buffer that holds the blob segmentation. [`M3dblobAllocResult`](../../Reference/3dblob/M3dblobAllocResult.md) with [`M_SEGMENTATION_RESULT`](../../Reference/3dblob/M3dblobAllocResult.md)).

## Enabling features for calculation

To compute blob features, call [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md). Before doing so, you must explicitly enable the features to calculate, using successive calls to [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md). Note that you can specify to enable or disable the calculation of all features at once, using [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_ALL_FEATURES`](../../Reference/3dblob/M3dblobControl.md). However, it is not recommended to use [`M_ALL_FEATURES`](../../Reference/3dblob/M3dblobControl.md) in your final application. This is because programs using [`M_ALL_FEATURES`](../../Reference/3dblob/M3dblobControl.md) set to [`M_ENABLE`](../../Reference/3dblob/M3dblobControl.md) might become slower when new Aurora Imaging Library versions are released, if new features have been added.

When you call [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md), the function calculates the requested blob features. Even if only a few features are enabled for calculation, the overhead of performing calculations for some specified features can be considerable. Typically, it is more efficient to enable many features for calculation and make one call to [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md), unless you are computing time-consuming features. For such features, select as few blobs as possible before calculation.

In terms of cost to time, feature calculations can be grouped into three categories:

- Cheap: bounding box, centroid, PCA box, Feret general (and its contact points), as well as moments with an order less than or equal to 4, which includes linearity, planarity, and principal components.
- Medium: semi-oriented box and moments with an order greater than 4.
- Expensive: minimum/maximum Ferets (and their contact points), and nearest blob.

Note, features that have already been calculated for the specified blobs are not recalculated if you call [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) again, unless you have modified the container's range component. Note that if the range component has changed, [`M3dblobCalculate`](../../Reference/3dblob/M3dblobCalculate.md) removes all feature results from the result buffer and then calculates the features enabled in the context.

Before enabling a feature for calculation, you should take the blob shapes into consideration. Some feature calculations are more appropriate for certain blob shapes than for others. For example, [`M_LINEARITY`](../../Reference/3dblob/M3dblobControl.md) should be used for elongated blobs instead of flattened ones, in which case [`M_PLANARITY`](../../Reference/3dblob/M3dblobControl.md) is more suitable. When trying to distinguish between two similar blobs, enabling certain features for calculation, rather than some other features that might also seem appropriate, might reveal a more notable difference. If two features allow you to come to the same conclusion, it is recommended that you enable the feature that is calculated more quickly. For example, minimum/maximum Feret diameters typically take longer to compute.
