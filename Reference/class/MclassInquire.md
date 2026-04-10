---
doctype: Reference
module: class
function: MclassInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassInquire"
---

# MclassInquire

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

> Inquire about a classifier context, dataset context, class definition, data preparation context, training context, training result buffer, prediction result buffer, or the source layer of a predefined classifier or ONNX classifier.

## Syntax

```c
AIL_INT MclassInquire(
    AIL_ID    ContextOrResultClassId,  //in
    AIL_INT64 LabelOrIndex,            //in
    AIL_INT64 InquireType,             //in
    void *    UserVarPtr               //out
)
```

## Description

This function inquires about a setting of a classifier context, a dataset context, a data preparation context, a training context, class definitions, training result buffer, prediction result buffer, or the source layer of a predefined classifier (for example, a CNN). Note, class definitions are held in dataset and classifier contexts.

If the inquired setting is set to [`M_DEFAULT`](../../Reference/class/MclassControl.md) (for example, using [`MclassControl`](../../Reference/class/MclassControl.md)), [`MclassInquire`](../../Reference/class/MclassInquire.md) will return [`M_DEFAULT`](../../Reference/class/MclassInquire.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/class/MclassInquire.md) to the [`InquireType`](../../Reference/class/MclassInquire.md) parameter.

## Parameters

### `ContextOrResultClassId` *(in, AIL_ID)*

Specifies the identifier of the classification context or result buffer about which to inquire.

*For specifying the context or result buffer*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of a predefined CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX classifier context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md). |
| `ContextDataPreparationId` | Specifies the identifier of a data preparation context (for image data). This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md). |
| `ContextDatasetId` | Specifies the identifier of an images or features dataset context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). |
| `ContextTrainId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble training context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md), or [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `ResultPredictId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble prediction result buffer. These buffers must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `ResultTrainId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer. These buffers must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies what to inquire. This parameter can be set to one of the following values:

*For specifying what to inquire*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTHOR_INDEX` | Specifies the author in a dataset context about which to inquire. |
| `M_CLASS_INDEX` | Specifies the class definition in a classifier or dataset context about which to inquire. |
| `M_PREDICT_ENGINE_INDEX` | Specifies the predict engine about which to inquire. |
| `M_CONTEXT` *(default)* | Specifies to inquire about a global setting of a classification context. |
| `M_DEFAULT_SOURCE_LAYER` | Specifies to inquire about the input (initial) layer of the classifier in a classification context (for example, the source layer of the predefined CNN). In this case, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined CNN, object detection, segmentation, anomaly detection, or ONNX classifier. |
| `M_GENERAL` | Specifies to inquire about a general setting of a classification result buffer. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MclassInquire`](../../Reference/class/MclassInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For a classifier context (predefined CNN, object detection, segmentation, anomaly detection, or ONNX)

To inquire about a global setting of a predefined CNN, object detection, segmentation, anomaly detection, or ONNX classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined CNN, object detection, segmentation, or anomaly detection classifier context, or the identifier of an ONNX classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md), unless otherwise specified.

---

### `M_CLASSIFIER_PREDEFINED_TYPE`

Inquires the type of predefined classifier context that you are using. These contexts are built by Aurora Imaging and specified when you call [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md).

| Value | Description |
| --- | --- |
| `M_ICNET_COLOR_XL` | Specifies an extra large ICNet classifier context that is for color images. |
| `M_ICNET_M` | Specifies a medium ICNet classifier context. |
| `M_ICNET_MONO_XL` | Specifies an extra large ICNet classifier context that is for monochrome images. |
| `M_ICNET_S` | Specifies a small ICNet classifier context. |
| `M_ICNET_XL` | Specifies an extra large ICNet classifier context. |
| `M_CSNET_COLOR_XL` | Specifies an extra large CSNet classifier context that is for color images. |
| `M_CSNET_M` | Specifies a medium CSNet classifier context. |
| `M_CSNET_MONO_XL` | Specifies an extra large CSNet classifier context that is for monochrome images. |
| `M_CSNET_S` | Specifies a small CSNet classifier context. |
| `M_CSNET_XL` | Specifies an extra large CSNet classifier context. |
| `M_ODNET` | Specifies an ODNet classifier context. |
| `M_ADNET` | Specifies an ADNet classifier context. |
| `M_CUSTOM` | Specifies a custom classifier context. [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) can only return [`M_CUSTOM`](../../Reference/class/MclassInquire.md) if you contacted Aurora Imaging (Zebra) and received a classifier context that was customized for you. |
| `M_FCNET_COLOR_XL` | Specifies an extra large legacy FCNet classifier context that is for color images. |
| `M_FCNET_M` | Specifies a medium legacy FCNet classifier context. |
| `M_FCNET_MONO_XL` | Specifies an extra large legacy FCNet classifier context that is for monochrome images. |
| `M_FCNET_S` | Specifies a small legacy FCNet classifier context. |
| `M_FCNET_XL` | Specifies an extra large legacy FCNet classifier context. |
| `M_UNDEFINED` | Specifies an undefined classifier context. [`M_UNDEFINED`](../../Reference/class/MclassInquire.md) is returned if you allocate a predefined classifier context ([`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md)) with the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter set to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md).  If your classifier context is undefined, and you copy the result of a trained classifier context into it, the type of that undefined context is modified according to the type of the context copied into it. For example, if you copy an [`M_ICNET_M`](../../Reference/class/MclassInquire.md) context into an undefined classifier context, that context's classifier type is now also [`M_ICNET_M`](../../Reference/class/MclassInquire.md). |
| `M_USER_ONNX` | Specifies a user-defined ONNX classifier context. This requires importing an ONNX file ([`MclassImport`](../../Reference/class/MclassImport.md)). |

---

### `M_EXPIRY_DATE_STRING`

Inquires the expiration date of the context in string format. The date returned is formatted as follow: "2999-12-31 23:59:59".

| Value | Description |
| --- | --- |
| `"2999-12-31 23:59:59"` | Specifies the expiration date, if there is one.  Returns the date as a regular string, consisting of all characters including the terminating null character. The date in the string is organized as follows: `_YYYY_-_MM_-_DD_ _HH_:_MM_:_SS_`, where _YYYY_ refers to the year, _MM_ refers to the month, _DD_ refers to the day, _HH_ refers to the hour, _MM_ refers to the minute, and _SS_ refers to the second. An example of an expiration date that Aurora Imaging Library might one day return is: "2999-12-31 23:59:59\0". |
| `"None"` | Specifies that there is no expiration date. |

---

### `M_NUMBER_OF_PREDICT_ENGINES`

Inquires the number of predict engines detected. You can inquire the specifics of each predict engine using [`M_PREDICT_ENGINE_PROVIDER`](../../Reference/class/MclassInquire.md), [`M_PREDICT_ENGINE_DESCRIPTION`](../../Reference/class/MclassInquire.md) and [`M_PREDICT_ENGINE_PRECISION`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of predict engines. |

---

### `M_PREDICT_ENGINE_DESCRIPTION`

Inquires the description of the predict engine at the index specified by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to **M_PREDICT_ENGINE_INDEX()**.

| Value | Description |
| --- | --- |
| `""` | The default description in the classifier context is an empty string. Note, this is only available by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to [`M_CONTEXT`](../../Reference/class/MclassInquire.md). |
| `"Description"` | Specifies the text description of the engine type seen by the execution provider. |

---

### `M_PREDICT_ENGINE_PRECISION`

Inquires the precision of the predict engine at the index specified by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to **M_PREDICT_ENGINE_INDEX()**.

| Value | Description |
| --- | --- |
| `M_DEFAULT_PREDICT_ENGINE_PRECISION` | Specifies the default precision in classifier context. Note, this is only available by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to [`M_CONTEXT`](../../Reference/class/MclassInquire.md). |
| `M_FP16` | Specifies that the precision used by the predict engine is floating-point 16-bit.  > **Note:** Note, selecting a floating-point 16-bit predict engine for a model trained on floating-point 32-bit might cause a loss of precision or in extreme circumstances, floating-point overflow. |
| `M_FP32` | Specifies that the precision used by the predict engine is floating-point 32-bit. |

---

### `M_PREDICT_ENGINE_PROVIDER`

Inquires the provider of the predict engine at the index specified by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to **M_PREDICT_ENGINE_INDEX()**.  You must have installed the appropriate [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md) to perform GPU accelerated prediction using CUDA or OpenVINO.

| Value | Description |
| --- | --- |
| `M_CUDA` | Specifies that the predict engine provider is CUDA. |
| `M_DEFAULT_CPU` | Specifies the default predict engine provider. |
| `M_DEFAULT_PREDICT_ENGINE_PROVIDER` | Specifies the default predict engine provider in the classifier context. Note, this is only available by setting [`LabelOrIndex`](../../Reference/class/MclassInquire.md) to [`M_CONTEXT`](../../Reference/class/MclassInquire.md). |
| `M_OPENVINO` | Specifies that the predict engine provider is OpenVINO. |

---

### `M_PREDICT_ENGINE_USED`

Inquires which predict engine is used by the specified context. This value should match the value set by using [`M_PREDICT_ENGINE`](../../Reference/class/MclassControl.md) unless the order of the predict engines is modified. This can happen if the environment running Aurora Imaging Library is modified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the predict engine being used is the predict engine that was selected in the **Aurora Imaging Configurator** utility. |
| `M_INVALID` | Specifies that the predict engine is not found. |
| `0 <= Value < M_NUMBER_OF_PREDICT_ENGINES` | Specifies the index of the predict engine used. |

---

### `M_TRAINABLE_COMPLETE`

Inquires whether you can perform a complete type of training on the context.  Note that this inquire type always returns [`M_FALSE`](../../Reference/class/MclassInquire.md) when[`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) is set to [`M_USER_ONNX`](../../Reference/class/MclassInquire.md).  [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) is not trainable and will always return [`M_FALSE`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that you cannot perform a complete training. That is, you cannot set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_COMPLETE`](../../Reference/class/MclassControl.md). |
| `M_TRUE` | Specifies that you can perform a complete training. That is, you can set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_COMPLETE`](../../Reference/class/MclassControl.md). |

---

### `M_TRAINABLE_FINE_TUNING`

Inquires whether you can perform a fine tuning type of training on the context.  Note that this inquire type returns [`M_FALSE`](../../Reference/class/MclassInquire.md) when[`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) is set to [`M_USER_ONNX`](../../Reference/class/MclassInquire.md).  [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) is not trainable and will always return [`M_FALSE`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that you cannot perform a fine tuning type of training. That is, you cannot set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_FINE_TUNING`](../../Reference/class/MclassControl.md). |
| `M_TRUE` | Specifies that you can perform a fine tuning type of training. That is, you can set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_FINE_TUNING`](../../Reference/class/MclassControl.md). |

---

### `M_TRAINABLE_TRANSFER_LEARNING`

Inquires whether you can perform a transfer learning type of training on the context.  Note that this inquire type returns [`M_FALSE`](../../Reference/class/MclassInquire.md) when[`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) is set to [`M_USER_ONNX`](../../Reference/class/MclassInquire.md).  [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) is not trainable and will always return [`M_FALSE`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that you cannot perform a transfer learning type of training. That is, you cannot set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md). |
| `M_TRUE` | Specifies that you can perform a transfer learning type of training. That is, you can set [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) to [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md). |

### For a classifier context (segmentation or ONNX)

To inquire about a global setting of predefined segmentation or ONNX classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined segmentation or ONNX classifier context and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_TARGET_IMAGE_SIZE_BAND`

Inquires the number of bands of the image input to [`MclassPredict`](../../Reference/class/MclassPredict.md).  If the size is equal to 0 (default value), the preprocessing assumes the size of the target image is the same as the default source layer, [`M_SIZE_BAND`](../../Reference/class/MclassInquire.md). [`M_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassInquire.md) must be set to a value greater than 0 when the source layer equals [`M_DEFINED_BY_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassInquire.md).  > **Note:** Note, this is only available for ONNX classifiers.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `Value >= 0` | Specifies the number of bands. |

---

### `M_TARGET_IMAGE_SIZE_X`

Inquires the X-size of the image input to [`MclassPredict`](../../Reference/class/MclassPredict.md).  If the size is equal to 0 (default value), the preprocessing assumes the size of the target image is the same as the default source layer, [`M_SIZE_X`](../../Reference/class/MclassInquire.md).  For ONNX classifiers, [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md) must be set to a value greater than 0 if the X-size of the network is defined by the target image X-size; that is, if inquiring [`M_SIZE_X`](../../Reference/class/MclassInquire.md) returns [`M_DEFINED_BY_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md). Otherwise, [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md) must be set to 0 ([`M_DEFAULT`](../../Reference/class/MclassInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `Value >= 0` | Specifies the X-size. |

---

### `M_TARGET_IMAGE_SIZE_Y`

Inquires the Y-size of the image input to [`MclassPredict`](../../Reference/class/MclassPredict.md).  If the size is equal to 0 (default value), the preprocessing assumes the size of the target image is the same as the default source layer, [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).  For ONNX classifiers, [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md) must be set to a value greater than 0 if the Y-size of the network is defined by the target image Y-size; that is, if inquiring [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) returns [`M_DEFINED_BY_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md). Otherwise, [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md) must be set to 0 ([`M_DEFAULT`](../../Reference/class/MclassInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `Value >= 0` | Specifies the Y-size. |

### For a classifier context (object detection)

To inquire about a global setting of predefined object detection classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined object detection classifier context and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_NMS_IOU_THRESHOLD`

Inquires the IOU threshold value used by the NMS algorithm to determine if a given box proposal is already covered by an existing box proposal.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the IOU threshold. |

---

### `M_NMS_POST_K`

Inquires the maximum number of detections to keep after applying the NMS algorithm to the detection results specified by [`M_NMS_TOP_K`](../../Reference/class/MclassInquire.md). The rest of the detections will be discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of detections to keep. |

---

### `M_NMS_TOP_K`

Inquires the top K number of detection results (in terms of score) that will have the non-maximum suppression (NMS) algorithm applied. A lower value reduces the computation complexity of the algorithm.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of top detection results. |

### For a classifier context (object detection or anomaly detection)

To inquire about a global setting of a predefined object detection or anomaly detection classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined object detection or anomaly detection classifier context and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_SCORE_THRESHOLD`

Inquires the minimum score threshold for a successful detection result. For object detection, scores above this threshold are kept. For anomaly detection, only images or pixels above this score are considered anomalous.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the score threshold. |

### For a classifier context (tree ensemble)

To inquire about a global setting of a tree ensemble classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a tree ensemble classifier context and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_NUMBER_OF_FEATURES`

Inquires the number of features in entries that are used to train the classifier.  Note that all entries used for a predict must have exactly the number of features returned by this inquire.  You can only retrieve this value after calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of features used to train the classifier. |

### For a classifier context (predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX)

To inquire about a global setting of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX classifier context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX classifier context unless otherwise specified, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_CLASSIFIER_STATUS`

Inquires the state of the classifier.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default state of the classifier.  For a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, predefined or ONNX classifier context, [`M_DEFAULT`](../../Reference/class/MclassInquire.md) is the same as [`M_UNDEFINED`](../../Reference/class/MclassInquire.md).  For a tree ensemble classifier context, [`M_DEFAULT`](../../Reference/class/MclassInquire.md) is the same as [`M_UNTRAINED`](../../Reference/class/MclassInquire.md). |
| `M_IMPORTED_ONNX` | Specifies a classifier that is imported by the user. This requires importing an ONNX file using [`MclassImport`](../../Reference/class/MclassImport.md) with [`M_ONNX_FILE`](../../Reference/class/MclassImport.md). A classifier in this state can be used for [`MclassPredict`](../../Reference/class/MclassPredict.md) but not for [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_PRETRAINED` | Specifies a classifier that was trained by Aurora Imaging. A classifier in this state can be used with [`MclassPredict`](../../Reference/class/MclassPredict.md) and [`MclassTrain`](../../Reference/class/MclassTrain.md). Note, calling [`MclassTrain`](../../Reference/class/MclassTrain.md) with a pretrained classifier is typically for fine-tuning or to perform transfer learning. |
| `M_UNDEFINED` | Specifies an undefined classifier.  A predefined CNN, predefined object detection, predefined segmentation, anomaly detection, or ONNX context is considered undefined if it was allocated with the [`ControlFlag`](../../Reference/class/MclassAlloc.md) parameter set to [`M_DEFAULT`](../../Reference/class/MclassAlloc.md). You cannot train (or predict) with such a classifier.  Note that this state only occurs for predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or ONNX classifier contexts. |
| `M_UNTRAINED` | Specifies a classifier that has not been trained yet.  A classifier in this state can be used for [`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_USER_TRAINED` | Specifies a classifier that is trained by the user.  A classifier in this state can be used for [`MclassPredict`](../../Reference/class/MclassPredict.md) and [`MclassTrain`](../../Reference/class/MclassTrain.md). |

---

### `M_TRAINED_PARAMS_DATE`

Inquires the date at which the trained parameters changed in the context either by training or importing.

| Value | Description |
| --- | --- |
| `"2999-12-31 23:59:59"` | Specifies the date at which the trained parameters changed, if there is one.  Returns the date as a regular string, consisting of all characters including the terminating null character. The date in the string is organized as follows: `_YYYY_-_MM_-_DD_ _HH_:_MM_:_SS_`, where _YYYY_ refers to the year, _MM_ refers to the month, _DD_ refers to the day, _HH_ refers to the hour, _MM_ refers to the minute, and _SS_ refers to the second. An example of a date that Aurora Imaging Library might one day return is: "2999-12-31 23:59:59\0". |
| `"None"` | Specifies that there is no date. |

---

### `M_UUID`

Inquires the unique identifier of a trained or imported context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DEFAULT_UUID` *(default)* | Specifies the default unique identifier. |
| `Value >= 0` | Specifies the unique identifier of the classifier context. |

### For a classifier context's source layer (predefined CNN, predefined segmentation, predefined object detection, or ONNX)

To inquire about the source layer of the classifier in a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or ONNX classifier context, the [`InquireType`](../../Reference/class/MclassInquire.md) parameter can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_DEFAULT_SOURCE_LAYER`](../../Reference/class/MclassInquire.md).

---

### `M_BAND_ORDER`

Inquires the order of the input bands of the network. This corresponds to the order in which the network interpreted the bands of the images that were used in the training phase. If the bands in the target image ([`MclassPredict`](../../Reference/class/MclassPredict.md)) do not follow this order, Aurora Imaging Library internally compensates by temporarily copying the bands to be ordered as the network expects.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BGR` | Specifies that the bands are ordered according to the BGR (Blue, Green, Red) convention. |
| `M_RGB` *(default)* | Specifies that the bands are ordered according to the RGB (Red, Green, Blue) convention. |

---

### `M_SIZE_BAND`

Inquires the number of input bands of the network. This corresponds to the number of bands of the images that were used in the training phase.

| Value | Description |
| --- | --- |
| `M_DEFINED_BY_TARGET_IMAGE_SIZE_BAND` | Specifies that the input tensor of the imported ONNX model has a variable width dimension. The width dimension must be set using[`M_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassInquire.md)prior to preprocessing. [`M_DEFINED_BY_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassInquire.md)can only be returned if you are inquiring about an ONNX classifier ([`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md)). |
| `Value >= 1` | Specifies the number of bands. |

---

### `M_STEP_X`

Inquires the increment, in the X-direction, of the target image size for prediction. Note the following calculation, to establish the target image's X-size: `[`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md) = [`M_SIZE_X`](../../Reference/class/MclassInquire.md) + _m_ * [`M_STEP_X`](../../Reference/class/MclassInquire.md)`, where _m_ is an integer that is greater than or equal to 0.  [`M_STEP_X`](../../Reference/class/MclassInquire.md) is only available for legacy FCNet classifiers (outdated technology for performing image classification or segmentation).

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the appropriate increment, in the X-direction. |

---

### `M_STEP_Y`

Inquires the increment, in the Y-direction, of the target image size for prediction. Note the following calculation, to establish the target image's Y-size: `[`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md) = [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) + _m_ * [`M_STEP_Y`](../../Reference/class/MclassInquire.md)`, where _m_ is an integer that is greater than or equal to 0.  [`M_STEP_Y`](../../Reference/class/MclassInquire.md) is only available for legacy FCNet classifiers (outdated technology for performing image classification or segmentation).

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the appropriate increment, in the X-direction. |

---

### `M_TRAIN_IMAGE_MIN_SIZE_X`

Inquires the minimum image size, along the X-axis, to train this context. Aurora Imaging Library recommends using ICNet, CSNet, ODNet, or ADNet classifiers; for them, this is a suggested minimum. If you are using a legacy FCNet classifier, this is a required minimum.  Note, this is not supported when [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) returns [`M_USER_ONNX`](../../Reference/class/MclassInquire.md) and is not supported by [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) contexts.

| Value | Description |
| --- | --- |
| `Value > 0` | Inquires the minimum image size, in the X-axis, required to train this context. |

---

### `M_TRAIN_IMAGE_MIN_SIZE_Y`

Inquires the minimum image size, along the Y-axis, to train this context. Aurora Imaging Library recommends using ICNet, ODNet, CSNet, or ADNet classifiers; for them, this is a suggested minimum. If you are using a legacy FCNet classifier, this is a required minimum.  Note, this is not supported when [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) returns [`M_USER_ONNX`](../../Reference/class/MclassInquire.md) and is not supported by [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) contexts.

| Value | Description |
| --- | --- |
| `Value > 0` | Inquires the minimum image size, in the Y-axis, required to train this context. |

---

### `M_TRAIN_IMAGE_STEP_X`

Inquires the increment in image size, along the X-axis, to train an FCNet context.  [`M_TRAIN_IMAGE_STEP_X`](../../Reference/class/MclassInquire.md) is only available for legacy FCNet classifiers (outdated technology for performing image classification or segmentation).

| Value | Description |
| --- | --- |
| `Value >= 0` | Inquires the minimum image size, in the X-axis, required to train this context. |

---

### `M_TRAIN_IMAGE_STEP_Y`

Inquires the increment in image size, along the Y-axis, to train this context.  [`M_TRAIN_IMAGE_STEP_X`](../../Reference/class/MclassInquire.md) is only available for legacy FCNet classifiers (outdated technology for performing image classification or segmentation).

| Value | Description |
| --- | --- |
| `Value >= 0` | Inquires the minimum image size, in the Y-axis, required to train this context. |

---

### `M_TYPE`

Inquires the input type of the network. This corresponds to the data type and depth of the images that were used in the training phase (for example, [`M_UNSIGNED`](../../Reference/buf/MbufAlloc2d.md) + 8).

| Value | Description |
| --- | --- |
| `M_FLOAT + Depth value` | Specifies the floating-point data type, along with the depth, in bits. |
| `M_SIGNED + Depth value` | Specifies the signed data type, along with the depth, in bits. |
| `M_UNSIGNED + Depth value` | Specifies the unsigned data type, along with the depth, in bits. |

### For a data preparation context (CNN, object detection or segmentation)

To inquire about a data preparation context for image data, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a data preparation context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_AUGMENT_BALANCING`

Inquires the proportion of augmented entries that will go towards class balancing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 80.0. |
| `0.0 <= Value <= 100.0` | Specifies the proportion of augmented entries that will go towards class balancing. |

---

### `M_AUGMENT_CONTEXT_ID`

Inquires the identifier of the internal image processing augmentation context. This identifier is then what gives the user access to set the required controls for the image processing augment context.

| Value | Description |
| --- | --- |
| `Augmentation context identifier` | Specifies the identifier of the augmentation context. |

---

### `M_AUGMENT_NUMBER_ABSOLUTE`

Inquires the number of augmented entries to add to the destination dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `Value >= 0` | Specifies the number of augmented entries that should be added to the destination dataset. |

---

### `M_AUGMENT_NUMBER_FACTOR`

Inquires the ratio with which to establish the number of augmentations to add to the destination dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 3.0. |
| `Value >= 0.0` | Specifies the ratio. |

---

### `M_AUGMENT_NUMBER_MODE`

Inquires the mode with which to establish the number of augmentations to add to the destination dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies that the number of augmentations is established with [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassInquire.md). |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically establishes the number of augmentations.When augmenting a dataset, [`M_AUTO`](../../Reference/class/MclassInquire.md) is the same as [`M_FACTOR`](../../Reference/class/MclassInquire.md). When augmenting an image buffer, [`M_AUTO`](../../Reference/class/MclassInquire.md) is the same as [`M_DISABLE`](../../Reference/class/MclassInquire.md). |
| `M_DISABLE` | Specifies that no augmentations are performed. This does not affect other data preparations (these are still performed). |
| `M_FACTOR` | Specifies that the number of augmentations is established with [`M_AUGMENT_NUMBER_FACTOR`](../../Reference/class/MclassInquire.md). |

---

### `M_CENTER_MODE`

Inquires the mode with which to establish the center of the destination image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the center of the destination image is taken to be the center of the source image. |
| `M_USER_DEFINED` | Specifies the center for cropping is taken to be the user-defined [`M_CENTER_X`](../../Reference/class/MclassInquire.md) and [`M_CENTER_Y`](../../Reference/class/MclassInquire.md). |

---

### `M_CENTER_X`

Inquires the X-coordinate of the center point used by [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md). [`M_CENTER_X`](../../Reference/class/MclassInquire.md)only has an effect if [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the X-coordinate. |
| `M_DEFAULT` |  |
| `M_INVALID` *(default)* | Specifies an invalid pixel x center. Set the center to a valid value, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CENTER_X`](../../Reference/class/MclassControl.md). |

---

### `M_CENTER_Y`

Inquires the Y-coordinate of the center point used by [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md). [`M_CENTER_Y`](../../Reference/class/MclassInquire.md)only has an effect if [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the Y-coordinate. |
| `M_DEFAULT` |  |
| `M_INVALID` | Specifies an invalid pixel y center. Set the center to a valid value, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CENTER_Y`](../../Reference/class/MclassControl.md). |

---

### `M_DESTINATION_FOLDER_MODE`

Inquires the mode Aurora Imaging Library uses to manage the folder in which it saves the prepared images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CREATE` *(default)* | Specifies that the data preparation process creates a unique _prepare_ destination folder. |
| `M_OVERWRITE` | Specifies that the data preparation process uses the same _prepare_ destination folder. |

---

### `M_PREPARE_ORIGINAL_IMAGES`

Inquires the mode with which to add source entries to destination dataset, when cropping/resizing images with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the source dataset entries, for cropped/resized images, are added to the destination dataset, if they were not previously added. |
| `M_DISABLE` | Specifies that the source dataset entries, for cropped/resized images, are not added to the destination dataset. |
| `M_ENABLE` | Specifies that the source entries, for cropped/resized images, are added to the destination dataset, regardless of whether they were previously added. |

---

### `M_PREPARED_DATA_FOLDER`

Inquires the path of the destination folder at which the prepared dataset is saved.  An empty string implies the default path, which you can inquire with [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassInquire.md) + [`M_DEFAULT_PATH`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path of the prepared dataset folder. |

---

### `M_PRESET_ASPECT_RATIO`

Inquires the aspect ratio augmentation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the aspect ratio augmentation operation. |
| `M_ENABLE` | Specifies to enable the aspect ratio augmentation operation with preset values. |

---

### `M_PRESET_CROP`

Inquires whether the preset crop augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the crop augmentation operation. |
| `M_ENABLE` | Specifies to enable the crop augmentation operation with preset values. |

---

### `M_PRESET_FLIP`

Inquires whether the preset flip augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the flip augmentation operation. |
| `M_ENABLE` | Specifies to enable the flip augmentation operation with preset values. |

---

### `M_PRESET_GAMMA`

Inquires whether the gamma augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the gamma augmentation operation. |
| `M_ENABLE` | Specifies to enable the gamma augmentation operation with preset values. |

---

### `M_PRESET_HUE_OFFSET`

Inquires whether the preset hue offset augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the hue offset augmentation operation. |
| `M_ENABLE` | Specifies to enable the hue offset augmentation operation with preset values. |

---

### `M_PRESET_INTENSITY_ADD`

Inquires whether the preset intensity add augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the intensity add augmentation operation. |
| `M_ENABLE` | Specifies to enable the intensity add augmentation operation with preset values. |

---

### `M_PRESET_INTENSITY_MULTIPLY`

Inquires whether the preset intensity multiply augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the intensity multiply augmentation operation. |
| `M_ENABLE` | Specifies to enable the intensity multiply augmentation operation with preset values. |

---

### `M_PRESET_LIGHTING_DIRECTIONAL`

Inquires whether the preset directional lighting augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the directional lighting augmentation operation. |
| `M_ENABLE` | Specifies to enable the directional lighting augmentation operation with preset values. |

---

### `M_PRESET_NOISE_GAUSSIAN_ADDITIVE`

Inquires whether the Gaussian additive noise augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the Gaussian additive noise augmentation operation. |
| `M_ENABLE` | Specifies to enable the Gaussian additive noise augmentation operation with preset values. |

---

### `M_PRESET_NOISE_SALT_PEPPER`

Inquires whether the preset salt and pepper augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the salt and pepper noise augmentation operation. |
| `M_ENABLE` | Specifies to enable the salt and pepper noise augmentation operation with preset values. |

---

### `M_PRESET_REDUCE`

Inquires whether the preset reduce augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the reduce augmentation operation. |
| `M_ENABLE` | Specifies to enable the reduce augmentation operation with preset values. |

---

### `M_PRESET_ROTATION`

Inquires whether the preset rotation augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the rotation augmentation operation. |
| `M_ENABLE` | Specifies to enable the rotation augmentation operation with preset values. |

---

### `M_PRESET_SATURATION_GAIN`

Inquires whether the preset saturation gain augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the saturation gain augmentation operation. |
| `M_ENABLE` | Specifies to enable the saturation gain augmentation operation with preset values. |

---

### `M_PRESET_SCALE`

Inquires whether the preset scale augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the scale augmentation operation. |
| `M_ENABLE` | Specifies to enable the scale augmentation operation with preset values. |

---

### `M_PRESET_SMOOTH_GAUSSIAN`

Inquires whether the preset smooth Gaussian augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the smooth Gaussian augmentation operation. |
| `M_ENABLE` | Specifies to enable the smooth Gaussian augmentation operation with preset values. |

---

### `M_PRESET_TRANSLATION`

Inquires whether the preset translation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the translation augmentation operation. |
| `M_ENABLE` | Specifies to enable the translation augmentation operation with preset values. |

---

### `M_RESIZE_SCALE_FACTOR`

Inquires whether the preset resize scale factor augmentation is applied.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FILL_DESTINATION_BILINEAR` | Specifies to make all entries scale to fill the destination image size x/y determined by [`M_SIZE_MODE`](../../Reference/class/MclassInquire.md), [`M_SIZE_X`](../../Reference/class/MclassInquire.md), [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) and classifier passed in the [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) call using bilinear interpolation. |
| `M_FILL_DESTINATION_NEAREST_NEIGHBOR` | Specifies to make all entries scale to fill the destination image size x/y determined by [`M_SIZE_MODE`](../../Reference/class/MclassInquire.md), [`M_SIZE_X`](../../Reference/class/MclassInquire.md), [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) and classifier passed in the [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) call using nearest neighbor interpolation. |
| `0.0 < Value <= 16.0` *(default)* | Specifies the factor by which to resize. |

---

### `M_SIZE_MODE`

Inquires the mode with which to establish the size of the destination image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the size of the destination image will be the same as the source image if no classifier was given. |
| `M_USER_DEFINED` | Specifies the size of the destination image will be the user-defined [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md). |

### For a classifier context's source layer (predefined CNN, object detection, predefined segmentation, predefined anomaly detection, or ONNX) or a data preparation context

To inquire about the source layer of the classifier in a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or ONNX classifier context, or a data preparation context for image data, the [`InquireType`](../../Reference/class/MclassInquire.md) parameter can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or ONNX classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_DEFAULT_SOURCE_LAYER`](../../Reference/class/MclassInquire.md), or set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a data preparation context and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_SIZE_X`

Inquires the width of the network's source layer (the width of the images that were used in the training phase) when specifying a classifier context, or the width of the destination image when specifying a data preparation context (in pixels).

| Value | Description |
| --- | --- |
| `M_DEFINED_BY_TARGET_IMAGE_SIZE_X` | Specifies that the input tensor of the imported ONNX model has a variable width dimension. The width dimension must be set using[`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md)prior to preprocessing.  [`M_DEFINED_BY_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md) is only supported by [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) contexts. |
| `M_INVALID` | Specifies an invalid width of the destination image for a data preparation context. Set the size to a valid value, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_SIZE_X`](../../Reference/class/MclassControl.md). |
| `Value > 0.0` | For a data preparation context, this value specifies the width of the destination image.  For a classifier context, this value specifies the width of the network's source layer. |

---

### `M_SIZE_Y`

Inquires the height of the network's source layer (the height of the images that were used in the training phase) when specifying a classifier context, or the height of the destination image when specifying a data preparation context (in pixels).

| Value | Description |
| --- | --- |
| `M_DEFINED_BY_TARGET_IMAGE_SIZE_Y` | Specifies that the input tensor of the imported ONNX model has a variable width dimension. The width dimension must be set using[`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md)prior to preprocessing.  [`M_DEFINED_BY_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md) is only supported by [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) contexts. |
| `M_INVALID` | Specifies an invalid height of the destination image for a data preparation context. Set the size to a valid value, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_SIZE_Y`](../../Reference/class/MclassControl.md). |
| `Value > 0.0` | For a data preparation context, this value specifies the height of the destination image.  For a classifier context, this value specifies the height of the network's source layer. |

### For a dataset context (images or features)

To inquire about a dataset context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a dataset context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md), unless otherwise specified.  Note, these values apply to an images or a features dataset context, unless otherwise specified.

---

### `M_ACTIVE_AUTHOR_INDEX`

Inquires the author that is automatically added for labeling operations performed on a dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_CURRENT_USER`](../../Reference/class/MclassInquire.md). |
| `M_CURRENT_USER` | Specifies the system's current username as the author that is automatically defined for each dataset entry. To get the index of the current user, use [`M_CURRENT_USER_AUTHOR_INDEX`](../../Reference/class/MclassInquire.md). |
| `AIL_INT` | Specifies the author index. |

---

### `M_ACTIVE_AUTHOR_UPDATE`

Inquires the behavior of the entry's author after a modification was performed on the entry.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to keep the existing author's information. |
| `M_ENABLE` *(default)* | Specifies that when an entry's information (an entry's region information) is modified, its author is automatically modified according to [`M_ACTIVE_AUTHOR_INDEX`](../../Reference/class/MclassInquire.md). |

---

### `M_ANOMALY_DETECTION_FOLDER`

Inquires the path at which to save the anomaly detection scores.  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_ANOMALY_DETECTION_FOLDER_ABS`

Inquires the absolute path at which to save the anomaly detection scores. This path corresponds to the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md) + [`M_ANOMALY_DETECTION_FOLDER`](../../Reference/class/MclassInquire.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_AUTHOR_KEY`

Inquires the author's key (UUID). You must set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_AUTHOR_INDEX()`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `AIL_UUID Value` | Specifies the key of the author. The key is defined as an Aurora Imaging Library universal unique identifier (UUID). |

---

### `M_AUTHOR_NAME`

Inquires the name of an author in the dataset. You must set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_AUTHOR_INDEX()`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `"AuthorName"` | Specifies the name of an author. |

---

### `M_CURRENT_USER_AUTHOR_INDEX`

Inquires the author index of the current user.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies the current user does not exist as an author. |
| `AIL_INT` | Specifies the author index of the current user. |

---

### `M_DONT_CARE_CLASS_DRAW_COLOR`

Inquires the color of regions where [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) has been set to [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControlEntry.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COLOR_DONT_CARE_CLASS`](../../Reference/class/MclassInquire.md). |
| `M_RGB888` | Specifies the RGB value to use as the region's color. |
| `M_COLOR_BLACK` | Specifies the color black. |
| `M_COLOR_BLUE` | Specifies the color blue. |
| `M_COLOR_BRIGHT_GRAY` | Specifies the color bright gray. |
| `M_COLOR_CYAN` | Specifies the color cyan. |
| `M_COLOR_DARK_BLUE` | Specifies the color dark blue. |
| `M_COLOR_DARK_CYAN` | Specifies the color dark cyan. |
| `M_COLOR_DARK_GREEN` | Specifies the color dark green. |
| `M_COLOR_DARK_MAGENTA` | Specifies the color dark magenta. |
| `M_COLOR_DARK_RED` | Specifies the color dark red. |
| `M_COLOR_DARK_YELLOW` | Specifies the color dark yellow. |
| `M_COLOR_DONT_CARE_CLASS` | Specifies the color (255, 255, 255). |
| `M_COLOR_GRAY` | Specifies the color gray. |
| `M_COLOR_GREEN` | Specifies the color green. |
| `M_COLOR_LIGHT_BLUE` | Specifies the color light blue. |
| `M_COLOR_LIGHT_GRAY` | Specifies the color light gray. |
| `M_COLOR_LIGHT_GREEN` | Specifies the color light green. |
| `M_COLOR_LIGHT_WHITE` | Specifies the color light white. |
| `M_COLOR_MAGENTA` | Specifies the color magenta. |
| `M_COLOR_RED` | Specifies the color red. |
| `M_COLOR_WHITE` | Specifies the color white. |
| `M_COLOR_YELLOW` | Specifies the color yellow. |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_NO_CLASS_DRAW_COLOR`

Inquires the color used for unlabeled pixels when [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to [`M_NO_CLASS`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COLOR_NO_CLASS` *(default)* | Specifies the color (16, 0, 0). |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_NO_REGION_PIXEL_CLASS`

Inquires the class of any pixels that are not associated with any regions and therefore do not have an assigned value for[`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md). As [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassInquire.md) is associated with the UUID of the specified class, the returned index will vary with the index of the class.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DONT_CARE_CLASS` | Specifies that all pixels not assigned to a region will not contribute to training loss. |
| `M_NO_CLASS` *(default)* | Specifies that no class index is used for pixels not assigned to a region. |
| `0 <= Value < M_NUMBER_OF_CLASSES` | Specifies the index of the class that is used for pixels not assigned to a region. |

---

### `M_NUMBER_OF_AUGMENTED_ENTRIES`

Inquires the number of entries that are augmented.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of augmented entries. |

---

### `M_NUMBER_OF_AUTHORS`

Inquires the number of authors currently defined in the dataset.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of authors. |

---

### `M_NUMBER_OF_ENTRIES`

Inquires the number of entries defined in the dataset.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of entries in the dataset. |

---

### `M_NUMBER_OF_ENTRIES_GROUND_TRUTH`

Inquires the number of entries that have their ground truth specified ([`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md)).  To inquire the number of entries for every class in the context, set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md). To inquire the number of entries for a specific class in the context, set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to that class using **M_CLASS_INDEX()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of entries that have a ground truth. |

---

### `M_NUMBER_OF_ENTRIES_PREDICTED`

Inquires the number of entries that have their predicted class specified, for classification predictions (not for segmentation). Specifically, [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md) returns the number of entries that return [`M_TRUE`](../../Reference/class/MclassGetResultEntry.md) when calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_PREDICT_INFO`](../../Reference/class/MclassGetResultEntry.md).  To inquire the number of entries for every class in the context, set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md). To inquire the number of entries for a specific class in the context, set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to that class using **M_CLASS_INDEX()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of entries that have a predicted class. |

---

### `M_PREDICTED_SCORE_AVERAGE`

Inquires the average predicted best score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the average predicted score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md). |

---

### `M_PREDICTED_SCORE_MAX`

Inquires the maximum predicted best score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the maximum predicted score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md). |

---

### `M_PREDICTED_SCORE_MIN`

Inquires the minimum predicted best score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the minimum predicted score among the entries counted in [`M_NUMBER_OF_ENTRIES_PREDICTED`](../../Reference/class/MclassInquire.md). |

---

### `M_REGION_MASKS_FOLDER`

Inquires the path at which region masks are saved as they are added by [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path of the region masks folder. |

---

### `M_REGION_MASKS_FOLDER_ABS`

Inquires the absolute path at which to save region masks. This path corresponds to the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md) + [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassInquire.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_ROOT_PATH`

Inquires the root file path, such that [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md) + [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md), [`M_REGION_MASK_PATH`](../../Reference/class/MclassControlEntry.md), [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md), or [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md) gives a valid path. If [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md) is empty, [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md) should be an absolute path, otherwise it should be a relative path to [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `""` | Specifies an empty path. |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md). |
| `"RootFilePath"` | Specifies the root file path. |

---

### `M_SEGMENTATION_FOLDER`

Inquires the path at which to save the segmentation scores.  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

---

### `M_SEGMENTATION_FOLDER_ABS`

Inquires the absolute path at which to save the segmentation scores. This path corresponds to the concatenation of [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md) + [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassInquire.md).  This value only applies to an images dataset context.

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path. |

### For a classifier or dataset context (images or features)

To inquire about a classifier or dataset context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a classifier or dataset context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md), unless otherwise specified.  Note, these values apply to a classifier context or an images or a features dataset context, unless otherwise specified.

---

### `M_NUMBER_OF_CLASSES`

Inquires the number of class definitions (classes).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of classes. |

### For a class definition in a classifier or dataset context (for CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX)

To inquire about a class definition in a classifier or dataset context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of any type of dataset or classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to a specific class definition using **M_CLASS_INDEX()**.  Note, these values apply to class definitions in a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX classifier context, or in an images or features dataset context, unless otherwise specified.

---

### `M_ANOMALOUS`

Inquires whether the specified class is considered anomalous.  > **Note:** Note, the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter must be the identifier of an anomaly detection classifier context or images dataset used for anomaly detection.

| Value | Description |
| --- | --- |
| `M_FALSE` *(default)* | Specifies that images containing this class, either at the image or pixel level, will not be considered anomalous unless it also contains a class with [`M_ANOMALOUS`](../../Reference/class/MclassInquire.md) set to [`M_TRUE`](../../Reference/class/MclassInquire.md). |
| `M_TRUE` | Specifies that images containing this class, either at the image or pixel level, will be considered anomalous. |

---

### `M_CLASS_DRAW_COLOR`

Inquires the color assigned to a class definition. This color is used as a visual label.

| Value | Description |
| --- | --- |
| *(see [`M_CLASS_DRAW_COLOR`](Reference/class/MclassControl.md))* |  |
| `Byte-encoded RGB value` | Specifies an encoded RGB value. To verify if the value is a byte-encoded RGB value, use the**M_IS_RGB888** macro. To retrieve the R, G, and B bands, use the **M_RGB888_R**, **M_RGB888_G**, and **M_RGB888_B** macros. |

---

### `M_CLASS_ICON_ID`

Inquires the identifier of the image associated to the specified class definition. The identifier returned is not the same as the one supplied using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md). Aurora Imaging Library copies that image to the context and assigns it a new identifier, which is returned here.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the image associated with the class definition is freed.  Note that if the specified class definition does not have an associated image, this setting has no effect. |
| `Image identifier` | Specifies the identifier of the image buffer that Aurora Imaging Library associates to the class definition. |

---

### `M_CLASS_KEY`

Inquires the UUID (key), which is to be used to link between datasets, class labels, and a context class definition.

| Value | Description |
| --- | --- |
| `AIL_UUID Value` | Specifies the unique identifier of the class definition. |

---

### `M_CLASS_NAME`

Inquires the name of a class definition.

| Value | Description |
| --- | --- |
| `"ClassDefinitionName"` | Specifies the name of a class definition. |

---

### `M_CLASS_WEIGHT`

Inquires the class definition's weight. [`M_CLASS_WEIGHT`](../../Reference/class/MclassInquire.md) can only have an effect if [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md)).  Note that this inquire is only supported for a features dataset context, an images dataset context being used for segmentation, or a tree ensemble classifier context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the weight. |

### For a training context (segmentation or tree ensemble)

To inquire about a global setting of a segmentation or tree ensemble training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a segmentation or tree ensemble training context, unless otherwise specified, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_CLASS_WEIGHT_MODE`

Inquires how to establish the weight of a class definition.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically sets how the weight is established. |
| `M_BALANCE` | Specifies a balanced weight. |
| `M_INVERSE_CLASS_FREQUENCY` | Specifies to use weight factors equal to the inverse of each class frequency. |
| `M_NONE` | Specifies no weight. |
| `M_SQUARED_INVERSE_CLASS_FREQUENCY` | Specifies to use weight factors equal to the squared inverse of each class frequency. |
| `M_USER_DEFINED` | Specifies to use the weight defined with [`M_CLASS_WEIGHT`](../../Reference/class/MclassInquire.md). |

### For a training context (tree ensemble)

To inquire about a global setting of a tree ensemble training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a tree ensemble training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_BOOTSTRAP`

Inquires whether to use the bootstrap samples when building trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use bootstrap aggregation. |
| `M_WITH_REPLACEMENT` *(default)* | Specifies to use bootstrap aggregation, and allow replacement (chosen entries in a training set can be chosen again). |
| `M_WITHOUT_REPLACEMENT` | Specifies to use bootstrap aggregation, and not allow replacement (chosen entries in a training set cannot be chosen again). |

---

### `M_COMPUTE_OUT_OF_BAG_RESULTS`

Inquires whether to use out-of-bag dataset entries to estimate the generalization accuracy.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use out-of-bag entries. |
| `M_ENABLE` *(default)* | Specifies to use out-of-bag entries. |

---

### `M_COMPUTE_PROXIMITY_MATRIX`

Inquires whether to calculate the proximity measure matrix when building trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the proximity measure matrix when building trees. |
| `M_ENABLE` | Specifies to calculate the proximity measure matrix when building trees. |

---

### `M_FEATURE_IMPORTANCE_MODE`

Inquires how to establish the importance of the features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the feature importance mode. |
| `M_DROP_COLUMN` | Specifies that the feature importance is based on the drop column of features. |
| `M_MEAN_DECREASE_IMPURITY` *(default)* | Specifies that the feature importance is based on a decreasing impurity process. |
| `M_PERMUTATION` | Specifies that the feature importance is based on feature permutation. |

---

### `M_FEATURE_IMPORTANCE_SET`

Inquires how to establish the group of features with which to determine feature importance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the feature set to use. |
| `M_DEV_DATASET` | Specifies to use the development dataset. |
| `M_OUT_OF_BAG` | Specifies to use the out-of-bag set. |

---

### `M_MIN_IMPURITY_DECREASE`

Inquires a decrease of impurity requirement for splitting a node. That is, Aurora Imaging Library will split a node if it induces a decrease of impurity greater than or equal to the specified value.  The weighted impurity decrease equation is as follows: `_N_t_ / _N_ * (_impurity_ - _N_t_R_ / _N_t_ * _right_impurity_ - _N_t_L_ / _N_t_ * _left_impurity_)`, where _N_ is the total number of dataset entries,_ N_t_ is the number of dataset entries related to the current node, _N_t_L_ is the number of dataset entries related to the left child, and _N_t_R_ is the number of dataset entries related to the right child. _N_, _N_t_, _N_t_R_, and _N_t_L_ all refer to the weighted sum, if weights (entry weights and class weights) are used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the impurity value. |

---

### `M_MIN_NUMBER_OF_ENTRIES_LEAF`

Inquires the minimum number of related dataset entries, or a percentage of the minimum number of related dataset entries, that Aurora Imaging Library uses to identify a leaf node. Aurora Imaging Library uses this value to establish [`M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`](../../Reference/class/MclassInquire.md).  Note that if `Round((Value/100) * _NumberEntriesLeaf_)`== 0, then _NumberEntriesLeaf_ = 1.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < MinNumPercent <= 100.0` | Specifies the minimum number of entries, as a percentage. |
| `Value >= 1` *(default)* | Specifies the minimum number of entries. |

---

### `M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`

Inquires how to establish the minimum number of related dataset entries required to be at a leaf node. A split point at any depth will only be considered if it leaves at least the minimum number of entries (leaf training entries) in each of the left and right branches.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_LEAF`](../../Reference/class/MclassInquire.md). |
| `M_USER_DEFINED_VALUE` *(default)* | Specifies to use the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_LEAF`](../../Reference/class/MclassInquire.md). |

---

### `M_MIN_NUMBER_OF_ENTRIES_SPLIT`

Inquires the minimum number of related dataset entries, or a percentage of the minimum number of related dataset entries, that Aurora Imaging Library uses to determine how to best split the internal nodes of the trees. Aurora Imaging Library uses this value to establish [`M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`](../../Reference/class/MclassInquire.md).  Note that if `Round((Value/100) * _NumberEntriesSplit_)` == 0, then _NumberEntriesSplit_ = 2.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < MinNumPercent <= 100.0` | Specifies the minimum number of entries, as a percentage. |
| `Value >= 1` *(default)* | Specifies the minimum number of entries. |

---

### `M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`

Inquires how to establish the minimum number of related dataset entries required to split an internal node.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_SPLIT`](../../Reference/class/MclassInquire.md). |
| `M_USER_DEFINED_VALUE` *(default)* | Specifies to use the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_SPLIT`](../../Reference/class/MclassInquire.md). |

---

### `M_MIN_WEIGHT_FRACTION_LEAF`

Inquires the minimum weighted fraction of the sum total of weights (of all the dataset entries) required to be at a leaf node. Entries gave equal weight when [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassInquire.md) is set to [`M_NONE`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 0.5` *(default)* | Specifies the minimum weighted fraction. |

---

### `M_NODE_MAX_NUMBER_OF_FEATURES`

Inquires the maximum number of features, or a percentage of the maximum number of features that Aurora Imaging Library can use to determine how to best split the nodes of the trees.  Note that if `Round((Value/100)*_NumberOfFeatures_)`== 0, then _NumberOfFeatures_ = 1 (see [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies no maximum. |
| `0.0 <= MaxNumPercent <= 100.0` | Specifies the percentage with which to determine the maximum number of features. |
| `Value >= 1` | Specifies the value with which to determine the maximum number of features. |

---

### `M_NODE_MAX_NUMBER_OF_FEATURES_MODE`

Inquires how to establish the maximum number of features that Aurora Imaging Library uses to determine how to best split the nodes of the trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies that all available features are used. |
| `M_LOG2` | Specifies to use a base 2 logarithm, determined from the total number of features. |
| `M_SQUARE_ROOT` *(default)* | Specifies to use a square root, determined from the total number of features. |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the total number of features, where the percentage is set by [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassInquire.md). |
| `M_USER_DEFINED_VALUE` | Specifies to use the maximum number of features defined with [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassInquire.md). |

---

### `M_SPLIT_QUALITY_TYPE`

Inquires the function with which to measure the quality of a node split.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ENTROPY` | Specifies that Aurora Imaging Library uses an Entropy based function to measure the quality of a node split. |
| `M_GINI` *(default)* | Specifies that Aurora Imaging Library uses a Gini based function to measure the quality of a node split. |

---

### `M_TREE_MAX_DEPTH`

Inquires the maximum depth of the trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies no maximum depth. |
| `Value >= 0` | Specifies the maximum depth. |

---

### `M_TREE_MAX_NUMBER_OF_LEAF_NODES`

Inquires the maximum number of terminal nodes in the tree.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies no maximum. |
| `Value >= 1` | Specifies the maximum number of terminal nodes in the tree. |

### For a data preparation context (images) or a tree ensemble training context

To inquire about a global setting of a tree ensemble training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a tree ensemble training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).  To inquire about a data preparation context for image data, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a data preparation context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_SEED_MODE`

Inquires the mode with which to initialize the seed for tree ensemble training or image data augmentation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default mode with which to initialize the seed. |
| `M_AUTO` | Specifies that the seed is chosen at random, and that Aurora Imaging Library generates and uses a new seed each time you call[`MclassTrain`](../../Reference/class/MclassTrain.md). |
| `M_USER_DEFINED` | Specifies the user-defined seed set with [`M_SEED_VALUE`](../../Reference/class/MclassInquire.md). |

---

### `M_SEED_VALUE`

Inquires the seed value that Aurora Imaging Library uses to generate random numbers for tree ensemble training or image data augmentation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the seed value. |

### For a training context (CNN, object detection, segmentation, or anomaly detection)

To inquire about a global setting of a CNN, object detection, segmentation, anomaly detection, training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a CNN, object detection, segmentation, anomaly detection training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_CPU_TRAIN_ENGINE_LOAD_STATUS`

Inquires the status of the CPU train engine. This information can help you detect issues related to training with the CPU. [`M_CPU_TRAIN_ENGINE_LOAD_STATUS`](../../Reference/class/MclassInquire.md) is only available if [`M_TRAIN_ENGINE`](../../Reference/class/MclassControl.md) is set to [`M_CPU`](../../Reference/class/MclassControl.md), or if it is set to [`M_AUTO`](../../Reference/class/MclassControl.md)or [`M_DEFAULT`](../../Reference/class/MclassControl.md) and Aurora Imaging Library selected the CPU.

| Value | Description |
| --- | --- |
| `M_FAILURE` | Specifies that an unknown error occurred while trying to load the CPU train engine. |
| `M_SUCCESS` | Specifies that the CPU train engine loaded successfully (no issues were detected) and is ready for training. |
| `M_UNABLE_TO_FIND_CPU_TRAIN_ENGINE` | Specifies that the CPU train engine was not found. |
| `M_UNABLE_TO_LOAD_CPU_TRAIN_ENGINE` | Specifies that the CPU train engine was not loaded. |

---

### `M_GPU_TRAIN_ENGINE_LOAD_STATUS`

Inquires the status of the GPU train engine. This information can help you detect issues related to training with the GPU. [`M_GPU_TRAIN_ENGINE_LOAD_STATUS`](../../Reference/class/MclassInquire.md) is only available if [`M_TRAIN_ENGINE`](../../Reference/class/MclassControl.md) is set to [`M_GPU`](../../Reference/class/MclassControl.md), or if it is set to [`M_AUTO`](../../Reference/class/MclassControl.md)or [`M_DEFAULT`](../../Reference/class/MclassControl.md) and Aurora Imaging Library selected the GPU.

| Value | Description |
| --- | --- |
| `M_CUDA_FAIL` | Specifies that an unknown error occurred with CUDA. |
| `M_FAILURE` | Specifies that an unknown error occurred while trying to load the GPU train engine. |
| `M_INVALID_GPU_DRIVER_VERSION` | Specifies that the GPU driver version is not up to date. Verify that the latest NVIDIA drivers and display drivers are installed. Refer to Chapter 43 of the Aurora Imaging Library User Guide for more information. |
| `M_JIT_COMPILATION_REQUIRED` | Specifies that the GPU has a compute capability that is higher than the train engine. There could be a delay before the training starts. |
| `M_JIT_COMPILER_NOT_FOUND` | Specifies that a JIT compiler was not found. A JIT compilation is required; verify that the proper NVIDIA drivers are installed. |
| `M_SUCCESS` | Specifies that the GPU train engine loaded successfully (no issues were detected) and is ready for training. |
| `M_UNABLE_TO_FIND_GPU_TRAIN_ENGINE` | Specifies that the GPU train engine was not found. |
| `M_UNABLE_TO_FIND_VALID_GPU` | Specifies that an NVIDIA GPU was not found, or that the GPU's compute capability is lower than 3.7. |
| `M_UNABLE_TO_LOAD_GPU_TRAIN_ENGINE` | Specifies that the GPU train engine was not loaded. |

---

### `M_MINI_BATCH_SIZE`

Inquires the size of each mini-batch.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 32. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 4. For anomaly detection ([`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md)), the default is 1. |
| `Value > 0` | Specifies the size. |

---

### `M_TRAIN_ENGINE`

Inquires the train engine specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies the engine automatically. |
| `M_CPU` | Specifies the CPU. |
| `M_GPU` | Specifies the GPU. |

---

### `M_TRAIN_ENGINE_IS_INSTALLED`

Inquires whether a train engine is installed.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that a train engine is not installed. |
| `M_TRUE` | Specifies that a train engine is installed. |

---

### `M_TRAIN_ENGINE_USED`

Inquires which train engine is used. Note, the context must be preprocessed.

| Value | Description |
| --- | --- |
| `M_CPU` | Specifies that a CPU is used as the train engine. |
| `M_GPU` | Specifies that a GPU is used as the train engine. |

---

### `M_TRAIN_ENGINE_USED_DESCRIPTION`

Inquires the description of the engine (device) on which training is done ([`M_TRAIN_ENGINE_USED`](../../Reference/class/MclassInquire.md)). To inquire this information, the context must be preprocessed.

| Value | Description |
| --- | --- |
| `"TrainingDeviceName"` | Specifies the description of the engine. Examples of descriptions you can retrieve are "GeForce RTX (tm) 2060 6GB" and "Intel(R) Core(TM) i7-8700 CPU 3.20GHZ". |

### For a training context (CNN, object detection, or segmentation)

To inquire about a global setting of a CNN, object detection, or segmentation, training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a CNN, object detection, or segmentation training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_FOCAL_LOSS_GAMMA`

Inquires the focusing value for the focal loss when training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the focusing value. |

---

### `M_INITIAL_LEARNING_RATE`

Inquires the initial learning rate of training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 0.005. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 0.001. |
| `Value > 0.0` | Specifies the learning rate. |

---

### `M_LATEST_USED_RESET_TRAINING_VALUES`

Inquires the latest value passed to [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md).  > **Note:** Note that this inquire does not indicate if any of the values set by [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md) have been changed since the last call with [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPLETE` *(default)* | Specifies a complete training mode. |
| `M_FINE_TUNING` | Specifies a fine tuning training mode. |
| `M_TRANSFER_LEARNING` | Specifies a transfer learning mode. |

---

### `M_LEARNING_RATE_DECAY`

Inquires the learning rate decay of training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 0.1. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 0.05. |
| `Value > 0.0` | Specifies the learning rate decay. |

---

### `M_LOSS_FUNCTION_TYPE`

Inquires the type of loss function to use for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically establishes the type of loss function to use. |
| `M_CROSS_ENTROPY` | Specifies a cross entropy type of loss. |
| `M_DET_LOSS` | Specifies a detection type of loss. |
| `M_DICE_LOSS` | Specifies a dice type of loss. |
| `M_FOCAL_LOSS` | Specifies a focal type of loss. |

---

### `M_MAX_EPOCH`

Inquires the maximum number of epochs.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number. |

---

### `M_PREPARE_DATA_CONTEXT_ID`

Inquires the identifier of the internal prepare data context, allowing access to the prepare data API. [`MclassStream`](../../Reference/class/MclassStream.md)with [`M_SAVE`](../../Reference/class/MclassStream.md) or [`M_LOAD`](../../Reference/class/MclassStream.md), [`MclassSave`](../../Reference/class/MclassSave.md), and [`MclassRestore`](../../Reference/class/MclassRestore.md) allows you to save/load a copy of the internal prepare data context.

| Value | Description |
| --- | --- |
| `Data preparation context identifier` | Specifies the identifier of the data preparation context. |

---

### `M_SCHEDULER_TYPE`

Inquires the scheduler type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_CYCLICAL_DECAY` | Specifies to decay the learning rate at an internally established cyclical schedule. |
| `M_DECAY` | Specifies to decay the learning rate, as weights are updated. |
| `M_FACTOR_DECAY` | Specifies to decay the learning rate by a factor at every specified epoch. |

---

### `M_SPLIT_PERCENTAGE`

Inquires the split percentage parameter of [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) used in single dataset training (percentage of the source dataset that has been assigned to the training dataset).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the percentage. |

---

### `M_SPLIT_SEED_MODE`

Inquires the split seed mode used in single dataset training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RANDOM`](../../Reference/class/MclassInquire.md). |
| `M_FIXED` | Specifies to use the [`M_SPLIT_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for image classification (CNN) or feature classification (tree ensemble), or the[`M_SPLIT_SEG_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for segmentation. |
| `M_RANDOM` | Specifies to use the [`M_SPLIT_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for image classification (CNN) or feature classification (tree ensemble), or the[`M_SPLIT_SEG_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for segmentation. |

---

### `M_TRAIN_DESTINATION_FOLDER`

Inquires the path of the destination folder in which to save training results.  An empty string implies the default path, which you can inquire with [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassInquire.md) + [`M_DEFAULT_PATH`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `"Path"` | Specifies the path of the train destination folder. |

### For a training context (segmentation)

To inquire about a global setting of a segmentation training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a segmentation training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_CLASS_WEIGHT_STRENGTH`

Inquires the class weight strength when training with an inverse class frequency weight mode ([`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the proportion of weighting to assign to [`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassInquire.md). |

---

### `M_FEATURE_SIZE_X`

Inquires the width (X-size) of the smallest feature to segment. This acts as a lower bound to the X-size of the class regions that the classifier expects during training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MIN` *(default)* | Specifies the smallest size possible. |
| `Value > 0` | Specifies that the classifier will adjust to encompass features of the size you set, provided they are larger than the minimum possible feature size. |

---

### `M_FEATURE_SIZE_Y`

Inquires the height (Y-size) of the smallest feature to segment. This acts as a lower bound to the Y-size of the class regions that the classifier expects during training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MIN` | Specifies the smallest size possible. |
| `M_SAME` *(default)* | Specifies to use the same value as [`M_FEATURE_SIZE_X`](../../Reference/class/MclassInquire.md). |
| `Value > 0` | Specifies that the classifier will adjust to encompass features of the size you set, provided they are larger than the minimum possible feature size. |

---

### `M_USE_MASK_DESCRIPTORS`

Inquires whether to use mask descriptors for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use mask descriptors for training. |
| `M_ENABLE` *(default)* | Specifies to use mask descriptors for training. |

---

### `M_USE_POLYGON_DESCRIPTORS`

Inquires whether to use polygon descriptors for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use polygon descriptors for training. |
| `M_ENABLE` *(default)* | Specifies to use polygon descriptors for training. |

### For a training context (anomaly detection)

To inquire about a global setting of an anomaly detection training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of an anomaly detection training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_MINI_BATCHES_PER_SAMPLING`

Inquires how many mini-batches are loaded into memory before sampling when [`M_TRAIN_MODE`](../../Reference/class/MclassInquire.md) is set to [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 1` *(default)* | Specifies the number of mini-batches. |

---

### `M_SAMPLES_FIXED_SIZE`

Inquires the number of samples learned by the classifier.

| Value | Description |
| --- | --- |
| `Value > 0` *(default)* | Specifies the number of samples. |

---

### `M_TRAIN_MODE`

Inquires the mode in which the samples are selected.

| Value | Description |
| --- | --- |
| `M_AUTO` *(default)* | Specifies to automatically select the mode in which the samples are selected. |
| `M_GLOBAL_SAMPLING` | Specifies a global selection mode. |
| `M_ITERATIVE_SAMPLING` | Specifies an iterative selection mode. |

### For a training context (CNN, segmentation, or feature classification)

To inquire about a global setting of a CNN, segmentation, or tree ensemble training context, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to the value below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a CNN, segmentation, or tree ensemble training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_SUPPORT_MISSING_GROUND_TRUTH`

Inquires whether entries without a ground truth on training and development datasets are supported.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that entries without a ground truth are not allowed. |
| `M_ENABLE` | Specifies that entries without a ground truth are allowed. |

### For a statistics classification, classifier, or training context (CNN, object detection, segmentation, anomaly detection, or tree ensemble)

To inquire about a global setting of a statistics classification context, a classifier context, or a training context, the [`InquireType`](../../Reference/class/MclassInquire.md) parameter can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a statistics classification context, or a classifier or training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).  Note, these values apply to a statistics classification context, a CNN, object detection, segmentation, anomaly detection, or tree ensemble classifier or training context, or an ONNX classifier context, unless otherwise specified.

---

### `M_NUMBER_OF_TREES`

Inquires the number of trees in the classifier or training context. You can only retrieve this value after calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).  > **Note:** Note, this is only available for tree ensemble training or classifier contexts.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value.  For tree ensemble training context, the default value is 10.  > **Note:** Note that you cannot specify this value for tree ensemble classifier contexts. |
| `Value >= 1` | Specifies the number of trees in the classifier or training context. |

---

### `M_PREPROCESSED`

Inquires whether the classification or training context is preprocessed.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the context has not been preprocessed using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md). |
| `M_TRUE` | Specifies that the context has been preprocessed using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md). |

---

### `M_TIMEOUT`

Inquires the maximum calculation time for [`MclassPredict`](../../Reference/class/MclassPredict.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of time. |
| `Value > 0.0` | Specifies the maximum time, in msec. |

### For a prediction result buffer (object detection)

To inquire about a general setting of a prediction result buffer, the [`InquireType`](../../Reference/class/MclassInquire.md) can be set to the following value. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of an object detection prediction result buffer and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_GENERAL`](../../Reference/class/MclassInquire.md).

---

### `M_RESULT_OUTPUT_UNITS`

Inquires which units and coordinate system to use for prediction results.  This setting only affects positional results within the Aurora Imaging Library Classification module.  > **Note:** Note, this setting can be changed at any time to get results in the required units.

| Value | Description |
| --- | --- |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies to use world units if the target image is associated with a camera calibration context; otherwise, specifies to use pixel units. |
| `M_PIXEL` | Specifies to use pixel units with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to use world units with respect to the relative coordinate system. |

### For any supported context

To inquire about any classification context supported by this function, set the [`InquireType`](../../Reference/class/MclassInquire.md) the value below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a classifier context, a dataset context, or a training context, and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value` | Specifies the current value of the modification counter. |

### For any supported context or result buffer

To inquire about any classification context or result buffer supported by this function, set the [`InquireType`](../../Reference/class/MclassInquire.md) to the value below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a classifier context, a dataset context, a training context, a prediction result, or a training result and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md) or [`M_GENERAL`](../../Reference/class/MclassInquire.md).

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the context or result was allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For statistics classification

To inquire about statistics classification, set the [`InquireType`](../../Reference/class/MclassInquire.md) parameter to one of the values below. In addition, you can also inquire [`M_TIMEOUT`](../../Reference/class/MclassInquire.md).

---

### `M_MATCH_IOU_THRESHOLD`

Inquires the IOU threshold for an object detection stat context, based on which predicted boxes are compared to their matched ground truth boxes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the IOU threshold. |

---

### `M_MODE`

Inquires whether to calculate statistics for anomaly detection on an image level (default) or a pixel level.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMAGE_LEVEL` *(default)* | Specifies to calculate statistics for anomaly detection on an image level. |
| `M_PIXEL_LEVEL` | Specifies to calculate statistics for anomaly detection on a pixel level. |

---

### `M_SCORE_THRESHOLD_STEP`

Inquires the step increment of threshold values from 0.0 to 100.0.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the step increment for threshold. |

### Combination Constants — For inquiring the default path of the folder

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default path of the folder.

#### `M_DEFAULT_PATH`

Inquires the default path of the specified folder related inquire type.  The default path depends on whether the dataset paths are relative or is absolute. For relative type datasets, the default is under [`M_ROOT_PATH`](../../Reference/class/MclassInquire.md)\_FolderName_. For absolute type datasets, the default path is under C:\Users\Public\Documents\Classification\_FolderName_ on Windows and /var/lib/aurora_imaging/Classification/_FolderName_ on Linux. You can modify the default absolute path with the **Default destination folder** option under the **DL Classifiers** item in the **Aurora Imaging Configurator** utility.

| Value | Description |
| --- | --- |
| `"DefaultPath"` | Specifies the path. |

### Combination Constants — For inquiring the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Inquires the number of characters in the string. This number accounts for every character, including the terminating null character.

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_HAS_DEFAULT`

Inquires whether the specified inquire type has a default value.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type does not have a default value. |
| `M_TRUE` | Specifies that the inquire type has a default value. |

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported for the classification context.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_. Note that [`M_TYPE_AIL_ID`](../../Reference/class/MclassInquire.md) should only be used with [`M_OWNER_SYSTEM`](../../Reference/class/MclassInquire.md).

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

#### `M_TYPE_AIL_UUID`

Casts the requested information to an _AIL_UUID_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

For legacy FCNets, valid image sizes are given by the following formulas (for X and Y), where _k_ and _j_ are integers greater than or equal to zero:

`[`M_TRAIN_IMAGE_MIN_SIZE_X`](../../Reference/class/MclassInquire.md) + _k_ * [`M_TRAIN_IMAGE_STEP_X`](../../Reference/class/MclassInquire.md) and [`M_TRAIN_IMAGE_MIN_SIZE_Y`](../../Reference/class/MclassInquire.md) + _j_ * [`M_TRAIN_IMAGE_STEP_Y`](../../Reference/class/MclassInquire.md)`

This operation is not supported for binary (1-bit) images.

This operation is not supported for binary (1-bit) and grayscale (1-band) images.

> **Note:** To use this inquire, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a statistics classification context for object detection ([`M_STAT_DET`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

> **Note:** To use this inquire, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of a statistics classification context for anomaly detection ([`M_STAT_ANO`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

> **Note:** To use this inquire, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassInquire.md) parameter to the identifier of any statistics classification context ([`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassInquire.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassInquire.md).

Note, this is not supported when [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md)is set to [`M_USER_ONNX`](../../Reference/class/MclassInquire.md) but is supported for [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) contexts.
