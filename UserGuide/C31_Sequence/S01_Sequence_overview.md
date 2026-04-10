---
doctype: UserGuide
part: "2D related information"
chapter: Sequence
section: Sequence_overview
module_tag: seq
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / Sequence / Sequence overview"
---

# Aurora Imaging Library Sequence module overview

The Aurora Imaging Library Sequence module allows you to perform operations on sequences of images. Depending on your settings, the Sequence module will perform one of two operations: H.264 compression or decompression. H.264 compression uses predictive encoding to compress sequences of images. You can feed the images to process all at once or individually. The H.264 compression that the Sequence module performs is highly customizable and you can tweak it for your application. For example, you can decide whether to put more emphasis on the quality of the elementary video stream or its resulting size. The module can also decompress an H.264 compressed stream and save the result in one or many files or buffer lists.

If you want to save your sequence such that the elementary video stream does not use predictive encoding, you can use [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md) to save images in MJPEG, Aurora Imaging Library, and non-compressed DIB format. You can use [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md)to import a sequence saved in one of these three formats into separate image buffers.
