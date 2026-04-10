---
doctype: Reference
module: dlocr
function: MdlocrAddImageToDataset
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrAddImageToDataset"
---

# MdlocrAddImageToDataset

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

> Add an image and read results to a Deep Learning OCR dataset.

## Syntax

```c
void MdlocrAddImageToDataset(
    AIL_ID    DlocrReadResultId,  //in
    AIL_ID    DlocrDatasetId,     //out
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function adds an image and selected results from a Deep Learning OCR read result buffer to a Deep Learning OCR dataset. If required, you can delete the most recently added dataset entry, using [`MdlocrDeleteLastImageFromDataset`](../../Reference/dlocr/MdlocrDeleteLastImageFromDataset.md).

## Parameters

### `DlocrReadResultId` *(in, AIL_ID)*

Specifies the identifier of the source Deep Learning OCR read result buffer from which to get the image and results.

### `DlocrDatasetId` *(out, AIL_ID)*

Specifies the identifier of the destination Deep Learning OCR dataset in which to add the image and results.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
