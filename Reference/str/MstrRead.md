---
doctype: Reference
module: str
function: MstrRead
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrRead"
---

# MstrRead

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

> Read strings from a target image.

## Syntax

```c
void MstrRead(
    AIL_ID ContextId,      //in
    AIL_ID TargetImageId,  //in
    AIL_ID ResultId        //out
)
```

## Description

This function reads one or more strings from the specified target image using the specified String Reader context. All specifications, such as the number of strings to read in the target image, the speed and/or robustness settings, the constraints on the characters in the string, and the font used, depend on the settings of the String Reader context, which can be set using [`MstrControl`](../../Reference/str/MstrControl.md), [`MstrSetConstraint`](../../Reference/str/MstrSetConstraint.md) and [`MstrEditFont`](../../Reference/str/MstrEditFont.md). All settings are taken into account by [`MstrRead`](../../Reference/str/MstrRead.md), and results are stored in the specified result buffer. Results can be read from the result buffer using the [`MstrGetResult`](../../Reference/str/MstrGetResult.md) function.

For a string to be read, it must not only meet all of the restrictions placed on it by the String Reader context, but it must also obey the basic rules of a string. That is, it must be a linear sequence of regular characters.

The string read will be ordered in the natural Latin-based reading order. The position of a string is the position of the center of the bounding box of its first character.

You can limit the read operation to a region of the image buffer, using a rectangular ROI set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

The target image must be 1-band 8-bit unsigned, and cannot exceed a maximum size of 65536x65536 pixels.

Note that before performing a read operation, the String Reader context must have at least one font, one character, and one string; it must also be preprocessed ([`MstrPreprocess`](../../Reference/str/MstrPreprocess.md)).

If your target image was associated with a camera calibration context, positional and dimensional results are, by default, returned with respect to the relative coordinate system of the image. Otherwise, these results are returned in pixels, relative to the center of top-left pixel in the target image.

If your target image was associated with a camera calibration context but you want to retrieve positional and dimensional results in pixel units, use [`MstrControl`](../../Reference/str/MstrControl.md) with the [`M_RESULT_OUTPUT_UNITS`](../../Reference/str/MstrControl.md) control type set to [`M_PIXEL`](../../Reference/str/MstrControl.md). However, if you set [`M_RESULT_OUTPUT_UNITS`](../../Reference/str/MstrControl.md) to [`M_WORLD`](../../Reference/str/MstrControl.md) without specifying a calibrated image in which to calculate the results, [`MstrGetResult`](../../Reference/str/MstrGetResult.md) generates an error.

In the presence of distortion, some results are meaningless when converted from real-world to pixel units (such as angle and scale). For example, if a string appears warped in the target image, but the camera calibration context compensates for this, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system. If complex distortions exist, correct the image before using it with [`MstrRead`](../../Reference/str/MstrRead.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the String Reader context to use for the read operation. The String Reader context must have been previously allocated on the required system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md), and preprocessed using [`MstrPreprocess`](../../Reference/str/MstrPreprocess.md).

### `TargetImageId` *(in, AIL_ID)*

Specifies the target image in which to read the strings.

### `ResultId` *(out, AIL_ID)*

Specifies the result buffer in which to write the results of the read operation.
