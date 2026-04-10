---
doctype: Reference
module: col
function: McolMask
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolMask"
---

# McolMask

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

> Mask regions of a color element in a specified color-sample.

## Syntax

```c
void McolMask(
    AIL_ID    ContextId,                //out
    AIL_INT   ColorSampleIndexOrLabel,  //in
    AIL_ID    MaskBufferId,             //in
    AIL_INT64 MaskType,                 //in
    AIL_INT64 ControlFlag               //in
)
```

## Description

This function allows you to apply masks to specific color elements of the specified color-sample in the color matching or relative color calibration context. To apply a mask to a color-sample with only one element, you must have added the color-sample to the context using [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_IMAGE`](../../Reference/col/McolDefine.md). To apply a mask to a specific color element of the specified color-sample, you must have added the color element to a color-sample using [`McolDefine`](../../Reference/col/McolDefine.md) with [`M_ADD_COLOR_TO_SAMPLE`](../../Reference/col/McolDefine.md).

With each mask, you can set pixels in the specified color element to a "don't care" state. Aurora Imaging Library does not consider "don't care" pixels in color-samples when you call [`McolPreprocess`](../../Reference/col/McolPreprocess.md), [`McolMatch`](../../Reference/col/McolMatch.md), or [`McolTransform`](../../Reference/col/McolTransform.md).

## Parameters

### `ContextId` *(out, AIL_ID)*

Specifies the context in which the color-sample containing the color element to be masked is located. You must first allocate the color matching or relative color calibration context on the system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_MATCHING`](../../Reference/col/McolAlloc.md) or [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md).

### `ColorSampleIndexOrLabel` *(in, AIL_INT)*

Specifies the color element for which to create a mask. Unless otherwise specified, values apply to color matching and relative color calibration contexts. This parameter can be set to one of the following values:

*For specifying the color element*

| Value | Description |
| --- | --- |
| `M_REFERENCE_SAMPLE_ITEM` | Specifies the index of a color element (item), relative to the reference color-sample. This value only applies to relative color calibration contexts ([`ContextId`](../../Reference/col/McolMask.md)). |
| `M_SAMPLE_INDEX` | Specifies the index of a color-sample that has one color element. If the color-sample has multiple color elements, use **M_SAMPLE_INDEX_ITEM()** or **M_SAMPLE_LABEL_ITEM()**. |
| `M_SAMPLE_INDEX_ITEM` | Specifies the index of a color element (item), relative to a color-sample index. |
| `M_SAMPLE_LABEL` | Specifies the label of a color-sample that has one color element. If the color-sample has multiple color elements, use **M_SAMPLE_INDEX_ITEM()** or **M_SAMPLE_LABEL_ITEM()**. |
| `M_SAMPLE_LABEL_ITEM` | Specifies the index of a color element (item), relative to a color-sample label. |
| `M_REFERENCE_SAMPLE` | Specifies the reference color-sample. This value only applies to relative color calibration contexts ([`ContextId`](../../Reference/col/McolMask.md)). |

### `MaskBufferId` *(in, AIL_ID)*

Specifies the identifier of the buffer to use to identify the masked (non-zero) pixels in the color element. This buffer must be a 1-band 8-bit unsigned image buffer.

### `MaskType` *(in, AIL_INT64)*

Specifies the type of mask to employ. Set this parameter to the following value:

*For specifying the type of mask*

| Value | Description |
| --- | --- |
| `M_DONT_CARE` | Specifies that Aurora Imaging Library ignores non-zero pixels in the color element's masked region. For example, masked pixels do not affect any of the resulting scores, when performing color matching. Masked pixels also do not affect the mapping Aurora Imaging Library establishes between each color-sample and the relevant color-sample, when transforming colors using relative color calibration. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
