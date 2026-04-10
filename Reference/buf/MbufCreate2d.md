---
doctype: Reference
module: buf
function: MbufCreate2d
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufCreate2d"
---

# MbufCreate2d

| Board | Supported |
| --- | --- |
| Host System | Yes |
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

> Create a two-dimensional data buffer.

## Syntax

```c
AIL_ID MbufCreate2d(
    AIL_ID       SystemId,     //in
    AIL_INT      SizeX,        //in
    AIL_INT      SizeY,        //in
    AIL_INT      Type,         //in
    AIL_INT64    Attribute,    //in
    AIL_INT64    ControlFlag,  //in
    AIL_INT      Pitch,        //in
    const void * DataPtr,      //in
    AIL_ID *     BufIdPtr      //out
)
```

## Description

This function creates a two-dimensional data buffer using memory at a specified location, and associates it with a specific Aurora Imaging Library system.

> **Note:** This function should be used with caution because, when using physical addresses, it provides direct access to any of your computer's memory mapped devices; when using logical addresses, memory protection errors could result.

It is generally better to leave buffer allocation, data loading, and memory control to Aurora Imaging Library ([`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md), [`MbufGet2d`](../../Reference/buf/MbufGet2d.md), [`MbufPut2d`](../../Reference/buf/MbufPut2d.md)), since Aurora Imaging Library might require special memory attributes (such as non-paged memory) or alignment to associate the buffer with a particular target system.

You must allocate the appropriate memory before calling [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md). [`MbufInquire`](../../Reference/buf/MbufInquire.md) can be used to get the pointer to the data of an Aurora Imaging Library allocated buffer.

After creating the two-dimensional data buffer, you should check if the operation was successful, using [`MappGetError`](../../Reference/app/MappGetError.md) or by verifying that the two-dimensional data buffer identifier returned is not [`M_NULL`](../../Reference/buf/MbufCreate2d.md) (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufCreate2d.md) was specified).

When the two-dimensional data buffer is no longer required, release it using[`MbufFree`](../../Reference/buf/MbufFree.md)unless [`M_UNIQUE_ID`](../../Reference/buf/MbufCreate2d.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/buf/MbufCreate2d.md) was specified, the smart identifier manages the two-dimensional data buffer's lifetime and you must not manually free it.

> **Note:** If you create a buffer using [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md), freeing the buffer (either manually using[`MbufFree`](../../Reference/buf/MbufFree.md) or automatically with a smart identifier) only releases the internal structure required for the mapped buffer. You are responsible for freeing the underlying memory used by the buffer.

### System specific

| Board(s) | Note |
|---|---|
| Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF | Note that to create a buffer mapped to on-board memory, it must be shared memory. See your _Hardware-specific Notes_ for more information regarding shared memory. |

## Parameters

### `SystemId` *(in, AIL_ID)*

Specifies the Aurora Imaging Library system on which to create the buffer.

*For specifying the Aurora Imaging Library system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `SizeX` *(in, AIL_INT)*

Specifies the buffer width.

*For specifying the buffer width*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer width is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreate2d.md). |
| `Value` | Specifies buffer width. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, width is specified in pixels. |

### `SizeY` *(in, AIL_INT)*

Specifies the buffer height.

*For specifying the buffer height*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the buffer height is identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreate2d.md). |
| `Value` | Specifies the buffer height. The units are determined by the selected buffer attribute. For example, if the buffer has an image buffer attribute, height is specified in pixels. |

### `Type` *(in, AIL_INT)*

Specifies a combination of two values: data type and data depth. Specify the depth of the buffer as one of the following:

*For specifying the data depth of the buffer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that the buffer data depth and type are identical to that of the source buffer when the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md) parameter is set to [`M_AIL_ID`](../../Reference/buf/MbufCreate2d.md). |
| `1` | Specifies a 1-bit (packed binary) buffer. Note that you cannot allocate a 1-bit LUT, kernel, or structuring element buffer. |
| `8` | Specifies an 8-bit buffer. |
| `16` | Specifies a 16-bit buffer. |
| `32` | Specifies a 32-bit buffer. |

*For specifying the data type of the buffer*

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the type of data is floating-point. The valid data depth is 32 bits. |
| `M_SIGNED` | Specifies that the type of data is signed. The valid data depths are 8, 16 or 32 bits. |
| `M_UNSIGNED` *(default)* | Specifies that the type of data is unsigned. The valid data depths are 1, 8, 16 or 32 bits. |

### `Attribute` *(in, AIL_INT64)*

Specifies the buffer usage. This allows you to specify the type of buffer, intended purpose, compression type, storage format, data direction, location, internal storage format, memory type, and overscan region of the created buffer; all of which provides additional information for other Aurora Imaging Library functions.

*For specifying the buffer usage*

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. Note that some functions take an [`M_ARRAY`](../../Reference/buf/MbufCreate2d.md) buffer rather than a user-defined array. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions.

The depth must be 8, 16, or 32-bit integer or floating-point. |
| `M_LUT` | Specifies a buffer to store lookup table data.

The valid data depths are 8, 16, or 32-bit. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions.

The data depth must be 32-bit. If signed, the range is -32768 to +32767. If unsigned, the range is 0 to +32767. Note that the data can be set to **M_DONT_CARE** to ignore specific structuring elements. |

*For specifying the intended purpose of the image buffer*

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. Note that a buffer with this attribute cannot have the [`M_SIGNED`](../../Reference/buf/MbufCreate2d.md) data type.

> **Note:** If [`M_COMPRESS`](../../Reference/buf/MbufCreate2d.md) is specified, the underlying data must already be compressed with the specified compression type. The [`Type`](../../Reference/buf/MbufCreate2d.md) [`SizeX`](../../Reference/buf/MbufCreate2d.md)and[`SizeY`](../../Reference/buf/MbufCreate2d.md)parameters must also be set to the exact size of the compressed image stored in the underlying data. It is not possible to create a compressed buffer mapped on to only part of a compressed image.

When mapping buffers for operations that require a source and destination buffer, and one of the buffers has an [`M_COMPRESS`](../../Reference/buf/MbufCreate2d.md) attribute, the data will be compressed or decompressed depending on the attributes of the destination buffer. If both the source and destination buffers have [`M_COMPRESS`](../../Reference/buf/MbufCreate2d.md) specifiers but different compression types, the data will be re-compressed according to the settings of the destination buffer.

> **Note:** Note that buffers created from preallocated memory with the[`M_COMPRESS`](../../Reference/buf/MbufCreate2d.md) attribute should typically only be passed to Aurora Imaging Library functions as a source. If you pass a compressed buffer created from preallocated memory as a destination for an Aurora Imaging Library function, an error will be generated if the compressed size in memory of the function output is not the same as the size of the allocated memory. This is true even if the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md)parameter is set to[`M_AIL_ID`](../../Reference/buf/MbufCreate2d.md). |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |

*For specifying the compression type*

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. The supported data formats are: 1-band, 8- or 16-bit data. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. The supported data format is 1-band, 8-bit data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. The supported data format is 1-band, 8-bit data. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the physical nature of the buffer. This parameter can be set to one of the values below.

*For specifying the physical nature of the buffer*

| Value | Description |
| --- | --- |
| `M_AIL_ID` | Specifies that the new buffer maps to an existing source buffer. [`DataPtr`](../../Reference/buf/MbufCreate2d.md) points to the Aurora Imaging Library identifier of the source buffer. |
| `M_HOST_ADDRESS` | Specifies that [`DataPtr`](../../Reference/buf/MbufCreate2d.md) is a logical address that points to the data.

Note that when using logical addresses, memory protection errors could result.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the logical address of an Aurora Imaging Library allocated buffer. |
| `M_PHYSICAL_ADDRESS` | Specifies that [`DataPtr`](../../Reference/buf/MbufCreate2d.md) is a physical address that points to the data.

Note that using physical addresses provides direct access to any of your computer's memory mapped devices.

Note that you can use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) to get the physical address of an Aurora Imaging Library allocated buffer. |

*For specifying how the pitch is measured*

| Value | Description |
| --- | --- |
| `M_PITCH` | Specifies that the pitch is in pixels. |
| `M_PITCH_BYTE` | Specifies that the pitch is in bytes. |

### `Pitch` *(in, AIL_INT)*

Specifies the size of the pitch. The pitch is the number of pixels or bytes (as specified by the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md) parameter) between the start of any two adjacent rows of the buffer. The actual length of a row in the buffer, in physical memory, might be different from the value of the [`SizeX`](../../Reference/buf/MbufCreate2d.md) parameter (for example, due to use of buffer overscan).

*For specifying the size of the pitch*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that when dealing with a 1-bit buffer, the pitch is a multiple of 4 bytes; otherwise the pitch equals the [`SizeX`](../../Reference/buf/MbufCreate2d.md) parameter. |
| `Value` | Specifies the pitch in pixels or bytes (as determined by the [`ControlFlag`](../../Reference/buf/MbufCreate2d.md) parameter). |

### `DataPtr` *(in, *void)*

Specifies the Aurora Imaging Library identifier of the buffer, or a pointer to the data array, on which to map the created Aurora Imaging Library buffer.

### `BufIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the buffer identifier or specifies the data type that the function should use to return the buffer identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated buffer; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_BUF_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the buffer (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the created buffer.

If allocation fails, [`M_NULL`](../../Reference/buf/MbufCreate2d.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the two-dimensional data buffer identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_BUF_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/buf/MbufCreate2d.md) was specified).

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> You can allocate an image buffer with an [`M_KERNEL`](../../Reference/buf/MbufCreate2d.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufCreate2d.md) attribute with Aurora Imaging Library Lite. However, these attributes are not required to work under Aurora Imaging Library Lite.
