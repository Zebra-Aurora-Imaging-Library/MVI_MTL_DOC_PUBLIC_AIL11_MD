---
doctype: Reference
module: dlocr
function: MdlocrDeleteLastImageFromDataset
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrDeleteLastImageFromDataset"
---

# MdlocrDeleteLastImageFromDataset

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

> Delete the last image added to the Deep Learning OCR dataset.

## Syntax

```c
void MdlocrDeleteLastImageFromDataset(
    AIL_ID    DlocrDatasetId,  //out
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function deletes the last image and associated results that were added to a Deep Learning OCR dataset using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md).

## Parameters

### `DlocrDatasetId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR dataset from which to delete the image and results.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
