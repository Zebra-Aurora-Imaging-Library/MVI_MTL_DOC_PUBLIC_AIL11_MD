---
doctype: Reference
module: class
function: MclassTrain
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassTrain"
---

# MclassTrain

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

> Train a classifier context.

## Syntax

```c
void MclassTrain(
    AIL_ID    TrainContextClassId,         //in
    AIL_ID    ClassifierContextClassId,    //in
    AIL_ID    TrainDatasetContextClassId,  //in
    AIL_ID    DevDatasetContextClassId,    //in
    AIL_ID    TrainResultClassId,          //out
    AIL_INT64 ControlFlag                  //in
)
```

## Description

This function trains a classifier context. This context gets trained according to a training context and dataset contexts. All training results are stored in a classification result buffer. To get these results, use [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also copy dataset-specific training results from the result buffer to a dataset, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md). If you are using an images dataset, dataset-specific results are retrievable directly from that dataset, using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md).

To modify training related settings, call [`MclassControl`](../../Reference/class/MclassControl.md). To hook functions to training events, call [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).

A training context must be preprocessed before it is trained. To inquire a context's preprocessing state, call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREPROCESSED`](../../Reference/class/MclassInquire.md).

You can train a predefined CNN, object detection, segmentation, anomaly detection classifier context, or a tree ensemble classifier context ([`ClassifierContextClassId`](../../Reference/class/MclassTrain.md)). All other training contexts ([`TrainContextClassId`](../../Reference/class/MclassTrain.md), [`TrainDatasetContextClassId`](../../Reference/class/MclassTrain.md), and [`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md)) must be for the same type of classifier. Unless otherwise specified, contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md). Image classification and segmentation support buffers with up to 1024 bands.

A classifier context is not trained when you allocate it. You must call [`MclassTrain`](../../Reference/class/MclassTrain.md) to train it, using the [`ClassifierContextClassId`](../../Reference/class/MclassTrain.md) parameter. If you set this parameter to [`M_NULL`](../../Reference/class/MclassTrain.md), [`MclassTrain`](../../Reference/class/MclassTrain.md) automatically selects a new classifier context to train.

You can also call this function to retrain a trained classifier context. To do so, you must copy the trained classifier context result that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces ([`TrainResultClassId`](../../Reference/class/MclassTrain.md)) into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md). You can then train that classifier context with [`MclassTrain`](../../Reference/class/MclassTrain.md). Before training a trained classifier context, you should modify your training settings. You can repeat this process (train, copy result, modify settings, train) as many times as required. Note, this is not possible for object detection.

When your classifier context is properly trained, you can copy the result that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces into a classifier context ([`MclassCopyResult`](../../Reference/class/MclassCopyResult.md)) and use that trained classifier context with [`MclassPredict`](../../Reference/class/MclassPredict.md).

The training process for image classification, object detection, segmentation, and anomaly detection requires two images datasets ([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)): a training dataset and a development dataset. You can, with the exception of anomaly detection, specify just one dataset, using the [`TrainDatasetContextClassId`](../../Reference/class/MclassTrain.md) parameter, and set the development dataset parameter ([`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md)) to [`M_NULL`](../../Reference/class/MclassTrain.md). When doing so, Aurora Imaging Library automatically splits your specified dataset into the training and development datasets, according to the related split control settings (for example, [`M_SPLIT_PERCENTAGE`](../../Reference/class/MclassControl.md) and [`M_SPLIT_SEED_MODE`](../../Reference/class/MclassControl.md)). At this time, if data preparation controls were set, Aurora Imaging Library prepares the datasets using an internal data preparation context. To inquire the identifier of this internal data preparation context, or the location at which prepared images are stored, use [`M_PREPARE_DATA_CONTEXT_ID`](../../Reference/class/MclassInquire.md) and [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassInquire.md). Note that signed images and anomaly detection tasks are not supported for single dataset training since, [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) does not support preparing signed images.

When training for feature classification, the development dataset is optional; typically, only the training dataset is used. In this case, if you specify one dataset, using the [`TrainDatasetContextClassId`](../../Reference/class/MclassTrain.md) parameter, and you set the [`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md) parameter to [`M_NULL`](../../Reference/class/MclassTrain.md), Aurora Imaging Library does not train with a development dataset (your data is not split), and the development dataset results will not be available (such as [`M_DEV_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md)). For feature classification, you must explicitly specify the identifier of the development dataset to use it ([`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md)).

> **Note:** Training time can vary considerably, depending on the complexity of the application, the available hardware, and the accuracy required. To produce a properly trained classifier, [`MclassTrain`](../../Reference/class/MclassTrain.md) might have to run for an extended period, and you might have to call it several times after modifying your training setup (for example, your training settings or your datasets). To take advantage of all available resources, such as GPU training, you should install the required Aurora Imaging Library add-on. For more information, see [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).

## Parameters

### `TrainContextClassId` *(in, AIL_ID)*

Specifies the identifier of the training context with which to train the classifier context. Training settings ([`MclassControl`](../../Reference/class/MclassControl.md)) are held in the training context.

### `ClassifierContextClassId` *(in, AIL_ID)*

Specifies the identifier of the classifier context to train. Depending on this parameter's setting, you can start a new training process, or continue a training process.

### `TrainDatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the training dataset context with which to train the classifier context. If training with an image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), or segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) training context, and you set the [`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md) parameter to [`M_NULL`](../../Reference/class/MclassTrain.md), the specified dataset ([`TrainDatasetContextClassId`](../../Reference/class/MclassTrain.md)) is split into the training dataset and the development dataset.

### `DevDatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the development dataset context with which to evaluate the performance of the classifier context's training. If not needed, set this parameter to `M_NULL`. If you explicitly provide the development dataset, ensure that it does not contain data (image or feature entries) that is in the training dataset, and that it does not contain augmented data.

### `TrainResultClassId` *(out, AIL_ID)*

Specifies the identifier of the classification result buffer in which to write the results of the training operation. This result buffer must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
