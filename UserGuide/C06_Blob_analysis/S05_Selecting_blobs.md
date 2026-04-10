---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Selecting_blobs
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Selecting blobs"
---

# Selecting blobs

Once all blobs are clearly identifiable by the Blob Analysis module, you are ready to perform calculations. However, in some cases, you will not want to make time-consuming feature extractions for every single blob in the blob identifier image. For example, you probably do not want to calculate features for blobs that are touching the edges of the image or that are noise artifacts. You might choose to preprocess these blobs out of your image, but you might lose a significant amount of information by doing so. In some other cases, some blobs might have been incorrectly segmented as two or more blobs. You would want to group those blobs together and calculate their features as one blob. You might also want to do a blob calculation on the whole image but excluding the noise particles.

The [`MblobSelect`](../../Reference/blob/MblobSelect.md) function deals with these cases. This function allows you to select (on the basis of calculations already made) a subset of blobs for which to make further calculations, and to group blobs with certain characteristics and consider them as one blob.

When selecting a subset of blobs, your approach for selecting depends on whether you have few or many unwanted blobs.

- If you do not have too many unwanted blobs, it is usually faster to calculate all required features for all blobs. After doing this, use [`MblobSelect`](../../Reference/blob/MblobSelect.md) to exclude or delete results obtained for blobs that do not meet your criteria.
- If you have many unwanted blobs, you will probably save time and memory by first calculating, for all blobs, only those features that allow you to distinguish between relevant and unwanted blobs. Then, exclude from future calculations (or delete altogether from the blob analysis result buffer) blobs that do not meet your criteria, using [`MblobSelect`](../../Reference/blob/MblobSelect.md). Finally, calculate all required features for remaining blobs using [`MblobCalculate`](../../Reference/blob/MblobCalculate.md).

If you are not able to exclude or delete a sufficient number of blobs using the second method, use the first.

If the blobs were taken from a calibrated image, you can set the limits of your selection criteria in pixel units or world units. To set the units, use [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_INPUT_SELECT_UNITS`](../../Reference/blob/MblobControl.md).

To group blobs that meet your criteria as one blob, use [`MblobSelect`](../../Reference/blob/MblobSelect.md) with [`M_MERGE`](../../Reference/blob/MblobSelect.md). Once grouped, the blobs are treated as one blob (a grouped blob) with a unique blob label. For example, if you want to calculate the area of the grouped blob, it will be the sum of the areas of the individual blobs within the group. Upon merging, only features that were calculated for all the individual blobs in the group are re-calculated for the new grouped blob; calculations are made using the results of the individual blobs in the group.

In any case, you can make as many calls as necessary to [`MblobSelect`](../../Reference/blob/MblobSelect.md) and [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) to arrive at the set of blobs with the right characteristics. However, you must always give the same identifier and grayscale buffers to [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) during this procedure. If you give different buffers or change the existing buffers in any way (for example, if you use [`MblobDraw`](../../Reference/blob/MblobDraw.md) to erase blobs from the identifier image), all current results in the result buffer will be discarded the next time you call [`MblobCalculate`](../../Reference/blob/MblobCalculate.md). In addition, all enabled features will be re-calculated for all blobs in the new identifier image. This means that you will have to restart the selection procedure. If you intend to calculate grayscale features during your analysis, you must include the grayscale image before starting your calculations.
