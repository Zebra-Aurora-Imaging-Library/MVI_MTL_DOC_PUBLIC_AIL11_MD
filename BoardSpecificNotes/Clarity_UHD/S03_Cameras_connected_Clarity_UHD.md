---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Cameras_connected_Clarity_UHD
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Cameras connected Clarity UHD"
---

# Cameras that can be connected to Zebra Clarity UHD

You can allocate up to eight independent Aurora Imaging Library digitizers on your Zebra Clarity UHD, using [`MdigAlloc`](../../Reference/dig/MdigAlloc.md).

The connector used determines the digitizer device number assigned. In all but two cases, each connector uses an independent acquisition path. However, [`M_DEV2`](../../Reference/dig/MdigAlloc.md) corresponds to both an HDMI and an analog connector; both of these connectors use the same independent acquisition path. Therefore, if both connectors are connected to a camera, you cannot grab simultaneously from both cameras. This is also the case for [`M_DEV3`](../../Reference/dig/MdigAlloc.md).

| Digitizer device number | Connector Name | Resolutions supported |
| --- | --- | --- |
| [`M_DEV0`](../../Reference/dig/MdigAlloc.md) | Mini-HDMI (type C) connector 0 | Supports grabbing at up to DVI-D and HDMI 2.0 non-interlaced (up to 2160p60) and interlaced resolutions (1080i50, 1080i59.94, and 1080i60). |
| [`M_DEV1`](../../Reference/dig/MdigAlloc.md) | Mini-HDMI (type C) connector 1 |
| [`M_DEV2`](../../Reference/dig/MdigAlloc.md) | Mini-HDMI (type C) connector 2 | Supports grabbing at up to DVI-D and HDMI 1.4 non-interlaced (up to 1920x1200p60) and interlaced resolutions (1080i50, 1080i59.94, and 1080i60). |
| Analog connector 0 | Supports grabbing at up to DVI-A (analog) non-interlaced (up to 1920x1200p60) and interlaced resolutions (1080i50 fps, 1080i59.94, and 1080i60); in addition, it supports grabbing at SD (analog) resolutions for RS170 (720 x 486 i30), CCIR (720 x 576 i25), NTSC (720 x 486 i30), NTSC/YC (720 x 486 i30), PAL (720 x 576 i25), PAL/YC (720 x 576 i25), NTSC RGB (640 x 480 i30), PAL RGB (768 x 576 i25). |
| [`M_DEV3`](../../Reference/dig/MdigAlloc.md) | Mini-HDMI (type C) connector 3 | Same as [`M_DEV2`](../../Reference/dig/MdigAlloc.md). |
| Analog connector 1 |
| [`M_DEV4`](../../Reference/dig/MdigAlloc.md) | Mini Display Port 1.2 connector 0 | Supports grabbing at Display Port 1.2 non-interlaced resolutions of up to 2160p60 and interlaced resolutions of 1080i50, 1080i59.94, and 1080i60. |
| [`M_DEV5`](../../Reference/dig/MdigAlloc.md) | Mini Display Port 1.2 connector 1 |
| [`M_DEV6`](../../Reference/dig/MdigAlloc.md) | SDI connector 0 | Supports grabbing at up to: SD resolutions (NTSC (720x486 @30fps) and PAL (720x576 @25 fps). HD resolutions (720p50, 720p59.94, 720p60, 1080p23.98, 1080p24, 1080p25, 1080p29.97, 1080p30, 1080p50, 1080p59.94, 1080p60, 1080i50, 1080i59.94, 1080i60). 2K DCI resolutions (2K 23.98p, 2K 24p, 2K 25p). UHD resolutions (2160p23.98, 2160p24, 2160p25, 2160p29.97, 2160p30, 2160p50, 2160p59.94, 2160p60). 4K DCI resolutions (4K 23.98p, 4K 24p, 4K 25p). |
| [`M_DEV7`](../../Reference/dig/MdigAlloc.md) | SDI connector 1 |

In addition, if not used with a component RGB camera,[`M_DEV2`](../../Reference/dig/MdigAlloc.md) and [`M_DEV3`](../../Reference/dig/MdigAlloc.md) have three data channels. You can connect a composite color/monochrome camera to each channel and switch between them, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_CHANNEL`](../../Reference/dig/MdigControl.md). You can only grab from one data channel per [`M_DEVn`](../../Reference/dig/MdigAlloc.md)at a time.

## Data input channels of acquisition paths

For [`M_DEV2`](../../Reference/dig/MdigAlloc.md) and [`M_DEV3`](../../Reference/dig/MdigAlloc.md), you can choose to use the 3 data input channels independently (meaning that you grab from only one channel at a time). If you have a camera that is connected to a channel other than the first of its digitizer, you must specify the channel, using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_CHANNEL`](../../Reference/dig/MdigControl.md).

*[Image: digitizer_multiplexed.png]*

## Switching between cameras of the same type

When connecting several cameras of the same data format to different data input channels of a digitizer, allocate a single digitizer with the appropriate DCF for the first camera and use [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_CHANNEL`](../../Reference/dig/MdigControl.md) to switch between the others of the same type.

## Switching between cameras of different types

When connecting cameras of different data formats to different data input channels of a digitizer, you must use a different DCF for each camera. You could allocate a digitizer, grab the required frame, free the digitizer, and then allocate the digitizer again with the second DCF; this can increase the time required for the operation. Aurora Imaging Library can circumvent this problem by using a fast DCF-switching technique which is outlined in the steps below:

1. Make as many calls to [`MdigAlloc`](../../Reference/dig/MdigAlloc.md) as you have cameras, with different formats, from which you want to grab.
2. Specify the required digitizer settings using [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigHookFunction`](../../Reference/dig/MdigHookFunction.md) for each allocated digitizer.
3. Specify the channel to use for the grab using [`M_CHANNEL`](../../Reference/dig/MdigControl.md).
4. Call [`MdigGrab`](../../Reference/dig/MdigGrab.md) with an allocated digitizer identifier. Note that if this call uses a digitizer identifier different from the previous call, a DCF switch will occur.
   > **Note:** If there is a grab in progress with one digitizer, calling any of the following functions with any other digitizer will result in an error: [`MdigGrab`](../../Reference/dig/MdigGrab.md), [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md), [`MdigInquire`](../../Reference/dig/MdigInquire.md), [`MdigProcess`](../../Reference/dig/MdigProcess.md), [`MdigAlloc`](../../Reference/dig/MdigAlloc.md), and [`MdigFree`](../../Reference/dig/MdigFree.md). To synchronize your grabs with different digitizers, use [`MthrWait`](../../Reference/thr/MthrWait.md).

## Grabbing a single field

With interlaced scanning cameras, 2 fields are grabbed by default; therefore one call to [`MdigGrab`](../../Reference/dig/MdigGrab.md) will grab both the odd and even fields. You can change the number of fields to 1 and have Aurora Imaging Library treat each field as one frame using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_FIELD_NUM`](../../Reference/dig/MdigControl.md), grabbing every second row and storing them in sequential rows. Therefore, the grab time is reduced by half. This control type can only be set to 1 or 2, and should only be used for interlaced video. When set to 1, each field is treated like a frame and the following digitizer events occur relative to the grabbed field: [`M_GRAB_FRAME_START`](../../Reference/dig/MdigHookFunction.md), [`M_GRAB_END`](../../Reference/dig/MdigHookFunction.md), and [`M_GRAB_FRAME_END`](../../Reference/dig/MdigHookFunction.md).

To achieve 60 fps in NTSC or 50 fps in PAL, the control type [`M_GRAB_START_MODE`](../../Reference/dig/MdigControl.md) must be set to [`M_FIELD_START`](../../Reference/dig/MdigControl.md).
