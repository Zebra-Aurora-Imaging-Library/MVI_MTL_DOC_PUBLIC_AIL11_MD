---
doctype: Reference
module: class
function: MclassCopyResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassCopyResult"
---

# MclassCopyResult

> Copy data from a training result buffer to a classifier or dataset context.

## Syntax

```c
void MclassCopyResult(
    AIL_ID    SrcResultClassId,   //in
    AIL_INT64 SrcIndex,           //in
    AIL_ID    DstContextClassId,  //out
    AIL_INT64 DstIndex,           //in
    AIL_INT64 CopyType,           //in
    AIL_INT64 ControlFlag         //in
)
```

## Description

This function copies data from a training result buffer to a classifier or dataset context. For example, you can copy the trained classifier result to a classifier context. The result buffer and classifier or dataset context must be compatible (that is, they must be for the same type of classifier).

You can only copy trained results after calling [`MclassTrain`](../../Reference/class/MclassTrain.md). Once you copy the trained results into a classifier context, you can continue to train that context ([`MclassTrain`](../../Reference/class/MclassTrain.md)) or you can predict with it ([`MclassPredict`](../../Reference/class/MclassPredict.md)).

## Parameters

### `SrcResultClassId` *(in, AIL_ID)*

Specifies the identifier of the source training result buffer from which to copy. The result buffer must have been allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md). The type of result buffer that you can specify depends on the copy operation ([`CopyType`](../../Reference/class/MclassCopyResult.md)). Set this parameter to the following value.

*For specifying the source result*

| Value | Description |
| --- | --- |
| `ResultBufTrainId` | Specifies the identifier of a training result buffer ([`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)) that holds the results produced from calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with a CNN training context, an object detection training context, a segmentation training context, an anomaly detection training context, or tree ensemble training context. |

### `SrcIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DstContextClassId` *(out, AIL_ID)*

Specifies the identifier of the destination classifier or dataset context in which to copy. The classifier or dataset context must have been allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md). The type of result buffer that you can specify depends on the copy operation ([`CopyType`](../../Reference/class/MclassCopyResult.md)). Set this parameter to one of the following values.

*For specifying the destination classification context*

| Value | Description |
| --- | --- |
| `ContextClassifierPredefinedId` | Specifies the identifier of a predefined CNN, object detection, segmentation, or anomaly detection classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md)). |
| `ContextClassifierTreeEnsembleId` | Specifies the identifier of a tree ensemble classifier context ([`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)). |
| `ContextDatasetFeaturesId` | Specifies the identifier of a features dataset context ([`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md)). |
| `ContextDatasetImagesId` | Specifies the identifier of an images dataset context ([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)). |

### `DstIndex` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `CopyType` *(in, AIL_INT64)*

Specifies the type of copy operation to perform.

*Specifies the type of copy operation*

| Value | Description |
| --- | --- |
| `M_DEV_DATASET_PREDICT_SCORES` | Copies the predicted scores of the development dataset established by [`MclassTrain`](../../Reference/class/MclassTrain.md). To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a CNN or tree ensemble training result buffer, and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a features or images dataset context.

If you used a tree ensemble classifier and did not specify a development dataset with [`MclassTrain`](../../Reference/class/MclassTrain.md), nothing is copied.

Copies are only performed if the corresponding entry already exists in the destination dataset (otherwise, nothing is copied). |
| `M_OUT_OF_BAG_PREDICT_SCORES` | Copies the predicted scores of the out-of-bag entries established by [`MclassTrain`](../../Reference/class/MclassTrain.md). To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a tree ensemble training result buffer and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a features dataset context.

Note, bagging information is typically unreliable if your training dataset has augmented entries. |
| `M_PREPARED_DEV_DATASET` | Copies the development dataset from the result to the destination dataset. To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a CNN, object detection, segmentation, or anomaly detection training result buffer, and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of an images dataset context. |
| `M_PREPARED_TRAIN_DATASET` | Copies the train dataset from the result to the destination dataset. To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a CNN, object detection, segmentation, or anomaly detection training result buffer, and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of an images dataset context. |
| `M_TRAIN_DATASET_PREDICT_SCORES` | Copies the predicted scores of the training dataset established by [`MclassTrain`](../../Reference/class/MclassTrain.md). To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a CNN or tree ensemble training result buffer, and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a features or images dataset context.

Copies are only performed if the corresponding entry already exists in the destination dataset (otherwise, nothing is copied). |
| `M_TRAINED_CLASSIFIER` | Copies the trained classifier context established by [`MclassTrain`](../../Reference/class/MclassTrain.md). To perform this copy operation, you must set the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a training result buffer, and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter to the identifier of a classifier context. This is available for CNN, object detection, segmentation, anomaly detection, and tree ensemble classifiers; both the [`SrcResultClassId`](../../Reference/class/MclassCopyResult.md) and the [`DstContextClassId`](../../Reference/class/MclassCopyResult.md) parameter must contain the same type of classifier. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
