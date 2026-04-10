---
doctype: Reference
module: class
function: MclassCopy
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassCopy"
---

# MclassCopy

> Copy data from one classification context to another.

## Syntax

```c
void MclassCopy(
    AIL_ID    SrcContextClassId,  //in
    AIL_INT64 SrcIndex,           //in
    AIL_ID    DstContextClassId,  //out
    AIL_INT64 DstIndex,           //in
    AIL_INT64 CopyType,           //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies data from the specified source classification context to the specified destination classification context. The source and the destination context must be for the same type of classifier (predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or an ONNX classifier). For example, you can copy data from one images dataset to another, or from an images dataset to a predefined CNN classifier context.

## Parameters

### `SrcContextClassId` *(in, AIL_ID)*

Specifies the identifier of the source classification context from which to copy. Set this parameter to one of the following values.

*For specifying the source classification context*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or an ONNX classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md).

> **Note:** This is not available when the [`CopyType`](../../Reference/class/MclassCopy.md) parameter is set to [`M_AUTHORS`](../../Reference/class/MclassCopy.md) or [`M_DATASET`](../../Reference/class/MclassCopy.md). |
| `ContextDatasetFeaturesId` | Specifies the identifier of a features dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md).

If the source is a features dataset context, the destination must be a features dataset context or a tree ensemble classifier context. |
| `ContextDatasetImagesId` | Specifies the identifier of an images dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md).

If the source is an images dataset context, the destination must be an images dataset context, a predefined CNN classifier context, a predefined object detection classifier context, a predefined segmentation classifier context, or a predefined anomaly detection classifier context. |

### `SrcIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DstContextClassId` *(out, AIL_ID)*

Specifies the identifier of the destination classification context in which to copy. Set this parameter to one of the following values.

*For specifying the destination classification context*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or an ONNX classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md).

> **Note:** This is not available when the [`CopyType`](../../Reference/class/MclassCopy.md) parameter is set to [`M_AUTHORS`](../../Reference/class/MclassCopy.md) or [`M_DATASET`](../../Reference/class/MclassCopy.md). |
| `ContextDatasetFeaturesId` | Specifies the identifier of the features dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md).

> **Note:** This is not available when the [`CopyType`](../../Reference/class/MclassCopy.md) parameter is set to [`M_DATASET`](../../Reference/class/MclassCopy.md) and [`SrcContextClassId`](../../Reference/class/MclassCopy.md) is set to an identifier of an images dataset context. |
| `ContextDatasetImagesId` | Specifies the identifier of the images dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md).

> **Note:** This is not available when the [`CopyType`](../../Reference/class/MclassCopy.md) parameter is set to [`M_DATASET`](../../Reference/class/MclassCopy.md) and [`SrcContextClassId`](../../Reference/class/MclassCopy.md) is set to an identifier of a features dataset context. |

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_AUTHORS` | Copies authors from the source dataset to the destination dataset or classifier object. This overwrites any existing authors in the destination object. If the destination is a dataset, it should not contain any entries.

> **Note:** When using [`M_AUTHORS`](../../Reference/class/MclassCopy.md), you must set the [`SrcContextClassId`](../../Reference/class/MclassCopy.md) parameter to the identifier of a features dataset or images dataset context. The [`DstContextClassId`](../../Reference/class/MclassCopy.md) parameter must be set to the identifier of a features dataset or images dataset context. |
| `M_CLASS_DEFINITIONS` | Copies the class definitions from the source classification context to the destination classification context. Class definitions already in the destination are overwritten.

> **Note:** When using [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassCopy.md), you must set the [`SrcContextClassId`](../../Reference/class/MclassCopy.md) parameter to the identifier of a predefined CNN classifier, a predefined object detection classifier, a predefined segmentation classifier, a predefined anomaly detection classifier, tree ensemble classifier, ONNX classifier, features dataset, or images dataset context. The [`DstContextClassId`](../../Reference/class/MclassCopy.md) parameter must be set to the identifier of a predefined CNN classifier, a predefined object detection classifier, a predefined segmentation classifier, a predefined anomaly detection classifier, tree ensemble classifier, ONNX classifier, features dataset, or images dataset context. |
| `M_DATASET` | Copies the entire dataset. This overwrites the entries, class definitions, authors, and controls related to the dataset (for example, [`M_ROOT_PATH`](../../Reference/class/MclassControl.md)). Note, included in the entries are the mask and segmentation paths (the files themselves are not copied).

> **Note:** When using [`M_DATASET`](../../Reference/class/MclassCopy.md), you must set the [`SrcContextClassId`](../../Reference/class/MclassCopy.md) parameter to the identifier of a features or images dataset context. The [`DstContextClassId`](../../Reference/class/MclassCopy.md) parameter must be set to the identifier of a features or images dataset context. The source and the destination context must be for the same type of dataset. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
