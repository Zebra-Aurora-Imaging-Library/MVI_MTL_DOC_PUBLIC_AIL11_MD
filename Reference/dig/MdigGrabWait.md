---
doctype: Reference
module: dig
function: MdigGrabWait
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / dig / MdigGrabWait"
---

# MdigGrabWait

| Board | Supported |
| --- | --- |
| Host System | Partial |
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

> Wait for the end of the grab in progress.

## Syntax

```c
void MdigGrabWait(
    AIL_ID    DigId,       //in
    AIL_INT64 ControlFlag  //in
)
```

## Description

This function allows you to temporarily override the grab mode on the specified digitizer (see [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_MODE`](../../Reference/dig/MdigControl.md)).

Using this function forces the grab to wait until the grab timeout value has expired (set using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GRAB_TIMEOUT`](../../Reference/dig/MdigControl.md)). Note that if the grab timeout is set to infinite, the grab will never end and this function will wait indefinitely. If the grabbed frame is not returned within the period of the timeout, an error is generated.

### System specific

| Board(s) | Note |
|---|---|
| Host System | When grabbing from a video or directory of images, once the last image is grabbed, grabbing will restart from the beginning of the video or from the first image in the directory. |

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to one of the following:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_GRAB_END` | Wait for the end of all queued grabs.

This value should not be used when grabbing data with [`MdigGrabContinuous`](../../Reference/dig/MdigGrabContinuous.md). |
| `M_GRAB_FRAME_END` | Waits for the end of the current grab. |
