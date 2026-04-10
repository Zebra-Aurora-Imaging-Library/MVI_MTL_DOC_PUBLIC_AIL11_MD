---
doctype: Reference
module: buf
function: MbufTransfer
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufTransfer"
---

# MbufTransfer

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

> Copy a two-dimensional region of one or all bands from the source buffer into a two-dimensional region of one or all bands in the destination buffer, using a specified transfer function and transfer type.

## Syntax

```c
void MbufTransfer(
    AIL_ID    SrcBufId,          //in
    AIL_ID    DestBufId,         //out
    AIL_INT   SrcOffX,           //in
    AIL_INT   SrcOffY,           //in
    AIL_INT   SrcSizeX,          //in
    AIL_INT   SrcSizeY,          //in
    AIL_INT   SrcBand,           //in
    AIL_INT   DestOffX,          //in
    AIL_INT   DestOffY,          //in
    AIL_INT   DestSizeX,         //in
    AIL_INT   DestSizeY,         //in
    AIL_INT   DestBand,          //in
    AIL_INT64 TransferFunction,  //in
    AIL_INT64 TransferType,      //in
    AIL_INT64 OperationFlag,     //in
    void *    ExtraParameterPtr  //in
)
```

## Description

This function copies a two-dimensional region of one or all bands of the source buffer to a two-dimensional region of one or all bands of the destination buffer, using a specified transfer function and transfer type.

If the size of the regions in the source and destination buffers differ, the size of the largest region is reduced so that the size of the source and destination regions become equal.

## Parameters

### `SrcBufId` *(in, AIL_ID)*

Specifies the identifier of the source buffer. When the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md), you must set this parameter to `M_NULL`; when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_BYTE_SWAP`](../../Reference/buf/MbufTransfer.md), you can set this parameter to [`M_NULL`](../../Reference/buf/MbufTransfer.md) to swap bytes within the same buffer.

### `DestBufId` *(out, AIL_ID)*

Specifies the identifier of the destination buffer.

### `SrcOffX` *(in, AIL_INT)*

Specifies the horizontal offset of the source region, relative to the top-left coordinate of the source buffer.

*For the horizontal offset of the source region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies that the horizontal offset of the source region is ignored when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md). |
| `Value` *(default)* | Specifies the horizontal offset of the source region, in pixels. |

### `SrcOffY` *(in, AIL_INT)*

Specifies the vertical offset of the source region, relative to the top-left coordinate of the source buffer.

*For the vertical offset of the source region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` | Specifies that the vertical offset of the source region is ignored when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md). |
| `Value` *(default)* | Specifies the vertical offset of the source region, in pixels. |

### `SrcSizeX` *(in, AIL_INT)*

Specifies the width of the source region.

*For the width of the source region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default width of the source region. The default is equal to the difference between the width of the source buffer and the horizontal offset of the source region. |
| `M_NULL` | Specifies that the width of the source region is ignored when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md). |
| `Value` | Specifies the width of the source region, in pixels. |

### `SrcSizeY` *(in, AIL_INT)*

Specifies the height of the source region.

*For the height of the source region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default height of the source region. The default is equal to the difference between the height of the source buffer and the vertical offset of the source region. |
| `M_NULL` | Specifies that the height of the source region is ignored when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md). |
| `Value` | Specifies the height of the source region, in pixels. |

### `SrcBand` *(in, AIL_INT)*

Specifies the band of the region from which to copy. This parameter can be set to one of the following values:

*For the band of the region from which to copy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Same as [`M_ALL_BANDS`](../../Reference/buf/MbufTransfer.md). |
| `M_NULL` | Specifies that the band of the source region is ignored when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_CLEAR`](../../Reference/buf/MbufTransfer.md). |
| `M_ALL_BANDS` | Specifies all bands (for multi-band buffers). |
| `M_BLUE` | Specifies the blue band (for RGB buffers). |
| `M_GREEN` | Specifies the green band (for RGB buffers). |
| `M_HUE` | Specifies the hue band (for HSL buffers). |
| `M_LUMINANCE` | Specifies the luminance band (for HSL buffers). |
| `M_RED` | Specifies the red band (for RGB buffers). |
| `M_SATURATION` | Specifies the saturation band (for HSL buffers). |
| `M_U` | Specifies the U band (for YUV buffers). |
| `M_V` | Specifies the V band (for YUV buffers). |
| `M_Y` | Specifies the Y band (for YUV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band to copy.

The relationship between the index value and band for RGB and HSL buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers or the hue band for HSL buffers. |
| 1 | Corresponds to the green band for RGB buffers or the saturation band for HSL buffers. |
| 2 | Corresponds to the blue band for RGB buffers or the luminance band for HSL buffers. | |

### `DestOffX` *(in, AIL_INT)*

Specifies the horizontal offset of the destination region, relative to the top-left coordinate of the destination buffer.

*For the horizontal offset of the destination region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default horizontal offset of the destination region. The default is equal to the horizontal offset of the source region. |
| `Value` | Specifies the horizontal offset of the destination region, in pixels. |

### `DestOffY` *(in, AIL_INT)*

Specifies the vertical offset of the destination region, relative to the top-left coordinate of the destination buffer.

*For the vertical offset of the destination region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default vertical offset of the destination region. The default is equal to the vertical offset of the source region. |
| `Value` | Specifies the value of the vertical offset of the destination region, in pixels. |

### `DestSizeX` *(in, AIL_INT)*

Specifies the width of the destination region.

*For the width of the destination region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default width of the destination region. The default is equal to the difference between the destination buffer width and the horizontal offset of the destination region. |
| `Value` | Specifies the width of the destination region, in pixels. |

### `DestSizeY` *(in, AIL_INT)*

Specifies the height of the destination region.

*For the height of the destination region*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default height of the destination region. The default is equal to the difference between the destination buffer height and the vertical offset of the destination region. |
| `Value` | Specifies the height of the destination region, in pixels. |

### `DestBand` *(in, AIL_INT)*

Specifies the band of the destination region in which to copy.

*For the band of the region in which to copy*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Same as [`M_ALL_BANDS`](../../Reference/buf/MbufTransfer.md). |
| `M_ALL_BANDS` | Specifies all bands (for multi-band buffers). |
| `M_BLUE` | Specifies the blue band (for RGB buffers). |
| `M_GREEN` | Specifies the green band (for RGB buffers). |
| `M_HUE` | Specifies the hue band (for HSL buffers). |
| `M_LUMINANCE` | Specifies the luminance band (for HSL buffers). |
| `M_RED` | Specifies the red band (for RGB buffers). |
| `M_SATURATION` | Specifies the saturation band (for HSL buffers). |
| `M_U` | Specifies the U band (for YUV buffers). |
| `M_V` | Specifies the V band (for YUV buffers). |
| `M_Y` | Specifies the Y band (for YUV buffers). |
| `0 <= Value < #bands` | Specifies the index of the band in which to copy.

The relationship between the index value and band for RGB and HSL buffers is the following:

|   |   |
| --- | --- |
| 0 | Corresponds to the red band for RGB buffers or the hue band for HSL buffers. |
| 1 | Corresponds to the green band for RGB buffers or the saturation band for HSL buffers. |
| 2 | Corresponds to the blue band for RGB buffers or the luminance band for HSL buffers. | |

### `TransferFunction` *(in, AIL_INT64)*

Specifies the function to use in the transfer. This parameter can be set to one of the following:

*For specifying the function to use in the transfer*

| Value | Description |
| --- | --- |
| `M_BYTE_SWAP` | Specifies a transfer function that extracts the bytes in each pixel of the source region, swaps them, and copies the new pixels into the destination region.

Note that [`M_BYTE_SWAP`](../../Reference/buf/MbufTransfer.md) is only supported when the source and destination buffers are both either 1-band 16-bit buffers or 1-band 32-bit buffers and have the same internal storage specifiers.

When using 16-bit buffers, the first byte is swapped with the last byte. When using 32-bit buffers, the first byte is swapped with the last byte and the two middle bytes are swapped.

You can swap bytes within the same buffer. To do so, set the [`SrcBufId`](../../Reference/buf/MbufTransfer.md) parameter to [`M_NULL`](../../Reference/buf/MbufTransfer.md). |
| `M_CLEAR` | Specifies a transfer function that clears the destination region. Set all source buffer parameters to [`M_NULL`](../../Reference/buf/MbufTransfer.md). |
| `M_COMPOSITION` | Specifies a transfer function that copies all pixels, except for pixels in the source region that are equal to the composition color specified by the [`OperationFlag`](../../Reference/buf/MbufTransfer.md) parameter. |
| `M_COPY` | Specifies a transfer function that copies the data from the source region into the destination region. If the source and destination buffers are of different data types, Aurora Imaging Library converts the data automatically. When copying from a floating-point buffer to an integer buffer, the values are truncated.

If the source is a 3-band region of a YUV buffer and the destination is a 1-band region, only the Y band (luminance) is copied. If the source is a 3-band region of an RGB buffer and the destination is a 1-band region, only the red band is copied. If both regions are multi-band, each source band is copied to the corresponding band of the destination.

Note that when copying from a non-binary buffer to a binary destination buffer, all non-zero pixels in the source buffer are represented as ones (1) in the binary destination buffer. When copying a binary buffer to a buffer of a different depth, each bit is copied into the least-significant bit of a different destination pixel using the specified transfer function. The remaining bits of the destination pixel are set to 0; to propagate the bit value to all bits, use [`MimBinarize`](../../Reference/im/MimBinarize.md). |

*For specifying the scale of*

| Value | Description |
| --- | --- |
| `M_SCALE` | Specifies that the data of the source region must be scaled to fit in the destination region. |

### `TransferType` *(in, AIL_INT64)*

Specifies the type of transfer. If [`M_DEFAULT`](../../Reference/buf/MbufTransfer.md) is not selected, then any combination of the transfer modes listed below can be specified for the [`TransferType`](../../Reference/buf/MbufTransfer.md) parameter. From this combination of transfer modes, Aurora Imaging Library selects the most efficient mode to transfer data. If the most efficient transfer mode fails, then Aurora Imaging Library automatically selects the next most efficient transfer mode from the combination of transfer modes. If all selected transfer modes fail, an error is returned. The efficiency of the transfer mode depends on the internal format of the source and destination buffers, as well as the hardware available in your computer.

*For specifying the type of transfer*

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Transfers data using the most efficient transfer mode among the transfer modes listed. |
| `M_AIL_MODE` | Transfers data using the standard Aurora Imaging Library mode.

This transfer mode is always possible. |
| `M_DIB_MODE` | Transfers data using the DIB interface. Both the source and destination buffers must have the[`M_DIB`](../../Reference/buf/MbufAllocColor.md) attribute.

This transfer mode is only possible when the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_COPY`](../../Reference/buf/MbufTransfer.md). |

*For specifying synchronous data transfer*

| Value | Description |
| --- | --- |
| `M_SYNCHRONOUS` | Specifies that the transfer of data is synchronous. |

### `OperationFlag` *(in, AIL_INT64)*

Specifies the color value that is passed to the transfer function. If the [`TransferFunction`](../../Reference/buf/MbufTransfer.md) parameter is set to [`M_COPY`](../../Reference/buf/MbufTransfer.md) or [`M_BYTE_SWAP`](../../Reference/buf/MbufTransfer.md), this parameter must be set to `M_NULL`.

*For specifying a color value*

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value. Specify an RGB value when the source buffer is a 3-band buffer. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |

*For specifying a grayscale value to be used with all bands*

| Value | Description |
| --- | --- |
| `Value` | Specifies a grayscale value used to clear the 1-band or multi-band buffer. This value is cast to the buffer type and depth. If the destination buffer is multi-band, this value will be replicated in each band. |

### `ExtraParameterPtr` *(in, *void)*

Specifies a pointer to the Aurora Imaging Library identifier of the overlay buffer, if applicable. Set this parameter to [`M_NULL`](../../Reference/buf/MbufTransfer.md) in all other cases.
