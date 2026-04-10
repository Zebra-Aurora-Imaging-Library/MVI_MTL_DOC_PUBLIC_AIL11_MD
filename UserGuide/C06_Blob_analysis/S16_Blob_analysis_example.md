---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Blob_analysis
section: Blob_analysis_example
module_tag: blob
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / blob / Blob analysis example"
---

# Blob analysis example

The following illustrates the source image before and after binarization and noise removal operations:

*[Image: boltbina.png]*

The example below binarizes an image, determines the center of gravity for each blob, and counts the number of bolts, nuts and washers. Note, the binarizing step produces a considerable number of spurious blobs and holes, so some processing is performed to clean up the blob identifier image before doing any calculations.

> **Code example:** [MBlob.cpp](MBlob.cpp)

To run this example, use the Aurora Imaging Example Launcher in the Aurora Imaging Control Center.
