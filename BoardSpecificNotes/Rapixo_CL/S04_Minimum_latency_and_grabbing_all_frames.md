---
doctype: BoardSpecificNotes
part: ""
chapter: Rapixo_CL
section: Minimum_latency_and_grabbing_all_frames
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / rapixo-cl-pro / Minimum latency and grabbing all frames"
---

# Minimum latency and grabbing all frames

When grabbing with either [`MdigGrab`](../../Reference/dig/MdigGrab.md) or [`MdigProcess`](../../Reference/dig/MdigProcess.md), Zebra Rapixo CL is optimized to have the smallest latency possible between the last pixel sent from the camera to the last pixel written into the buffer. This allows the buffer to be available for processing with a minimum amount of delay.

On-board memory is used to protect against PCIe latency. By default, Zebra Rapixo CL grabs into off-board buffers. To perform real-time grabs, your rate of acquisition (grabbing bandwidth) must be lower than the PCIe maximum transfer rate (PCIe bandwidth). If the acquisition rate is higher than the transfer rate across the PCIe bus, frames will be skipped rather than allowing a grab over-run to occur. To determine the image transfer speed from Zebra Rapixo CL to the Host, use the Rapixo CL Bench utility.

## Detecting missed frames when using MdigProcess

To be certain that no frames were missed when [`MdigProcess`](../../Reference/dig/MdigProcess.md) was last used, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_PROCESS_FRAME_MISSED`](../../Reference/dig/MdigInquire.md). This value is based on a comparison between the frame rate ([`M_PROCESS_FRAME_RATE`](../../Reference/dig/MdigInquire.md)) and the frame count ([`M_PROCESS_FRAME_COUNT`](../../Reference/dig/MdigInquire.md)). If a grab is triggered from a temperamental trigger or if the hooked function is slow, the value returned by [`M_PROCESS_FRAME_MISSED`](../../Reference/dig/MdigInquire.md) will be affected. This value is most trustworthy with a camera in continuous mode. You can inquire the frame time-stamp sampled at the end of the frame using [`MdigGetHookInfo`](../../Reference/dig/MdigGetHookInfo.md) with [`M_TIME_STAMP`](../../Reference/dig/MdigGetHookInfo.md).

Use [`M_PROCESS_FRAME_COUNT`](../../Reference/dig/MdigInquire.md) to determine the total number of frames grabbed in the sequence.

Use [`M_PROCESS_FRAME_RATE`](../../Reference/dig/MdigInquire.md) to return an estimate of the frame rate in frames per second. Note that this value is determined based on the average of the camera's frame rate, and the total amount of time to process the user-defined hooked function(s).

## Quickly copying an on-board buffer to a Host destination buffer

When copying an on-board buffer into a Host destination buffer, the copy operation should be performed by your Zebra Rapixo CL using the Zebra Rapixo CL's DMA engine, rather than the Host. Your Zebra Rapixo CL will perform the operation faster than the Host. To ensure that Zebra Rapixo CL performs the operation, use the following:

- To copy an on-board buffer into a destination buffer on the Host, use [`MbufCopy`](../../Reference/buf/MbufCopy.md).
- To resize an on-board buffer while copying it to a destination buffer on the Host, use[`MimResize`](../../Reference/im/MimResize.md) with a factor of 1 to 16 and with an interpolation mode of [`M_NEAREST_NEIGHBOR`](../../Reference/im/MimResize.md). Alternatively, you can use [`MbufTransfer`](../../Reference/buf/MbufTransfer.md) to perform the same operation.
- To flip an on-board buffer while copying it to a destination buffer on the Host, use [`MimFlip`](../../Reference/im/MimFlip.md).

> **Note:** Note that for your Zebra Rapixo CL to perform these operations, the destination Host buffer must be allocated in non-paged memory (that is, using [`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with + [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+ [`M_NON_PAGED`](../../Reference/buf/MbufAllocColor.md)).

## Quickly converting buffer formats while copying

When copying an on-board buffer into a destination Host buffer, the color space converter and image formatter can automatically convert the bit-depth and color format of the source buffer to the bit-depth and color format of the destination buffer.

For on-board conversion of buffers to take place, the following on-board image and destination depth and color formats are supported.

| On-board buffer depth and color format (specified in the DCF). | Host destination buffer depth and color format. |
| --- | --- |
| 8-bit monochrome. | 16-bit monochrome. | [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR24`](../../Reference/buf/MbufAllocColor.md). | [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR32`](../../Reference/buf/MbufAllocColor.md). | [`M_YUV16_YUYV`](../../Reference/buf/MbufAllocColor.md)/[`M_YUV16`](../../Reference/buf/MbufAllocColor.md). | [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)+ [`M_RGB24`](../../Reference/buf/MbufAllocColor.md). | [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md)+ [`M_RGB48`](../../Reference/buf/MbufAllocColor.md). |
| 8-bit monochrome. | Yes. | No. | Yes. | No. | Yes. | Yes. | No. |
| 16-bit monochrome. | Yes. | Yes. | Yes. | Yes. | Yes. | Yes. | Yes. |
| [`M_PACKED`](../../Reference/buf/MbufAllocColor.md)+ [`M_BGR24`](../../Reference/buf/MbufAllocColor.md). | Yes. | No. | Yes. | No. | Yes. | Yes. | No. |
| [`M_RGB48`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | Yes. | Yes. | Yes. | Yes. | Yes. | Yes. | Yes. |

For information about YUV or RGB/BGR buffers, refer to [Data buffers](../../UserGuide/C23_Data_buffers/ChapterInformation.md). For the calculations used to convert to YUV, refer to the Zebra Rapixo CL Pro_ Hardware and Installation_ manual.
