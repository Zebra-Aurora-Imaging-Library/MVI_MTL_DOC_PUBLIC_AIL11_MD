---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CXP
section: Using_Rapixo-CXP
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cxp / Using Rapixo-CXP"
---

# Using Zebra Rapixo CXP with Aurora Imaging Library

To use Zebra Rapixo CXP, you must allocate it as an Aurora Imaging Library Rapixo CXP system (using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_RAPIXOCXP`](../../Reference/sys/MsysAlloc.md)). This allocation opens communication with the Zebra Rapixo CXP board and allows Aurora Imaging Library to use its resources. You can allocate an Aurora Imaging Library Rapixo CXP system for your board in multiple processes (executables). However, different processes cannot allocate a digitizer for the same acquisition paths.

All Zebra Rapixo CXP boards feature 4 quadrature decoders that can decode input from linear or rotary encoders with quadrature output. For information on this feature, refer to [Using quadrature input from a rotary encoder](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md). This chapter also provides information on how to configure and use the auxiliary I/O signals.

For information on how to use the Processing FPGA of your Zebra Rapixo CXP Pro, see[Using Aurora Imaging Library for FPGA processing](../../UserGuide/C67_Using_AIL_with_FPGA_processing/ChapterInformation.md).

## Grabbing and the formats to use for each camera pixel format

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

[^_3]: All of the values available for RGB are also available in BGR format.

[^_1]: 3-band 8-bit buffers can be in any of the following formats: [`M_RGB24`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md), [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md), [`M_BGR24`](../../Reference/buf/MbufAllocColor.md) +[`M_PACKED`](../../Reference/buf/MbufAllocColor.md), or [`M_BGR32`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md).

[^_2]: 3-band 16-bit buffers can be in either an [`M_RGB48`](../../Reference/buf/MbufAllocColor.md)+ [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) or [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) format.

## Using the CoaXPress trigger signal on Zebra Rapixo CXP

The CoaXPress trigger signal is an embedded signal transmitted along the physical transport layer connection of your camera. Typically, the CoaXPress trigger signal is reserved for trigger information and is sent with other control and data signals along the same cable. On Zebra Rapixo CXP, the CoaXPress trigger signal is only supported from the frame grabber to the camera (that is, as an output signal).

To use this signal to trigger the camera, use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_IO_SOURCE`](../../Reference/dig/MdigControl.md) + [`M_TL_TRIGGER`](../../Reference/dig/MdigControl.md) set to the appropriate auxiliary input signals (for example, [`M_AUX_IO4`](../../Reference/dig/MdigControl.md)).

> **Code example:** [boardspecific.Rapixo-CoF.Send_trigger](boardspecific.Rapixo-CoF.Send_trigger)

Note that when you set the camera to capture an image only upon a trigger, you should disable grab triggered mode on the frame grabber ([`M_GRAB_TRIGGER_STATE`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md)). For more information on performing a triggered grab, see [Grabbing with triggers](../../UserGuide/C27_Grabbing_with_your_digitizer/S11_Grabbing_with_triggers_and_exposures.md).

## Performing Bayer color conversion in hardware

When Zebra Rapixo CXP grabs color images from a video source with a Bayer color filter (as specified by the DCF), it performs Bayer color conversion in hardware, as it transfers the images to the Host. If the images require white balancing, Zebra Rapixo CXP can perform this automatically if white balancing is enabled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). If performing white balancing, you can use the default white balance coefficients, automatically have them calculated (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md)), or set explicit coefficients ([`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)). For information on Bayer color conversion, refer to [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

If you don't want to perform Bayer color conversion in hardware, disable it using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md).

The [`M_BAYER...`](../../Reference/dig/MdigControl.md) control types of [`MdigControl`](../../Reference/dig/MdigControl.md) can only be used when grabbing from a camera that has a Bayer color filter (as specified by the DCF); otherwise, an error will be generated.

## Using frame burst with a multi-frame buffer

Zebra Rapixo CXP supports frame burst technology. This technology allows you to grab a group of sequential frames into a multi-frame buffer with one grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md), or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)); the defined number of frames are stored contiguously in the same buffer. The end-of-grab event only occurs once the entire group of frames has been grabbed, reducing the number of events that need to be handled. This is useful in cases where you have a high frame rate and need to ensure that no frames are missed. Note that a user-defined function hooked to the end-of-grab event ([`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md)) is executed only once the entire group of frames has been grabbed. For information on creating a multi-frame image buffer to store sequential frames, see [Specifying the dimensions of a multi-frame image buffer](../../UserGuide/C23_Data_buffers/S03_Specifying_the_dimensions_of_a_data_buffer.md).

## Using on-board flat-field correction

Zebra Rapixo CXP Pro Quad CXP-12 supports flat-field correction on-board without Host intervention. Images taken in environments with uneven lighting can affect the quality of your processing. To correct this, an on-board flat-field correction can be applied to images so that the intensity across the image is even before processing begins. To use flat-field correction, you need to specify an on-board buffer that contains the gain values, and optionally another that contains the offset values, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_SHADING_CORRECTION_GAIN_ID`](../../Reference/dig/MdigControl.md) and [`M_SHADING_CORRECTION_OFFSET_ID`](../../Reference/dig/MdigControl.md), respectively. The buffers should be the same size as the grabbed image, and the gain values should be in the specified fixed point format ([`M_SHADING_CORRECTION_GAIN_FIXED_POINT`](../../Reference/dig/MdigControl.md)). The aggregate bandwidth to read the gain buffer and offset buffer from on-board memory cannot be bigger than the internal maximum bandwidth of the DMA port (8 Gbytes/sec). There is a line limitation of 64 K pixels per line and 1 M lines per frame. Once you have set the buffers, enable the correction using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_SHADING_CORRECTION`](../../Reference/dig/MdigControl.md).

> **Note:** Note that, flat-field correction is only available on Zebra Rapixo CXP Pro Quad CXP-12 with the appropriate firmware installed. Flat-field correction is not available when using Zebra Rapixo CXP Pro Quad CXP-12 with GenTL.

## Extracting peaks from an on-board multi-frame image buffer

Zebra Rapixo CXP Pro Quad CXP-12 can perform [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md)on-board without Host intervention if the specified buffer is an on-board buffer. This functionality is only available if the loaded FPGA configuration contains the Zebra PU that supports this functionality. Supporting this function on-board allows the board to perform sheet of light (laser line) extraction. The sheet of light must appear horizontally in the image. The board can extract more than one peak per lane (up to 3). In addition, the board can operate on a single frame or multi-frame on-board image buffer. For more information on how to use [`MimLocatePeak1d`](../../Reference/im/MimLocatePeak1d.md) for peak extraction from a multi-frame image buffer, see [Extraction of peaks from a multi-frame buffer](../../UserGuide/C05_Specialized_image_processing/S10_Peak_detection_and_depth_maps.md).

> **Note:** Note that, peak-extraction is only available on Zebra Rapixo CXP Pro Quad CXP-12 with the appropriate firmware installed. Peak-extraction is not available when using Zebra Rapixo CXP Pro Quad CXP-12 with GenTL.

## Data forwarding with Zebra Rapixo CXP

The Zebra Rapixo CXP Quad Data Forwarding model supports data forwarding. This allows you to distribute the image processing workload across multiple computers. This feature enables the relaying of images to another computer using up to four output connections running at up to 12.5 Gbits/sec. The number of output connections used must equal the number of input connections. Data forwarding is necessary in situations where the amount of information to process is too much for a single computer to keep up with, without dropping frames. Images can be retransmitted to multiple computers in a daisy chain fashion by equipping each computer with a Zebra Rapixo CXP Quad Data Forwarding board; the last node in the chain does not need the Data Forwarding model. Depending on the processing that must be done, each computer in the chain can process a different portion of each incoming image, process alternate images (or nth image), or perform different independent required operations on each image.

The following diagram depicts two Data Forwarding boards in daisy chain fashion, with a main connection and an extension connection.

*[Image: Rapixo_DF_connect.png]*

> **Note:** Note that, you should connect the main connection to connector C0.

To use data forwarding, the Data Forwarding board in the first computer in the chain must go through the standard discovery procedure with the main connection and establish its speed. Only the first Data Forwarding board in the chain has direct communication with the camera. The subsequent Data Forwarding boards will need to have their DCFs configured with the speed, number of links, size of the image, and pixel format information. The Data Forwarding DCF Maker creates a DCF file that stores the connection details and feature settings of a CoaXpress camera connected to a Zebra Rapixo CXP. This type of DCF can only be used by additional Zebra CXP boards daisy-chained to the first Zebra Rapixo CXP DF. Once the speed is established with the camera and the DCFs are configured, start a grab on the input of the first Data Forwarding board before an output can be grabbed by the next board.

> **Note:** Note that, when using PoCXP with the Data Forwarding board, only the board connected to the camera needs to have its 12 V auxiliary connector connected to a 12 V auxiliary source, since it is only this board that will provide power to the camera.
