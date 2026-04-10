---
doctype: Reference
module: dlocr
function: MdlocrFinetune
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrFinetune"
---

# MdlocrFinetune

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

> Fine-tune a Deep Learning OCR read context on a dataset.

## Syntax

```c
void MdlocrFinetune(
    AIL_ID    FinetuneContextDlocrId,  //in
    AIL_ID    ReadContextDlocrId,      //in
    AIL_ID    DlocrDatasetId,          //in
    AIL_ID    FinetuneResultDlocrId,   //out
    AIL_INT64 ControlFlag              //in
)
```

## Description

This function uses a Deep Learning OCR dataset to fine-tune a Deep Learning OCR read context. Fine-tuning results are stored in a Deep Learning OCR result buffer. To get the results, use [`MdlocrGetResult`](../../Reference/dlocr/MdlocrGetResult.md).

The fine-tuning process is time-consuming and heavily relies on the accuracy of the dataset's annotations. That is, the extent to which fine-tuning improves the read operation depends on the image quality of the characters or strings in the source dataset. Training time can vary considerably depending on the complexity of the images, the available hardware, and the accuracy required. Images can be added to the dataset, using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md). You can remove the last image that was added to the dataset, using [`MdlocrDeleteLastImageFromDataset`](../../Reference/dlocr/MdlocrDeleteLastImageFromDataset.md). You can export a dataset to the disk, using [`MdlocrExport`](../../Reference/dlocr/MdlocrExport.md) or restore a dataset from the disk, using [`MdlocrImport`](../../Reference/dlocr/MdlocrImport.md).

Before performing a fine-tuning operation, the Deep Learning OCR read and fine-tuning contexts must have been previously allocated, using[`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md) and be in a preprocessed state ([`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md)).

You can manually stop the fine-tuning operation, using [`MdlocrControl`](../../Reference/dlocr/MdlocrControl.md) with.

## Parameters

### `FinetuneContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR fine-tuning context to use for the fine-tuning operation. The context must have been previously allocated on the required system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md), and preprocessed using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).

### `ReadContextDlocrId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR read context to use for the fine-tuning operation. The context must have been previously allocated on the required system using [`MdlocrAlloc`](../../Reference/dlocr/MdlocrAlloc.md), and preprocessed using [`MdlocrPreprocess`](../../Reference/dlocr/MdlocrPreprocess.md).

### `DlocrDatasetId` *(in, AIL_ID)*

Specifies the identifier of the Deep Learning OCR dataset to use for the fine-tuning operation. The dataset must have been previously allocated on the required system using [`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md).

### `FinetuneResultDlocrId` *(out, AIL_ID)*

Specifies the identifier of the Deep Learning OCR fine-tuning result buffer in which to write the results of the fine-tuning operation. The fine-tuning result buffer must have been previously allocated on the required system using [`MdlocrAllocResult`](../../Reference/dlocr/MdlocrAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
