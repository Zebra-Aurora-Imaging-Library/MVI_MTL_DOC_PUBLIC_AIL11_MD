---
doctype: Reference
module: buf
function: MbufClear
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / buf / MbufClear"
---

# MbufClear

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Clears a buffer, or the components of a container, to a specified color.

## Syntax

```c
void MbufClear(
    AIL_ID     DstContainerOrBufId,  //out
    AIL_DOUBLE Color                 //in
)
```

## Description

This function clears the specified buffer, or the components of the specified container, to the specified color.

To clear a 16-bit or 32-bit color image buffer to a color value, use [`MgraClear`](../../Reference/gra/MgraClear.md).

For an image buffer, the clear can be limited to a region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

Using this function with a container is equivalent to using it individually with each component of the container. An error will be generated if the container has a component that cannot be cleared (for example, because it has an ROI defined only in a vector format).

Using this function with a buffer always results in an uncalibrated image. Any previously associated camera calibration context is removed. You can use [`MgraClear`](../../Reference/gra/MgraClear.md) to clear a buffer while preserving the camera calibration context associated with the empty image.

## Parameters

### `DstContainerOrBufId` *(out, AIL_ID)*

Specifies the identifier of the buffer or container to clear. If you specify an image buffer, it can have an ROI set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The ROI must be defined in raster format ([`M_RASTER`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)). An error is generated if the ROI is only in vector format ([`M_VECTOR`](../../Reference/buf/MbufInquire.md)).

### `Color` *(in, AIL_DOUBLE)*

Specifies the color value with which to clear the buffer.

*For specifying a color value*

| Value | Description |
| --- | --- |
| `M_RGB888` | Specifies an RGB value used to clear an 8-bit 3-band buffer to an RGB color. Each value (red, green or blue) must not exceed 8 bits. |
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
