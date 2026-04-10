---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: Selecting_combining_and_sorting_blobs
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / Selecting combining and sorting blobs"
---

# Selecting, combining, and sorting blobs

Once you have calculated all required blob results, you can select, combine, and sort the results to better suit your needs, such as preparing a subset of blobs for which to make further calculations.

## Selecting blobs

Using [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md), you can select which blobs meet a criterion and copy the corresponding blob identifying information and feature results into a new result buffer, so that only the selected blobs are recognized for future operations. Note that you can use in-place processing, and specify the same result buffer for the source and the destination. Non-selected data is deleted from the result buffer when selecting in-place. Also note that the points that make up all blobs (whether selected or not) are still held in the corresponding point cloud container. To populate a container only with blobs of interest, use [`M3dblobExtract`](../../Reference/3dblob/M3dblobExtract.md).

### Choosing a selection criterion

You can choose any calculated feature result upon which to base a selection, provided the result is not an array. Note that you can base the selection on whether or not the specified feature result is available in the source result buffer.

Once you have specified a selection criterion, you must then set a condition that the criterion must satisfy. For example, if your selection is based on the longest Feret of a blob ([`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobSelect.md)), you can set a condition that limits the Feret's acceptable length to within a specified range ([`M_IN_RANGE`](../../Reference/3dblob/M3dblobSelect.md)), and then set the necessary lower and upper limits for the range. In general, conditions use an inclusive range, an exclusive range, or specify that the selection criterion's value must be equal to, not equal to, above, or below a specified limit. Alternatively, you can specify to select for feature results that are available or not available.

## Combining blobs

The [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) function allows you to perform compound selections from results that you have selected and saved to separate result buffers. For example, to select blobs that meet criterion 1 OR criterion 2, first call [`M3dblobSelect`](../../Reference/3dblob/M3dblobSelect.md) twice with two different destination 3D blob analysis result buffers: once to select for criterion 1, and then a second time to select for criterion 2 (the source buffer is the same for each call). Then, combine the resulting two sets of blob identifying information and feature results using [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) with [`M_OR`](../../Reference/3dblob/M3dblobCombine.md). Note that different feature results for the same blob in two source result buffers are combined and assigned to that same blob in the destination result buffer. For example, if source 1 has blob 42's maximum Feret result and source 2 has blob 42's centroid result, then the destination result buffer will have both maximum Feret and centroid results for blob 42 (provided the specified combine operation doesn't exclude blob 42). An error is reported if both sources contain results for the same feature, but the feature's values are different.

The following illustration shows separate results for the same blob combined into a single result buffer using [`M3dblobCombine`](../../Reference/3dblob/M3dblobCombine.md) with [`M_OR`](../../Reference/3dblob/M3dblobCombine.md).

*[Image: 3dblob_Combine.png]*

> **Note:** If you just want to treat two blobs as one blob, combining different result buffers is not required. To merge two blobs into one blob (when the blobs are identified in a single result buffer) use [`M3dblobControl`](../../Reference/3dblob/M3dblobControl.md) with [`M_MERGE`](../../Reference/3dblob/M3dblobControl.md).

## Sorting blobs

Using [`M3dblobSort`](../../Reference/3dblob/M3dblobSort.md), you can sort the indices assigned to blobs based on their blob features. You can specify up to three features with which to sort. If the blob comparison is indeterminate using the first feature, the function tries the comparison using the second and, if necessary, the third feature. For each specified feature, you can set the sorting order, either ascending (default) or descending. To do so, add the respective combination value [`M_UP`](../../Reference/3dblob/M3dblobSort.md) or [`M_DOWN`](../../Reference/3dblob/M3dblobSort.md) when specifying the feature.

For example, to sort primarily for blobs that are linear (long and straight in form), you can specify the sorting features as follows:[`M_LINEARITY`](../../Reference/3dblob/M3dblobSort.md) +[`M_DOWN`](../../Reference/3dblob/M3dblobSort.md), [`M_FERET_MAX_DIAMETER`](../../Reference/3dblob/M3dblobSort.md) + [`M_DOWN`](../../Reference/3dblob/M3dblobSort.md), and then [`M_FERET_MIN_DIAMETER`](../../Reference/3dblob/M3dblobSort.md) + [`M_UP`](../../Reference/3dblob/M3dblobSort.md). This approach uses a blob's linearity as the first sorting feature and its longest Feret length as the second sorting feature. If neither of the first two features provides a difference, the third sorting feature (shortest Feret) can break the tie. Note the sort order ([`M_UP`](../../Reference/3dblob/M3dblobSort.md) or [`M_DOWN`](../../Reference/3dblob/M3dblobSort.md)) in the example. For [`M_LINEARITY`](../../Reference/3dblob/M3dblobSort.md), adding the [`M_DOWN`](../../Reference/3dblob/M3dblobSort.md) combination value specifies to put the blob index of the most linear blob at the front of the list. This agrees with how Aurora Imaging Library computes the linearity, since the most linear blob receives the largest calculated feature value, and [`M_DOWN`](../../Reference/3dblob/M3dblobSort.md) specifies a descending sort order.
