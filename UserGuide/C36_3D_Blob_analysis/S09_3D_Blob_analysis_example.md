---
doctype: UserGuide
part: "3D processing and analysis"
chapter: 3D_Blob_analysis
section: 3D_Blob_analysis_example
module_tag: 3dblob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 3D processing and analysis / 3D_Blob_analysis / 3D Blob analysis example"
---

# 3D blob analysis example

The examples below illustrate applications of the 3D Blob Analysis module. To run these and other examples, use Aurora Imaging Example Launcher.

## Segmenting a point cloud and identifying blobs based on blob features

The _m3dblob.cpp_ example segments a point cloud into several blobs, and further identifies blobs using their calculated linearity and planarity features.

*[Image: 3dblob_SegmentationExample.png]*

## Continuous segmentation

The _continuous3dsegmentation.cpp_ example performs continuous segmentation to count rocks on a conveyor.

*[Image: 3dblob_ContinuousSegmentationExample.png]*

## Segmentation bin picking

The _segmentationbinpicking.cpp_ example performs segmentation to identify and pick up objects in a bin.

*[Image: 3dblob_SegmentationBinPickingExample.png]*
