---
doctype: Reference
module: buf
function: MbufControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufControl"
---

# MbufControl

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Yes |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Control a specified data buffer setting.

## Syntax

```c
void MbufControl(
    AIL_ID     BufId,        //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function allows you to control a specified data buffer setting.

## Parameters

### `BufId` *(out, AIL_ID)*

Specifies the identifier of the buffer to control.

### `ControlType` *(in, AIL_INT64)*

Specifies the buffer setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value needed for the setting.

## Parameter Associations

### For specifying general buffer settings

The following [`ControlType`](../../Reference/buf/MbufControl.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControl.md) parameter settings are used to control general buffer settings.

---

### `M_DC_ALLOC`

**Board availability:** GenTL, GevIQ, GigE Vision, Host System, USB3 Vision, V4L2

Allocates a device context (DC) for drawing. Determine the DC handle (HDC) using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with the [`M_DC_HANDLE`](../../Reference/buf/MbufInquire.md) inquire type. When using this control type, the buffer should be internally stored in [`M_GDI`](../../Reference/buf/MbufAllocColor.md) format, and cannot be a child buffer. No Aurora Imaging Library operation can be done between a device context allocation and free, therefore you should use the device context for a short period of time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_DC_FREE`

**Board availability:** GenTL, GevIQ, GigE Vision, Host System, USB3 Vision, V4L2

Frees a device context (DC).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_GC_FEATURE_BROWSER`

**Board availability:** GenTL, V4L2

Sets whether to open or close a dialog box that allows you to view and edit the GenTL buffer configuration information interactively. This window is referred to as the Feature Browser.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSE` | Closes Feature Browser. |
| `M_OPEN` *(default)* | Opens Feature Browser. |

---

### `M_LOCK`

Locks the specified buffer to allow a memory pointer to obtain a valid buffer address. The address is valid while the buffer is locked, but becomes invalid after it is unlocked. The calling thread will be blocked if the buffer is already locked by a different thread.  To unlock the specified buffer, use [`M_UNLOCK`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_MAX`

Sets the expected maximum pixel value of the buffer. This information is used to optimize certain processing operations on the buffer and the display of the buffer. Note that this information is not validated against the content of the buffer and is not updated automatically.  Note that [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) cannot be used with child buffers; this information is obtained directly from the parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected maximum pixel value within the range of the buffer type. For example, for an 8-bit unsigned buffer, set this value to a value between 0 and 255, inclusive. |

---

### `M_MIN`

Sets the expected minimum pixel value of the buffer. This information is used to optimize certain processing operations on the buffer and the display of the buffer. Note that this information is not validated against the content of the buffer and is not updated automatically.  Note that [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) cannot be used with child buffers; this information is obtained directly from the parent buffer.

| Value | Description |
| --- | --- |
| `Value` | Specifies the expected minimum pixel value within the range of the buffer type. For example, for an 8-bit unsigned buffer, set this value to a value between 0 and 255, inclusive. |

---

### `M_MODIFICATION_HOOK`

Sets whether to run the user-defined functions hooked to the buffer's modification event upon the event. These user-defined functions are initially hooked to the buffer modification event using [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that the user-defined functions should not be called.  Note that the modification counter of the image buffer will still be incremented. To inquire the count, use [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_MODIFICATION_COUNT`](../../Reference/buf/MbufInquire.md). |
| `M_ENABLE` *(default)* | Specifies that the user-defined functions should be called. |

---

### `M_MODIFIED`

Signals Aurora Imaging Library that the buffer content was modified without using Aurora Imaging Library. This control must be used to ensure that Aurora Imaging Library updates its internal information on the buffer. For example, if a display buffer was modified outside Aurora Imaging Library, the display will not be updated until you use this control. Note, if only a certain region of the buffer was modified, it is more efficient to specify an appropriate child buffer as [`BufId`](../../Reference/buf/MbufControl.md).  If a specific area of the buffer was modified without using Aurora Imaging Library, it is faster to use [`MbufControlArea`](../../Reference/buf/MbufControlArea.md) with the control type to signal Aurora Imaging Library to update the content of the specified area of the buffer.  [`M_MODIFIED`](../../Reference/buf/MbufControl.md) increments the buffer's modification counter. You can inquire about the counter's current value using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_MODIFICATION_COUNT`](../../Reference/buf/MbufInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_SURFACE_ALLOC`

**Board availability:** GenTL, GevIQ, GigE Vision, Host System, USB3 Vision, V4L2

Allocates a Cairo surface for drawing. Determine the Cairo surface handle using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with the [`M_SURFACE_HANDLE`](../../Reference/buf/MbufInquire.md) inquire type. When using this control type, the buffer should be internally stored in [`M_GDI`](../../Reference/buf/MbufAllocColor.md) format, and cannot be a child buffer. No Aurora Imaging Library operation can be done between a Cairo surface allocation and free; therefore, you should use the device context for a short period of time.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_SURFACE_FREE`

**Board availability:** GenTL, GevIQ, GigE Vision, Host System, USB3 Vision, V4L2

Frees a Cairo surface.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

---

### `M_UNLOCK`

Unlocks the specified buffer.  The buffer must have been previously locked with [`M_LOCK`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Implements the default behavior. |

### Combination Constants — For specifying whether Feature Browser should be synchronous or asynchronous

> *Essential.*

> **Usage:** You must add one of the following values to the above-mentioned values to set whether the Feature Browser should be synchronous or asynchronous.

> **Board availability:** GenTL, V4L2

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies that this function returns immediately once Feature Browser window opens. |
| `M_SYNCHRONOUS` *(default)* | Specifies that this function is blocked until Feature Browser window closes. |

### Combination Constants — For

> *Optional.*

> **Usage:** You can add one or more of the following values to the above-mentioned values to set the buffer's read and write permission.

| Value | Description |
| --- | --- |
| `M_READ` | Locks the buffer to be read only. |
| `M_READ+M_WRITE` *(default)* | Locks the buffer to be read and write. |
| `M_WRITE` | Locks the buffer to be write only. |

### For specifying image buffer settings

For buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)attribute, [`ControlType`](../../Reference/buf/MbufControl.md) and [`ControlValue`](../../Reference/buf/MbufControl.md) can also be set to one of the values below.

---

### `M_ASSOCIATED_LUT`

Sets whether to associate or disassociate a LUT buffer with the specified image buffer. The image buffer must be a 1-band 8-bit or 16-bit buffer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to remove the association between the LUT buffer and the image buffer. |
| `LUT buffer identifier` | Specifies the Aurora Imaging Library identifier of the LUT buffer to associate with the image buffer. If and when the image buffer is selected to a display, the required changes occur to produce the display effect of the LUT, unless the display is also associated with a LUT ([`MdispLut`](../../Reference/disp/MdispLut.md)). If associated with a LUT buffer, the image buffer cannot be selected to a display whose view mode is set to [`M_AUTO_SCALE`](../../Reference/disp/MdispControl.md) or [`M_MULTI_BYTES`](../../Reference/disp/MdispControl.md). |

---

### `M_DISPLAY_BANDS`

Sets the bands to use for display when the buffer is not a 1-band or 3-band buffer.

| Value | Description |
| --- | --- |
| `M_DISPLAY_MONO_BAND` | Specifies to use 1 band for display. |
| `M_DISPLAY_RGB_BAND` | Specifies to use 3 bands for display. |

---

### `M_INFORMATION_TYPE`

Sets the GenDC part type of the buffer.

| Value | Description |
| --- | --- |
| `M_INFO_1D` | Specifies to set the GenDC part type to 1D. |
| `M_INFO_2D` | Specifies to set the GenDC part type to 2D. |
| `M_INFO_2D_H264` | Specifies to set the GenDC part type to H.264 data. |
| `M_INFO_2D_JPEG` | Specifies to set the GenDC part type to JPEG data. |
| `M_INFO_2D_JPEG2000` | Specifies to set the GenDC part type to JPEG 2000 data. |
| `M_INFO_GENICAM_CHUNK` | Specifies to set the GenDC part type to GenICam chunk data. |
| `M_INFO_GENICAM_XML` | Specifies to set the GenDC part type to GenICam XML data. |

---

### `M_REGION_USE`

| Value | Description |
| --- | --- |
| `M_IGNORE` |  |
| `M_USE` *(default)* |  |

---

### `M_RESOLUTION_X`

Sets the X-resolution of the image buffer in pixels per inch (PPI). Typically, you would specify the X-resolution if you are exporting the image, using[`MbufExport`](../../Reference/buf/MbufExport.md), for third-party software that makes use of the specified pixels per inch.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the X resolution in PPI. |

---

### `M_RESOLUTION_Y`

Sets the Y-resolution of the image buffer in pixels per inch (PPI). Typically, you would specify the Y-resolution if you are exporting the image, using[`MbufExport`](../../Reference/buf/MbufExport.md), for third-party software that makes use of the specified pixels per inch.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the Y resolution in PPI. |

---

### `M_YCBCR_RANGE`

Sets whether the YUV buffer's pixel values are YCbCr encoded; this limits the range of the buffer's pixel values. The Y-band's pixel values are limited between 16 and 235. The Cb- and Cr-bands are limited to pixel values between 16 and 240.  This control type is not supported for an RGB buffer. The buffer must be of an uncompressed packed YUV16 type; otherwise, an error is generated.

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

### For

For buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) attribute, [`ControlType`](../../Reference/buf/MbufControl.md) and [`ControlValue`](../../Reference/buf/MbufControl.md) can also be set to one of the values below.  > **Note:** Note that, if the buffer contains any data, setting one of these control types automatically deletes the data. This is because, for Aurora Imaging Library to decompress the buffer's data, it must know the control values that were used in the compression. If you change one of these controls, Aurora Imaging Library will be unable to decompress the data and the data is therefore irrelevant. Note that the control type [`M_TARGET_SIZE`](../../Reference/buf/MbufControl.md) is the only control type that can be changed without affecting the data. For more information, see [JPEG and JPEG2000 compression](../../UserGuide/C30_JPEG_and_JPEG2000_compression/ChapterInformation.md).

---

### `M_DECOMPOSITION_LEVEL`

Sets the number of iterations (decomposition levels) the discrete wavelet transform is applied to the image. This control type is only supported with JPEG2000 buffers (both lossy and lossless).  The number of decomposition levels is applied to all bands by default.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of iterations (decomposition levels). Only integer values are accepted. |

---

### `M_HUFFMAN_AC`

Associates an AC Huffman table to the buffer. This control type is only supported with JPEG lossy buffers.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. If the buffer is 3-band, the same table is applied to all bands. |

---

### `M_HUFFMAN_AC_CHROMINANCE`

Associates an AC Huffman table to the U and V bands (chrominance) of the buffer. This control type is only supported with JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_AC_LUMINANCE`

Associates an AC Huffman table to the Y band (luminance) of the buffer. This control type is only supported with JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_DC`

Associates a DC Huffman table to the buffer. This control type is only supported with JPEG buffers (both lossy and lossless).

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. If the buffer is 3-band, the same table is applied to all bands. |

---

### `M_HUFFMAN_DC_CHROMINANCE`

Associates a DC Huffman table to the U and V bands (chrominance) of the buffer. This control type is only supported with JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_HUFFMAN_DC_LUMINANCE`

Associates a DC Huffman table to the Y band (luminance) of the buffer. This control type is only supported with JPEG lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_PREDICTOR`

Sets the type of predictor. This control type is only supported with JPEG lossless buffers. If the buffer is 3-band, the same predictor is applied to all bands.

| Value | Description |
| --- | --- |
| `0` | Specifies predictor #0 (no prediction). |
| `1` *(default)* | Specifies predictor #1 (the "pixel-to-the-left" predictor). |
| `2` | Specifies predictor #2 (the "pixel-above" predictor). |

---

### `M_Q_FACTOR`

Sets the quantization factor. This control type is only supported with lossy buffers.  For JPEG lossy buffers, the Q factor is always applied to all bands.  For JPEG2000 lossy buffers, the Q factor is applied to all bands by default. To change this default behavior, see the combination values below.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the quantization factor. Only integer values are accepted. The higher the factor, the more the compression, but the lower the image quality. |

---

### `M_Q_FACTOR_CHROMINANCE`

Sets the chrominance quantization factor. This control type is only supported with JPEG or JPEG2000 lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the factor. Only integer values are accepted. The higher the factor, the more the compression, but the lower the image quality.  The factor is applied to the U and V bands (chrominance). |

---

### `M_Q_FACTOR_LUMINANCE`

Sets the luminance quantization factor. This control type is only supported with JPEG or JPEG2000 lossy buffers in YUV format.

| Value | Description |
| --- | --- |
| `1 <= Value <= 99` *(default)* | Specifies the factor. Only integer values are accepted. The higher the factor, the more the compression, but the lower the image quality.  The factor is applied only to the Y band (luminance). |

---

### `M_QUANTIZATION`

Associates a quantization table to the buffer. This control type is only supported with lossy buffers.  For JPEG lossy buffers, this table is associated with all bands.  For JPEG2000 lossy buffers, this table is associated with all bands by default.  Note that setting this control type will reset the [`M_Q_...`](../../Reference/buf/MbufControl.md) control types to their default value (50). If you set the [`M_Q_FACTOR`](../../Reference/buf/MbufControl.md) control type after specifying a custom table with the [`M_QUANTIZATION`](../../Reference/buf/MbufControl.md) control type, the custom table will be scaled accordingly.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_QUANTIZATION_CHROMINANCE`

Associates a quantization table to the U and V bands (chrominance) of the buffer. This control type is only supported with JPEG or JPEG2000 lossy buffers in YUV format.  Note that setting this control type will reset the [`M_Q_FACTOR_CHROMINANCE`](../../Reference/buf/MbufControl.md) control type to its default value (50). If you set an[`M_Q_...`](../../Reference/buf/MbufControl.md) control type after specifying a custom table with the [`M_QUANTIZATION_CHROMINANCE`](../../Reference/buf/MbufControl.md) control type, the custom table will be scaled accordingly.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_QUANTIZATION_LUMINANCE`

Associates a quantization table to the Y band (luminance) of the buffer. This control type is only supported with JPEG or JPEG2000 lossy buffers in YUV format.  Note that setting this control type will reset the [`M_Q_FACTOR_LUMINANCE`](../../Reference/buf/MbufControl.md) control type to its default value (50). If you set an [`M_Q_...`](../../Reference/buf/MbufControl.md) control type after specifying a custom table with the [`M_QUANTIZATION_LUMINANCE`](../../Reference/buf/MbufControl.md) control type, the custom table will be scaled accordingly.

| Value | Description |
| --- | --- |
| `Array buffer identifier` | Specifies the identifier of the array buffer containing the table. |

---

### `M_RESTART_INTERVAL`

Sets the number of rows or 8x8 blocks between restart markers in a compressed image.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies after how many rows (for JPEG lossless buffers) or 8x8 blocks (for JPEG lossy buffers) of data to place restart markers. Only integer values are accepted. The default value is 8 for JPEG lossy buffers and 2 for JPEG lossless buffers. |

---

### `M_TARGET_SIZE`

Sets the size of the buffer into which the source image is compressed. This control type is only supported with JPEG2000 lossy buffers.  To ensure that the compressed data fits into the specified buffer size, less-significant data is discarded.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the size, in bytes. Only integer values are accepted. |

### Combination Constants — For specifying the color bands to affect

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to specify the color bands to affect.

When dealing with [`M_Q_FACTOR`](../../Reference/buf/MbufControl.md) and [`M_QUANTIZATION`](../../Reference/buf/MbufControl.md), the following values are only supported with JPEG2000 lossy buffers.

| Value | Description |
| --- | --- |
| `M_ALL_BANDS` *(default)* | Applies the specified control value to all bands of the buffer. |
| `M_BLUE` | Applies the specified control value to the blue band only (for RGB buffers). |
| `M_GREEN` | Applies the specified control value to the green band only (for RGB buffers). |
| `M_RED` | Applies the specified control value to the red band only (for RGB buffers). |
| `M_U` | Applies the specified control value to the U band only (for YUV buffers). |
| `M_V` | Applies the specified control value to the V band only (for YUV buffers). |
| `M_Y` | Applies the specified control value to the Y band only (for YUV buffers). |

### For controlling general neighborhood operation settings of an

The following [`ControlType`](../../Reference/buf/MbufControl.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControl.md) parameter settings can be specified for an [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) or an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) data buffer.  The following [`ControlType`](../../Reference/buf/MbufControl.md) values change the setting of a neighborhood operation for the specified kernel buffer or structuring element buffer. The [`ControlType`](../../Reference/buf/MbufControl.md) values establish how to perform a neighborhood operation when using the specified kernel buffer or structuring element buffer. Neighborhood operations that do not use a kernel buffer or structuring element buffer typically use the default values.

---

### `M_DEFAULT`

Sets all neighborhood operation control types to their default value.

| Value | Description |
| --- | --- |
| `M_NULL` *(default)* | Implements the default behavior. |

---

### `M_OFFSET_CENTER_X`

Sets the X-coordinate of the center pixel of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the X-coordinate of the top-left pixel of the central elements. |
| `0 <= Value < SizeX` | Specifies the value of the X-coordinate. |

---

### `M_OFFSET_CENTER_Y`

Sets the Y-coordinate of the center of the kernel or structuring element.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the Y-coordinate of the top-left pixel of the central elements. |
| `0 <= Value < SizeY` | Specifies the value of the Y-coordinate. |

---

### `M_OVERSCAN`

Sets the type of overscan used to handle the border pixels of a source image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies that Aurora Imaging Library automatically selects the type of overscan to optimize speed and logic according to the specified operation and the target system. |
| `M_DISABLE` | Specifies that no overscan will be used, unless processing the border pixels is faster than ignoring them; in the latter case, Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified operation and the target system. |
| `M_FAST` | Specifies that Aurora Imaging Library automatically selects the overscan to optimize speed according to the specified neighborhood operation and the target system. The overscan could be hardware-specific thereby having a different behavior than the other supported overscan modes.  Note that when using [`M_FAST`](../../Reference/buf/MbufControl.md), the destination pixels in the overscan area are undefined. The pixels can therefore contain different values from one function call to the next, even if the function's parameter values are the same. |
| `M_MIRROR` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that mirror the source buffer pixel values. That is, the overscan pixel values will be a mirror copy of the source buffer's borders. For example:  *[Image: KernelOverscan.png]* |
| `M_REPLACE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values set to the overscan replacement value ([`M_OVERSCAN_REPLACE_VALUE`](../../Reference/buf/MbufControl.md)). |
| `M_REPLICATE` | Specifies a type of overscan that processes the border pixels of a source image using overscan pixel values that replicate the border pixels. For example:  *[Image: ReplicateOverscan.png]* |
| `M_TRANSPARENT` | Specifies a type of overscan that processes the border pixels of a source image using transparent overscan pixel values. The overscan pixel values will be those of the ancestor buffer, and a mirror is done on the ancestor for any missing pixels. |

---

### `M_OVERSCAN_REPLACE_VALUE`

Sets a replacement value for the overscan pixel values.  Note that to use this control type, [`M_OVERSCAN`](../../Reference/buf/MbufControl.md) must be set to [`M_REPLACE`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REPLACE_MAX` | Specifies that the overscan neighborhood pixel values will be set to the maximum value of the source buffer. |
| `M_REPLACE_MIN` | Specifies that the overscan neighborhood pixel values will be set to the minimum value of the source buffer. |
| `Value` *(default)* | Specifies the value of the overscan neighborhood pixels. |

---

### `M_SATURATION`

Sets whether to saturate the results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to saturate results, except when Aurora Imaging Library can take advantage of optimization routines to accelerate the processing. In the latter case, results will be saturated and processing will be done in hardware.  When it is not possible to run hardware processing optimization routines, results that overflow the buffer are undefined. |
| `M_ENABLE` | Specifies to saturate results. A result that overflows or underflows will be set to the maximum or minimum value (respectively) that can be represented in the destination buffer.  If the saturation, normalization, and absolute value settings are specified for a kernel buffer, the saturation is performed after the normalization factor and the absolute value operations have been applied. |

### For the neighborhood operation settings of

The following [`ControlType`](../../Reference/buf/MbufControl.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControl.md) parameter settings can be specified for an [`M_KERNEL`](../../Reference/buf/MbufAlloc2d.md) data buffer. These settings establish how to perform a neighborhood operation when using the specified kernel buffer.

---

### `M_ABSOLUTE_VALUE`

Sets whether to take the absolute value of the results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to take the absolute value of the result. |
| `M_ENABLE` | Specifies to take the absolute value of the result.  Note that for a structuring element buffer, you cannot take the absolute value of a result. |

---

### `M_NORMALIZATION_FACTOR`

Sets the normalization factor to apply to the result.  Note that for a structuring element buffer, you cannot specify a normalization factor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the normalization factor.  > **Note:** If the factor produces an overflow in the destination, saturation might occur, depending on the [`M_SATURATION`](../../Reference/buf/MbufControl.md) control type. |

### For the overscan operation settings of

The following [`ControlType`](../../Reference/buf/MbufControl.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControl.md) parameter settings can be specified for an [`M_STRUCT_ELEMENT`](../../Reference/buf/MbufAlloc2d.md) data buffer. These settings are useful for overscan operations with iterations when using the specified structuring element.

---

### `M_ITER_OVERSCAN`

Sets the how to handle overscan operations with multiple iterations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL` | Specifies that for overscan operations with multiple iterations, to use a size equivalent to all iterations. |
| `M_PER_ITER` *(default)* | Specifies that for overscan operations with multiple iterations, each iteration will be according to the structuring element's size and center pixel. |

### For specifying settings useful with buffers that are components

The following [`ControlType`](../../Reference/buf/MbufControl.md) and corresponding [`ControlValue`](../../Reference/buf/MbufControl.md) parameter settings are used to control buffer settings that are primarily useful when working with a buffer that is a component of a container, or a buffer that will be copied to a component of a container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md).

---

### `M_COMPONENT_TYPE`

Sets the component type of the buffer, used when the buffer is a component of a container. The component type specifies the type of information stored in the component.

| Value | Description |
| --- | --- |
| `M_COMPONENT_CONFIDENCE` | Specifies that the component stores confidence information for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufControl.md)or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControl.md)component of the container. Coordinates associated with the confidence value 0 are considered invalid data and will not be used by 3D image processing functions.  A confidence component is associated with a range or disparity component in the same container when there are no other range, disparity, or confidence components in the container. If there is more than one range, disparity, and/or confidence component in a container (for example, because a single grab into the container transmitted multiple range and confidence components), you will need to either create a child container which contains only the required components using [`MbufChildContainer`](../../Reference/buf/MbufChildContainer.md), or free the extra components using [`MbufFreeComponent`](../../Reference/buf/MbufFreeComponent.md). |
| `M_COMPONENT_COORDINATE_MAP_A_AIL` | Specifies that the component stores the A coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the column index of each corresponding pixel and will result in a better representation of the camera's lens model when generating X-coordinates.  A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.  > **Note:** Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_COORDINATE_MAP_B_AIL` | Specifies that the component stores the B coordinate map provided by the camera. When using [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) with a projective map, this component can be used instead of the row index of each corresponding pixel and will result in a better representation of the camera's lens model when generating Y-coordinates.  A coordinate map component is associated with a range component in the same container when there is only one range component and only one type of each coordinate map component in that container.  > **Note:** Note that this component is only available if it is provided by the camera. For more information refer to your cameras manual. |
| `M_COMPONENT_CUSTOM + n` | Specifies that the component has a custom component type, identified by _n_, where n can be a value between 0 and 255.  You can use custom component types to identify components which store information that does not fit one of the standard component types. In addition, some cameras transmit components with custom component types. Refer to your camera manual to determine what type of information is stored in these components. |
| `M_COMPONENT_DISPARITY` | Specifies that the component stores a disparity map. Each pixel of a disparity map indicates the apparent distance (typically measured in pixel units) between where an object appears in the left and right images captured by a stereoscopic camera. |
| `M_COMPONENT_INFRARED` | Specifies that the component stores an intensity image of infrared light. |
| `M_COMPONENT_INTENSITY` | Specifies that the component stores an intensity image of visible light. |
| `M_COMPONENT_MATRIX_AIL` | Specifies that the component stores a 4x4 matrix that transforms depth map coordinates. Only depth map containers can have a matrix component.  A matrix component is associated with a range component in the same container when there is only one range component and no other matrix components in that container. |
| `M_COMPONENT_MESH_AIL` | Specifies that the component stores mesh information for the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControl.md)component of the container.  A mesh component is associated with a range component in the same container when there are no other range or mesh components in that container. A point cloud container which has a mesh component is referred to as a meshed point cloud container. |
| `M_COMPONENT_METADATA` | Specifies that the component stores metadata information. Metadata components are used by Aurora Imaging Library internally. Typically, you should ignore metadata components in your application. |
| `M_COMPONENT_MULTISPECTRAL` | Specifies that the component stores an intensity image where each band represents the intensity of a specific wavelength of light. Unlike an intensity component, a multispectral component might include information about non-visible light. |
| `M_COMPONENT_NORMALS_AIL` | Specifies that the buffer stores normals information for each point in the [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControl.md)component of the container.  A normals component is associated with a range component in the same container when there are no other range or normals components in that container. |
| `M_COMPONENT_RANGE` | Specifies that the component stores 3D distance/position information. The component can be either a 1-band buffer that stores a depth map, or a 3-band buffer that stores coordinates of 3D points. |
| `M_COMPONENT_REFLECTANCE` | Specifies that the component stores a reflectance map. Each pixel of a reflectance map indicates how much of the light hitting an object at that location is reflected back. Typically, this is an intensity image of the light wavelength used by a 3D sensor to detect 3D distance/position information. Typically, if the map was generated by a laser profiler, each line indicates the detected intensity of the laser for a single scan. |
| `M_COMPONENT_REGION_AIL` | Specifies that the component stores a region of interest (ROI) for the[`M_COMPONENT_RANGE`](../../Reference/buf/MbufControl.md) component of the container. 3D image processing functions will only operate on points inside the ROI. Only depth map containers can have a region component.  A region component is associated with a range component in the same container when there is only one range component and no other region components in that container. |
| `M_COMPONENT_SCATTER` | Specifies that the component stores a scatter map. Each pixel of a scatter map indicates how much of the light hitting an object at that location is detected scattering beneath the object's surface. |
| `M_COMPONENT_ULTRAVIOLET` | Specifies that the component stores an intensity image of ultraviolet light. |
| `M_COMPONENT_UNDEFINED` | Specifies that the component contains information of an unknown type. |

### For specifying settings useful with image buffers that store 3D data and are components

For buffers with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)attribute and with [`M_COMPONENT_TYPE`](../../Reference/buf/MbufControl.md) set to [`M_COMPONENT_RANGE`](../../Reference/buf/MbufControl.md) or [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControl.md), [`ControlType`](../../Reference/buf/MbufControl.md) and [`ControlValue`](../../Reference/buf/MbufControl.md) can also be set to one of the values below. These settings are used only when the buffer is the range or disparity component of a container passed as a source to[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).  A container cannot be 3D-processable or 3D-displayable unless it has a single range component with all of these settings at their default values (except for [`M_3D_DISTANCE_UNIT`](../../Reference/buf/MbufControl.md) which can be any value and[`M_3D_REPRESENTATION`](../../Reference/buf/MbufControl.md), which must be set to [`M_CALIBRATED_XYZ`](../../Reference/buf/MbufControl.md)or[`M_CALIBRATED_XYZ_UNORGANIZED`](../../Reference/buf/MbufControl.md)).

---

### `M_3D_ASPECT_RATIO`

Sets by how much the focal length in the Y-direction stored in the buffer will be scaled in the destination point cloud to correct for non-square pixels in 3D reconstruction modes that rely on projective geometry.  This is useful when the focal length parameters in the camera intrinsic matrix are not equal (in the X- and Y- directions).

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies by how much the focal length the Y-direction will be scaled. |

---

### `M_3D_COORDINATE_SYSTEM_TYPE`

Sets which type of coordinate system to use to interpret the coordinates stored in the bands of the buffer if it is a range component.  > **Note:** Note that Aurora Imaging Library does not support any functionality with coordinates stored using a coordinate system type other than [`M_CARTESIAN`](../../Reference/buf/MbufControl.md). If a buffer stores information defined using another type of coordinate system, you will need to manually convert that information to cartesian coordinates before the buffer can be used with any Aurora Imaging Library processing function. This conversion cannot be done using[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CARTESIAN` *(default)* | Specifies that the buffer stores right-handed cartesian coordinates.  If the buffer is a 1-band buffer, its values will be interpreted as Z-coordinates (equivalent to the pixels of a depth map).  If the buffer is a 3-band buffer, the bands store the following:  | Band | Information | | --- | --- | |  | | Band 0 | Stores the X-coordinates. | | Band 1 | Stores the Y-coordinates. | | Band 2 | Stores the Z-coordinates. | |
| `M_CYLINDRICAL` | Specifies that the buffer stores cylindrical coordinates (theta-Y-rho).  In a cylindrical coordinate system, points are defined relative to a reference axis and reference angle perpendicular to that axis. The Y-coordinate is the distance from the origin along the reference axis, theta (θ) is the polar angle of the point relative to a reference angle, and rho is the distance from that axis along the line described by theta and perpendicular to the reference axis (the radius).  *[Image: buf_cylindrical_coordinates.png]*  If the buffer is a 1-band buffer, its values store rho, the distance from a chosen reference axis.  If the buffer is a 3-band buffer, the bands store the following:  | Band | Information | | --- | --- | |  | | Band 0 | Stores theta, the polar angle coordinates relative to the chosen reference angle. | | Band 1 | Stores Y-coordinates, the distances along the chosen reference axis. | | Band 2 | Stores rho, the distances from the chosen reference axis. | |
| `M_SPHERICAL` | Specifies that the buffer stores spherical coordinates (theta-phi-rho).  In a spherical coordinate system, points are defined relative to a reference axis and a reference angle perpendicular to that axis. Rho is the distance from the origin (the radius), theta (θ) is the elevation angle of the point relative to reference axis, and phi is the azimuthal angle relative to the reference angle.  *[Image: buf_spherical_coordinates.png]*  If the buffer is a 1-band buffer, its values store rho, the distance from a chosen reference axis.  If the buffer is a 3-band buffer, the bands store the following:  | Band | Information | | --- | --- | |  | | Band 0 | Stores theta, the elevation angle coordinates relative to the chosen reference axis. | | Band 1 | Stores phi, the azimuth angle coordinates relative to a chosen reference angle. | | Band 2 | Stores rho, the distances from the chosen reference axis. | |
| `M_UNKNOWN` | Specifies that the coordinate system type is unknown. |

---

### `M_3D_DISPARITY_BASELINE`

Sets the stereo baseline value of the stereoscopic camera used to generate the data in the buffer. This is the physical distance between the lenses of the camera.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true physical distance between the lenses of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the stereo baseline value, expressed in meters. |

---

### `M_3D_DISTANCE_UNIT`

Sets the unit to use when the buffer is part of a container and stores natively calibrated distance data.

| Value | Description |
| --- | --- |
| `M_INCH` | Specifies that the distance data is provided in inches. |
| `M_MILLIMETER` | Specifies that the distance data is provided in millimeters. |
| `M_PIXEL` | Specifies that the distance data is provided in pixels. This setting is only useful for buffers that have the component type [`M_COMPONENT_DISPARITY`](../../Reference/buf/MbufControl.md)and store a disparity map. |
| `M_UNKNOWN` *(default)* | Specifies that the distance unit is unknown. |

---

### `M_3D_FOCAL_LENGTH`

Sets the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer.  > **Note:** Refer to your camera manual to determine the correct value for this setting, which might differ from the true focal length of your camera.

| Value | Description |
| --- | --- |
| `Value > 0.0` *(default)* | Specifies the focal length of the lenses of the stereoscopic camera used to generate the data in the buffer, expressed in pixels. For example, if the buffer has 1000 pixels per millimeter of a single image sensor (after calibration), and the focal length of the lenses in millimeters is 35, the focal length in pixels is 35000. |

---

### `M_3D_INVALID_DATA_FLAG`

Sets whether the buffer uses a specific value to indicate invalid data. For 3-band buffers that store coordinates, the invalid data flag should be stored in the Z-axis band. Specify the value that indicates invalid data using [`M_3D_INVALID_DATA_VALUE`](../../Reference/buf/MbufControl.md).

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that the buffer does not use a special value to indicate invalid data. |
| `M_TRUE` | Specifies that the buffer uses a special value to indicate invalid data. |

---

### `M_3D_INVALID_DATA_VALUE`

Sets the value used to indicate missing data when [`M_3D_INVALID_DATA_FLAG`](../../Reference/buf/MbufControl.md) is set to [`M_TRUE`](../../Reference/buf/MbufControl.md).  > **Note:** Note that this value is not used or respected by any Aurora Imaging Library function except for[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md) when the buffer is a component of the source container.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies the value used to indicate missing data. |

---

### `M_3D_OFFSET_X`

Sets by how much the X-coordinates stored in the buffer will be offset in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Y`

Sets by how much the Y-coordinates stored in the buffer will be offset in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Y-coordinates stored in the buffer will be offset. |

---

### `M_3D_OFFSET_Z`

Sets by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset. |

---

### `M_3D_PRINCIPAL_POINT_X`

Sets the X-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies X-position of the principal point, expressed in pixels. |

---

### `M_3D_PRINCIPAL_POINT_Y`

Sets the Y-position of the principal point of the buffer. This is the point in the buffer which the optical axis of the camera intersects.

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies Y-position of the principal point, expressed in pixels. |

---

### `M_3D_REPRESENTATION`

Sets how 3D data is stored in the buffer; this information is used when the buffer is a range or disparity component of a container. For more information, see [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `M_CALIBRATED_XYZ` | Specifies that the component stores organized and natively calibrated X, Y, and Z-coordinates.  This is the default value for 3-band buffers with [`M_SIZE_Y`](../../Reference/buf/MbufInquire.md) greater than 1.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedABC_Grid. |
| `M_CALIBRATED_XYZ_UNORGANIZED` | Specifies that the component stores unorganized and natively calibrated X, Y, and Z-coordinates.  This is the default value for 3-band buffers with [`M_SIZE_Y`](../../Reference/buf/MbufInquire.md) of 1.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedABC_PointCloud. |
| `M_CALIBRATED_XZ_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates stored in a separate array buffer.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedAC_Linescan. |
| `M_CALIBRATED_XZ_UNIFORM_Y` | Specifies that the component stores organized and natively calibrated X and Z-coordinates, with Y-coordinates identified by the row index.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedAC. |
| `M_CALIBRATED_Z` | Specifies that the component stores organized and natively calibrated Z-coordinates, without X and Y-coordinates.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedC. |
| `M_CALIBRATED_Z_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates without X-coordinates; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container.  This corresponds to the GenICam Scan3dOutputMode feature set to CalibratedC_Linescan. |
| `M_CALIBRATED_Z_UNIFORM_X_EXTERNAL_Y` | Specifies that the component stores organized and natively calibrated Z-coordinates, with the X-coordinates identified by column index; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container.  This corresponds to the GenICam Scan3dOutputMode feature set to RectifiedC_Linescan. |
| `M_CALIBRATED_Z_UNIFORM_XY` | Specifies that the component stores organized and natively calibrated Z-coordinates, with X and Y-coordinates identified by column and row index respectively. The manufacturer of your camera might refer to a range component with this setting as a depth map.  This is the default value for 1-band buffers.  This corresponds to the GenICam Scan3dOutputMode feature set to RectifiedC. |
| `M_DISPARITY` | Specifies that the component stores a disparity map with perspective distortion along the Y-axis. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point in the resulting point cloud will be generated using the row index of the corresponding pixel and compensating for perspective.  This corresponds to the GenICam Scan3dOutputMode feature set to DisparityC.  > **Note:** Typically, this setting should only be used with disparity components that have been generated using area scan cameras. |
| `M_DISPARITY_EXTERNAL_Y` | Specifies that the component stores a disparity map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. When used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point in the resulting point cloud will be taken from the specified buffer.  This corresponds to the GenICam Scan3dOutputMode feature set to DisparityC_Linescan.  > **Note:** Typically, this setting should only be used with disparity components that have been generated using line scan cameras. |
| `M_DISPARITY_UNIFORM_Y` | Specifies that the component stores a disparity map, with Y-values identified by row index. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinates of each point in the resulting point cloud will be generated using the row index of the corresponding pixel. Since the Y-values are uniform, no perspective correction is applied.  This partially corresponds to the GenICam Scan3dOutputMode feature set to DisparityC_Linescan, except that the Y-values are not stored in a separate buffer.  > **Note:** Typically, this setting should only be used with disparity components that have been generated using line scan cameras. |
| `M_PROJECTED_Z` | Specifies that the component stores a projective map. When used with[`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the X- and Y-coordinates of each point will be generated using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the column and row index for each corresponding pixel or the A and B coordinate maps provided by the camera.  Providing the A and B coordinate maps ([`M_COMPONENT_COORDINATE_MAP_A_AIL`](../../Reference/buf/MbufControl.md), and [`M_COMPONENT_COORDINATE_MAP_B_AIL`](../../Reference/buf/MbufControl.md), respectively) will result in a more accurate representation of the camera's lens model (properties of the camera). If the coordinate maps are not provided when used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the column and row index for each corresponding pixel will be used to generate the X- and Y-coordinates.  This corresponds to the GenICam Scan3dOutputMode feature set to ProjectedC.  > **Note:** Typically, this setting should only be used with projective maps generated using area scan cameras. |
| `M_PROJECTED_Z_EXTERNAL_Y` | Specifies that the component stores a projective map; Y-coordinates are stored in a separate buffer that has an [`M_ARRAY`](../../Reference/buf/MbufAlloc1d.md) attribute and is not part of the container. When used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the Y-coordinate of each point will be taken from the specified buffer. In this case, only the X-coordinates of each point will be generated using the transmitted depth (Z-coordinate values), the camera's intrinsic parameters, and either the column index for each corresponding pixel or the A coordinate map provided by the camera.  Providing the A coordinate map ([`M_COMPONENT_COORDINATE_MAP_A_AIL`](../../Reference/buf/MbufControl.md)) will result in a more accurate representation of the camera's lens model (a property of the camera). If the coordinate map is not provided when used with [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md), the column index for each corresponding pixel will be used to generate the X-coordinates.  This corresponds to the GenICam Scan3dOutputMode feature set to ProjectedC_Linescan.  > **Note:** Typically, this setting should only be used with projective maps generated using line scan cameras. |
| `M_UNCALIBRATED_Z` | Specifies that the component stores organized and uncalibrated Z-coordinates, without X and Y-coordinates.  This corresponds to the GenICam Scan3dOutputMode feature set to UncalibratedC. |

---

### `M_3D_SCALE_X`

Sets by how much the X-coordinates stored in the buffer will be scaled in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Y`

Sets by how much the Y-coordinates stored in the buffer will be scaled in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Y-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SCALE_Z`

Sets by how much the Z-coordinates stored in the buffer will be scaled in the destination point cloud when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value != 0.0` *(default)* | Specifies how much the Z-coordinates stored in the buffer will be scaled. |

---

### `M_3D_SHEAR_X`

Sets by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the X-coordinates stored in the buffer will be offset in the destination point cloud from the X-coordinates in the previous row. |

---

### `M_3D_SHEAR_Z`

Sets by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row when the buffer is a component of a container passed as a source to [`MbufConvert3d`](../../Reference/buf/MbufConvert3d.md).

| Value | Description |
| --- | --- |
| `Value` *(default)* | Specifies by how much the Z-coordinates stored in the buffer will be offset in the destination point cloud from the Z-coordinates in the previous row. |

## Remarks

> Note that during development and at runtime, compression support, particularly for an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.

[`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) should specify the expected range of pixel values in the buffer. Pixel values between [`M_MIN`](../../Reference/buf/MbufControl.md) and [`M_MAX`](../../Reference/buf/MbufControl.md) are remapped linearly to values between the minimum and maximum possible display values.
