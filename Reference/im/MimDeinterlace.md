---
doctype: Reference
module: im
function: MimDeinterlace
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / im / MimDeinterlace"
---

# MimDeinterlace

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

> Produce a sequence of deinterlaced images from a sequence of images acquired from an interlaced camera.

## Syntax

```c
void MimDeinterlace(
    AIL_ID         DeinterlaceContextImId,  //in
    const AIL_ID * SrcImageArrayPtr,        //in
    const AIL_ID * DstImageArrayPtr,        //in
    AIL_INT        SrcImageCount,           //in
    AIL_INT        DstImageCount,           //in
    AIL_INT64      ControlFlag              //in
)
```

## Description

This function produces a sequence of deinterlaced images from a sequence of images grabbed from an interlaced camera. When an image is acquired using an interlaced camera, the even and odd fields are not taken at the exact same moment in time. If the object in the image was in motion, there is an offset between the position of the object in one field and its position in the other. Simply combining these two fields to produce a deinterlaced image results in noticeable deformities in moving objects, called interlacing artifacts. [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md) uses averaging techniques to reduce or remove these interlacing artifacts and produce a higher quality deinterlaced image. To perform the deinterlacing operation, you can select one of several deinterlacing algorithms using [`MimControl`](../../Reference/im/MimControl.md) with [`M_DEINTERLACE_TYPE`](../../Reference/im/MimControl.md). They can be applied to the entire image or only to the pixels that are considered part of a moving object. To apply the algorithm to the latter, select the adaptive version of the algorithm (for example, [`M_ADAPTIVE_DISCARD`](../../Reference/im/MimControl.md)).

Often, the order in which the fields are grabbed will depend on the camera being used. When using an algorithm that produces two output frames (for example, [`M_BOB`](../../Reference/im/MimControl.md) or [`M_ADAPTIVE_BOB`](../../Reference/im/MimControl.md)), it is important to set the field that is grabbed first using [`MimControl`](../../Reference/im/MimControl.md) with [`M_FIRST_FIELD`](../../Reference/im/MimControl.md). This will ensure that the sequence of output frames occur in chronological order and that the video stream is fluid.

The adaptive algorithms apply a motion detection algorithm to dynamically determine which pixels are part of an object in motion and which pixels are part of the background, prior to performing the deinterlacing algorithm. Using [`MimControl`](../../Reference/im/MimControl.md), you can set the number of neighboring frames used for motion detection ([`M_MOTION_DETECT_NUM_FRAMES`](../../Reference/im/MimControl.md)), the location of the frame to be processed within this group of frames ([`M_MOTION_DETECT_REFERENCE_FRAME`](../../Reference/im/MimControl.md)), and the threshold value used to differentiate between pixels of objects in motion and pixels of background objects ([`M_MOTION_DETECT_THRESHOLD`](../../Reference/im/MimControl.md)). To visualize which pixels are affected, you can have [`MimDeinterlace`](../../Reference/im/MimDeinterlace.md) output an image of these pixels, using [`MimControl`](../../Reference/im/MimControl.md) with [`M_MOTION_DETECT_OUTPUT`](../../Reference/im/MimControl.md); pixels that are part of an object in motion are set to the maximum value (0xFF for an a bit image), while the other pixels are set to 0. In this case, the deinterlacing part of the adaptive algorithm is not executed.

## Parameters

### `DeinterlaceContextImId` *(in, AIL_ID)*

Specifies the identifier of the image processing context to be used for deinterlacing. The image processing context must have been previously allocated on the system using [`MimAlloc`](../../Reference/im/MimAlloc.md).

### `SrcImageArrayPtr` *(in, *AIL_ID)*

Specifies an array containing the identifiers of the buffers of the images to deinterlace. Only 8-bit and 16-bit buffers are supported. All source images must be of the same type, same format, and on the same system. Note that all source and destination images must have the same size.

### `DstImageArrayPtr` *(in, *AIL_ID)*

Specifies an array containing the identifiers of the buffers of the images in which to save the deinterlaced images. Only 8-bit and 16-bit buffers are supported. All destination images must be of the same type, same format, and on the same system. Note that all source and destination images must have the same size.

### `SrcImageCount` *(in, AIL_INT)*

Specifies the number of images in the source sequence.

### `DstImageCount` *(in, AIL_INT)*

Specifies the number of deinterlaced images in the destination sequence.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future use. Should be set to `M_DEFAULT`.
