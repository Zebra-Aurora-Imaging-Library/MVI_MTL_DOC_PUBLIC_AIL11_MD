---
doctype: BoardSpecificNotes
part: ""
chapter: Radient_eV-CL
section: Using_Radient_eV-CL
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / radient_ev-cl / Using Radient eV-CL"
---

# Using Zebra Radient eV-CL with Aurora Imaging Library

To use Zebra Radient eV-CL, you must allocate it as an Aurora Imaging Library Radient eV-CL system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_RADIENTEVCL`](../../Reference/sys/MsysAlloc.md)). This allocation opens communications with your Zebra Radient eV-CL and allows Aurora Imaging Library to use its resources.

You can allocate an Aurora Imaging Library Radient eV-CL system for your board in multiple processes (executables). If more than one process allocates a digitizer for the same acquisition path, grabs from the different processes will be queued in the order that they arrive (first in, first out).

Zebra Radient eV-CL features at least one quadrature decoder that can decode input from a linear or rotary encoder with quadrature output. For more information on this feature, refer to [Using quadrature input from a rotary encoder](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md). This chapter also provides information on how to configure and use the I/O signals.

Zebra Radient eV-CL provides a GenICam CLProtocol driver to communicate with your Camera Link camera using the GenICam SFNC standard. For information regarding using Aurora Imaging Library to access the GenICam SFNC-compatible features of your camera, see [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

## Grabbing and the buffer formats to use for each camera pixel format on Zebra Radient eV-CL

For Zebra Radient eV-CL, the following table outlines which Aurora Imaging Library buffer format to use with each supported camera pixel format. Note that these are the officially supported camera pixel formats for Zebra Radient eV-CL.

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

[^_3]: All of the values available for RGB are also available in BGR format.

[^_1]: 3-band 8-bit buffers can be in any of the following formats: [`M_RGB24`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md), [`M_BGR24`](../../Reference/buf/MbufAllocColor.md) +[`M_PACKED`](../../Reference/buf/MbufAllocColor.md), or [`M_BGR32`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md).

[^_2]: 3-band 16-bit buffers can be in either an [`M_RGB48`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) or [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) format.

## Performing Bayer color conversion in hardware

When Zebra Radient eV-CL boards grab color images from a video source with a Bayer color filter (as specified by the DCF), it performs Bayer color conversion in hardware, as it transfers the images to the Host. If the images require white balancing, Zebra Radient eV-CL boards can perform this automatically if white balancing is enabled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). If performing white balancing, you can use the default white balance coefficients, automatically have them calculated (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md)), or set explicit coefficients ([`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)). For information on Bayer color conversion, refer to [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

If you don't want to perform Bayer color conversion in hardware, disable it using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md).

The [`M_BAYER...`](../../Reference/dig/MdigControl.md) control types of [`MdigControl`](../../Reference/dig/MdigControl.md) can only be used when grabbing from a camera that has a Bayer color filter (as specified by the DCF); otherwise, an error will be generated.

## Using frame burst with a multi-frame buffer

All version of Zebra Radient eV-CL support frame burst technology. This technology allows you to grab a group of sequential frames into a multi-frame buffer with one grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md), or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)); the defined number of frames are stored contiguously in the same buffer. The end-of-grab event only occurs once the entire group of frames has been grabbed, reducing the number of events that need to be handled. This is useful in cases where you have a high frame rate and need to ensure that no frames are missed. Note that a user-defined function hooked to the end-of-grab event ([`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md)) is executed only once the entire group of frames has been grabbed. For information on creating a multi-frame image buffer to store sequential frames, see [Specifying the dimensions of a multi-frame image buffer](../../UserGuide/C23_Data_buffers/S03_Specifying_the_dimensions_of_a_data_buffer.md).

## Using on-board flat-field correction

Zebra Radient eV-CL supports flat-field correction on-board without Host intervention. Images taken in environments with uneven lighting can affect the quality of your processing. To correct this, an on-board flat-field correction can be applied to images so that the intensity across the image is even before processing begins. To use flat-field correction, you need to specify an on-board buffer that contains the gain values, and optionally another that contains the offset values, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md) and [`M_SHADING_CORRECTION_OFFSET_ID`](../../Reference/dig/MdigControl.md), respectively. The buffers should be the same size as the grabbed image, and the gain values should be in the specified fixed point format ([`M_SHADING_CORRECTION_GAIN_FIXED_POINT`](../../Reference/dig/MdigControl.md)). The aggregate bandwidth to read the gain buffer and offset buffer from on-board memory cannot be bigger than the internal maximum bandwidth of the DMA port. There is a line limitation of 64 K pixels per line and 1 M lines per frame. Once you have set the buffers, enable the correction using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_SHADING_CORRECTION`](../../Reference/dig/MdigControl.md).

> **Note:** Note that for Zebra Radient eV-CL, flat-field correction is not supported on Zebra Radient eV-CL SB.
