---
doctype: Reference
module: class
function: MclassPredict
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassPredict"
---

# MclassPredict

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

> Predict the class to which the target belongs or the position of objects to which each entry of a dataset belongs.

## Syntax

```c
void MclassPredict(
    AIL_ID    ContextClassId,                 //in
    AIL_ID    TargetAilObjectId,              //in
    AIL_ID    DatasetContextOrResultClassId,  //out
    AIL_INT64 ControlFlag                     //in
)
```

## Description

This function uses a trained classifier context to predict the class to which the target belongs or the position of objects in the target. For a trained CNN, segmentation, object detection, or anomaly detection classifier context, you can specify an image or an images dataset context as the target. Image classification and segmentation support buffers with up to 1024 bands. For a trained tree ensemble classifier context, you can specify an array of features or a features dataset context as the target. For a trained ONNX classifier, you can only specify an image.

This function is typically called with a target image (for image classification, segmentation, object detection, or anomaly detection) or with a target array of features (for feature classification). In these cases, results are stored in a result buffer ([`DatasetContextOrResultClassId`](../../Reference/class/MclassPredict.md)) and retrievable using [`MclassGetResult`](../../Reference/class/MclassGetResult.md).

You can also call this function with a target dataset. In this case, the predicted data is stored in a dataset context ([`DatasetContextOrResultClassId`](../../Reference/class/MclassPredict.md)). Calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a dataset can be done, for example, to predict the class that represents the dataset's entries, which you can retrieve using [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResultEntry.md). You can then use these predictions to help manually define the ground truth of the entries during training.

Before calling [`MclassPredict`](../../Reference/class/MclassPredict.md), the classifier context must be in a preprocessed state ([`MclassPreprocess`](../../Reference/class/MclassPreprocess.md)) and it must be trained ([`MclassTrain`](../../Reference/class/MclassTrain.md)). To inquire this information, use [`M_PREPROCESSED`](../../Reference/class/MclassInquire.md) and [`M_CLASSIFIER_STATUS`](../../Reference/class/MclassInquire.md).

If your target is a dataset context, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) (for example, with [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md)). To get information about the events that caused the hook-handler function to execute, call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md).

Classification contexts and result buffers must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) and [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md), or restored from disk using [`MclassRestore`](../../Reference/class/MclassRestore.md).

Note, you can also call [`MclassPredict`](../../Reference/class/MclassPredict.md) with an images dataset context to perform assisted labeling (that is, label images in an existing dataset).

By default, [`MclassPredict`](../../Reference/class/MclassPredict.md) uses the default predict engine established by the **Aurora Imaging Configurator** utility. You can modify this by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_PREDICT_ENGINE`](../../Reference/class/MclassControl.md) or in **Aurora Imaging Configurator**.

> **Note:** To take advantage of all available hardware resources, such as accelerating prediction using OpenVINO and CUDA, you should install the required Aurora Imaging Library add-on. For more information, see [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).

## Parameters

### `ContextClassId` *(in, AIL_ID)*

Specifies the identifier of the trained classifier context to use for the prediction operation. You can specify a trained CNN classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a trained segmentation classifier context ([`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a trained object detection classifier context ([`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a trained anomaly detection classifier ([`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a trained tree ensemble classifier context ([`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)), or a trained ONNX classifier context ([`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md)).

### `TargetAilObjectId` *(in, AIL_ID)*

Specifies the identifier of the target to classify. Set this parameter to one of the following values:

*For specifying the target to classify*

| Value | Description |
| --- | --- |
| `ArrayBufId` | Specifies the identifier of a one-dimensional 32-bit float array that holds a list of features. In this case, the prediction operation will predict the best class to which the array of features belongs. The number of features in the array must be the same as the number of trained features in the dataset used to train the classifier context ([`DatasetContextOrResultClassId`](../../Reference/class/MclassPredict.md)).

To specify an array, you must set the [`ContextClassId`](../../Reference/class/MclassPredict.md) parameter to the identifier of a trained tree ensemble classifier context. |
| `ContextDatasetId` | Specifies the identifier of a features or images dataset context. For CNN and tree ensemble classifiers, the prediction operation will predict the best class to which each dataset entry (feature list or image) belongs. For segmentation classifiers, the prediction operation will predict the best class to which each pixel belongs in a image entry. For object detection classifiers, the prediction operation will detect the positions of each object in an image entry. For anomaly detection classifiers, the prediction operation will predict the anomaly score of the target image as well as the anomaly score for each pixel within the image. Image classification and segmentation support buffers with up to 1024 bands.

For image datasets, you must set the[`ContextClassId`](../../Reference/class/MclassPredict.md) parameter to the identifier of a trained CNN, segmentation, object detection, or anomaly detection classifier context. For feature datasets, you must set the [`ContextClassId`](../../Reference/class/MclassPredict.md) parameter to the identifier of a trained tree ensemble. |
| `ImageBufId` | Specifies the identifier of a 1- or 3-band target image. Image classification and segmentation support buffers with up to 1024 bands. When [`ContextClassId`](../../Reference/class/MclassPredict.md) is a trained CNN classifier context, the prediction operation will predict the best class to which the target image belongs. When [`ContextClassId`](../../Reference/class/MclassPredict.md) is a trained segmentation classifier context, the prediction operation will predict the class of each pixel within the target image. When [`ContextClassId`](../../Reference/class/MclassPredict.md) is a trained object detection classifier context, the prediction operation will detect the position of each object within the image. When [`ContextClassId`](../../Reference/class/MclassPredict.md) is a trained anomaly detection classifier context, the prediction operation will predict the anomaly score of the target image as well as the anomaly score for each pixel within the image.

You must set the [`ContextClassId`](../../Reference/class/MclassPredict.md) parameter to the identifier of a trained CNN, segmentation, object detection, anomaly detection or ONNX classifier context.

When predicting for image classification (using [`M_ICNET_...`](../../Reference/class/MclassAlloc.md)), the target image size must be the same as the training images. When predicting for segmentation (using [`M_CSNET_...`](../../Reference/class/MclassAlloc.md)), or if you are using a predefined legacy classifier ([`M_FCNET_...`](../../Reference/class/MclassInquire.md)), the target image size can be bigger than the training images (this requires calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md)). By default, Aurora Imaging Library considers the size of the target image to be the same as the source layer size. |

### `DatasetContextOrResultClassId` *(out, AIL_ID)*

Specifies the identifier of the result buffer or dataset in which to write the results of the prediction operation. Set this parameter to one of the following values:

*For specifying the result buffer or dataset in which to write the results of the prediction*

| Value | Description |
| --- | --- |
| `ContextDatasetId` | Specifies the identifier of a features dataset context ([`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md)) or images dataset context ([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)). You must call [`MclassPredict`](../../Reference/class/MclassPredict.md) with a features or images dataset context as your target ([`TargetAilObjectId`](../../Reference/class/MclassPredict.md)). The dataset context specified by the [`DatasetContextOrResultClassId`](../../Reference/class/MclassPredict.md) parameter must either be empty, or be the same dataset as your target.

If the destination is an images dataset context, Aurora Imaging Library might modify its root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md)), if one is specified, to maintain the dataset's coherence. For example, dataset paths should all be either relative or absolute. |
| `ResultBufPredictId` | Specifies the identifier of a CNN prediction result buffer ([`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), a segmentation prediction result buffer ([`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)), an object detection prediction result buffer ([`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), an anomaly detection prediction result buffer ([`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) a tree ensemble prediction result buffer ([`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), or an ONNX prediction result buffer ([`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md)). In this case, you must call [`MclassPredict`](../../Reference/class/MclassPredict.md) with an image or an array of features as your target ([`TargetAilObjectId`](../../Reference/class/MclassPredict.md)). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
