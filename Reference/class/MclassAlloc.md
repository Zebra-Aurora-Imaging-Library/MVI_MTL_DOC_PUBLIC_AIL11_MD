---
doctype: Reference
module: class
function: MclassAlloc
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassAlloc"
---

# MclassAlloc

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

> Allocate a classifier context, a dataset context, a data preparation context, a statistics classification context, or a training context.

## Syntax

```c
AIL_ID MclassAlloc(
    AIL_ID    SysId,             //in
    AIL_INT64 ContextType,       //in
    AIL_INT64 ControlFlag,       //in
    AIL_ID *  ContextClassIdPtr  //out
)
```

## Description

This function allocates a classifier context, a dataset context, a data preparation context, a statistics classification context, or a training context on the specified system. These contexts are for anomaly detection, feature classification, image classification, object detection, or segmentation.

In general, classifier contexts and dataset contexts are for training ([`MclassTrain`](../../Reference/class/MclassTrain.md)) and prediction ([`MclassPredict`](../../Reference/class/MclassPredict.md)), while training contexts, dataset contexts, and data preparation contexts are for training. You can import data to, and export data from, a dataset context by calling [`MclassImport`](../../Reference/class/MclassImport.md) and [`MclassExport`](../../Reference/class/MclassExport.md). Datasets are typically exported after they are created and imported the next time they are needed.

When a context is no longer required, release it using[`MclassFree`](../../Reference/class/MclassFree.md) unless [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified during allocation; if [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified, the smart identifier manages the context's lifetime and you must not manually free it.

## Parameters

### `SysId` *(in, AIL_ID)*

Specifies the identifier of the system on which to allocate the context. Set this parameter to one of the following values.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `ContextType` *(in, AIL_INT64)*

Specifies the type of context to allocate. Set this parameter to one of the following values.

*For specifying the type of context to allocate*

| Value | Description |
| --- | --- |
| `M_CLASSIFIER_ANO_PREDEFINED` | Specifies a predefined anomaly detection classifier context. This is for anomaly detection. For this context, you should set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_ADNET`](../../Reference/class/MclassAlloc.md)). |
| `M_CLASSIFIER_CNN_PREDEFINED` | Specifies a predefined CNN classifier context. This is for image classification. For this context, you should typically specify a specific CNN classifier that Aurora Imaging has built (that is, set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_ICNET_...`](../../Reference/class/MclassAlloc.md)).

CNN and image classification are used interchangeably unless otherwise specified. |
| `M_CLASSIFIER_DET_PREDEFINED` | Specifies a predefined object detection classifier context. This is for object detection. For this context, you should set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_ODNET`](../../Reference/class/MclassAlloc.md)). |
| `M_CLASSIFIER_ONNX` | Specifies an ONNX classifier context. For this context, do not specify a specific classifier that Aurora Imaging has built (that is, set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md)).

ONNX is an open-source machine learning format that you can use to create a classifier model. When you allocate an ONNX classifier context, you typically import a trained ONNX classifier model into it, and then predict with it [`MclassPredict`](../../Reference/class/MclassPredict.md). The type of classification that is performed depends on the ONNX classifier that you created and trained prior to importing it. |
| `M_CLASSIFIER_SEG_PREDEFINED` | Specifies a predefined segmentation classifier context. This is for segmentation. For this context, you should typically specify a specific segmentation classifier that Aurora Imaging has built (that is, set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_CSNET_...`](../../Reference/class/MclassAlloc.md)). |
| `M_CLASSIFIER_TREE_ENSEMBLE` | Specifies a tree ensemble classifier context. This is for feature classification. For this context, do not specify a specific classifier that Aurora Imaging has built (that is, set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md)).

Tree ensemble and feature classification are used interchangeably unless otherwise specified. |
| `M_DATASET_FEATURES` | Specifies a features dataset context.

This context is for feature classification; it holds the feature data with which to train a tree ensemble classifier ([`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)). |
| `M_DATASET_IMAGES` | Specifies an images dataset context.

This context (dataset) is for image classification, segmentation, object detection, or anomaly detection; it holds the image data with which to train a CNN classifier ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md)), a segmentation classifier ([`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md)), an object detection classifier ([`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md)), or an anomaly detection classifier ([`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md)). Image classification and segmentation support buffers with up to 1024 bands.

You can also use an images dataset context with [`MclassPredict`](../../Reference/class/MclassPredict.md) to label images in an existing dataset (assisted labeling). |
| `M_PREPARE_IMAGES_CNN` | Specifies a CNN data preparation context.

This context is for image classification; it holds the settings with which to prepare images for CNN training or prediction (for example, cropping, resizing, or augmenting), using [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). |
| `M_PREPARE_IMAGES_DET` | Specifies an object detection data preparation context.

This context is for object detection; it holds the settings with which to prepare images for object detection training or prediction (for example, cropping, resizing, or augmenting), using [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). |
| `M_PREPARE_IMAGES_SEG` | Specifies a segmentation data preparation context.

This context is for segmentation; it holds the settings with which to prepare images for segmentation training or prediction (for example, cropping, resizing, or augmenting), using [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). |
| `M_STAT_ANO` | Specifies a statistics classification context for anomaly detection. This context holds the settings for which to calculate statistics on a dataset using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). |
| `M_STAT_CNN` | Specifies a statistics classification context for image classification. This context holds the settings for which to calculate statistics on a dataset using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). |
| `M_STAT_DET` | Specifies a statistics classification context for object detection. This context holds the settings for which to calculate statistics on a dataset using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). |
| `M_STAT_SEG` | Specifies a statistics classification context for segmentation. This context holds the settings for which to calculate statistics on a dataset using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). |
| `M_STAT_TREE_ENSEMBLE` | Specifies a statistics classification context for feature classification. This context holds the settings for which to calculate statistics on a dataset using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). |
| `M_TRAIN_ANO` | Specifies an anomaly detection training context.

This context is for anomaly detection; it holds the training settings with which to train an anomaly detection classifier context, using [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_TRAIN_CNN` | Specifies a CNN training context.

This context is for image classification; it holds the training settings with which to train a CNN classifier context, using [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_TRAIN_DET` | Specifies an object detection training context.

This context is for object detection; it holds the training settings with which to train an object detection classifier context, using [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_TRAIN_SEG` | Specifies a segmentation training context.

This context is for segmentation; it holds the training settings with which to train a segmentation classifier context, using [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_TRAIN_TREE_ENSEMBLE` | Specifies a tree ensemble training context.

This context is for feature classification; it holds the training settings with which to train a tree ensemble classifier context, using [`MclassTrain`](../../Reference/class/MclassTrain.md). |

### `ControlFlag` *(in, AIL_INT64)*

Specifies the predefined classifier to use. These classifiers are built by Aurora Imaging; you cannot modify their underlying architecture. If you are not allocating a predefined classifier context, set this parameter to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md).

*For specifying no predefined classifier*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies no predefined classifier context.

> **Note:** [`M_DEFAULT`](../../Reference/class/MclassAlloc.md) is the only possible setting if you are not allocating a predefined classifier context.

If you specify [`M_DEFAULT`](../../Reference/class/MclassAlloc.md) when allocating a predefined classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md)), you cannot directly train that classifier. In this case, you would typically copy a resulting trained classifier into that context ([`MclassCopyResult`](../../Reference/class/MclassCopyResult.md)). |

*For specifying a predefined CNN classifier*

| Value | Description |
| --- | --- |
| `M_ICNET_COLOR_XL` | Specifies an extra large ICNet classifier context that is for color images. This corresponds to the _ICNET_COLOR_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is intended for transfer learning. It has a recommended minimum receptive field of 195 pixels. You must use color (3-band) images with this classifier. |
| `M_ICNET_M` | Specifies a medium ICNet classifier context. This corresponds to the _ICNET_M.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a recommended minimum receptive field of 83 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |
| `M_ICNET_MONO_XL` | Specifies an extra large ICNet classifier context that is for monochrome images. This corresponds to the _ ICNET_MONO_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is intended for transfer learning. It has a recommended minimum receptive field of 195 pixels. You must use grayscale (1-band) images with this classifier. |
| `M_ICNET_S` | Specifies a small ICNet classifier context. This corresponds to the _ICNET_S.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a recommended minimum receptive field of 43 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |
| `M_ICNET_XL` | Specifies an extra large ICNet classifier context. This corresponds to the _ICNET_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a recommended minimum receptive field of 195 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |

*For specifying a predefined anomaly detection classifier*

| Value | Description |
| --- | --- |
| `M_ADNET` | Specifies an ADNet classifier context. This corresponds to the _ADNET.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. |

*For specifying a predefined object detection classifier*

| Value | Description |
| --- | --- |
| `M_ODNET` | Specifies an ODNet classifier context. This corresponds to the _ODNET.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. You cannot use float images with this classifier. Note, the receptive field concept does not apply to object detection. |

*For specifying a predefined segmentation classifier*

| Value | Description |
| --- | --- |
| `M_CSNET_COLOR_XL` | Specifies an extra large CSNet classifier context that is for color images. This corresponds to the _CSNET_COLOR_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is intended for transfer learning. It has a minimum receptive field of 195 pixels that increases in step sizes of 32 pixels. You must use color (3-band) images with this classifier. |
| `M_CSNET_M` | Specifies a medium CSNet classifier context. This corresponds to the _CSNET_M.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a minimum receptive field of 83 pixels that increases in step sizes of 8 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |
| `M_CSNET_MONO_XL` | Specifies an extra large CSNet classifier context that is for monochrome images. This corresponds to the _CSNET_MONO_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is intended for transfer learning. It has a minimum receptive field of 195 pixels that increases in step sizes of 32 pixels. You must use grayscale (1-band) images with this classifier. |
| `M_CSNET_S` | Specifies a small CSNet classifier context. This corresponds to the _ CSNET_S.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a minimum receptive field of 43 pixels that increases in step sizes of 4 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |
| `M_CSNET_XL` | Specifies an extra large CSNet classifier context. This corresponds to the _CSNET_XL.mclass_ context file that Aurora Imaging Library installed.

This classifier is for a complete training. It has a minimum receptive field of 195 pixels that increases in step sizes of 32 pixels. You can use images with up to 1024 bands (N-band) with this classifier. |

### `ContextClassIdPtr` *(out, *AIL_ID)*

Specifies the address of the variable in which to write the classifier, dataset, data preparation, or training context identifier or specifies the data type that the function should use to return the context identifier.

*For retrieving the identifier or specifying how to return it*

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that you will use this function's return value to obtain the identifier of the allocated classifier, dataset, data preparation,
                              statistics classification, or training context ; in this case, a standard Aurora Imaging Library identifier of type _AIL_ID_ is returned. |
| `M_UNIQUE_ID` | Specifies that you will use this function's return value to obtain the identifier of the allocated classifier, dataset, data preparation,
                              statistics classification, or training context ; in this case, an Aurora Imaging Library smart identifier of type _AIL_UNIQUE_CLASS_ID_is returned instead of a standard Aurora Imaging Library identifier.This setting is only available when using C++11 (or later).An Aurora Imaging Library smart identifier manages the lifespan of the Aurora Imaging Library object it owns (similar to a _std::unique_ptr_). Note, you can use an Aurora Imaging Library smart identifier as though it were a standard Aurora Imaging Library identifier, except that you cannot use it to manually free the classifier, dataset, data preparation,
                              statistics classification, or training context  (it is freed automatically). For more information, see [Aurora Imaging Library smart identifiers](../../UserGuide/C02_Building_an_application/S16_Custom_data_types_extensions_and_portability_functions.md). |
| `Address in which to write the classifier context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated classifier context.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAlloc.md) is written as the identifier. |
| `Address in which to write the data preparation context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated data preparation context.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAlloc.md) is written as the identifier. |
| `Address in which to write the dataset context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated dataset context.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAlloc.md) is written as the identifier. |
| `Address in which to write the statistics classification context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated statistics classification context.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAlloc.md) is written as the identifier. |
| `Address in which to write the training context identifier` | Specifies the address of an _AIL_ID_ in which to write the identifier of the allocated training context.

If allocation fails, [`M_NULL`](../../Reference/class/MclassAlloc.md) is written as the identifier. |

## Return Value

**Type:** `AIL_ID`

The returned value is the classifier context identifier either as a standard identifier (_AIL_ID_) or a smart identifier (_AIL_UNIQUE_CLASS_ID_). If allocation fails, `M_NULL` is returned (or _nullptr_ if[`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified).

When you allocate a classifier context, you must usually train it ([`MclassTrain`](../../Reference/class/MclassTrain.md)) before predicting with it ([`MclassPredict`](../../Reference/class/MclassPredict.md)).

For this context, do not specify a specific classifier that Aurora Imaging has built (that is, set the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md)).
