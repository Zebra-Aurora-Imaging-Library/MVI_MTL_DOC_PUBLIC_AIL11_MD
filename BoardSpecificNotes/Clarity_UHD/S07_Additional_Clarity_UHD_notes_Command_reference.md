---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Additional_Clarity_UHD_notes_Command_reference
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Additional Clarity UHD notes Command reference"
---

# Miscellaneous reference information for Zebra Clarity UHD

This section includes additional Zebra Clarity UHD information regarding the command reference. The information found in this section might be a reiteration of content previously documented.

## H.264 encoding

The following section outlines additional settings regarding H.264 encoding.

| Function(s) | Parameter/value | Information |
| --- | --- | --- |
| [`MseqAlloc`](../../Reference/seq/MseqAlloc.md) | [`InitFlag`](../../Reference/seq/MseqAlloc.md) | Must be set to [`M_DEFAULT`](../../Reference/seq/MseqAlloc.md). |
| [`MseqControl`](../../Reference/seq/MseqControl.md)/[`MseqInquire`](../../Reference/seq/MseqInquire.md) | [`M_CODEC_TYPE`](../../Reference/seq/MseqInquire.md) | **M_HARDWARE** + **M_CLARITY_UHD_H264** |
| [`M_STREAM_BIT_RATE`](../../Reference/seq/MseqControl.md) | The default value is 15000 kbits/sec. |
| [`M_STREAM_BIT_RATE_MAX`](../../Reference/seq/MseqControl.md) | The default value is 30000 kbits/sec. |
| [`M_STREAM_GROUP_OF_PICTURE_SIZE`](../../Reference/seq/MseqControl.md) | The default value is 90 frames per group. |
| [`M_STREAM_LEVEL`](../../Reference/seq/MseqControl.md) | Additional supported level: [`M_LEVEL_5_2`](../../Reference/seq/MseqControl.md). |
| [`M_STREAM_PROFILE`](../../Reference/seq/MseqControl.md) | Additional supported profiles: - **M_AUTOMATIC ** is the default, which selects the profile that matches the pixel format of the buffer passed to [`MseqFeed`](../../Reference/seq/MseqFeed.md).<br/>- **M_PROFILE_EXTENDED**<br/>- **M_PROFILE_HIGH422 ** |

Zebra Clarity UHD supports encoding buffers in multiple buffer formats. This buffer can be used when calling [`MseqFeed`](../../Reference/seq/MseqFeed.md).

The most efficient encoding buffer pixel formats when using [`M_PROFILE_HIGH`](../../Reference/seq/MseqControl.md) are:

- **M_YUV12** + **M_PLANAR** + **M_ON_BOARD**.
- **M_DYNAMIC** +** M_ON_BOARD** with **M_PFNC_TARGET_FORMAT** set to **PFNC_YCbCr411_8**.

The most efficient encoding buffer pixel formats when using **M_PROFILE_HIGH422** are:

- **M_YUV16** + **M_PACKED** + **M_ON_BOARD**.
- **M_DYNAMIC** + **M_ON_BOARD** with **M_PFNC_TARGET_FORMAT** set to **PFNC_YCbCr422_10p**.

Other combinations of buffer formats and profiles are supported, but will result in an internal buffer conversion, which reduces encoding performance.

## Specifying image buffer

The following combination value is used in [`MbufAlloc1d`](../../Reference/buf/MbufAlloc1d.md) to specify the intended purpose of the image buffer.

|   |   |
| --- | --- |
| **M_DYNAMIC** | Specifies an image buffer whose bit-depth and data format are specified by the camera (see **M_PFNC_TARGET_FORMAT**). To determine the required size of the image buffer, use [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_SIZE_X`](../../Reference/dig/MdigInquire.md) and [`M_SIZE_Y`](../../Reference/dig/MdigInquire.md), respectively. This value can only be used with a buffer with the [`M_IMAGE`](../../Reference/buf/MbufAlloc1d.md) + [`M_GRAB`](../../Reference/buf/MbufAlloc1d.md) attribute. |
