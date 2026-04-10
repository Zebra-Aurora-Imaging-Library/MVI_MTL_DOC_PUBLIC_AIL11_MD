---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Additional_Clarity_UHD_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Additional Clarity UHD notes User guide"
---

# Miscellaneous user guide information for Zebra Clarity UHD

This section includes additional user guide information for Zebra Clarity UHD. The information found in this section might be a reiteration of content previously documented.

## Grabbing capabilities

In addition to the grab buffer pixel formats listed in the [Using Zebra Clarity UHD](S02_Using_Clarity_UHD.md), the formats in the following table are also supported:

Before grabbing into the buffer, if you allocate a grab buffer with an **M_DYNAMIC** attribute, you must specify the format in which to convert and store the input data using [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_PFNC_TARGET_FORMAT**. The format can be changed in between grabs. A buffer with an**M_DYNAMIC** attribute cannot be used with most other Aurora Imaging Library functions. However, you can access the data using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_HOST_ADDRESS`](../../Reference/buf/MbufInquire.md) and process the data directly using a custom function. You can also use the buffer with the Sequence module (Mseq).

| Aurora Imaging Library grab buffer format | [`MdigControl`](../../Reference/dig/MdigControl.md) with **M_PFNC_TARGET_FORMAT** | Description |
| --- | --- | --- |
| **M_DYNAMIC** | PFNC_BGRa10p | 10-bit color depth with a BGRa format (B:10,G:10,R:10, A:2). This format is not supported when grabbing from a 3840x2160@50Hz source or higher. |
| **M_DYNAMIC** | PFNC_YCbCr411_8 | 8-bit color depth with a YUV 4:2:0 format. Note that the actual data is in YUV 4:2:0 (not YUV 4:1:1) because it is not yet defined in the PFNC specification. The data is in 2 planes. One for the luminance and the other for the chrominance. |
| **M_DYNAMIC** | PFNC_YCbCr422_10p | 10-bit packed color depth with a V210 format (YUV 4:2:2 10-bit) |

## Encoding capabilities

The following table lists the maximum number of streams that can be simultaneously grabbed, displayed, and encoded depending on the stream resolution, frame rate, and the destination buffer pixel format.

| Buffer pixel format | Encoding profile ([`MseqControl`](../../Reference/seq/MseqControl.md) with [`M_STREAM_PROFILE`](../../Reference/seq/MseqControl.md)), using the default encoding settings | Maximum number of streams that can be grabbed, displayed, and encoded. |
| --- | --- | --- |
| Monochrome 8-bit | **M_PROFILE_HIGH** Buffer is internally converted to YUV 4:2:0 before encoding | one 2160p60 stream or |
| 3 x 2160p30 streams or |
| 6 x 1080p60 streams |
| YUV 4:2:0 8-bit | **M_PROFILE_HIGH** | 2 x 2160p60 streams or |
| 4 x 2160p30 streams or |
| 8 x 1080p60 streams |
| YUV 4:2:2 8-bit | **M_PROFILE_HIGH422** | one 2160p60 stream or |
| 3 x 2160p30 streams or |
| 6 x 1080p60 streams |
| YUV 4:2:2 10-bit | **M_PROFILE_HIGH422** (Encoding in 10-bit) | one 2160p60 stream or |
| 2 x 2160p30 streams or |
| 5 x 1080p60 streams |
| RGB 8-bit planar | **M_PROFILE_HIGH** Buffer is internally converted to YUV 4:2:0 before encoding | one 2160p60 stream or |
| 3 x 2160p30 streams or |
| 6 x 1080p60 streams |
| BGR32 8-bit packed | **M_PROFILE_HIGH** Buffer is internally converted to YUV 4:2:0 before encoding | one 2160p60 stream or |
| 3 x 2160p30 streams or |
| 5 x 1080p60 streams |
| RGB 10-bit packed | **M_PROFILE_HIGH** Buffer is internally converted to YUV 4:2:0 before encoding | 2 x 2160p30 streams or |
| 5 x 1080p60 streams |

## Particularities

The following are some particularities specific to the Clarity UHD.

- If autodetect DCF does not recognize some analog component YPbPr cameras, you can manually specify the DCF using the Aurora Imaging Configurator or using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).
- When grabbing from an SDI source into a monochrome 8-bit or color BGR32 packed buffer, the Clarity UHD automatically selects the proper color space equations depending on the input resolution (for example, ITU-R BT.601 for SD, ITU-R BT.709 for HD and ITU-R BT.2020 for UHD). Note that when grabbing into a YUV16 packed buffer, the input data is preserved in its original format. The color space can be inquired using [`MbufInquire`](../../Reference/buf/MbufInquire.md) with [`M_YCBCR_RANGE`](../../Reference/buf/MbufInquire.md).
- For the SDI inputs, dual-link, 3D, and RGB 4:4:4 are not supported.
