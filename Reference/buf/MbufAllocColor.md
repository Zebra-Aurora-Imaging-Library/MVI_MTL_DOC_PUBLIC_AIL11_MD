---
doctype: Reference
module: buf
function: MbufAllocColor
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufAllocColor"
---

# MbufAllocColor

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Allocate a multi-band data buffer.

## Syntax

```c
AIL_ID MbufAllocColor(
    AIL_ID    SystemId,   //in
    AIL_INT   SizeBand,   //in
    AIL_INT   SizeX,      //in
    AIL_INT   SizeY,      //in
    AIL_INT   Type,       //in
    AIL_INT64 Attribute,  //in
    AIL_ID *  BufIdPtr    //out
)
```

## Description

This function allocates a data buffer with multiple bands on the specified system. This type of buffer allows for the representation of color images (for example, RGB).

Note that you can also allocate non-color multi-band image buffers. Image buffers allocated with 2 bands or more than 3 bands are always non-color.

This function allocates buffers that have a two-dimensional surface for each specified band. You can use [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) and [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) to create single band one- or two-dimensional data buffers, respectively.

For more information about color buffer formats, refer to [RGB buffers](../../UserGuide/C23_Data_buffers/S10_RGB_buffers.md) and [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md).

After allocating the data buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the data buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufAllocColor.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocColor.md) was specified).

When the data buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocColor.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufAllocColor.md) was specified, the smart identifier manages the data buffer's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to allocate the buffer. This parameter should be set to one of the following values:

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SizeBand` *(in, AIL_INT)*

Specifies the number of (x,y) surfaces (also called bands) to allocate to the buffer.

*For specifying the number of surfaces to allocate*

| Value | Description |
| --- | --- |
| `1 <= Value <= 1024` | Specifies the number of bands.

Specify 1 band for each color band, or other type of data, that the buffer needs to store (for example, monochrome images require 1 band; RGB color images require 3 color bands).

This value can be any non-zero integer value up to 1024 for an image buffer, or 1 or 3 for an array or LUT buffer. |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, width is specified in pixels.

### `SizeY` *(in, AIL_INT)*

Specifies the buffer height. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, height is specified in pixels.

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: data type and data depth per band. Specify the depth of the buffer per band as one of the following:

*For the data depth of the buffer*

| Value | Description |
| --- | --- |
| `1` | Specifies a 1-bit (packed binary) buffer. Note that you cannot allocate a 1-bit LUT buffer. |
| `8` | Specifies an 8-bit buffer. |
| `16` | Specifies a 16-bit buffer. |
| `32` | Specifies a 32-bit buffer. |

*For specifying the data type of the buffer*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the type of data is floating-point. The valid data depth is 32 bits. |
| `M_SIGNED` | Specifies that the type of data is signed. The valid data depths are 8, 16, or 32 bits. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. The valid data depths are 1, 8, 16, or 32 bits. |

### `Attribute` *(in, AIL_INT64)*

Specifies the buffer usage. Aurora Imaging Library uses this information to determine where to allocate the buffer in physical memory.

*For specifying the buffer usage*

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufAllocColor.md) buffer rather than a user-defined array.

Note that this attribute is only supported for 1-band and 3-band buffers. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data. The valid data depths are 8, 16, or 32 bits and only the planar internal storage format is valid.

Note that this attribute is only supported for 1-band and 3-band buffers. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. Note that a buffer with this attribute must have the [`M_UNSIGNED`](../../Reference/buf/MbufAllocColor.md) data type.

When allocating buffers for operations that require a source and destination buffer, and one of the buffers has an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute, the data will be compressed or decompressed depending on the attributes of the destination buffer. If both the source and destination buffers have [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) specifiers but different compression types, the data will be re-compressed according to the settings of the destination buffer.

Only 1-band and 3-band buffers can have an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md)attribute. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. This type of buffer is allocated in Aurora Imaging Library-reserved, physically contiguous, non-paged memory unless otherwise specified.

For Host buffers ([`M_HOST_MEMORY`](../../Reference/buf/MbufAllocColor.md)), the maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory ([`M_NON_PAGED`](../../Reference/buf/MbufAllocColor.md)).

For on-board buffers ([`M_ON_BOARD`](../../Reference/buf/MbufAllocColor.md)), the maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)) buffers that can be allocated is restricted by the total amount of on-board memory.

Only 1-band and 3-band buffers can have an [`M_GRAB`](../../Reference/buf/MbufAllocColor.md)attribute. |
| `M_PROC` | Specifies an image buffer that can be processed. If you intend to use the buffer as the source or destination buffer of a processing or analysis operation, you must allocate the buffer with an [`M_PROC`](../../Reference/buf/MbufAllocColor.md) attribute. |

*For allocating an overscan region*

| Value | Description |
| --- | --- |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. Specify the size of the region using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ALLOCATION_OVERSCAN_SIZE`](../../Reference/sys/MsysControl.md). |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_H264` | Specifies that the buffer will be used to hold H.264 data. This compression type is used when specifying a source of an input or a destination of an output for a sequence operation as an array of buffers ([`MseqDefine`](../../Reference/seq/MseqDefine.md) with [`M_BUFFER_LIST`](../../Reference/seq/MseqDefine.md)). |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), or [`M_YUV24`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) format. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocColor.md), [`M_RGB48`](../../Reference/buf/MbufAllocColor.md), [`M_YUV9`](../../Reference/buf/MbufAllocColor.md), [`M_YUV12`](../../Reference/buf/MbufAllocColor.md), [`M_YUV16`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), or [`M_YUV24`](../../Reference/buf/MbufAllocColor.md) format. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band data in [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) or [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_RGB24`](../../Reference/buf/MbufAllocColor.md), [`M_YUV24`](../../Reference/buf/MbufAllocColor.md), [`M_YUV12`](../../Reference/buf/MbufAllocColor.md), [`M_YUV9`](../../Reference/buf/MbufAllocColor.md), [`M_YUV16`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), or [`M_YUV16`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_YUV16`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) format. |

*For specifying a storage format and a location specifier*

| Value | Description |
| --- | --- |
| `M_DIB` | Forces the buffer to be a DIB buffer. This ensures your buffer is stored with a DIB header. Use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_BITMAPINFO`](../../Reference/buf/MbufInquire.md) to return a pointer (LPBITMAPINFO) to the header of the Aurora Imaging Library buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI.

When using a device context (using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DC_ALLOC`](../../Reference/buf/MbufControl.md)) for drawing, the buffer should be internally stored in GDI format, and cannot be a child buffer. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |

*For specifying that the buffer is FPGA accessible*

| Value | Description |
| --- | --- |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. The exact bank of on-board memory is determined by the other attributes used with this value.

If combined with the [`M_HOST_MEMORY`](../../Reference/buf/MbufAllocColor.md) attribute, the memory allocated will be in non-paged Host memory. |

*For specifying a storage format for image buffers*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). Typically, packed buffers are used for display ([`M_DISP`](../../Reference/buf/MbufAllocColor.md)). Note that it might be slower to process buffers with the [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) attribute. Also, note that you cannot allocate a packed LUT buffer. |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. Typically, planar buffers are used for processing ([`M_PROC`](../../Reference/buf/MbufAllocColor.md)). |

*For specifying a planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar pixels. |
| `M_YUV12` | Specifies YUV12 planar pixels. |
| `M_YUV24` | Specifies YUV24 planar pixels. |

*For specifying a packed color buffer format*

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth packed pixels (BGRXBGRX). The most-significant byte is a "don't care" byte. |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). Note that when accessing an [`M_RGB15`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). Note that when accessing an [`M_RGB16`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md).

This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)) buffers. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |

*For specifying a packed or planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). packed or planar pixels. |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 (4:2:2) pixels. |

*For specifying a location for a buffer*

| Value | Description |
| --- | --- |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory. |
| `M_ON_BOARD` | Forces the buffer in on-board memory. |

*For specifying a memory bank to be used*

| Value | Description |
| --- | --- |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank. |

*For specifying a location in a specific type of memory*

| Value | Description |
| --- | --- |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. This memory is mapped on the PCI bus.

Note that this combination value can only be added to [`M_ON_BOARD`](../../Reference/buf/MbufAllocColor.md). |

*For specifying the attribute of the specified physical memory type*

| Value | Description |
| --- | --- |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible.

The maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAllocColor.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory (specified at installation time or using the Aurora Imaging Configurator utility). |
| `M_PAGED` | Forces the buffer in pageable memory. |

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer identifier or specifies the data type that the function should use to return the buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the image buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated image buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocColor.md) is written as the identifier. |
| `Address in which to write the LUT buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated LUT buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAllocColor.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the data buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAllocColor.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md), ensure that the call is not made from the `DllMain()` function, because [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) from an initialization function in your DLL instead.

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
