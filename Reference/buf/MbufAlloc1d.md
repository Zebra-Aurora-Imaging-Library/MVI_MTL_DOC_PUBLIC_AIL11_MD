---
doctype: Reference
module: buf
function: MbufAlloc1d
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufAlloc1d"
---

# MbufAlloc1d

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

> Allocate a 1D data buffer.

## Syntax

```c
AIL_ID MbufAlloc1d(
    AIL_ID    SystemId,   //in
    AIL_INT   SizeX,      //in
    AIL_INT   Type,       //in
    AIL_INT64 Attribute,  //in
    AIL_ID *  BufIdPtr    //out
)
```

## Description

This function allocates a one-dimensional 1-band data buffer on the specified system.

After allocating the one-dimensional 1-band data buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the one-dimensional 1-band data buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufAlloc1d.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAlloc1d.md) was specified).

When the one-dimensional 1-band data buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufAlloc1d.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufAlloc1d.md) was specified, the smart identifier manages the one-dimensional 1-band data buffer's lifetime and you must not manually free it.

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to allocate the buffer. This parameter should be set to one of the following values:

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has a LUT buffer attribute, specify the number of LUT entries to allocate.

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: the depth and type of the data. Specify the depth of the buffer as one of the following:

*For the data depth of the buffer*

| Value | Description |
| --- | --- |
| `1` | Specifies a 1-bit (packed binary) buffer. Note that you cannot allocate a 1-bit LUT, kernel, or structuring element buffer. |
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
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) buffer rather than a user-defined array. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions.

The depth must be 8, 16, or 32-bit integer or floating-point. |
| `M_LUT` | Specifies a buffer to store lookup table data.

The valid data depths are 8, 16, or 32 bits. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions.

The data depth must be 32-bit. If signed, the range is -32767 to +32767. If unsigned, the range is 0 to +32767. Note that the data can be set to **M_DONT_CARE** to ignore specific structuring elements. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. Note that a buffer with this attribute must have the [`M_UNSIGNED`](../../Reference/buf/MbufAlloc1d.md) data type.

When allocating buffers for operations that require a source and destination buffer, and one of the buffers has an [`M_COMPRESS`](../../Reference/buf/MbufAlloc1d.md) attribute, the data will be compressed or decompressed depending on the attributes of the destination buffer. If both the source and destination buffers have [`M_COMPRESS`](../../Reference/buf/MbufAlloc1d.md) specifies but different compression types, the data will be re-compressed according to the settings of the destination buffer. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. This type of buffer is allocated in Aurora Imaging Library-reserved, physically contiguous, non-paged memory unless otherwise specified.

For Host buffers ([`M_HOST_MEMORY`](../../Reference/buf/MbufAlloc1d.md)), the maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAlloc1d.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory ([`M_NON_PAGED`](../../Reference/buf/MbufAlloc1d.md)).

For on-board buffers ([`M_ON_BOARD`](../../Reference/buf/MbufAlloc1d.md)), the maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAlloc1d.md)) buffers that can be allocated is restricted by the total amount of on-board memory. |
| `M_PROC` | Specifies an image buffer that can be processed. If you intend to use the buffer as the source or destination buffer of a processing or analysis operation, you must allocate the buffer with an [`M_PROC`](../../Reference/buf/MbufAlloc1d.md) attribute. |

*For allocating an overscan region*

| Value | Description |
| --- | --- |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. Specify the size of the region using [`MsysControl`](../../Reference/sys/MsysControl.md) with [`M_ALLOCATION_OVERSCAN_SIZE`](../../Reference/sys/MsysControl.md). |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data format is 1-band, 8-bit data. If no attribute is specified, [`M_JPEG_LOSSY`](../../Reference/buf/MbufAlloc1d.md) is assumed. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data format is 1-band, 8-bit data. |

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

If combined with the [`M_HOST_MEMORY`](../../Reference/buf/MbufAlloc1d.md) attribute, the memory allocated will be in non-paged Host memory. |

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

Note that this combination value can only be added to [`M_ON_BOARD`](../../Reference/buf/MbufAlloc1d.md). |

*For specifying the attribute of the specified physical memory type*

| Value | Description |
| --- | --- |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible.

The maximum (total) number of grab ([`M_GRAB`](../../Reference/buf/MbufAlloc1d.md)) buffers that can be allocated might be restricted by the total amount of Aurora Imaging Library-reserved, non-paged memory (specified at installation time or using the Aurora Imaging Configurator utility). |
| `M_PAGED` | Forces the buffer in pageable memory. |

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer identifier or specifies the data type that the function should use to return the buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufAlloc1d.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the one-dimensional 1-band data buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufAlloc1d.md) was specified).

## Remarks

> If you are creating a DLL that includes a call to [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md), ensure that the call is not made from the `DllMain()` function, because [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) might load a required DLL and you cannot load a DLL from `DllMain()`. If necessary, call [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) from an initialization function in your DLL instead.

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAlloc1d.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> You can allocate an image buffer with an [`M_KERNEL`](../../Reference/buf/MbufAlloc1d.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc1d.md) attribute with Aurora Imaging Library Lite. However, these attributes are not required to work under Aurora Imaging Library Lite.
