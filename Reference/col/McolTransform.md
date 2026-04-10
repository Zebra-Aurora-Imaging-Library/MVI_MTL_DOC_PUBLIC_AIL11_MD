---
doctype: Reference
module: col
function: McolTransform
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / col / McolTransform"
---

# McolTransform

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

> Transform color data using relative color calibration.

## Syntax

```c
void McolTransform(
    AIL_ID    ContextId,           //in
    AIL_INT   SampleIndexOrLabel,  //in
    AIL_ID    SrcImageBufId,       //in
    AIL_ID    DestImageBufId,      //out
    AIL_INT64 ControlFlag          //in
)
```

## Description

This function transforms a source image's color data according to the mapping associated with a color-sample in a relative color calibration context. The transformed image is written in the destination image buffer.

This function supports in-place processing; you can specify the identifier of the same image buffer for both the source and the destination.

The mapping associated with the color-sample ([`SampleIndexOrLabel`](../../Reference/col/McolTransform.md)) is established with [`McolDefine`](../../Reference/col/McolDefine.md) and [`McolPreprocess`](../../Reference/col/McolPreprocess.md). [`McolDefine`](../../Reference/col/McolDefine.md) defines the color-samples, and reference color-sample. [`McolPreprocess`](../../Reference/col/McolPreprocess.md) establishes the mapping with which to interpret the color data of each color-sample so it is similar to the color data of the reference color-sample (for example, similar color temperature and distribution). By applying the color-sample mapping, [`McolTransform`](../../Reference/col/McolTransform.md) corrects the color data of its source images to resemble the color data of the reference color-sample. You can then use these transformed source images, which now resemble each other, to perform other color operations, such as color matching ([`McolMatch`](../../Reference/col/McolMatch.md)).

To influence the transformation, you can alter how Aurora Imaging Library establishes the color-sample mapping by changing the processing strategy with [`McolSetMethod`](../../Reference/col/McolSetMethod.md). Such changes require you to call [`McolPreprocess`](../../Reference/col/McolPreprocess.md) before [`McolTransform`](../../Reference/col/McolTransform.md).

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the relative color calibration context containing the color-sample and its associated mapping. The relative color calibration context must have been previously allocated on the required system using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md). The relative color calibration context must also have been previously preprocessed using [`McolPreprocess`](../../Reference/col/McolPreprocess.md).

### `SampleIndexOrLabel` *(in, AIL_INT)*

Specifies the index or label of a color-sample in the specified relative color calibration context. This function transforms the color of the source image according to the mapping associated to the color-sample. Ensure the color-sample is appropriate for the specified source image.

*For specifying a color-sample*

| Value | Description |
| --- | --- |
| `M_SAMPLE_INDEX` | Specifies the index of a color-sample. |
| `M_SAMPLE_LABEL` | Specifies the label of a color-sample. |

### `SrcImageBufId` *(in, AIL_ID)*

Specifies the identifier of the source image with which to perform the relative color calibration. The buffer must be an 8-bit color (3-band) unsigned image buffer (same as the destination).

### `DestImageBufId` *(out, AIL_ID)*

Specifies the identifier of destination image in which to write the transformed color. The buffer must be an 8-bit color (3-band) unsigned image buffer (same as the source).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
