---
doctype: BoardSpecificNotes
part: ""
chapter: GevIQ
section: Using_GevIQ
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / geviq / Using GevIQ"
---

# Using Zebra GevIQ

To use Zebra GevIQ, you must allocate it as an Aurora Imaging Library GevIQ system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_GEVIQ`](../../Reference/sys/MsysAlloc.md)). This allocation opens general communication with (allows you to discover) all GigE Vision-compliant cameras (or other devices) connected to Zebra GevIQ directly or behind a switch. You must then allocate a digitizer for each camera that you want to use to grab images and/or access directly, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md). If you have multiple GigE Vision-compliant interface cameras, you should use the [`M_GC_CAMERA_ID()`](../../Reference/dig/MdigAlloc.md) macro and specify the device's name; this will make for more portable code. You can use the Aurora Imaging Capture Works utility to view a list of available GigE Vision-compliant cameras and their user-defined name. For information on this utility, refer to [Zebra GevIQ utilities and tools](S03_GevIQ_utilities.md). For information regarding working with multiple GigE Vision-compliant devices, refer to [Working with one or more GigE Vision-compliant cameras](../GigE_Vision/S02_Using_GigE_Vision.md). You can allocate an Aurora Imaging Library GevIQ system for your board in multiple processes (executables).

> **Note:** Note that you must only allocate an [`M_SYSTEM_GEVIQ`](../../Reference/sys/MsysAlloc.md) system per Zebra GevIQ board and not for each SFP transceiver per board.

Zebra GevIQ is a network adapter that supports digital video sources compliant with the GigE Vision 2.2 standard. Zebra GevIQ can grab monochrome, RGB color, and Bayer color-encoded data. The board can also perform pixel unpacking, image sub-sampling, and color-conversion. Depending on how your video sources are configured/connected to your Zebra GevIQ (for example, using a switch), an SFP connector could have multiple offloaded stream channels behind it. For information regarding working with multiple GigE Vision-compliant devices using a switch, refer to [Using a Gbit Ethernet switch with GigE Vision-compliant cameras](../GigE_Vision/S09_Using_an_Ethernet_switch_with_GigE_vision_complaint_cameras.md).

## Grabbing and the Aurora Imaging Library buffer formats to use for each camera pixel format

The following table outlines which Aurora Imaging Library buffer format to use with each supported camera pixel format. Note that these are the officially supported camera pixel formats.

| Camera pixel format[^_3] | Aurora Imaging Library buffer format (type and attribute) | Operation |
| --- | --- | --- |
| Mono8, Mono10, Mono12, Mono14, Mono16, Mono32 | 1-band 8-bit, 1-band 16-bit, 1-band 32-bit, 3-band 8-bit[^_1], 3-band 16-bit[^_2] | Data will automatically be bit-shifted to have most-significant bits (MSB) in destination buffer |
| BayerXX8, BayerXX10, BayerXX12, BayerXX14, BayerXX16 where XX = GR, GB, BG or RG | 1-band 8-bit, 1-band 16-bit | Demosaicing followed by RGB to Y (bit shift for MSB) |
| 3-band 8-bit, 3-band 16-bit | Demosaicing (bit shift for MSB) |
| 3-band YUV16 + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | Demosaicing followed by RGB to YUV |
| RGB8, RGBA8, | 1-band 8-bit | RGB to Y |
| 3-band 8-bit | Direct copy |
| 3-band YUV16 + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | RGB to YUV |
| RGB10, RGBA10, RGB12, RGBA12, RGB14, RGBA14, RGB16, RGBA16 | 1-band 8-bit | RGB to Y (bit shift for MSB) |
| 1-band 16-bit | RGB to Y |
| 3-band 16-bit [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) | Packed destination buffers are not supported |
| YUV422_8, YCbCr601_422_8, YCbCr709_422_8 | 1-band 8-bit | Only Y band |
| 3-band YUV16 + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | Direct copy |

[^_1]: 3-band 8-bit buffers can be in any of the following formats: [`M_RGB24`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md), [`M_BGR24`](../../Reference/buf/MbufAllocColor.md) +[`M_PACKED`](../../Reference/buf/MbufAllocColor.md), or [`M_BGR32`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md).

[^_2]: 3-band 16-bit buffers can be in either an [`M_RGB48`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) or [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) format.

[^_3]: All of the values available for RGB are also available in BGR format.

> **Note:** Note that for Zebra GevIQ, allocating a buffer with [`M_GRAB`](../../Reference/buf/MbufAlloc1d.md) will force the buffer in pageable memory ([`M_PAGED`](../../Reference/buf/MbufAlloc1d.md)) by default.

## Performing Bayer color conversion in hardware

When Zebra GevIQ grabs color images from a video source with a Bayer color filter, it performs Bayer color conversion in hardware, as it transfers the images to the Host. If the images require white balancing, Zebra GevIQ can perform this automatically if white balancing is enabled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). If performing white balancing, you can use the default white balance coefficients, automatically have them calculated (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md)), or set explicit coefficients ([`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)). For information on Bayer color conversion, refer to [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

If you don't want to perform Bayer color conversion in hardware, disable it using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md).

The [`M_BAYER...`](../../Reference/dig/MdigControl.md) control types of [`MdigControl`](../../Reference/dig/MdigControl.md) can only be used when grabbing from a camera that has a Bayer color filter; otherwise, an error will be generated.
