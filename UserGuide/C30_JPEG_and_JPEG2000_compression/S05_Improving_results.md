---
doctype: UserGuide
part: "2D related information"
chapter: JPEG_and_JPEG2000_compression
section: Improving_results
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / jpeg / Improving results"
---

# Improving results

If the defaults do not meet your application requirements, you can try to improve your compression ratio using the following techniques. We recommend trying these techniques in the order they appear.

> **Note:** Note that during development and at runtime, compression support is reliant upon the presence of a compression/decompression package license. This license is only included by default with the development dongle for the full version of Aurora Imaging Library. In other cases, this license must be purchased separately.

> **Note:** Regardless of the type of your compression operation, you should first remove extraneous noise from the image (if possible) using Aurora Imaging Library processing functions.

For JPEG lossy compression:

- Allocate a YUV buffer for compression.
- Increase the quantization factor using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_Q_FACTOR`](../../Reference/buf/MbufControl.md).
- Decrease the restart interval.
- Change the quantization table (see [Working with tables](S06_Working_with_tables.md)).
- Change the Huffman table (see [Working with tables](S06_Working_with_tables.md)).

For JPEG lossless compression:

- Try the other supported predictors using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_PREDICTOR`](../../Reference/buf/MbufControl.md).
- Decrease the restart interval.

For JPEG2000 lossy compression:

- Allocate a YUV buffer for compression.
- Increase the quantization factor using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_Q_FACTOR`](../../Reference/buf/MbufControl.md).
- Decrease the target size using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_TARGET_SIZE`](../../Reference/buf/MbufControl.md).
- Change the number of decomposition levels of the DWT using with [`M_DECOMPOSITION_LEVEL`](../../Reference/buf/MbufControl.md).
- Change the quantization table (see [Working with tables](S06_Working_with_tables.md)).

For JPEG2000 lossless compression:

- Change the number of decomposition levels of the DWT using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DECOMPOSITION_LEVEL`](../../Reference/buf/MbufControl.md).
