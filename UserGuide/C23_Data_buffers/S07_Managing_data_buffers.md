---
doctype: UserGuide
part: "2D related information"
chapter: Data_buffers
section: Managing_data_buffers
module_tag: buf
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / data-buffers / Managing data buffers"
---

# Managing data buffers

Aurora Imaging Library provides several data buffer management functions. These allow you to copy data from one buffer to another, clone a buffer, transfer data between an array and a buffer, load data into a buffer, generate ramp data into a buffer, save a buffer to disk, and perform operations on sequences of image buffers.

## Copying data from one buffer to another

You can copy different required portions of one buffer to another as follows:

- Copy an entire image buffer to another buffer, using [`MbufCopy`](../../Reference/buf/MbufCopy.md)or using [`MbufClone`](../../Reference/buf/MbufClone.md)with [`M_COPY_SOURCE_DATA`](../../Reference/buf/MbufClone.md). While [`MbufCopy`](../../Reference/buf/MbufCopy.md)requires you to allocate a destination buffer before calling the function,[`MbufClone`](../../Reference/buf/MbufClone.md)will automatically allocate one similar to the original.
- Copy an image buffer to another buffer at the specified offset, using [`MbufCopyClip`](../../Reference/buf/MbufCopyClip.md). Data that falls outside of the destination buffer will be automatically clipped.
- Copy specific non-sequential areas to another buffer based on a condition buffer, using [`MbufCopyCond`](../../Reference/buf/MbufCopyCond.md). Source buffer data is copied to the destination buffer if corresponding data in the specified condition buffer satisfies the copy condition. Other data in the destination buffer is left unaffected.
- Copy specific non-consecutive bits to another buffer based on a mask, using [`MbufCopyMask`](../../Reference/buf/MbufCopyMask.md). Only destination bits that correspond to non-zero bits in the mask are modified with source bits.
- Copy one or all bands of a buffer to another buffer, using [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md) or [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md).

If the source and destination buffers have different depth and size, Aurora Imaging Library converts data according to the following general rules:

| Case | Result |
| --- | --- |
| Source depth > destination depth | The most significant bits are truncated when the data is copied into the destination. |
| Destination depth > source depth | The source data is zero or sign-extended (depending on the type of the source) when copied into the destination. |
| Destination size > source size | Exceeding areas of the destination buffer are unaffected. |

If the source and destination buffers have the same number of band(s), all band(s) will be copied correspondingly. Otherwise, the following rules apply:

| When copying from | Result |
| --- | --- |
| Multi-band to 1-band | Only the first band is copied (for example, the Y band of a YUV buffer and the R band of an RGB buffer). Note that [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md) and [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md) allow you to specify an explicit band to copy. |
| 1-band to multi-band | All the bands of the destination buffer are filled with the same data. Note that if the source buffer is associated with a LUT buffer and the destination is a 3-band buffer, the source data will be first mapped through the LUT. Note that [`MbufCopyColor`](../../Reference/buf/MbufCopyColor.md) and [`MbufCopyColor2d`](../../Reference/buf/MbufCopyColor2d.md) allow you to specify an explicit band to fill. |

Note that when copying all bands from a multi-band buffer to another multi-band buffer, they must have the same number of bands; otherwise, an error will occur.

Aurora Imaging Library automatically handles data type and data format conversions. When copying from a floating-point buffer to an integer buffer, the values are truncated. When converting an [`M_RGB15`](../../Reference/buf/MbufCreateColor.md) buffer into an [`M_BGR24`](../../Reference/buf/MbufCreateColor.md) buffer, the least-significant bits of each band are set to 0.

> **Note:** Note that when copying from a non-binary buffer to a binary buffer, all non-zero pixels in the source buffer are represented as ones (1) in the binary buffer. When copying a binary buffer to a buffer of a different depth, each single-bit pixel is copied into the least-significant bit of its corresponding destination pixel. The remaining bits of the destination pixel are set to 0; to propagate the bit value to all bits, use [`MimBinarize`](../../Reference/im/MimBinarize.md).

Aurora Imaging Library also automatically handles source and destination buffers with different compression types.

| When copying from | Result |
| --- | --- |
| [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) to uncompressed | The data will be automatically decompressed. |
| Uncompressed to [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) | The data will be automatically compressed. |
| [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) to [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) (with different compression types) | The data will be re-compressed according to the settings in the destination buffer. |

## Cloning a data buffer

You can clone a data buffer using [`MbufClone`](../../Reference/buf/MbufClone.md). This function takes a specified source buffer and allocates a new buffer with similar characteristics (a clone). The degree of similarity between the two buffers is determined by the settings that you pass to [`MbufClone`](../../Reference/buf/MbufClone.md). By default, [`MbufClone`](../../Reference/buf/MbufClone.md) allocates a clone on the same system as the original, and the two buffers have the same size, data type, and attributes. However, when calling [`MbufClone`](../../Reference/buf/MbufClone.md), you can specify, for example, for the clone to have a different size than the source. You can also specify whether or not to copy the source buffer's data to the clone. The buffer's data is not copied by default.

> **Note:** Note that regardless of the specified settings,[`MbufClone`](../../Reference/buf/MbufClone.md) does not clone the source buffer's region of interest.

## Transferring data between an array and a buffer

You can put data from an array into a data buffer, using [`MbufPut`](../../Reference/buf/MbufPut.md), [`MbufPut1d`](../../Reference/buf/MbufPut1d.md), [`MbufPut2d`](../../Reference/buf/MbufPut2d.md), [`MbufPutColor`](../../Reference/buf/MbufPutColor.md), or [`MbufPutColor2d`](../../Reference/buf/MbufPutColor2d.md). [`MbufPut`](../../Reference/buf/MbufPut.md) puts data in the entire buffer, while [`MbufPutColor`](../../Reference/buf/MbufPutColor.md) or [`MbufPutColor2d`](../../Reference/buf/MbufPutColor2d.md) put data into one or all bands of a multi-band buffer. The other two functions allow you to put data in a selected area of a buffer, respectively.

In addition, you can retrieve data from a data buffer and place it into an array, using [`MbufGet`](../../Reference/buf/MbufGet.md), [`MbufGet1d`](../../Reference/buf/MbufGet1d.md), [`MbufGet2d`](../../Reference/buf/MbufGet2d.md), [`MbufGetColor`](../../Reference/buf/MbufGetColor.md), or [`MbufGetColor2d`](../../Reference/buf/MbufGetColor2d.md). [`MbufGet`](../../Reference/buf/MbufGet.md) gets data from the entire buffer, while [`MbufGetColor`](../../Reference/buf/MbufGetColor.md) or [`MbufGetColor2d`](../../Reference/buf/MbufGetColor2d.md) get data from one or all bands of a multi-band buffer. The other two functions, like their "put in buffer" counterparts, allow you to get data from a selected area of a buffer, respectively.

> **Note:** Note that you can also access the contents of an Aurora Imaging Library buffer from an array using [`MbufInquire`](../../Reference/buf/MbufInquire.md). Inquire the Host address of the buffer, and then using a pointer access the buffer as an array. This is discussed in more detail later.

## Loading a data buffer

You can load data into an Aurora Imaging Library data buffer, using one of two methods:

- Load data into an automatically allocated Aurora Imaging Library data buffer, using [`MbufImport`](../../Reference/buf/MbufImport.md) with [`M_RESTORE`](../../Reference/buf/MbufImport.md), or using [`MbufRestore`](../../Reference/buf/MbufRestore.md).
- Load data into a previously allocated Aurora Imaging Library data buffer, using [`MbufImport`](../../Reference/buf/MbufImport.md) with [`M_LOAD`](../../Reference/buf/MbufImport.md) or using [`MbufLoad`](../../Reference/buf/MbufLoad.md).

These functions internally handle the opening and closing of the file. With [`MbufImport`](../../Reference/buf/MbufImport.md), you can specify the file's format. [`MbufLoad`](../../Reference/buf/MbufLoad.md) and [`MbufRestore`](../../Reference/buf/MbufRestore.md) will read the data in the file to determine the format; therefore, they might take more time to return a result.

## Generating data into a data buffer

2D ramp data is data that linearly increases or decreases in both X and Y. You can generate 2D ramp data into a buffer, using [`MgenRamp`](../../Reference/gen/MgenRamp.md). Generating 2D ramp data into a buffer can be useful, for example, to simulate non-uniform lighting in an image; non-uniform lighting is generally approximated by an intensity ramp that is added to the image. You can also generate 2D ramp data into a buffer, for example, to retrieve the index of any pixel in the buffer. To do so, call [`MgenRamp`](../../Reference/gen/MgenRamp.md) and set [`ScaleX`](../../Reference/gen/MgenRamp.md) to 1, [`ScaleY`](../../Reference/gen/MgenRamp.md) to [`M_PITCH`](../../Reference/buf/MbufInquire.md), and [`Offset`](../../Reference/gen/MgenRamp.md) to 0.

[`MgenRamp`](../../Reference/gen/MgenRamp.md) generates the ramp data using the following formula: [`BufId`](../../Reference/gen/MgenRamp.md)[x,y] = [`ScaleX`](../../Reference/gen/MgenRamp.md) * x + [`ScaleY`](../../Reference/gen/MgenRamp.md) * y + [`Offset`](../../Reference/gen/MgenRamp.md).

You can specify to generate data into any Aurora Imaging Library buffer, such as an Aurora Imaging Library array, image, kernel, LUT, or structuring element buffer. The following code snippets provide two examples of valid calls to [`MgenRamp`](../../Reference/gen/MgenRamp.md) to generate 2D ramp data into a buffer.

> **Code example:** [userguide.data_buffers.generating_data_into_a_data_buffer01](userguide.data_buffers.generating_data_into_a_data_buffer01)

> **Code example:** [userguide.data_buffers.generating_data_into_a_data_buffer02](userguide.data_buffers.generating_data_into_a_data_buffer02)

The following images depict the 2D ramp data generated by the previous snippets, and their resulting buffers.

*[Image: buf_ramp_examples.png]*

## Saving a data buffer

You can save a data buffer to disk, using [`MbufExport`](../../Reference/buf/MbufExport.md) or [`MbufSave`](../../Reference/buf/MbufSave.md). [`MbufExport`](../../Reference/buf/MbufExport.md) is the most general of these functions and can save data in any Aurora Imaging Library-supported file format. [`MbufSave`](../../Reference/buf/MbufSave.md) can only save a buffer in an [`M_AIL_TIFF`](../../Reference/buf/MbufExport.md) file format.

These functions internally handle opening and closing the file. If the given file name already exists, the file will be overwritten.

> **Note:** You can use the [`M_AIL_NATIVE`](../../Reference/buf/MbufExport.md)format to store any data buffer, including its settings, ROI, and calibration information. You can use the[`M_AIL_TIFF`](../../Reference/buf/MbufExport.md)format to save an image buffer with its ROI and calibration information in a file that can be viewed by any software that supports the TIFF 6.0 specification.

## Performing operations on a sequence of image buffers

You can perform operations on a sequence of image buffers. You can import a sequence of images from a file using [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md). You can export a sequence of image buffers to a file using [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md). You can also use the Aurora Imaging Library Sequence module to perform operations such as H.264 compression and decompression on sequences. For more information about using the Sequence module, see [Sequence overview](../C31_Sequence/S01_Sequence_overview.md).
