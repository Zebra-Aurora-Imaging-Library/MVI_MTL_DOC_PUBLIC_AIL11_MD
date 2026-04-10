---
doctype: Reference
module: buf
function: MbufClone
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufClone"
---

# MbufClone

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

> Clones a data buffer or container.

## Syntax

```c
AIL_ID MbufClone(
    AIL_ID    SrcContainerOrBufId,    //in
    AIL_ID    SysId,                  //in
    AIL_INT   SizeX,                  //in
    AIL_INT   SizeY,                  //in
    AIL_INT   Type,                   //in
    AIL_INT64 Attribute,              //in
    AIL_INT64 ControlFlag,            //in
    AIL_ID *  VarContainerOrBufIdPtr  //out
)
```

## Description

This function allocates a buffer or container that is similar to the source buffer or container. If the source is a buffer, the clone is allocated with the same size, number of bands, type, and attributes as the source, unless you specify that certain characteristics of the source buffer be different for the clone (for example, its size). If the source is a container, the clone is allocated with the same attributes as the source and components that are clones of the components in the source.

If you clone a child buffer, the clone will be similar to the source child, but it will not be a child itself. A clone of a band child of a packed buffer will be a one band planar equivalent.

If requested, you can also copy the source buffer or container's data to the clone (using [`M_COPY_SOURCE_DATA`](../../Reference/buf/MbufClone.md)).

After cloning the buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufClone.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufClone.md) was specified).

When the cloned buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufClone.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufClone.md) was specified, the smart identifier manages the cloned buffer's lifetime and you must not manually free it.

## Parameters

### `SrcContainerOrBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer or container to clone.

### `SysId` *(in, AIL_ID)*

Specifies the system on which to allocate the clone.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the clone is allocated on the same system as the source buffer or container. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SizeX` *(in, AIL_INT)*

Specifies the width of the clone. If the source is a container, this parameter must be set to [`M_DEFAULT`](../../Reference/buf/MbufClone.md).

*For specifying the width of the buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the width of the clone is the same as the source buffer, or that the destination is a container. |
| `Value >= 1` | Specifies the width of the clone buffer, in pixels. |

### `SizeY` *(in, AIL_INT)*

Specifies the height of the clone. If the source is a container, this parameter must be set to [`M_DEFAULT`](../../Reference/buf/MbufClone.md).

*For specifying the height of the buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the height of the clone is the same as the source buffer. |
| `Value >= 1` | Specifies the height of the clone buffer, in pixels. |

### `Type` *(in, AIL_INT)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Attribute` *(in, AIL_INT64)*

Specifies the attributes of the clone. For both buffers and containers, you can specify that the clone will retain the same attributes as the source by setting this parameter to [`M_DEFAULT`](../../Reference/buf/MbufClone.md).

*For specifying the clone attributes*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to allocate the clone with the same attributes as the source. |
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufClone.md) buffer rather than a user-defined array. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions.

The depth must be 8, 16, or 32-bit integer or floating-point. |
| `M_LUT` | Specifies a buffer to store lookup table data. The valid data depths are 8, 16, or 32 bits and only the planar internal storage format is valid. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions.

The data depth must be 32-bit. If signed, the range is -32767 to +32767. If unsigned, the range is 0 to +32767. Note that the data can be set to **M_DONT_CARE** to ignore specific structuring elements. |

*For specifying the intended purpose of the image buffer or container*

| Value | Description |
| --- | --- |
| `M_DISP` | Specifies an image buffer or container intended for display. |
| `M_GRAB` | Specifies an image buffer or container in which to grab data.

Only 1-band and 3-band buffers can have an [`M_GRAB`](../../Reference/buf/MbufClone.md)attribute. |
| `M_PROC` | Specifies an image buffer or container intended for processing. |

*For specifying that the image buffer is compressed*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |

*For allocating an overscan region*

| Value | Description |
| --- | --- |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_H264` | Specifies that the buffer will be used to hold H.264 data. This compression type is used when specifying a source of an input or a destination of an output for a sequence operation as an array of buffers ([`MseqDefine`](../../Reference/seq/MseqDefine.md) with [`M_BUFFER_LIST`](../../Reference/seq/MseqDefine.md)). |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufClone.md) + [`M_PLANAR`](../../Reference/buf/MbufClone.md), [`M_RGB48`](../../Reference/buf/MbufClone.md) + [`M_PLANAR`](../../Reference/buf/MbufClone.md), or [`M_YUV24`](../../Reference/buf/MbufClone.md) + [`M_PLANAR`](../../Reference/buf/MbufClone.md) format. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band, 8- or 16-bit data in [`M_RGB24`](../../Reference/buf/MbufClone.md), [`M_RGB48`](../../Reference/buf/MbufClone.md), [`M_YUV9`](../../Reference/buf/MbufClone.md), [`M_YUV12`](../../Reference/buf/MbufClone.md), [`M_YUV16`](../../Reference/buf/MbufClone.md) + [`M_PLANAR`](../../Reference/buf/MbufClone.md), or [`M_YUV24`](../../Reference/buf/MbufClone.md) format. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data, and 3-band data in [`M_RGB24`](../../Reference/buf/MbufClone.md) or [`M_RGB48`](../../Reference/buf/MbufClone.md) format. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_RGB24`](../../Reference/buf/MbufClone.md), [`M_YUV24`](../../Reference/buf/MbufClone.md), [`M_YUV12`](../../Reference/buf/MbufClone.md), [`M_YUV9`](../../Reference/buf/MbufClone.md), [`M_YUV16`](../../Reference/buf/MbufClone.md) + [`M_PLANAR`](../../Reference/buf/MbufClone.md), or [`M_YUV16`](../../Reference/buf/MbufClone.md) + [`M_PACKED`](../../Reference/buf/MbufClone.md) format. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data formats are: 1-band 8-bit data, and 3-band 8-bit data in [`M_YUV16`](../../Reference/buf/MbufClone.md) + [`M_PACKED`](../../Reference/buf/MbufClone.md) format. |

*For specifying a storage format and a location specifier*

| Value | Description |
| --- | --- |
| `M_DIB` | Forces the buffer to be a DIB buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |

*For specifying that the buffer is FPGA accessible*

| Value | Description |
| --- | --- |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. The exact bank of on-board memory is determined by the other attributes used with this value. If combined with the [`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md) attribute, the memory allocated will be in non-paged Host memory. |

*For specifying a storage format for image buffers*

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |

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
| `M_BGR24` | Specifies 24-bit color depth packed pixels (BGRBGR).   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers. |
| `M_BGR32` | Specifies 32-bit color depth packed pixels (BGRXBGRX). The most-significant byte is a "don't care" byte.   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers. |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). Note that when accessing an [`M_RGB15`](../../Reference/buf/MbufClone.md) + [`M_PACKED`](../../Reference/buf/MbufClone.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). Note that when accessing an [`M_RGB16`](../../Reference/buf/MbufClone.md) + [`M_PACKED`](../../Reference/buf/MbufClone.md) buffer as a 3-band 8-bit buffer, the least significant bits are set to 0. |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md). This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. For more information, see [YUV buffers](../../UserGuide/C23_Data_buffers/S12_YUV_buffers.md).   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers.

   This value can be used with grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers if those buffers are also packed ([`M_PACKED`](../../Reference/buf/MbufClone.md)). |

*For specifying a packed or planar color buffer format*

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels.   This value can be used with planar ([`M_PLANAR`](../../Reference/buf/MbufClone.md)) grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers.

   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufClone.md)) grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). packed or planar pixels.   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufClone.md)) grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers.

   This value can be used with planar ([`M_PLANAR`](../../Reference/buf/MbufClone.md)) grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers if the buffer is allocated on the Host ([`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md)) and not on-board. |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 (4:2:2) pixels.   This value can be used with packed ([`M_PACKED`](../../Reference/buf/MbufClone.md)) grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers. |

*For specifying a location for a buffer*

| Value | Description |
| --- | --- |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory.   To ensure that an [`M_OFF_BOARD`](../../Reference/buf/MbufClone.md) buffer can be used by the DMA transfer section of the board, the buffer must be allocated in non-paged memory ([`M_OFF_BOARD`](../../Reference/buf/MbufClone.md)+[`M_NON_PAGED`](../../Reference/buf/MbufClone.md) or [`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md)+[`M_NON_PAGED`](../../Reference/buf/MbufClone.md)). |
| `M_ON_BOARD` | Forces the buffer in on-board memory.   By default, all on-board buffers are allocated in memory that is not mapped on the PCI bus. This unshared memory cannot be accessed by the Host or any system other than the one on which it was allocated. See combination values below ([`M_SHARED`](../../Reference/buf/MbufClone.md)) to change this default behavior.

   This is only available for grab buffers ([`M_GRAB`](../../Reference/buf/MbufClone.md)).

   This is only available for image buffers ([`M_IMAGE`](../../Reference/buf/MbufClone.md)).

   This is only available for image ([`M_IMAGE`](../../Reference/buf/MbufClone.md)) and LUT buffers ([`M_LUT`](../../Reference/buf/MbufClone.md)). |

*For specifying a memory bank to be used*

| Value | Description |
| --- | --- |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank.   Valid values for _n_ are between 0 and 6, inclusive. However, not all memory banks are necessarily present on a given system. Typically, if there is more than 1 memory bank, it is NUMA-enabled. Note that, to ensure that you are accessing a NUMA-enabled memory bank, you must specify the[`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md) combination value.

 To determine the actual number of memory banks available on the Aurora Imaging Library system, use [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_NUM`](../../Reference/app/MappInquireMp.md) and [`MappInquireMp`](../../Reference/app/MappInquireMp.md) with [`M_MEMORY_BANK_AFFINITY_MASK`](../../Reference/app/MappInquireMp.md). For more information, refer to [NUMA Support](../../UserGuide/C66_Using_AIL_with_multiprocessing_and_under_multithread_systems/S02_Transparent_MultiCore_Use.md).

 Note that this value cannot be used with [`M_COMPRESS`](../../Reference/buf/MbufClone.md) or [`M_NON_PAGED`](../../Reference/buf/MbufClone.md).

 Also note that the combination of [`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md) with this value, in paged memory, is only useful when doing multi-core processing on a Host system.

   Note that this value is not available with the[`M_HOST_MEMORY`](../../Reference/buf/MbufClone.md) combination value.

   Valid values for _n_ are between 0 and 1, inclusive. Refer to your Hardware-specific Notes for more information about on-board memory. Note that, to assure you are accessing an on-board memory bank, you must specify the[`M_ON_BOARD`](../../Reference/buf/MbufClone.md) combination value.

 |   |   |   |
| --- | --- | --- |
| Memory bank number | Type of memory | Description |
| 0 | Acquisition (SDRAM) | This is the default memory bank for an on-board buffer created with the [`M_GRAB`](../../Reference/buf/MbufClone.md) attribute. |

   In this case,_n_ must be set to 0. This is the default memory bank for an on-board buffer created with the [`M_GRAB`](../../Reference/buf/MbufClone.md) attribute. Refer to your Hardware-specific Notes for more information about on-board memory. Note that, to assure you are accessing an on-board memory bank, you must specify the[`M_ON_BOARD`](../../Reference/buf/MbufClone.md) combination value. |

*For specifying a location in a specific type of memory*

| Value | Description |
| --- | --- |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. This memory is mapped on the PCI bus. Note that this combination value can only be added to [`M_ON_BOARD`](../../Reference/buf/MbufClone.md). |

*For specifying the attribute of the specified physical memory type*

| Value | Description |
| --- | --- |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible. The maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufClone.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory (specified at installation time or using the Aurora Imaging Configurator utility). |
| `M_PAGED` | Forces the buffer in pageable memory. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the control settings for the cloning operation.

*For specifying which information to copy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to copy only the number of bands, type, and attributes of the source buffer (or components of the source container). The data, information dependent on the content of the buffer (such as [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md)), and the camera calibration information associated with the source buffer are not copied. |
| `M_COPY_SOURCE_DATA` | Specifies to copy the number of bands, type, attributes, and data of the source buffer (or components of the source container), including the camera calibration information and information dependent on the content of the buffer (such as [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md)). |

### `VarContainerOrBufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer or container identifier or specifies the data type that the function should use to return the buffer or container identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer or container ; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer or container ; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer or container  (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the buffer identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated buffer.

If cloning fails, [`M_NULL`](../../Reference/buf/MbufClone.md) is written as the identifier. |
| `Address in which to write the container identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated container.

If cloning fails, [`M_NULL`](../../Reference/buf/MbufClone.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the buffer or container identifier (of the new, cloned buffer or container) either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufClone.md) was specified).

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufClone.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> You can allocate a buffer with an [`M_KERNEL`](../../Reference/buf/MbufClone.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufClone.md) attribute with Aurora Imaging Library Lite. However, these attributes are not required to work under Aurora Imaging Library Lite.
