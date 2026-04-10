---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Using_Rapixo_CL
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Using Rapixo CL"
---

# Using Zebra Rapixo CL with Aurora Imaging Library

To use Zebra Rapixo CL, you must allocate it using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_RAPIXOCL`](../../Reference/sys/MsysAlloc.md). This allocation opens communication with the Zebra Rapixo CL board and allows Aurora Imaging Library to use its resources. You can allocate an Aurora Imaging Library Rapixo CL system for your board in multiple processes (executables). However, different processes cannot allocate a digitizer for the same acquisition paths.

Zebra Rapixo CL features four quadrature decoders that can decode input from rotary or linear encoders with quadrature output. For more information on this feature, refer to [Using quadrature input from a rotary encoder](../../UserGuide/C56_IO_signals/S07_Using_quadrature_input_from_a_rotary_encoder.md). This chapter also provides information on how to configure and use the I/O signals.

Camera Link video sources, attached to Zebra Rapixo CL, can use a GenICam interface. For information regarding using Aurora Imaging Library to access the GenICam SFNC-compatible features of your camera, see [Using Aurora Imaging Library with GenICam](../../UserGuide/C27_Grabbing_with_your_digitizer/S14_Using_GenICam.md).

For information regarding the use of your Zebra Rapixo CL Pro's Processing FPGA, see [Using Aurora Imaging Library for FPGA processing](../../UserGuide/C67_Using_AIL_with_FPGA_processing/ChapterInformation.md).

## Performing Bayer color conversion in hardware

When Zebra Rapixo CL grabs color images from a video source with a Bayer color filter (as specified by the DCF), it performs Bayer color conversion in hardware, as it transfers the images to the Host. If the images require white balancing, Zebra Rapixo CL can perform this automatically if white balancing is enabled using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_ENABLE`](../../Reference/dig/MdigControl.md). If performing white balancing, you can use the default white balance coefficients, automatically have them calculated (using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_WHITE_BALANCE`](../../Reference/dig/MdigControl.md) set to [`M_CALCULATE`](../../Reference/dig/MdigControl.md)), or set explicit coefficients ([`M_BAYER_COEFFICIENTS_ID`](../../Reference/dig/MdigControl.md)). For information on Bayer color conversion, refer to [Using images acquired with a Bayer color filter](../../UserGuide/C23_Data_buffers/S16_Using_buffers_with_the_Bayer_color_filter.md).

If you don't want to perform Bayer color conversion in hardware, disable it using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_BAYER_CONVERSION`](../../Reference/dig/MdigControl.md) set to [`M_DISABLE`](../../Reference/dig/MdigControl.md).

The [`M_BAYER...`](../../Reference/dig/MdigControl.md) control types of [`MdigControl`](../../Reference/dig/MdigControl.md) can only be used when grabbing from a camera that has a Bayer color filter (as specified by the DCF); otherwise, an error will be generated.

## Using frame burst with a multi-frame buffer

Zebra Rapixo CL supports frame burst technology. This technology allows you to grab a group of sequential frames into a multi-frame buffer with one grab command ([`MdigGrab`](../../Reference/dig/MdigGrab.md), or one grab of [`MdigProcess`](../../Reference/dig/MdigProcess.md)); the defined number of frames are stored contiguously in the same buffer. The end-of-grab event only occurs once the entire group of frames has been grabbed, reducing the number of events that need to be handled. This is useful in cases where you have a high frame rate and need to ensure that no frames are missed. Note that a user-defined function hooked to the end-of-grab event ([`MdigProcess`](../../Reference/dig/MdigProcess.md), or [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) with [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md)) is executed only once the entire group of frames has been grabbed. For information on creating a multi-frame image buffer to store sequential frames, see [Specifying the dimensions of a multi-frame image buffer](../../UserGuide/C23_Data_buffers/S03_Specifying_the_dimensions_of_a_data_buffer.md).
