---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo-CoF
section: Minimum_latency_and_grabbing_all_frames
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cof / Minimum latency and grabbing all frames"
---

# Minimum latency and grabbing all frames

When grabbing with either [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md), Zebra Rapixo CoF is optimized to have the smallest latency possible between the last pixel sent from the camera to the last pixel written into the buffer. This allows the buffer to be available for processing with a minimum amount of delay.

By default, Zebra Rapixo CoF grabs into buffers in Host memory. On-board memory is used to protect against PCIe latency. To perform real-time grabs, your rate of acquisition (grabbing bandwidth) must be lower than the PCIe maximum transfer rate (PCIe bandwidth). If the acquisition rate is higher than the transfer rate across the PCIe bus, frames will be skipped rather than allowing a grab over-run to occur. To determine the image transfer speed from Zebra Rapixo CoF to the Host, use the Zebra Rapixo CoF Bench utility.

## Detecting missed frames

To be certain that no frames were missed when [`MdigProcess`](../../Reference/dig/MdigProcess.md) was last used, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_PROCESS_FRAME_MISSED`](../../Reference/dig/MdigInquire.md). This value reads the number of frames that were transmitted by the camera when no buffer was available to receive the frame; for example, if a grab is triggered and the associated hooked function is slow, then a frame can be missed.

## Efficiently copying an on-board buffer to a destination buffer in Host memory

When copying an on-board buffer into a destination buffer in Host memory, the copy operation is performed by your Zebra Rapixo CoF using the board's DMA write engine, rather than the Host. Your Zebra Rapixo CoF will perform the operation faster than the Host. To ensure that Zebra Rapixo CoF performs the operation, use the following:

- To copy an on-board buffer into a destination buffer in Host memory, use [`MbufCopy`](../../Reference/buf/MbufCopy.md).
- To resize an on-board buffer while copying it to a destination buffer in Host memory, use[`MimResize`](../../Reference/im/MimResize.md) with a factor of 1/n, where n is a value between 1 to 16, and with an interpolation mode of [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimResize.md). Alternatively, you can use [`MbufTransfer`](../../Reference/buf/MbufTransfer.md) to perform the same operation.
- To flip an on-board buffer while copying it to a destination buffer in Host memory, use [`MimFlip`](../../Reference/im/MimFlip.md).

> **Note:** Note that for your Zebra Rapixo CoF to perform these operations, the destination buffer must be allocated in non-paged memory (that is, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with + [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+ [`M_NON_PAGED`](../../Reference/buf/MbufAllocColor.md)).

## Efficiently converting buffer formats while copying

When copying an on-board buffer into a destination buffer in Host memory, the color space converter and image formatter can automatically convert the bit-depth and color format of the source buffer to the bit-depth and color format of the destination buffer.

For on-board conversion of buffers to take place, the following on-board buffer and destination buffer depth and color formats are supported.

| On-board buffer depth and color format (specified in the DCF) | Destination buffer depth and color format |
| --- | --- |
| 8-bit monochrome | 16-bit monochrome | [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR24`](../../Reference/buf/MbufAllocColor.md) | [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR32`](../../Reference/buf/MbufAllocColor.md) | [`M_YUV16_YUYV`](../../Reference/buf/MbufAllocColor.md)/[`M_YUV16`](../../Reference/buf/MbufAllocColor.md) | [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)+ [`M_RGB24`](../../Reference/buf/MbufAllocColor.md) | [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)+ [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) |
| 8-bit monochrome | Yes | No | Yes | Yes | Yes | Yes | No |
| 16-bit monochrome | Yes | Yes | Yes | Yes | Yes | Yes | Yes |
| [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR24`](../../Reference/buf/MbufAllocColor.md) | Yes | No | Yes | Yes | Yes | Yes | No |
| [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | Yes | Yes | Yes | Yes | Yes | Yes | Yes |

For information about YUV or RGB/BGR buffers, refer to [Data buffers](../../UserGuide/C23_Data_buffers/ChapterInformation.md). For the calculations used to convert to YUV, refer to the _Zebra Rapixo CoF Hardware and Installation_ manual.
