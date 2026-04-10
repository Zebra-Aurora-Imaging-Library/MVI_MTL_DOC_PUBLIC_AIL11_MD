---
doctype: Reference
module: edge
function: MedgeMask
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeMask"
---

# MedgeMask

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

> Mask regions of the Edge Finder context.

## Syntax

```c
void MedgeMask(
    AIL_ID    ContextId,    //out
    AIL_ID    MaskImageId,  //in
    AIL_INT64 ControlFlag   //in
)
```

## Description

This function allows you to set a mask for an Edge Finder context. The mask is applied to the image that uses the context. Subsequent calls to [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) will therefore extract edges only in the context's unmasked regions.

You can also mask edges during [post-calculation](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md). In this case, masked edges are excluded from the result buffer and subsequent calculations. Note that partially masked edges are cropped.

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the Edge Finder context in which to set the mask. The Edge Finder context must have been previously allocated on the required system using [`MedgeAlloc`](../../Reference/edge/MedgeAlloc.md).

### `MaskImageId` *(in, AIL_ID)*

Specifies the identifier of the image buffer used to identify the masked pixels in the Edge Finder context.

*For the identifier of the image buffer*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to ignore this parameter. This parameter must be set to [`M_NULL`](../../Reference/edge/MedgeMask.md) when removing the mask. |
| `Image buffer identifier` | Specifies the image buffer to use as a mask. If the size of the mask image is greater than the source image, the mask will be clipped.

A masked pixel corresponds to a non-zero value in the mask buffer. Note that this buffer can be calibrated; however, this is not necessary, since the mask is applied on a pixel basis.

This image buffer must be a 1-band buffer and must not have a region of interest (ROI) associated with it. Using an image buffer with an ROI will cause an error. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
