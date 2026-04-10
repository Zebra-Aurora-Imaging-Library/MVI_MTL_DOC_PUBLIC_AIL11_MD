---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Merging_results
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Merging results"
---

# Merging results

In some cases, you might need to merge results from previous blob analysis calculations. For example, this might be required in an application where a line-scan camera captures a series of images of a continuous stream of rice falling off a conveyor belt as shown below. When grabbing images of the continuous stream, some objects might be split between two images. Performing blob analysis operations on these images produces erroneous results for these objects since an object split between two images is considered as two separate blobs. Results for one portion of the object will be in one result buffer and results for the other portion will be in another result buffer. To obtain valid results for these objects, you would need to merge the results of the objects that are split between two result buffers.

*[Image: rice.png]*

You can use [`MdigProcess`](../../Reference/dig/MdigProcess.md) to grab sequential frames of the continuous stream into a list of image buffers (or child buffers of a large parent buffer), perform the required blob analysis operation on the frames as they are being grabbed, and store the results in different blob analysis result buffers. You can then merge the results from the required result buffers using [`MblobMerge`](../../Reference/blob/MblobMerge.md).

[`MblobMerge`](../../Reference/blob/MblobMerge.md) merges two source result buffers at a time as if they were taken from two vertically adjacent child buffers of one image. By merging the result buffers as such, the destination result buffer uses the coordinate system of the first result buffer and positional results from the second result buffer are translated in the Y-direction to take into account this coordinate system change. In addition, border blobs that would touch if they were in one image are grouped into one grouped blob, assigned a single label, and recalculated as if the grouped blob were one blob. The merged results can be copied or moved (depending on the [`ControlFlag`](../../Reference/blob/MblobMerge.md) parameter of [`MblobMerge`](../../Reference/blob/MblobMerge.md)) into a destination result buffer.

Call [`MblobMerge`](../../Reference/blob/MblobMerge.md) function as many times as necessary to merge the results of all the required result buffers to obtain an ultimate blob analysis result buffer for your continuous stream.

## A simple merge

A simple merge operation is illustrated below. In this example, the area, perimeter, and center of gravity of blobs in two images have been calculated. After merging the results, the area and perimeter remain the same, but the center of gravity is recalculated with respect to the coordinate system of the destination result buffer. Also note that the labels of the blobs are changed after the merge to properly differentiate the blobs in the merged result buffer.

The following animation demonstrates the new coordinates and center of gravity for each blob when images are merged.

*[Image: BlobMergingResults_SimpleOnly]*

## Border blobs

Most probably, you will have border blobs in your images that would touch if they were in one image. When merging results, these blobs are grouped into one grouped blob, assigned a single label, and recalculated as if the grouped blob were one blob. For example, in the illustration below, border blob 2 in image A and border blob 1 in image B have separate results before the merge. After the merge, however, these border blobs are considered as one blob with its own results in the new coordinate system.

*[Image: Merge6.png]*

In the event that different blob analysis calculations are performed on the two images, only features that were calculated for all the individual blobs in the grouped blob are recalculated, and these are recalculated using the results of the individual blobs in the group. This case is illustrated below. The area and center of gravity are computed for image A, but the perimeter and center of gravity are computed for image B. Upon merging the results, only the center of gravity is computed for the grouped blob (blob 2).

Note that trying to retrieve a result in the destination result buffer (using [`MblobGetResult`](../../Reference/blob/MblobGetResult.md)) which is not available generates an error.

*[Image: Merge8.png]*

## Inclusion state

In most cases, the inclusion state of the blobs remains the same after the merge. Consider the example below. The included blobs remain included after the merge, and the excluded ones remain excluded after the merge.

*[Image: Merge4.png]*

If an included border blob is grouped with an excluded blob, the inclusion state of the merged blob is undetermined, as illustrated below. For this reason, it is not recommended that you use the merge operation under these circumstances.

*[Image: Merge5.png]*

Note that after merging the results, you can perform a [`MblobSelect`](../../Reference/blob/MblobSelect.md) operation to further include or exclude blobs in your destination result buffer, but if you perform an [`MblobCalculate`](../../Reference/blob/MblobCalculate.md) operation after the merge, all the results in the destination result buffer will be discarded.

## Other remarks

For the merge operation to work properly, the results in the source result buffers must satisfy some constraints. The results in the source result buffers must have been calculated by a context that:

- Has the same run information saving mode ([`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) set to [`M_ENABLE`](../../Reference/blob/MblobControl.md) or [`M_DISABLE`](../../Reference/blob/MblobControl.md)).
- Has the same pixel aspect ratio ([`M_PIXEL_ASPECT_RATIO`](../../Reference/blob/MblobControl.md)).
- Has the same lattice ([`M_CONNECTIVITY`](../../Reference/blob/MblobControl.md)).
- Has the same blob identification mode ([`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md)). If the blob result buffers have [`M_BLOB_IDENTIFICATION_MODE`](../../Reference/blob/MblobControl.md) set to [`M_LABELED`](../../Reference/blob/MblobControl.md), they cannot be merged.
- Has the same number of Ferets ([`M_NUMBER_OF_FERETS`](../../Reference/blob/MblobControl.md)).

Note that if you enable the calculation of a specified moment in the context (using [`MblobControl`](../../Reference/blob/MblobControl.md) with [`M_MOMENT_GENERAL`](../../Reference/blob/MblobControl.md)) and [`M_SAVE_RUNS`](../../Reference/blob/MblobControl.md) is disabled, [`M_MERGE`](../../Reference/blob/MblobSelect.md) will not be able to perform the moment calculation.

Furthermore, if you specify a sorting key for result retrieval, the sorting order will not be respected after the merge.
