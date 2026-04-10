---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_image_processing
section: Labeling
module_tag: im
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-image / Labeling"
---

# Labeling

You can label objects or particles (known as blobs) in an image with [`MimLabel`](../../Reference/im/MimLabel.md). Labeling is useful for several operations:

- Identifying and distinguishing blobs.
- Finding the area of a blob. Once a blob is labeled, you find the area by generating a histogram and noting the number of pixels associated with that label value.
- Counting the number of blobs in the image. The label number assigned to the last blob is also the number of blobs in the image (assuming there are fewer blobs than possible labels).
- Using the result to eliminate some blobs based on their label value (for example, [`MimClip`](../../Reference/im/MimClip.md) or [`MimLutMap`](../../Reference/im/MimLutMap.md)).

The [`MimLabel`](../../Reference/im/MimLabel.md) function numerically identifies each blob in the specified image. Each non-zero pixel within a blob is given the same numerical value, and blobs within an image are given consecutive values.

*[Image: label.png]*

You can specify that the operation is performed using one of two types of connectivity modes:

- [`M_4_CONNECTED`](../../Reference/im/MimLabel.md): If two pixels touch on the vertical or horizontal, they are considered part of the same blob.
- [`M_8_CONNECTED`](../../Reference/im/MimLabel.md): If two pixels touch on the vertical, horizontal, or diagonal, they are considered part of the same blob.

To distinguish between touching blobs, separate the blobs by performing an erosion operation before the labeling operation.
