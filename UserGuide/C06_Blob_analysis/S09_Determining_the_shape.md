---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Determining_the_shape
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Determining the shape"
---

# Determining the shape

Other useful features during classification are those that give you information about the blob shape. Two blobs can have similar sizes but different shapes because of a different number of holes, curves, or edges.

*[Image: blobshap.png]*

For example, in the illustration above, the blobs have similar sizes, but can be distinguished by the shape of their holes. If you treat the holes as the actual blobs (set non-zero pixels as foreground pixels and zero pixels as background pixels), you can extract the differences in shape of the holes.

Two features, enabled for calculation using [`MblobControl`](../../Reference/blob/MblobControl.md), that can qualify the shape of these holes are:

- Compactness ([`M_COMPACTNESS`](../../Reference/blob/MblobControl.md)).
- Roughness ([`M_ROUGHNESS`](../../Reference/blob/MblobControl.md)).

The compactness is a measure of how close all particles in the blob are from one another. It is derived from the perimeter and area. A circular blob is most compact and is defined to have a theoretical compactness measure of 1.0 (the minimum); more convoluted shapes have larger values. In practice, due to square pixel discretization, the minimum obtainable compactness is slightly above 1.0.

The roughness is a measure of the unevenness or irregularity of a blob's surface. It is a ratio of the perimeter to the convex perimeter of a blob. Smooth convex blobs have a roughness of 1.0, whereas rough blobs have a higher value because their true perimeter is bigger than their convex perimeter.

Although either of these features can be used in the classification process, compactness is faster to calculate since it is derived using only the area and perimeter.

For a program that, among other things, distinguishes between nuts, bolts and washers by analyzing the compactness of their holes, see [Blob analysis example](S16_Blob_analysis_example.md).

In some cases, you can also distinguish between blobs by determining the number of holes that they have ([`M_NUMBER_OF_HOLES`](../../Reference/blob/MblobControl.md)). For example, you could distinguish between bolts and nuts in the _BoltsNutsWashers.mim_ image by counting blob holes. However, this is not a very robust measure since a single noise pixel in a bolt blob would count as a hole.
