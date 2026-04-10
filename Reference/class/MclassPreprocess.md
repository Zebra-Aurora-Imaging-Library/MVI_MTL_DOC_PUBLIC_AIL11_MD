---
doctype: Reference
module: class
function: MclassPreprocess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassPreprocess"
---

# MclassPreprocess

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

> Preprocess a classification context.

## Syntax

```c
void MclassPreprocess(
    AIL_ID    ContextClassId,  //in
    AIL_INT64 ControlFlag      //in
)
```

## Description

This function preprocesses a classification context for data preparation ([`MclassPrepareData`](../../Reference/class/MclassPrepareData.md)), training ([`MclassTrain`](../../Reference/class/MclassTrain.md)), statistics calculation ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)), or prediction ([`MclassPredict`](../../Reference/class/MclassPredict.md)). You must call [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md) before the first call to any of these functions.

During preprocessing, Aurora Imaging Library analyzes the content of the context and makes internal refinements to execute an optimized and robust operation. Changes to a context or any of its content often require preprocessing the context again. To inquire whether you should preprocess the context, call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREPROCESSED`](../../Reference/class/MclassInquire.md). The context must be in a preprocessed state before calling the data preparation, training, or prediction operation.

Saving a context does not save preprocessing changes. Upon restoration, you must preprocess the context again before calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), [`MclassTrain`](../../Reference/class/MclassTrain.md), [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md), or[`MclassPredict`](../../Reference/class/MclassPredict.md).

## Parameters

### `ContextClassId` *(in, AIL_ID)*

Specifies the identifier of the classification context to preprocess. The classification context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to preprocess the classification context. Set this parameter to one of the following values:

*For specifying whether to preprocess the context*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Preprocesses the context. |
| `M_RESET` | Un-preprocesses the context.

Un-preprocessing the context can be useful if you want to conserve system memory within an application and preserve context settings. |
