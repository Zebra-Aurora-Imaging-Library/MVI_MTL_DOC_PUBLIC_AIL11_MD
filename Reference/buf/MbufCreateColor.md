---
doctype: Reference
module: buf
function: MbufCreateColor
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCreateColor"
---

# MbufCreateColor

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

> Create a multi-band data buffer.

## Syntax

```c
AIL_ID MbufCreateColor(
    AIL_ID    SystemId,        //in
    AIL_INT   SizeBand,        //in
    AIL_INT   SizeX,           //in
    AIL_INT   SizeY,           //in
    AIL_INT   Type,            //in
    AIL_INT64 Attribute,       //in
    AIL_INT64 ControlFlag,     //in
    AIL_INT   Pitch,           //in
    void **   ArrayOfDataPtr,  //in
    AIL_ID *  BufIdPtr         //out
)
```

## Description

This function creates a multi-band data buffer using memory at a specified location, and associates it with a specific Aurora Imaging Library system.

> **Note:** This function should be used with caution because, when using physical addresses, it provides direct access to any of your computer's memory mapped devices; when using logical addresses, memory protection errors could result.

It is generally better to leave buffer allocation, data loading, and memory control to Aurora Imaging Library ([`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md), [`MbufGetColor`](../../Reference/buf/MbufGetColor.md), [`MbufPutColor`](../../Reference/buf/MbufPutColor.md)), since Aurora Imaging Library might require special memory attributes (such as non-paged memory) or alignment to associate the buffer with a particular target system.

You must allocate the appropriate memory before calling [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md). [`MbufInquire`](../../Reference/buf/MbufInquire.md) can be used to get the pointer to the data of an Aurora Imaging Library allocated buffer.

If creating a buffer over a JPEG stream in one of the supported formats, Aurora Imaging Library can automatically set the size, pitch, and type. To do so, set [`SizeBand`](../../Reference/buf/MbufCreateColor.md), [`SizeX`](../../Reference/buf/MbufCreateColor.md), [`SizeY`](../../Reference/buf/MbufCreateColor.md), [`Type`](../../Reference/buf/MbufCreateColor.md), and [`Pitch`](../../Reference/buf/MbufCreateColor.md) to [`M_DEFAULT`](../../Reference/buf/MbufCreateColor.md), and set [`Attribute`](../../Reference/buf/MbufCreateColor.md) to [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) without any compression type. Note that in this case, you must provide a host address ([`M_HOST_ADDRESS`](../../Reference/buf/MbufCreateColor.md)).

[`ControlFlag`](../../Reference/buf/MbufCreateColor.md) set to [`M_HOST_ADDRESS`](../../Reference/buf/MbufCreateColor.md); and [`M_IMAGE`](../../Reference/buf/MbufCreateColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md)be set without any compression type.

If creating a 3-band buffer, be as specific as possible when setting the buffer attributes (using the [`Attribute`](../../Reference/buf/MbufCreateColor.md) parameter) and make sure to specify the color format of the buffer. The more information known about the associated buffer, the better the results.

After creating the data buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the data buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufCreateColor.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufCreateColor.md) was specified).

When the data buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufCreateColor.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufCreateColor.md) was specified, the smart identifier manages the data buffer's lifetime and you must not manually free it.

> **Note:** If you create a buffer using [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md), freeing the buffer (either manually using[`MbufFree`](../../Reference/buf/MbufFree.md) or automatically with a smart identifier) only releases the internal structure required for the mapped buffer. You are responsible for freeing the underlying memory used by the buffer.

### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Note that to create a buffer mapped to on-board memory, it must be shared memory. See your _Hardware-specific Notes_ for more information regarding shared memory. |

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to create the buffer. This parameter should be set to one of the following values:

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SizeBand` *(in, AIL_INT)*

Specifies the number of (x,y) surfaces (also called bands) that the buffer should have to contain each color or other type of data.

*For specifying the number of surfaces*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the same number of bands as the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateColor.md). |
| `1 <= Value <= 1024` | Specifies the number of bands. There are generally either 1 or 3 bands.

When acquiring or processing monochrome images, the buffer requires only 1 band. For RGB color images, it requires 3 color bands.

This value can be any non-zero integer value up to 1024 for an image buffer, or 1 or 3 for an array or LUT buffer. |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width.

*For the buffer width*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer width is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateColor.md). |
| `Value` | Specifies the buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, width is specified in pixels. |

### `SizeY` *(in, AIL_INT)*

Specifies the buffer height.

*For the buffer height*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer height is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateColor.md). |
| `Value` | Specifies the buffer height. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, height is specified in pixels. |

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: data depth and data type, per band. Specify the depth of the buffer per band as one of the following:

*For the depth of the buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the data depth and type are identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreateColor.md). |
| `1` | Specifies a 1-bit (packed binary) buffer. Note that you cannot create a 1-bit LUT buffer. |
| `8` | Specifies an 8-bit buffer. |
| `16` | Specifies a 16-bit buffer. |
| `32` | Specifies a 32-bit buffer. |

*For specifying the data type*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the type of data is floating-point. The valid data depth is 32 bits. |
| `M_SIGNED` | Specifies that the type of data is signed. The valid data depths are 8, 16, or 32 bits. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. The valid data depths are 1, 8, 16, or 32 bits. |

### `Attribute` *(in, AIL_INT64)*

Specifies the buffer usage. This allows you to specify the type of buffer, intended purpose, compression type, storage format, data direction, location, internal storage format, memory type, and overscan region of the created buffer; all of which provides additional information for other Aurora Imaging Library functions.

*For specifying the buffer usage*

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufCreateColor.md) buffer rather than a user-defined array.

Note that this attribute is only supported for 1-band and 3-band buffers. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data.

The depth must be 8, 16, or 32-bit integer or floating-point and the internal storage format must be planar ([`M_PLANAR`](../../Reference/buf/MbufCreateColor.md)).

Note that this attribute is only supported for 1-band and 3-band buffers. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. Note that a buffer with this attribute cannot have the [`M_SIGNED`](../../Reference/buf/MbufCreateColor.md) data type.

> **Note:** If [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) without any compression type is specified and the underlying data is of type JPEG, [`SizeBand`](../../Reference/buf/MbufCreateColor.md), [`Type`](../../Reference/buf/MbufCreateColor.md), [`SizeX`](../../Reference/buf/MbufCreateColor.md), and [`SizeY`](../../Reference/buf/MbufCreateColor.md)parameters can be set to [`M_DEFAULT`](../../Reference/buf/MbufCreateColor.md). For all other cases, the underlying data must already be compressed with the specified compression type and color format. The [`SizeBand`](../../Reference/buf/MbufCreateColor.md), [`Type`](../../Reference/buf/MbufCreateColor.md) [`SizeX`](../../Reference/buf/MbufCreateColor.md)and[`SizeY`](../../Reference/buf/MbufCreateColor.md)parameters must also be set to the exact size of the compressed image stored in the underlying data. It is not possible to create a compressed buffer mapped on to only part of a compressed image.

When mapping buffers for operations that require a source and destination buffer, and one of the buffers has an [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) attribute, the data will be compressed or decompressed depending on the attributes of the destination buffer. If both the source and destination buffers have [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) specifiers but different compression types, the data will be re-compressed according to the settings of the destination buffer.

> **Note:** Note that buffers created from preallocated memory with the[`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md) attribute should typically only be passed to Aurora Imaging Library functions as a source. If you pass a compressed buffer created from preallocated memory as a destination for an Aurora Imaging Library function, an error will be generated if the compressed size in memory of the function output is not the same as the size of the allocated memory. This is true even if the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md)parameter is set to[`M_AIL_ID`](../../Reference/buf/MbufCreateColor.md).

Only 1-band and 3-band buffers can have an [`M_COMPRESS`](../../Reference/buf/MbufCreateColor.md)attribute. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data.

Only 1-band and 3-band buffers can have an [`M_GRAB`](../../Reference/buf/MbufCreateColor.md)attribute. |
| `M_PROC` | Specifies an image buffer that can be processed. |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateColor.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateColor.md), [`M_RGB48`](../../Reference/buf/MbufCreateColor.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateColor.md), or[`M_YUV24`](../../Reference/buf/MbufCreateColor.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateColor.md) format. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateColor.md), [`M_RGB48`](../../Reference/buf/MbufCreateColor.md), [`M_YUV9`](../../Reference/buf/MbufCreateColor.md), [`M_YUV12`](../../Reference/buf/MbufCreateColor.md), [`M_YUV16`](../../Reference/buf/MbufCreateColor.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateColor.md), or [`M_YUV24`](../../Reference/buf/MbufCreateColor.md) format. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band data in [`M_RGB24`](../../Reference/buf/MbufCreateColor.md) or [`M_RGB48`](../../Reference/buf/MbufCreateColor.md) format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_RGB24`](../../Reference/buf/MbufCreateColor.md), [`M_YUV24`](../../Reference/buf/MbufCreateColor.md), [`M_YUV12`](../../Reference/buf/MbufCreateColor.md), [`M_YUV9`](../../Reference/buf/MbufCreateColor.md), [`M_YUV16`](../../Reference/buf/MbufCreateColor.md) + [`M_PLANAR`](../../Reference/buf/MbufCreateColor.md), or [`M_YUV16`](../../Reference/buf/MbufCreateColor.md) + [`M_PACKED`](../../Reference/buf/MbufCreateColor.md) format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_YUV16`](../../Reference/buf/MbufCreateColor.md) + [`M_PACKED`](../../Reference/buf/MbufCreateColor.md) format. |

*For specifying a storage format for image buffers*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). Typically, packed buffers are used for display. Note that it might be slower to process buffers with the [`M_PACKED`](../../Reference/buf/MbufCreateColor.md) attribute. Also, note that you cannot allocate a packed LUT buffer. |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. Typically, planar buffers are used for processing. |

*For specifying a planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar standard. |
| `M_YUV12` | Specifies YUV12 planar standard. |
| `M_YUV24` | Specifies YUV24 planar standard. |

*For specifying a packed color buffer format*

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth (BGR) packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth (BGR) packed pixels (BGRXBGRX). The most-significant byte is a "don't care" byte. |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5).

Note that when accessing an [`M_RGB15`](../../Reference/buf/MbufCreateColor.md) + [`M_PACKED`](../../Reference/buf/MbufCreateColor.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5).

Note that when accessing an [`M_RGB16`](../../Reference/buf/MbufCreateColor.md) + [`M_PACKED`](../../Reference/buf/MbufCreateColor.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). |

*For specifying a packed or planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). packed or planar pixels. |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 packed or planar (4:2:2) standard. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the physical nature of the buffer. This parameter can be set to one of the values below.

*For specifying the physical nature of the buffer*

| Value | Description |
| --- | --- |
| `M_AIL_ID` | Specifies that the new buffer maps to an existing source buffer. [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateColor.md) is a pointer to an array that stores a pointer to the Aurora Imaging Library identifier of the source buffer. |
| `M_HOST_ADDRESS` | Specifies that [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateColor.md)is a pointer to an array of host addresses that point to the data. The number of pointers in the array depends on the number of bands and whether the data is compressed or packed.

Note that when using logical addresses, memory protection errors could result.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the logical address of an Aurora Imaging Library allocated buffer. |
| `M_PHYSICAL_ADDRESS` | Specifies that [`ArrayOfDataPtr`](../../Reference/buf/MbufCreateColor.md)is a pointer to an array of physical addresses that point to the data. The number of pointers in the array depends on the number of bands and whether the data is compressed or packed.

Note that using physical addresses provides direct access to any of your computer's memory mapped devices.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the physical address of an Aurora Imaging Library allocated buffer. |

*For specifying how the pitch is measured*

| Value | Description |
| --- | --- |
| `M_PITCH` | Specifies that the pitch is in pixels. |
| `M_PITCH_BYTE` | Specifies that the pitch is in bytes. |

### `Pitch` *(in, AIL_INT)*

Specifies the size of the pitch. The pitch is the number of pixels or bytes (as specified by the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter) between the start of any two adjacent rows of the buffer. The actual length of a row in the buffer, in physical memory, might be different from the value of the [`SizeX`](../../Reference/buf/MbufCreateColor.md) parameter (for example, due to use of buffer overscan).

*For specifying the size of the pitch*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that when dealing with a 1-bit buffer, the pitch is a multiple of 4 bytes; otherwise the pitch equals the [`SizeX`](../../Reference/buf/MbufCreateColor.md) parameter. |
| `Value` | Specifies the pitch in pixels or bytes (as determined by the [`ControlFlag`](../../Reference/buf/MbufCreateColor.md) parameter). |

### `ArrayOfDataPtr` *(in, **void)*

Specifies one or more data pointers that address the memory to which to map the created Aurora Imaging Library buffer.

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the data buffer identifier or specifies the data type that the function should use to return the data buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address for the array buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the created array buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufCreateColor.md) is written as the identifier. |
| `Address for the image buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the created image buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufCreateColor.md) is written as the identifier. |
| `Address for the LUT buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the created LUT buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufCreateColor.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the data buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufCreateColor.md) was specified).

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
