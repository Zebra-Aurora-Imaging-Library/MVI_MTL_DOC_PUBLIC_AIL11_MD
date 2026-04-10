---
doctype: UserGuide
part: "2D related information"
chapter: JPEG_and_JPEG2000_compression
section: Working_with_tables
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / jpeg / Working with tables"
---

# Working with tables

In some applications, the default quantization or Huffman tables might not be suitable. Aurora Imaging Library allows you to create your own. You can inquire the default table values to help you determine appropriate values. You might have to select values by trial and error to determine the best ones for your application.

> **Note:** For JPEG2000 compression, quantization multiplies coefficients, while in JPEG compression, quantization divides values.

Whether you are inquiring the default tables or customizing your own, you must allocate arrays that are large enough to contain the data. The table below lists the tables that you can manipulate and their required array size for each compression algorithm.

| Compression algorithm | Table type | Buffer type, size, and attribute |
| --- | --- | --- |
| JPEG lossless | DC Huffman | 1-dimensional, 8+[`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md), 28 entries, [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) |
| JPEG lossy | DC Huffman | 1-dimensional, 8+[`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md), 28 entries, [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) |
| AC Huffman | 1-dimensional, 8+[`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md), 178 entries, [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) |
| Quantization | 2-dimensional, 8+[`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md), 8x8 entries, [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) |
| JPEG2000 lossy | Quantization | 1-dimensional, 32+[`M_FLOAT`](../../Reference/buf/MbufAlloc2d.md), 3_N_ + 1 entries, where _N_ is the number of decomposition levels, [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) |

> **Note:** Note that during development and at runtime, compression support is reliant upon the presence of a compression/decompression package license. This license is only included by default with the development dongle for the full version of Aurora Imaging Library. In other cases, this license must be purchased separately.

## Inquiring values in default tables

Inquiring the default values of a table is useful to determine values for your custom tables. The steps below outline this procedure.

1. First, inquire the Aurora Imaging Library identifier of the default table using [`MbufInquire`](../../Reference/buf/MbufInquire.md). Then, inquire the size of the table using the same function.
2. Allocate a user array of the appropriate size for storing the default table values.
3. Get the values from the inquired table in Step 1 and store them in the user array using [`MbufGet...`](../../Reference/buf/MbufGet1d.md).

## Using your own table

To use your own table:

1. Allocate a buffer with an [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) attribute and of the same data type as the default table.
2. Transfer the custom table values from the user array to the array buffer, using [`MbufPut...`](../../Reference/buf/MbufPut1d.md), depending on the type of table.
3. Associate the [`M_ARRAY`](../../Reference/buf/MbufAlloc2d.md) buffer to the required [`M_COMPRESS`](../../Reference/buf/MbufAlloc2d.md) image buffer, using the [`MbufControl`](../../Reference/buf/MbufControl.md) control types specific to your table (for example, use the [`M_HUFFMAN_AC`](../../Reference/buf/MbufControl.md) control type for an AC Huffman table). Specifying these control types as-is, or combined with [`M_ALL_BANDS`](../../Reference/buf/MbufControl.md), controls all bands for JPEG2000. This is the default setting.
   For a JPEG2000 compression, you can associate a different table with each band of a multi-band buffer. To do so, add [`M_RED`](../../Reference/buf/MbufControl.md), [`M_BLUE`](../../Reference/buf/MbufControl.md), or [`M_GREEN`](../../Reference/buf/MbufControl.md) to your control type for an RGB buffer, whereas for a YUV buffer, add [`M_Y`](../../Reference/buf/MbufControl.md), [`M_U`](../../Reference/buf/MbufControl.md), or [`M_V`](../../Reference/buf/MbufControl.md).
   For JPEG lossy compressions of YUV buffers, use the luminance and chrominance control types. The control types without these suffixes control all bands.

> **Note:** If you set the [`M_Q_FACTOR`](../../Reference/buf/MbufControl.md) control type after specifying a custom table, the custom table will be scaled.
