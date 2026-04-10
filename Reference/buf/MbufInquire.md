---
doctype: Reference
module: buf
function: MbufInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufInquire"
---

# MbufInquire

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

> Inquire about an Aurora Imaging Library data buffer setting.

## Syntax

```c
AIL_INT MbufInquire(
    AIL_ID    BufId,        //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about a specified setting of an Aurora Imaging Library data buffer.

## Parameters

### `BufId` *(in, AIL_ID)*

Specifies the identifier of the source data buffer. The buffer must have been previously allocated on the required system using a function of the Buffer module (for example, [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) or [`MbufImport`](../../Reference/buf/MbufImport.md)).

### `InquireType` *(in, AIL_INT64)*

Specifies the type of buffer setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MbufInquire`](../../Reference/buf/MbufInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`, except when the specified [`InquireType`](../../Reference/buf/MbufInquire.md) requires the [`UserVarPtr`](../../Reference/buf/MbufInquire.md) parameter to be set to the address of an _AIL_INT64_ or _AIL_DOUBLE_. In this case, you must set this parameter to the address of an _AIL_INT64_ or _AIL_DOUBLE_, respectively.

## Parameter Associations

### For inquiring general buffer settings

The following inquire types allow you to inquire about general types of buffer settings.

---

### `M_ALLOCATION_OVERSCAN_SIZE`

Inquires the size of the overscan region of the image buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of the overscan region, in pixels. |

---

### `M_ANCESTOR_ID`

Inquires the Aurora Imaging Library identifier of the ancestor buffer. Only child buffers have an ancestor buffer. The ancestor buffer is the buffer from which the specified buffer ([`BufId`](../../Reference/buf/MbufInquire.md)) ultimately originated. It is the root buffer; it does not have a parent buffer (it is not a child buffer of another buffer). Note that the identifier of the specified buffer is returned if it does not have an ancestor buffer.  To establish the parent buffer of the specified buffer, use [`M_PARENT_ID`](../../Reference/buf/MbufInquire.md) instead.

| Value | Description |
| --- | --- |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of the ancestor buffer. |

---

### `M_ANCESTOR_OFFSET_BAND`

Inquires the band offset relative to the ancestor buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the band offset. |

---

### `M_ANCESTOR_OFFSET_BIT`

Inquires the bit offset relative to the ancestor buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the offset, in bits. |

---

### `M_ANCESTOR_OFFSET_X`

Inquires the X-offset relative to the ancestor buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset. |

---

### `M_ANCESTOR_OFFSET_Y`

Inquires the Y-offset relative to the ancestor buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset. |

---

### `M_BITMAPINFO`

Inquires a pointer (LPBITMAPINFO) to the header of the DIB associated with the Aurora Imaging Library buffer.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that there is no DIB associated with the Aurora Imaging Library buffer. |
| `Value` | Specifies the pointer to the DIB header. |

---

### `M_CAMERA_CONTAINER_ID`

**Board availability:** GenTL, GevIQ, GigE Vision, Rapixo CXP, Rapixo CoF, V4L2

Inquires the GenDC container identifier from the camera. The camera sends the identifier with each transmitted frame.

| Value | Description |
| --- | --- |
| `0` | Specifies that GenDC container identifiers are not supported by the camera. |
| `Value > 0` | Specifies the GenDC container identifier. |

---

### `M_CAMERA_FRAME_ID`

**Board availability:** GenTL, GevIQ, GigE Vision, USB3 Vision, V4L2

Inquires the frame identifier from the camera. The camera sends the identifier with each transmitted frame.

| Value | Description |
| --- | --- |
| `0` | Specifies that frame identifiers are not supported by the camera. |
| `Value > 0` | Specifies the frame identifier. |

---

### `M_CAMERA_TIME_STAMP`

Inquires the time stamp from the camera expressed in seconds.

| Value | Description |
| --- | --- |
| `0.0` | Specifies that this time stamp is not supported by the camera. |
| `Value > 0.0` | Specifies the time stamp, in seconds. |

---

### `M_CAMERA_TIME_STAMP_NS`

Inquires time stamp from the camera expressed in nanoseconds.

| Value | Description |
| --- | --- |
| `0` | Specifies that this time stamp is not supported by the camera. |
| `Value > 0` | Specifies the time stamp, in nanoseconds. |

---

### `M_COMPONENT_TIME_STAMP`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the time stamp given by the camera GenDC stream.

| Value | Description |
| --- | --- |
| `0.0` | Specifies that this time stamp is not supported by the camera. |
| `Value > 0.0` | Specifies the time stamp, in seconds. |

---

### `M_COMPONENT_TIME_STAMP_IS_PTP`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires if [`M_COMPONENT_TIME_STAMP_NS`](../../Reference/buf/MbufInquire.md) is relative to the Unix epoch (January 1st 1970 00:00:00).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that [`M_COMPONENT_TIME_STAMP_NS`](../../Reference/buf/MbufInquire.md) is not calculated based on the Unix epoch (January 1st 1970 00:00:00). |
| `M_TRUE` | Specifies that [`M_COMPONENT_TIME_STAMP_NS`](../../Reference/buf/MbufInquire.md) is relative to the Unix epoch (January 1st 1970 00:00:00). |

---

### `M_COMPONENT_TIME_STAMP_NS`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquiries the time stamp given by the camera GenDC stream in nanoseconds.

| Value | Description |
| --- | --- |
| `0` | Specifies that this time stamp is not supported by the camera. |
| `Value > 0` | Specifies the time stamp, in nanoseconds. |

---

### `M_DATA_FORMAT`

Inquires the data storage format of the buffer. This is the color or monochrome storage format of the buffer and includes whether it is in packed or planar format. Note that non-color buffers are stored in planar format and lack a color space.  To retrieve the entire list of attributes, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquire.md) or [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquire.md).

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |

---

### `M_DATA_TYPE`

Inquires the buffer data type.

| Value | Description |
| --- | --- |
| `M_FLOAT` | Specifies that the buffer uses the float data type. |
| `M_SIGNED` | Specifies that the buffer uses the signed data type. |
| `M_UNSIGNED` | Specifies that the buffer uses the unsigned data type. |

---

### `M_DC_HANDLE`

Inquires the device context handle (HDC) of the buffer. The buffer device context must have been successfully allocated using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DC_ALLOC`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no device context is associated with the buffer. |
| `Value` | Specifies the handle of the device context that is associated with the buffer. |

---

### `M_DIB_HANDLE`

Inquires the handle (_HBITMAP_) of the DIB associated with the Aurora Imaging Library buffer. To ensure that the buffer has a DIB handle, the buffer must have been successfully allocated using with [`M_DIB`](../../Reference/buf/MbufAlloc2d.md)+[`M_GDI`](../../Reference/buf/MbufAlloc2d.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no DIB handle is associated with the buffer. |
| `Value` | Specifies the handle of the DIB associated with the buffer. |

---

### `M_EXTENDED_ATTRIBUTE`

Inquires the attributes of the specified buffer. This inquire type only returns attributes that were explicitly set; any attribute left to its default is not returned.  To retrieve only the data storage format of the buffer, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_DATA_FORMAT`](../../Reference/buf/MbufInquire.md). To retrieve the detailed storage format of the specified buffer, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquire.md).  > **Note:** Note that you cannot set the [`UserVarPtr`](../../Reference/buf/MbufInquire.md) parameter to [`M_NULL`](../../Reference/buf/MbufInquire.md) when [`InquireType`](../../Reference/buf/MbufInquire.md) is set to [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquire.md). In addition, a valid _AIL_INT64_ pointer must be passed to the function; otherwise, an error will occur.

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_DIB` | Forces the buffer to be a DIB buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory. |
| `M_ON_BOARD` | Forces the buffer in on-board memory. |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank. |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible. |
| `M_PAGED` | Forces the buffer in pageable memory. |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar standard. |
| `M_YUV12` | Specifies YUV12 planar standard. |
| `M_YUV24` | Specifies YUV24 planar standard. |
| `M_BGR24` | Specifies 24-bit color depth (BGR) packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth (BGR) packed pixels (BGRXBGRX). |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 packed or planar (4:2:2) standard. |

---

### `M_EXTENDED_ATTRIBUTE_NAME`

Inquires the [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquire.md) of the buffer, expressed in a human readable format.

---

### `M_EXTENDED_FORMAT`

Inquires the detailed storage format of the specified buffer.  When you inquire [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquire.md), the returned value includes any buffer attributes that you set, any setting that was left to its default, and values that were internally calculated by Aurora Imaging Library. To retrieve only the data storage format of the buffer that you set, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_DATA_FORMAT`](../../Reference/buf/MbufInquire.md). To retrieve all the attributes of the buffer as set using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`Attribute`](../../Reference/buf/MbufAlloc1d.md), use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_EXTENDED_ATTRIBUTE`](../../Reference/buf/MbufInquire.md).  You can determine generally whether the buffer is in an RGB/BGR, YUV, or monochrome storage format using the buffer's extended format with the **M_IS_FORMAT_RGB_BGR**,**M_IS_FORMAT_YUV**, or **M_IS_FORMAT_MONO** macro respectively.  > **Note:** Note that you cannot set the [`UserVarPtr`](../../Reference/buf/MbufInquire.md) parameter to [`M_NULL`](../../Reference/buf/MbufInquire.md) when [`InquireType`](../../Reference/buf/MbufInquire.md) is set to [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquire.md). In addition, a valid _AIL_INT64_ pointer must be passed to the function; otherwise, an error will occur.  > **Note:** Some buffer attributes are not returned when using a remote computer because they are not applicable to a remote computer. These attributes include, but are not limited to, [`M_LINUX_MXIMAGE`](../../Reference/buf/MbufInquire.md), [`M_DIB`](../../Reference/buf/MbufInquire.md), [`M_GDI`](../../Reference/buf/MbufInquire.md), [`M_HOST_MEMORY`](../../Reference/buf/MbufInquire.md).

#### System specific

| Board(s) | Note |
|---|---|
| GenTL, V4L2 | Note that this inquire type only returns attributes that were explicitly set; any attribute left to its default value is not returned. |

| Value | Description |
| --- | --- |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_KERNEL` | Specifies a kernel buffer to store a custom filter for convolution functions. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_STRUCT_ELEMENT` | Specifies a buffer to store structuring element data for morphology functions. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_DIB` | Forces the buffer to be a DIB buffer. |
| `M_GDI` | Forces the buffer to be compatible with GDI. |
| `M_LINUX_MXIMAGE` | Forces the buffer to be an X11 Ximage. |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. |
| `M_HOST_MEMORY` *(default)* | Forces the buffer in Host memory. |
| `M_OFF_BOARD` | Ensures that the buffer is not in on-board memory. |
| `M_ON_BOARD` | Forces the buffer in on-board memory. |
| `M_MEMORY_BANK_n` | Forces the buffer to be allocated in the specified memory bank. |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. |
| `M_NON_PAGED` | Forces the buffer in Aurora Imaging Library reserved, non-pageable memory, if possible. |
| `M_PAGED` | Forces the buffer in pageable memory. |
| `M_ARRAY` | Specifies a buffer to store array type data. |
| `M_IMAGE` | Specifies a buffer to store image data. |
| `M_LUT` | Specifies a buffer to store lookup table data. |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar standard. |
| `M_YUV12` | Specifies YUV12 planar standard. |
| `M_YUV24` | Specifies YUV24 planar standard. |
| `M_BGR24` | Specifies 24-bit color depth (BGR) packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth (BGR) packed pixels (BGRXBGRX). |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 packed or planar (4:2:2) standard. |

---

### `M_EXTENDED_FORMAT_NAME`

Inquires the [`M_EXTENDED_FORMAT`](../../Reference/buf/MbufInquire.md) of the buffer, expressed in a human readable format.

---

### `M_GRAB_TIME_STAMP`

Inquires the time stamp of when the grab operation completed, in seconds.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the time stamp of when the grab operation completed, in seconds. |

---

### `M_GRAB_TIME_STAMP_NS`

Inquires the time stamp of when the grab operation completed, in nanoseconds.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the time stamp of when the grab operation completed, in nanoseconds. |

---

### `M_HOST_ADDRESS`

Inquires the Host address of the buffer, if the buffer is visible from the Host address space and is not a planar multi-band buffer. For a planar multi-band buffer, use [`M_HOST_ADDRESS_BAND + n`](../../Reference/buf/MbufInquire.md)instead.  If available, this address can be used to directly access the data of an Aurora Imaging Library buffer with the Host CPU.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the buffer is not visible from the Host address space or the buffer is a planar multi-band buffer. |
| `Value` | Specifies the Host address of the buffer. |

---

### `M_HOST_ADDRESS_BAND + n`

Inquires the Host address for the band of the buffer identified by _n_, if the buffer is visible from the Host address space and is a planar multi-band buffer. For any other buffer, use [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md)instead.  If available, this address can be used to directly access the data of an Aurora Imaging Library buffer with the Host CPU.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the buffer is not visible from the Host address space or the buffer is not a planar multi-band buffer. |
| `Value` | Specifies the Host address of the specified band of the buffer. |

---

### `M_MAX`

Inquires the maximum pixel value possible in the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected maximum pixel value within the range of the buffer type. |

---

### `M_MIN`

Inquires the minimum pixel value possible in the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected minimum pixel value within the range of the buffer type. |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter of the image buffer. When the image buffer is allocated, the modification counter is initialized to a random non-zero value. The modification counter is incremented each time the image buffer is modified by an Aurora Imaging Library function.  If the image buffer is accessed externally, for example, when using [`MbufCreateColor`](../../Reference/buf/MbufCreateColor.md) or [`MbufCreate2d`](../../Reference/buf/MbufCreate2d.md), [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MODIFIED`](../../Reference/buf/MbufControl.md) must be called to indicate that the image buffer's contents have been modified. Calling this function will increment the counter.  This feature is useful for optimization. For example, you can avoid repeating certain computations (for example, analysis computations) if you know that the image buffer has not been modified. In this case, inquire the count before the first computation in the sequence of computations, and then inquire it again before repeating the same sequence. If no modifications have been made to the image buffer, you can avoid repeating the sequence unnecessarily.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

---

### `M_MODIFICATION_HOOK`

Inquires the status of the modification hook, which runs a user-defined function upon an event. These user-defined functions are initially hooked to the buffer modification event using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the user-defined functions should not be called. |
| `M_ENABLE` *(default)* | Specifies that the user-defined functions should be called. |

---

### `M_NB_CHILD`

Inquires the number of child buffers associated with the specified buffer ([`BufId`](../../Reference/buf/MbufInquire.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of child buffers. |

---

### `M_OWNER_CONTAINER_ID`

Inquires the Aurora Imaging Library identifier of the container in which this buffer is a component.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the buffer is not a component of a container. |
| `ContainerID` | Specifies the identifier of the container of which this buffer is a component. |

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_OWNER_SYSTEM_TYPE`

**Board availability:** Clarity UHD, GenTL, GevIQ, GigE Vision, Host System, Iris GTX, Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF, USB3 Vision, V4L2

Inquires the type of system on which the buffer was allocated.

| Value | Description |
| --- | --- |
| `M_SYSTEM_CLARITY_UHD_TYPE` | Specifies an Aurora Imaging Library Clarity UHD system. |
| `M_SYSTEM_CONCORD_POE_TYPE` | Specifies an Aurora Imaging Library Concord POE system. |
| `M_SYSTEM_GENTL_TYPE` | Specifies an Aurora Imaging Library GenTL system. |
| `M_SYSTEM_GEVIQ_TYPE` | Specifies an Aurora Imaging Library GevIQ system. |
| `M_SYSTEM_GIGE_VISION_TYPE` | Specifies an Aurora Imaging Library GigE Vision system. |
| `M_SYSTEM_HOST_TYPE` | Specifies the Host. |
| `M_SYSTEM_INDIO_TYPE` | Specifies an Aurora Imaging Library Indio system. |
| `M_SYSTEM_IRIS_GTX_TYPE` | Specifies an Aurora Imaging Library Iris GTX system. |
| `M_SYSTEM_RADIENTEVCL_TYPE` | Specifies an Aurora Imaging Library Radient eV-CL system. |
| `M_SYSTEM_RAPIXOCL_TYPE` | Specifies an Aurora Imaging Library Rapixo Pro CL system. |
| `M_SYSTEM_RAPIXOCOF_TYPE` | Specifies an Aurora Imaging Library Rapixo CoF system. |
| `M_SYSTEM_RAPIXOCXP_TYPE` | Specifies an Aurora Imaging Library Rapixo CXP system. |
| `M_SYSTEM_USB3_VISION_TYPE` | Specifies an Aurora Imaging Library USB3 Vision system. |
| `M_SYSTEM_V4L2_TYPE` | Specifies an Aurora Imaging Library Video4Linux2 system. |

---

### `M_PARENT_ID`

Inquires the Aurora Imaging Library identifier of the parent buffer. Only child buffers have a parent buffer. The parent buffer is the buffer from which the specified buffer ([`BufId`](../../Reference/buf/MbufInquire.md)) was defined. The parent buffer can itself have a parent buffer. If the specified buffer has no parent buffer, the identifier of the specified buffer is returned.  To establish the ancestor buffer (root buffer) of the specified buffer, use [`M_ANCESTOR_ID`](../../Reference/buf/MbufInquire.md) instead.

| Value | Description |
| --- | --- |
| `Buffer identifier` | Specifies the Aurora Imaging Library identifier of the parent buffer. |

---

### `M_PARENT_OFFSET_BAND`

Inquires the band offset relative to the parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the band offset. |

---

### `M_PARENT_OFFSET_X`

Inquires the X-offset relative to the parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-offset. |

---

### `M_PARENT_OFFSET_Y`

Inquires the Y-offset relative to the parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-offset. |

---

### `M_PHYSICAL_ADDRESS`

Inquires the physical address of the buffer if it is not a planar multi-band buffer. For a planar multi-band buffer, you can determine its physical address by allocating a child buffer for the required band and then using [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) to determine its physical address.  This type of address is available only for a non-paged buffer mapped to the Host or for a buffer allocated in a frame grabber's on-board memory. This type of address is used mostly for access by DMA capable devices other than the Host CPU.  To use this inquire type, the buffer must not have been allocated on a Distributed Aurora Imaging Library remote system. If it is, use [`M_PHYSICAL_ADDRESS_REMOTE`](../../Reference/buf/MbufInquire.md) instead.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the buffer is not visible from the physical address space. |
| `Value` | Specifies the address of the buffer. |

---

### `M_PHYSICAL_ADDRESS_REMOTE`

Inquires the physical address of the buffer on a Distributed Aurora Imaging Library remote system, if it is not a planar multi-band buffer. For a planar multi-band buffer, you can determine its physical address by allocating a child buffer for the required band and then using [`M_PHYSICAL_ADDRESS_REMOTE`](../../Reference/buf/MbufInquire.md) to determine its physical address.  This type of address is available only for a non-paged buffer mapped to the remote system. This type of address is used mostly for access by DMA capable devices other than the Host CPU.  To use this inquire type, the buffer must have been allocated on a Distributed Aurora Imaging Library remote system. If it is not, use [`M_PHYSICAL_ADDRESS`](../../Reference/buf/MbufInquire.md) instead.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the buffer is not visible from the physical address space. |
| `Value` | Specifies the address of the buffer. |

---

### `M_PITCH`

Inquires the number of pixels between the beginnings of any two adjacent lines of the buffer data.

| Value | Description |
| --- | --- |
| `Value` | Specifies the pitch, in pixels. |

---

### `M_PITCH_BYTE`

Inquires the number of bytes between the beginnings of any two adjacent lines of the buffer data.

| Value | Description |
| --- | --- |
| `Value` | Specifies the pitch, in bytes. |

---

### `M_SIZE_BAND`

Inquires the number of buffer bands.

| Value | Description |
| --- | --- |
| `1 <= Value <= 1024` | Specifies the number of bands. |

---

### `M_SIZE_BIT`

Inquires the depth per band.

| Value | Description |
| --- | --- |
| `Value` | Specifies the depth per band, in bits. |

---

### `M_SIZE_X`

Inquires the width of the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width of the buffer, in pixels. |

---

### `M_SIZE_Y`

Inquires the height of the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height of the buffer, in pixels. |

---

### `M_SURFACE_HANDLE`

Inquires the Cairo surface handle of the buffer. The buffer's Cairo surface must have been successfully allocated using [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_SURFACE_ALLOC`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that no Cairo surface is associated with the buffer. |
| `Value` | Specifies the handle of the Cairo surface that is associated with the buffer. |

---

### `M_SYSTEM_LOCATION`

Inquires whether the buffer is allocated on a local system or a Distributed Aurora Imaging Library remote system.

| Value | Description |
| --- | --- |
| `M_LOCAL` | Specifies that the buffer is allocated on a local system. |
| `M_REMOTE` | Specifies that the buffer is allocated on a remote system. |

---

### `M_TYPE`

Inquires the buffer data type and depth. Depth is returned in bits.

| Value | Description |
| --- | --- |
| `M_FLOAT + Depth value` | Specifies the data depth and that the data type is floating-point. |
| `M_SIGNED + Depth value` | Specifies the data depth and that the data type is signed. |
| `M_UNSIGNED + Depth value` | Specifies the data depth and that the data type is unsigned. |

---

### `M_YCBCR_RANGE`

Inquires whether pixel values are limited to a signal's YCbCr range.

#### System specific

| Board(s) | Note |
|---|---|
| Clarity UHD | When grabbing into the specified buffer from a source that transmits in a YCbCr format (for example, an SDI source), the data is not converted to match the encoding that you specify. Instead, this control type is automatically changed to match the encoding of the grabbed data. |

| Value | Description |
| --- | --- |
| `M_DISABLE` *(default)* | Specifies not to encode the YUV buffer's pixel values in YCbCr. |
| `M_YCBCR_HD` | Specifies to encode the YUV buffer's pixel values using the high-definition YCbCr standard. |
| `M_YCBCR_SD` | Specifies to encode the YUV buffer's pixel values using the standard-definition YCbCr standard. |
| `M_YCBCR_UHD` | Specifies to encode the YUV buffer's pixel values using the ultra-high-definition YCbCr standard. |

### Combination Constants — Returns the location of the buffer

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the location of the buffer.

| Value | Description |
| --- | --- |
| `M_HOST_MEMORY` *(default)* | Specifies that the buffer
                           is in Host memory. |
| `M_OFF_BOARD` | Specifies that the buffer is not in on-board memory. |
| `M_ON_BOARD` | Specifies that the buffer
                           is in on-board memory. |

### Combination Constants — Returns whether the buffer was allocated in paged or non-paged memory

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer was allocated in paged or non-paged memory.

| Value | Description |
| --- | --- |
| `M_NON_PAGED` | Specifies that the buffer
                           is in Aurora Imaging Library reserved, non-pageable memory, if possible. |
| `M_PAGED` | Specifies that the buffer
                           is in pageable memory. |

### For

For [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) and [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) data buffers only (see [`MbufControl`](../../Reference/buf/MbufControl.md) for possible values), you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_OFFSET_CENTER_X`

Inquires the X-coordinate of the center of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the X-coordinate of the top-left pixel of the central elements. |
| `0 <= Value < SizeX` | Specifies the value of the X-coordinate. |

---

### `M_OFFSET_CENTER_Y`

Inquires the Y-coordinate of the center of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the Y-coordinate of the top-left pixel of the central elements. |
| `0 <= Value < SizeY` | Specifies the value of the Y-coordinate. |

---

### `M_OVERSCAN`

Inquires the overscan type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that Aurora Imaging Library automatically selects the type of overscan to optimize speed and logic according to the specified operation and the target system. |
| `M_DISABLE` | Specifies that no overscan will be used, unless processing the border pixels is faster than ignoring them; in the latter case, Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified operation and the target system. |
| `M_FAST` | Specifies that Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified neighborhood operation and the target system. |
| `M_MIRROR` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that mirror the source buffer pixel values. |
| `M_REPLACE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values set to the overscan replacement value ([`M_OVERSCAN_REPLACE_VALUE`](../../Reference/buf/MbufControl.md)). |
| `M_REPLICATE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that replicate the border pixels. |
| `M_TRANSPARENT` | Specifies a type of overscan that processes the border pixels of a source image using transparent overscan pixel values. |

---

### `M_OVERSCAN_REPLACE_VALUE`

Inquires the replacement value for the overscan pixel values.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REPLACE_MAX` | Specifies that the overscan neighborhood pixel values will be set to the maximum value of the source buffer. |
| `M_REPLACE_MIN` | Specifies that the overscan neighborhood pixel values will be set to the minimum value of the source buffer. |
| `Value` *(default)* | Specifies the value of the overscan neighborhood pixels. |

---

### `M_SATURATION`

Inquires whether results are saturated.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to saturate results, except when Aurora Imaging Library can take advantage of optimization routines to accelerate the processing. |
| `M_ENABLE` | Specifies to saturate results. |

### For

For [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) data buffers only (see [`MbufControl`](../../Reference/buf/MbufControl.md) for possible values), you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_ABSOLUTE_VALUE`

Inquires whether the absolute value should be taken.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to take the absolute value of the result. |
| `M_ENABLE` | Specifies to take the absolute value of the result. |

---

### `M_NORMALIZATION_FACTOR`

Inquires the normalization factor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the normalization factor. |

### For

For [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) data buffers only, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_ITER_OVERSCAN`

Inquires the type of overscan used to handle overscan operations with iterations for structuring elements.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL` | Specifies that for overscan operations with multiple iterations, to use a size equivalent to all iterations. |
| `M_PER_ITER` *(default)* | Specifies that for overscan operations with multiple iterations, each iteration will be according to the structuring element's size and center pixel. |

---

### `M_NUMBER_OF_ELEMENT_VALUES_VALID`

Inquires the number of valid structuring element values in the structuring element. This is the number of values that were not set to **M_DONT_CARE**.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of valid structuring element values. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAlloc2d.md) data buffers only, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_ASSOCIATED_LUT`

Inquires the identifier of the LUT buffer associated with the image buffer, if there is one.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that no LUT buffer is associated with the image buffer. |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library identifier of the LUT buffer that is associated with the image buffer. |

---

### `M_DISPLAY_SIZE_BAND`

Inquires the number of bands used for display.

| Value | Description |
| --- | --- |
| `0` | Specifies that no bands are used for display; the buffer is not a 1-band or 3-band buffer and the display bands were not set ([`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_DISPLAY_BANDS`](../../Reference/buf/MbufControl.md)). |
| `1` | Specifies that 1 band is used for display. |
| `3` | Specifies that 3 bands are used for display. |

---

### `M_INFORMATION_TYPE`

Inquires the GenDC part type of the buffer.

| Value | Description |
| --- | --- |
| `M_INFO_1D` | Specifies  the GenDC part type is 1D. |
| `M_INFO_2D` | Specifies  the GenDC part type is 2D. |
| `M_INFO_2D_H264` | Specifies  the GenDC part type is H.264 data. |
| `M_INFO_2D_JPEG` | Specifies  the GenDC part type is JPEG data. |
| `M_INFO_2D_JPEG2000` | Specifies  the GenDC part type is JPEG 2000 data. |
| `M_INFO_GENICAM_CHUNK` | Specifies  the GenDC part type is GenICam chunk data. |
| `M_INFO_GENICAM_XML` | Specifies  the GenDC part type is GenICam XML data. |

---

### `M_INFORMATION_TYPE_NAME`

Inquires the GenDC part type of the buffer as a string.

---

### `M_PFNC_NAME`

Inquires the name of the buffer's pixel format, as defined by the GenICam Pixel Format Naming Convention.

---

### `M_PFNC_SIZE_BIT`

Inquires the number of bits per pixel, as per the GenICam Pixel Format Naming Convention.  > **Note:** Note that this is the total number of bits used to store each pixel across all bands. For example, if the buffer has 3 8-bit bands that store RGB information, this value will be 24.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of bit per pixel as per the PFNC. |

---

### `M_PFNC_SUPPORT`

Inquires whether Aurora Imaging Library supports processing the buffer's given it's pixel format. Note that this setting will only return [`M_NO`](../../Reference/buf/MbufInquire.md)if [`M_PFNC_VALUE`](../../Reference/buf/MbufInquire.md)was set to an unsupported value during a grab.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that image processing is not supported for the buffer's pixel format. An error will be generated when you attempt to use an Aurora Imaging Library processing function with the buffer. |
| `M_WITH_COMPENSATION` | Specifies that Aurora Imaging Library will process the buffer by copying its data to a temporary buffer with a supported pixel format. |
| `M_YES` | Specifies that the buffer will be processed as normal. |

---

### `M_PFNC_VALUE`

Inquires the pixel format of the buffer, as defined by the GenICam Pixel Format Naming Convention. This value is either set by the digitizer when grabbing into the buffer, or is determined based on the [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquire.md)of the buffer and the attributes specified when the buffer was allocated.  > **Note:** If this value is determined from the attributes of the buffer, and there is no ratified format in the PFNC corresponding to those attributes, the value will be 0.

| Value | Description |
| --- | --- |
| `PFNCvalue` | Specifies the pixel format of the buffer using a value from PFNC (as defined in _pfnc.h_). |

---

### `M_REGION_ACTIVE`

Inquires whether the image buffer has an active ROI. The ROI associated with a buffer is active when the buffer has [`M_REGION_USE`](../../Reference/buf/MbufInquire.md) set to [`M_USE`](../../Reference/buf/MbufInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the image buffer does not have an active ROI. |
| `M_TRUE` | Specifies that the image buffer has an active ROI. |

---

### `M_REGION_LINK`

Inquires whether the image buffer is a child buffer with an ROI linked to that of its parent. A child buffer's ROI is linked by default, or using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)with [`M_LINK_TO_PARENT`](../../Reference/buf/MbufSetRegion.md).  > **Note:** Note, it is possible for the ROI of the child buffer to be linked even if there is no ROI currently defined for the parent buffer. In this case, setting the ROI for the parent buffer will automatically set the ROI for the child buffer.

| Value | Description |
| --- | --- |
| `M_LINK_TO_PARENT` | Specifies that the image buffer is a child buffer with an ROI linked to that of its parent. |
| `M_NONE` | Specifies that the image buffer is not a child buffer, or that its ROI is not linked to that of its parent. |

---

### `M_REGION_TYPE`

Inquires whether the image buffer is associated with ROI information, and, if so, in which format the ROI is saved. ROIs are set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

| Value | Description |
| --- | --- |
| `M_NONE` | Specifies that no ROI is defined. |
| `M_RASTER` | Specifies that the ROI is in raster format. |
| `M_VECTOR` | Specifies that the ROI is in vector format. |
| `M_VECTOR_AND_RASTER` | Specifies that the ROI is in raster and vector format. |

---

### `M_REGION_USE`

Inquires whether the ROI associated with the buffer is used with supported image processing operations.

| Value | Description |
| --- | --- |
| `M_IGNORE` |  |
| `M_USE` *(default)* |  |

---

### `M_RESOLUTION_X`

Inquires the X resolution of the image buffer in pixels per inch (PPI). If [`M_RESOLUTION_X`](../../Reference/buf/MbufInquire.md)in [`MbufControl`](../../Reference/buf/MbufControl.md)is not set, the inquire returns 0.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the X resolution in PPI. |

---

### `M_RESOLUTION_Y`

Inquires the Y resolution of the image buffer in pixels per inch (PPI). If [`M_RESOLUTION_Y`](../../Reference/buf/MbufInquire.md)in [`MbufControl`](../../Reference/buf/MbufControl.md)is not set, the inquire returns 0.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the Y resolution in PPI. |

### Combination Constants — Returns the intended purpose of the image buffer

> *Essential.*

> **Usage:** You must add one or more of the following values to the above-mentioned values to determine the intended purpose of the buffer.

| Value | Description |
| --- | --- |
| `M_COMPRESS` | Specifies an image buffer that can hold compressed data. |
| `M_DISP` | Specifies an image buffer that can be displayed. |
| `M_GRAB` | Specifies an image buffer in which to grab data. |
| `M_PROC` | Specifies an image buffer that can be processed. |

### Combination Constants — Returns the compression type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the compression type.

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |

### Combination Constants — Returns whether the buffer was allocated with an overscan region

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer was allocated with an overscan region.

| Value | Description |
| --- | --- |
| `M_ALLOCATION_OVERSCAN` | Specifies that the buffer is allocated with an overscan region. |

### Combination Constants — Returns the storage format and location specifier

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the storage format and location specifier.

You might have set this value, or it could have been automatically selected by Aurora Imaging Library.

| Value | Description |
| --- | --- |
| `M_DIB` | Specifies that the buffer
                           is a DIB buffer. |
| `M_GDI` | Specifies that the buffer
                           is compatible with GDI. |
| `M_LINUX_MXIMAGE` | Specifies that the buffer
                           is an X11 Ximage. |

### Combination Constants — Returns whether the buffer is FPGA accessible

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the buffer is FPGA accessible.

| Value | Description |
| --- | --- |
| `M_FPGA_ACCESSIBLE` | Forces the buffer to be allocated in a bank of memory that is accessible from the Processing FPGA. |

### Combination Constants — Returns the memory bank used

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the memory bank in which the buffer was allocated.

| Value | Description |
| --- | --- |
| `M_MEMORY_BANK_n` | Inquires the buffer allocated in the specified memory bank. |

### Combination Constants — For specifying a location in a specific type of memory

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine if the buffer is in a specific type of memory.

> **Board availability:** Radient eV-CL, Rapixo CL, Rapixo CXP, Rapixo CoF

| Value | Description |
| --- | --- |
| `M_SHARED` | Specifies that the buffer is in shared processing memory. |

### Combination Constants — Returns the format in which image buffers were stored

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the format in which image buffers were stored.

| Value | Description |
| --- | --- |
| `M_PACKED` | Specifies that the buffer's bands are stored in packed format; that is, each pixel's bands are stored together (RGB RGB RGB...). |
| `M_PLANAR` | Specifies that the buffer's bands are stored in planar format; that is, each pixel's bands are stored in separate planes. |

### Combination Constants — Returns the packed or planar color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the packed or planar color buffer format.

| Value | Description |
| --- | --- |
| `M_RGB24` | Specifies 24-bit color depth (RGB 8:8:8) packed or planar pixels. |
| `M_RGB48` | Specifies 48-bit color depth (RGB 16:16:16). |
| `M_RGB96` | Specifies 96-bit color depth (RGB 32:32:32) packed or planar pixels. |
| `M_YUV16` | Specifies YUV16 (4:2:2) pixels. |

### Combination Constants — Returns the packed color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the packed color buffer format.

| Value | Description |
| --- | --- |
| `M_BGR24` | Specifies 24-bit color depth packed pixels (BGRBGR). |
| `M_BGR32` | Specifies 32-bit color depth packed pixels (BGRXBGRX). |
| `M_RGB15` | Specifies 16-bit color depth packed pixels (XRGB 1:5:5:5). |
| `M_RGB16` | Specifies 16-bit color depth packed pixels (RGB 5:6:5). |
| `M_YUV16_UYVY` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the UYVY order. |
| `M_YUV16_YUYV` | Specifies YUV16 packed (4:2:2) pixels, whereby the bands of each pixel are stored in the YUYV order. |

### Combination Constants — Returns the planar color buffer format

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the planar color buffer format.

| Value | Description |
| --- | --- |
| `M_RGB3` | Specifies 3-bit color depth (RGB 1:1:1) planar pixels. |
| `M_YUV9` | Specifies YUV9 planar pixels. |
| `M_YUV12` | Specifies YUV12 planar pixels. |
| `M_YUV24` | Specifies YUV24 planar pixels. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers (see [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) for possible values), you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_COMPRESSED_DATA_SIZE_BYTE`

Inquires the size of a compressed buffer, in bytes.  Use [`M_TARGET_SIZE`](../../Reference/buf/MbufInquire.md) to establish the requested size, in bytes.

| Value | Description |
| --- | --- |
| `Value` | Specifies the size of a compressed buffer, in bytes. |

---

### `M_COMPRESSION_TYPE`

Inquires the type of compression.

| Value | Description |
| --- | --- |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |
| `M_JPEG2000_LOSSLESS` | Specifies that the buffer will be used to hold JPEG2000 lossless data. |
| `M_JPEG2000_LOSSY` | Specifies that the buffer will be used to hold JPEG2000 lossy data. |
| `M_JPEG_LOSSLESS` | Specifies that the buffer will be used to hold JPEG lossless data. |
| `M_JPEG_LOSSLESS_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossless data in separate fields. |
| `M_JPEG_LOSSY` *(default)* | Specifies that the buffer will be used to hold JPEG lossy data. |
| `M_JPEG_LOSSY_INTERLACED` | Specifies that the buffer will be used to hold JPEG lossy data in separate fields. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG_LOSSY`](../../Reference/buf/MbufAllocColor.md), [`M_JPEG_LOSSY_INTERLACED`](../../Reference/buf/MbufAllocColor.md), or [`M_JPEG2000_LOSSY`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_Q_FACTOR`

Inquires the quantization factor for both JPEG2000 lossy and JPEG lossy buffers. Note that for 3-band buffers, only the quantization factor associated with the first band is returned.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the quantization factor. |

---

### `M_Q_FACTOR_CHROMINANCE`

Inquires the quantization factor of the U and V bands for both JPEG2000 lossy and JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the factor. |

---

### `M_Q_FACTOR_LUMINANCE`

Inquires the quantization factor of the Y band for both JPEG2000 lossy and JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the factor. |

---

### `M_QUANTIZATION`

Inquires the identifier of the array buffer containing the quantization table (for a JPEG lossy or JPEG2000 lossy buffer), which is associated with the image buffer. Note that for 3-band buffers, only the identifier of the array buffer associated with the first band is returned.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_QUANTIZATION_CHROMINANCE`

Inquires the identifier of the array buffer containing the quantization table associated with the U and V bands, for both JPEG2000 lossy and JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_QUANTIZATION_LUMINANCE`

Inquires the identifier of the array buffer containing the quantization table associated with the Y band, for both JPEG2000 lossy and JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG2000_LOSSY`](../../Reference/buf/MbufAllocColor.md) or [`M_JPEG2000_LOSSLESS`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_DECOMPOSITION_LEVEL`

Inquires the number of iterations the discrete wavelet transform is applied to the image (for single-band images) or on the first band of the image (for 3-band images).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of iterations (decomposition levels). |

---

### `M_NUMBER_SUBBAND`

Inquires the number of sub-bands rendered from the discrete wavelet transform passed on an image (for 1-band images) or on the first band of a 3-band image.

| Value | Description |
| --- | --- |
| `Value` | Specifies the number of sub-bands. |

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set the band about which to inquire.

> **Note:** Note that when dealing with [`M_Q_FACTOR`](../../Reference/buf/MbufInquire.md) and [`M_QUANTIZATION`](../../Reference/buf/MbufInquire.md), the following combination constants are available only for use with JPEG2000 lossy buffers.

| Value | Description |
| --- | --- |
| `M_BLUE` | Applies the specified control value to the blue band only (for RGB buffers). |
| `M_GREEN` | Applies the specified control value to the green band only (for RGB buffers). |
| `M_RED` | Applies the specified control value to the red band only (for RGB buffers). |
| `M_U` | Applies the specified control value to the U band only (for YUV buffers). |
| `M_V` | Applies the specified control value to the V band only (for YUV buffers). |
| `M_Y` | Applies the specified control value to the Y band only (for YUV buffers). |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG2000_LOSSY`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to the following value.

---

### `M_TARGET_SIZE`

Inquires the requested size of the compressed buffer in bytes.  Use [`M_COMPRESSED_DATA_SIZE_BYTE`](../../Reference/buf/MbufInquire.md) to establish the real size of the buffer.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the size, in bytes. |
| `M_DEFAULT` | Specifies compression will be applied without a target image size. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG_LOSSY`](../../Reference/buf/MbufAllocColor.md), [`M_JPEG_LOSSY_INTERLACED`](../../Reference/buf/MbufAllocColor.md), [`M_JPEG_LOSSLESS`](../../Reference/buf/MbufAllocColor.md), or [`M_JPEG_LOSSLESS_INTERLACED`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_HUFFMAN_DC`

Inquires the identifier of the array buffer containing the DC Huffman table which is associated with the image buffer. For 3-band buffers, only the identifier of the array buffer associated with the first band is returned.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_DC_CHROMINANCE`

Inquires the identifier of the array buffer containing the DC Huffman table that is associated with the U and V bands of a JPEG lossy buffer in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_DC_LUMINANCE`

Inquires the identifier of the array buffer containing the DC Huffman table that is associated with the Y band of a JPEG lossy buffer in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG_LOSSLESS`](../../Reference/buf/MbufAllocColor.md) or [`M_JPEG_LOSSLESS_INTERLACED`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_PREDICTOR`

Inquires the type of predictor. This inquire type is supported for JPEG lossless buffers only.

| Value | Description |
| --- | --- |
| `0` | Specifies predictor #0 (no prediction). |
| `1` *(default)* | Specifies predictor #1 (the "pixel-to-the-left" predictor). |
| `2` | Specifies predictor #2 (the "pixel-above" predictor). |

---

### `M_RESTART_INTERVAL`

Inquires the number of lines between restart markers (for JPEG lossless buffers) or number of 8x8 blocks of data between restart markers (for JPEG lossy buffers).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies after how many rows (for JPEG lossless buffers) or 8x8 blocks (for JPEG lossy buffers) of data to place restart markers. |

### For

For [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) image buffers with an [`M_JPEG_LOSSY`](../../Reference/buf/MbufAllocColor.md) or [`M_JPEG_LOSSY_INTERLACED`](../../Reference/buf/MbufAllocColor.md) compression type, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values.

---

### `M_HUFFMAN_AC`

Inquires the identifier of the array buffer containing the AC Huffman table that is associated with the image buffer. For 3-band buffers, only the identifier of the array buffer associated with the first band is returned.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_AC_CHROMINANCE`

Inquires the identifier of the array buffer containing the AC Huffman table that is associated with the U and V bands of a JPEG buffer in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_AC_LUMINANCE`

Inquires the identifier of the array buffer containing the AC Huffman table that is associated with the Y band of a JPEG buffer in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

### For inquiring settings useful with buffers that are components

For all buffers, you can set the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter to one of the following values. These settings are primarily useful when working with a buffer that is a component of a container, or a buffer that will be copied to a component of a container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md).

---

### `M_COMPONENT_GROUP_ID`

Inquires the component group ID assigned to the buffer during acquisition. Refer to your camera manual to learn what component group IDs are assigned to transmitted components.

| Value | Description |
| --- | --- |
| `Value` | Specifies the component group ID of the buffer. |

---

### `M_COMPONENT_INVALID`

Inquires whether the information in the buffer was marked as invalid during acquisition.

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the information in the buffer has not been marked invalid by the camera. |
| `M_TRUE` | Specifies that the information in the buffer has been marked invalid by the camera. |

---

### `M_COMPONENT_REGION_ID`

Inquires the component region ID assigned to the buffer during acquisition. This identifies which region of the image sensor was used to generate the data in the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the component region ID of the buffer. |

---

### `M_COMPONENT_REGION_OFFSET_X`

Inquires the X-offset of the top left corner of the region of the image sensor used to generate the data in the buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the X-offset of the component region of the buffer, expressed in pixels. |

---

### `M_COMPONENT_REGION_OFFSET_Y`

Inquires the Y-offset of the top left corner of the region of the image sensor used to generate the data in the buffer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the Y-axis offset of the component region ID of the buffer, expressed in pixels. |

---

### `M_COMPONENT_SOURCE_ID`

Inquires the component source ID assigned to the buffer during acquisition. This indicates which of the camera's data sources was used to generate the data in the buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the component source ID of the buffer. |

---

### `M_COMPONENT_TYPE`

Inquires the component type of the buffer, used when the buffer is a component of a container. The component type specifies what kind of information is stored in the component.

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquire.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquire.md)component of the container. |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 255. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquire.md)component of the container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the buffer stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquire.md)component of the container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquire.md) component of the container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component contains information of an unknown type. |

---

### `M_COMPONENT_TYPE_NAME`

Inquires the name of the component type of the buffer.

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### For inquiring settings useful with

For image buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)attribute and with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufInquire.md) set to [`M_COMPONENT_RANGE`](../../Reference/buf/MbufInquire.md) or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufInquire.md), [`InquireType`](../../Reference/buf/MbufInquire.md) can also be set to one of the values below. These settings are primarily useful when working with a buffer that is a component of a container, or a buffer that will be copied as a component of a container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md).  These settings are used only when the buffer is the range or disparity component of a container passed as a source to[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md). Additionally, a container cannot be 3D-processable or 3D-displayable unless it has a single range component with all of these settings at their default values (except for [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufInquire.md) which can be any value and[`M_3D_REPRESENTATION`](../../Reference/buf/MbufInquire.md), which must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufInquire.md)or[`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufInquire.md)).

---

### `M_3D_ASPECT_RATIO`

Inquires by how much the focal length in the Y-direction stored in the buffer will be scaled in the destination point cloud to correct for non-square pixels in 3D reconstruction modes that rely on projective geometry.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies by how much the focal length the Y-direction will be scaled. |

---

### `M_3D_COORDINATE_SYSTEM_TYPE`

Inquires which type of coordinate system to use to interpret the coordinates stored in the bands of the buffer if it is a range component.  > **Note:** Note that Aurora Imaging Library does not support any functionality with coordinates stored using a coordinate system type other than [`M_CARTESIAN`](../../Reference/buf/MbufInquire.md). If a buffer stores information defined using another type of coordinate system, you will need to manually convert that information to cartesian coordinates before the buffer can be used with any Aurora Imaging Library processing function. This conversion cannot be done using[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CARTESIAN` *(default)* | Specifies that the buffer stores right-handed cartesian coordinates. |
| `M_CYLINDRICAL` | Specifies that the buffer stores cylindrical coordinates (theta-Y-rho). |
| `M_SPHERICAL` | Specifies that the buffer stores spherical coordinates (theta-phi-rho). |
| `M_UNKNOWN` | Specifies that the coordinate system type is unknown. |

---

### `M_3D_DISPARITY_BASELINE`

Inquires the stereo baseline value of the stereoscopic camera used to generate the data in the buffer. This is the physical distance between the lenses of the camera.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true physical distance between the lenses of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the stereo baseline value, expressed in meters. |

---

### `M_3D_DISTANCE_UNIT`

Inquires the unit to use when the buffer is part of a container and stores natively calibrated distance data.

| Value | Description |
| --- | --- |
| `M_INCH` | Specifies that the distance data is provided in inches. |
| `M_MILLIMETER` | Specifies that the distance data is provided in millimeters. |
| `M_PIXEL` | Specifies that the distance data is provided in pixels. |
| `M_UNKNOWN` *(default)* | Specifies that the distance unit is unknown. |

---

### `M_3D_FOCAL_LENGTH`

Inquires the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true focal length of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer, expressed in pixels. |

---

### `M_3D_INVALID_DATA_FLAG`

Inquires whether the buffer uses a specific value to indicate invalid data. For 3-band buffers that store coordinates, the invalid data flag should be stored in the Z-axis band. Specify the value that indicates invalid data using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the buffer does not use a special value to indicate invalid data. |
| `M_TRUE` | Specifies that the buffer uses a special value to indicate invalid data. |

---

### `M_3D_INVALID_DATA_VALUE`

Inquires the value used to indicate missing data when [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufInquire.md) is set to [`M_TRUE`](../../Reference/buf/MbufInquire.md).  > **Note:** Note that this value is not used or respected by any Aurora Imaging Library functions except for[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when the buffer is a component of the source container.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value used to indicate missing data. |

---

### `M_3D_OFFSET_X`

Inquires by how much the X-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Y`

Inquires by how much the Y-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Y-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Z`

Inquires by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset. |

---

### `M_3D_PRINCIPAL_POINT_X`

Inquires the X-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies X-position of the principal point, expressed in pixels. |

---

### `M_3D_PRINCIPAL_POINT_Y`

Inquires the Y-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies Y-position of the principal point, expressed in pixels. |

---

### `M_3D_REPRESENTATION`

Inquires how 3D data is stored in the buffer; this information is used when the buffer is a range or disparity component of a container. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CALIBRATED_XYZ` | Specifies that the component stores organized and natively calibrated X, Y, and Z-coordinates. |
| `M_CALIBRATED_XYZ_UNORGANIZED` | Specifies that the component stores unorganized and natively calibrated X, Y, and Z-coordinates. |
| `M_CALIBRATED_XZ_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates stored in a separate array buffer. |
| `M_CALIBRATED_XZ_UNIFORM_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates identified by the row index. |
| `M_CALIBRATED_Z` | Specifies that the component stores organized and natively calibrated Z-coordinates, without X and Y-coordinates. |
| `M_CALIBRATED_Z_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates without X-coordinates; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. |
| `M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates, with the X-coordinates identified by column index; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. |
| `M_CALIBRATED_Z_UNIFORM_XY` | Specifies that the component stores organized and natively calibrated Z-coordinates, with X and Y-coordinates identified by column and row index respectively. |
| `M_DISPARITY` | Specifies that the component stores a disparity map with perspective distortion along the Y-axis. |
| `M_DISPARITY_EXTERNAL_Y` | Specifies that the component stores a disparity map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. |
| `M_DISPARITY_UNIFORM_Y` | Specifies that the component stores a disparity map, with Y-values identified by row index. |
| `M_PROJECTED_Z` | Specifies that the component stores a projective map. |
| `M_PROJECTED_Z_EXTERNAL_Y` | Specifies that the component stores a projective map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. |
| `M_UNCALIBRATED_Z` | Specifies that the component stores organized and uncalibrated Z-coordinates, without X and Y-coordinates. |

---

### `M_3D_SCALE_X`

Inquires by how much the X-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Y`

Inquires by how much the Y-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Y-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Z`

Inquires by how much the Z-coordinates stored in the buffer will be scaled in the destination point cloud when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Z-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SHEAR_X`

Inquires by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row. |

---

### `M_3D_SHEAR_Z`

Inquires by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row when it is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row. |

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

> While an image buffer with an [`M_KERNEL`](../../Reference/buf/MbufAlloc1d.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc1d.md) attribute are available under Aurora Imaging Library Lite, these attributes are not required for the image buffer to be available to other Aurora Imaging Library Lite functions.

To convert the returned attributes, select the **Benchmarks and Utilities** item in the tree structure of the Aurora Imaging Configurator utility. Then, select the **Buffer format** item. On the **Value type** pane, paste the returned attributes in the text box provided. Then, click on the **Value lookup** button. The results of the translation are presented below the **Value lookup** button.

> **Note:** Due to clock differences, this time stamp should not be compared to the other available time stamps.
