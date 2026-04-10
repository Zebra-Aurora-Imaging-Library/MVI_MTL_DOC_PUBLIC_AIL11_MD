---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: 3D_Blob_analysis_overview
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / 3D Blob analysis overview"
---

# Aurora Imaging Library 3D Blob analysis module

The 3D Blob Analysis module allows you to segment a point cloud into blobs, calculate blob features, and select and sort for blobs of interest.

*[Image: 3dblob_SegmentedWithBoxes.png]*

In 3D, blobs are, typically, subsets of neighboring points. Aurora Imaging Library performs the segmentation based on distances between points in the point cloud or on other metrics such as the color distance between neighboring point data or the similarity of normal vector angles in a point neighborhood.

Instead of subsets of neighboring points, you can use a label image to define the blob segmentation. Each pixel in a label image is set to a value that will identify corresponding points in a point cloud as belonging to a specific blob, using a label that is unique for each blob.

Once you have defined a blob segmentation, you can use the 3D Blob Analysis module to calculate features of blobs, and select and sort for blobs of interest. For example, you can select blobs that meet a specified Feret size, and then sort the indices of these blobs so that they are ordered in the result buffer according to increasing or decreasing Feret size.

You can calculate various blob features such as the bounding box of the blob, its planarity, linearity, and distance to the nearest neighboring blob. You can also calculate moments and principal component analysis (PCA) results.

You can select which blobs meet a criterion and copy the corresponding blob identifying information and feature results into a new result buffer, so that only the selected blobs are recognized for future operations. You can also combine the blob identifying information and feature results from different result buffers, to perform compound selections (criterion 1 AND/OR criterion 2).

You can retrieve results for individual blobs or all blobs. You can also copy blob feature results that correspond to geometries into a geometry object. In addition, you can copy results into an image buffer to create a label image, index image, or mask image.
