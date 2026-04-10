---
doctype: Reference
module: dig
function: MdigGrabContinuous
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dig / MdigGrabContinuous"
---

# MdigGrabContinuous

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Partial |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Grab data continuously from a camera.

## Syntax

```c
void MdigGrabContinuous(
    AIL_ID DigId,                     //in
    AIL_ID DestContainerOrImageBufId  //out
)
```

## Description

This function uses the specified digitizer to continuously acquire frames of data and store this data in the destination image buffer or container, until [`MdigHalt`](../../Reference/dig/MdigHalt.md) is called. If the destination buffer/container is selected to a 2D or 3D display, the display is continuously updated, however only the last frame grabbed is written to the specified buffer/container.

When grabbing an image, you should use an image buffer as a destination. When grabbing any other type of data (such as 3D data), you should use a container as a destination. You can determine what type of Aurora Imaging Library object the digitizer is designed to grab into using [`MdigInquire`](../../Reference/dig/MdigInquire.md)with [`M_TARGET_BUFFER_OBJECT`](../../Reference/dig/MdigInquire.md).

When acquiring data from a line-scan type of camera, the exact number of rows grabbed is determined by the DCF.

If you need to save/process each grabbed frame, you should perform the grab using [`MdigProcess`](../../Reference/dig/MdigProcess.md). To perform a single grab, use[`MdigGrab`](../../Reference/dig/MdigGrab.md).

### System specific

| Board(s) | Note |
|---|---|
| Host System | When grabbing from a video or directory of images, once the last image is grabbed, grabbing will restart from the beginning of the video or from the first image in the directory. |

## Parameters

### `DigId` *(in, AIL_ID)*

Specifies the identifier of the digitizer.

### `DestContainerOrImageBufId` *(out, AIL_ID)*

Specifies the identifier of the destination image buffer or container.

*For the identifier of the destination image buffer or container*

| Value | Description |
| --- | --- |
| `Color image buffer identifier` | Specifies the identifier of a color destination image buffer with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute.This image buffer must have been previously allocated, typically using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. If the grab buffer is smaller than the digitizer's frame size, the image buffer will be filled and all other data will be lost. If the grab buffer is larger than the digitizer's frame size, the buffer will be filled up to the digitizer's frame size. The rest of the buffer will remain untouched. If you grab into a buffer from a digitizer that outputs data in a format suitable for a container, the first intensity component in the frame of data will be grabbed. If there is no intensity component, the first component that is not metadata will be grabbed.  The destination buffer's depth can only be 8 bits per band.

   The destination buffer's depth can be 8 or 16 bits per band.

   When dealing with color smart-cameras, the destination image buffer can only be a BGR packed buffer with a 32-bit color depth (allocated using [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) with [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) + [`M_BGR32`](../../Reference/buf/MbufAllocColor.md)). When dealing with monochrome smart-cameras, you cannot grab into an Aurora Imaging Library color image buffer. |
| `Container identifier` | Specifies the identifier of a container with an [`M_GRAB`](../../Reference/buf/MbufAllocContainer.md) attribute. This container must have been previously allocated, typically using [`MbufAllocContainer`](../../Reference/buf/MbufAllocContainer.md).When grabbing into a container for the first time, Aurora Imaging Library will free all existing components and allocate the components required to store the current frame of data. Aurora Imaging Library will typically reuse these automatically allocated components during subsequent grabs, unless you add or remove components from the container (in which case all components will be freed and new components will again be allocated for the grabbed data).Aurora Imaging Library will not grab into components of the container that you have allocated manually using [`MbufAllocComponent`](../../Reference/buf/MbufAllocComponent.md), [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md), or [`MbufCreateComponent`](../../Reference/buf/MbufCreateComponent.md).In rare applications, you might configure your camera/3D sensor to transmit components of a different size or format with each grab. If there is any mismatch between the components from the previous grab and those required to store the current frame of data (for example, if the ordering, component type, or size has changed), all components will be automatically freed and new components will be allocated; the components will be assigned new identifiers. You can inquire the new buffer identifiers using [`MbufInquireContainer`](../../Reference/buf/MbufInquireContainer.md) with [`M_COMPONENT_ID`](../../Reference/buf/MbufInquireContainer.md).If you need the buffer identifier of a component, and it is possible that the components might be reallocated, you should inquire the current buffer identifier after each grab. |
| `Monochrome image buffer identifier` | Specifies the identifier of a monochrome destination image buffer with an [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md) + [`M_GRAB`](../../Reference/buf/MbufAllocColor.md) attribute. This image buffer must have been previously allocated, typically using [`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md). This image buffer must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. It is not possible to grab into a color-band child buffer (that is, a child buffer created from one band of a color parent buffer) when the parent buffer is packed. When grabbing into an image buffer, if the grab destination buffer is smaller than the digitizer's frame size, the image buffer will be filled and all other data will be lost. If the grab destination buffer is larger than the digitizer's frame size, the buffer will be filled up to the digitizer's frame size. The rest of the buffer will remain untouched. If you grab into a buffer from a digitizer that outputs data in a format suitable for a container, the first intensity component in the frame of data will be grabbed. If there is no intensity component, the first component that is not metadata will be grabbed.  When using a color camera, the color data is first converted into YUV, and then the Y (luminance) component is stored in the buffer. The color space conversion is done in hardware.

   The destination buffer's depth can only be 8 bits per band.

   The destination buffer's depth can be 8 or 16 bits per band.

   The destination buffer can only be an 8-bit monochrome buffer. |
