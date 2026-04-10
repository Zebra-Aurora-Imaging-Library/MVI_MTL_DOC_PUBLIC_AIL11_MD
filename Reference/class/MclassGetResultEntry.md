---
doctype: Reference
module: class
function: MclassGetResultEntry
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassGetResultEntry"
---

# MclassGetResultEntry

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

> Get results from an entry in a dataset.

## Syntax

```c
void MclassGetResultEntry(
    AIL_ID    DatasetContextClassId,  //in
    AIL_INT64 EntryIndex,             //in
    AIL_UUID  EntryKey,               //in
    AIL_INT64 TaskType,               //in
    AIL_INT64 LabelOrIndex,           //in
    AIL_INT64 ResultType,             //in
    void *    ResultArrayPtr          //out
)
```

## Description

This function retrieves results of the specified type from an entry in an images or a features dataset context.

Before retrieving results from a dataset, you must call [`MclassTrain`](../../Reference/class/MclassTrain.md) or [`MclassPredict`](../../Reference/class/MclassPredict.md) with it. If you want to get results from a dataset after calling [`MclassTrain`](../../Reference/class/MclassTrain.md), you must copy the dataset-specific results from the result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces to a dataset ([`MclassCopyResult`](../../Reference/class/MclassCopyResult.md)) before calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md). This copy is not required for [`MclassPredict`](../../Reference/class/MclassPredict.md), since it can write results directly in the dataset.

## Parameters

### `DatasetContextClassId` *(in, AIL_ID)*

Specifies the identifier of the images or features dataset context from which to get results. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) and you must have called [`MclassTrain`](../../Reference/class/MclassTrain.md) and copied the results using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md) or have called [`MclassPredict`](../../Reference/class/MclassPredict.md) with it.

### `EntryIndex` *(in, AIL_INT64)*

Specifies the index of the entry for which to get results.

*For specifying the entry's index*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the [`EntryIndex`](../../Reference/class/MclassGetResultEntry.md) parameter is not required. |
| `Value >= 0` | Specifies the index of an entry. |

### `EntryKey` *(in, AIL_UUID)*

Specifies the key (_AIL_UUID_) of the entry for which to get results. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).

*For specifying the entry's key*

| Value | Description |
| --- | --- |
| `M_DEFAULT_KEY` | Specifies that the [`EntryKey`](../../Reference/class/MclassGetResultEntry.md) parameter is not required. |
| `AIL_UUID Value` | Specifies the key (_AIL_UUID_) of an entry. |

### `TaskType` *(in, AIL_INT64)*

Specifies the task (classification, segmentation, object detection, or anomaly detection) for which to get results. An entry in an images dataset can contain classification, segmentation, object detection, or anomaly detection results. An entry in a features dataset can only contain classification results.

*For specifying the type of task for which to get results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md). |
| `M_ANOMALY_DETECTION` | Specifies that you are getting anomaly detection results. |
| `M_AUTO` | Specifies the task for which to get results automatically, given the content in the dataset used ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)).

If the dataset selected is an images dataset, and it contains only image classification results,[`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md). If the dataset contains only segmentation results,[`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md). If the dataset only contains object detection results, [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md). If the dataset only contains anomaly detection results, [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_ANOMALY_DETECTION`](../../Reference/class/MclassGetResultEntry.md).

If the dataset contains two types of results, then [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) (if segmentation results are present), or [`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md) (if no segmentation results are present). If a result type ([`ResultType`](../../Reference/class/MclassGetResultEntry.md)) is specified that only applies to object detection, [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md).

If the dataset contains image classification, segmentation, object detection, and anomaly detection results, [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md).

If the dataset selected is a features dataset, [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) is the same as [`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md), since segmentation and object detection are not supported for a features dataset.

To determine whether [`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md), [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md), [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md), or [`M_ANOMALY_DETECTION`](../../Reference/class/MclassGetResultEntry.md) results are available in the dataset, use the [`M_PREDICT_INFO`](../../Reference/class/MclassGetResultEntry.md) result. Typically, a dataset should be set up for a specific task (either image classification, segmentation, object detection, or anomaly detection). |
| `M_CLASSIFICATION` | Specifies that you are getting classification results (image or feature). |
| `M_OBJECT_DETECTION` | Specifies that you are getting object detection results. |
| `M_SEGMENTATION` | Specifies that you are getting segmentation results. |

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies the class for which to get the entry result, or specifies that you are getting a general entry result. The values in this table are available for all tasks unless otherwise specified. Set this parameter to one of the following values.

*For specifying the class for which to get the entry result, or specifies to get a general entry result*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLASS_INDEX` | Specifies the index of the class for which to get the entry result. To get the number of classes, use [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResultEntry.md). |
| `M_INSTANCE_INDEX` | Specifies the index of the instance for which to get results. You can get the number of included instances in the result buffer using [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md).

This is only available for images datasets with object detection results. |
| `M_ALL_INSTANCES` | Specifies to retrieve results for all instances.

This is only available for images datasets with object detection results. |
| `M_GENERAL` *(default)* | Specifies to get a general entry result. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve from the dataset entry.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results.

## Parameter Associations

### For retrieving general entry results (images or features dataset)

To retrieve general results from an entry in the images or features dataset ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)), the [`ResultType`](../../Reference/class/MclassGetResultEntry.md) parameter can be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResultEntry.md) unless otherwise specified. You can retrieve results for any task ([`TaskType`](../../Reference/class/MclassGetResultEntry.md)).

---

### `M_BEST_CLASS_INDEX`

Retrieves the index of the class with the highest score for the entry.  For classification, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResultEntry.md) retrieves one index.  For segmentation, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResultEntry.md) retrieves an array of values (indices) that can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md). Each value is the index of the best class score for that X- Y-pixel location. The index of the class begins at 0.  For object detection, to retrieve the class index with the highest score for all instances, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). The size of the retrieved array is equal to the number of instances ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) set to [`M_GENERAL`](../../Reference/class/MclassGetResultEntry.md)) and will contain the class index with the highest score for each instance.  For object detection, to retrieve the class index with the highest score for all instances of a given class, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to **M_CLASS_INDEX()**. The size of the retrieved array is equal to the number of instances ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) set to **M_CLASS_INDEX()**) and will contain the class index with the highest score for each instance of a given class.  For object detection, to retrieve the class index with the highest score for a specific instance, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to **M_INSTANCE_INDEX()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the index of the class with the highest score for the entry. |

---

### `M_BEST_CLASS_SCORE`

Retrieves the highest class score for the entry.  For classification, [`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResultEntry.md) retrieves one score.  For segmentation, [`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResultEntry.md) retrieves an array of values (scores) that can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md). Each value is the best class score for that X- Y-pixel location. The index of the class begins at 0.  For object detection, to retrieve the highest score for all instances, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). The size of the retrieved array is equal to the number of instances ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) set to [`M_GENERAL`](../../Reference/class/MclassGetResultEntry.md)) and will contain the highest score for each instance.  For object detection, to retrieve the highest score for all instances of a given class, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to **M_CLASS_INDEX()**. The size of the retrieved array is equal to the number of instances ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) set to **M_CLASS_INDEX()**) and will contain the highest score for each instance of a given class.  For object detection, to retrieve the highest score for a specific instance, set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to **M_INSTANCE_INDEX()**.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the best class score. |

---

### `M_IMAGE_PREDICTED_ANOMALOUS`

Retrieves the anomaly class index at the image-level.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the image is normal. |
| `M_TRUE` | Specifies that the image is anomalous. |

---

### `M_IMAGE_SCORE`

Retrieves the anomaly score of the image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the best class score. |

---

### `M_MASK_IMAGE`

Retrieves the anomaly class index for each pixel in the image.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the pixel is normal. |
| `M_TRUE` | Specifies that the pixel is anomalous. |

---

### `M_NUMBER_OF_CLASSES`

Retrieves the total number of classes available. This refers to the number of outputs in the classifier's last layer.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of classes. |

---

### `M_PIXEL_SCORES`

Retrieves the anomaly score of each pixel in the image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the pixel's anomaly score. |

### For retrieving general entry results (images dataset)

To retrieve general results from an entry in the images dataset ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)), the [`ResultType`](../../Reference/class/MclassGetResultEntry.md) parameter can be set to one of the following values. In this case, you must set [`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md) parameter to the identifier of an images dataset, and the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResultEntry.md). You can retrieve results for any task ([`TaskType`](../../Reference/class/MclassGetResultEntry.md)).

---

### `M_BEST_INDEX_IMAGE_TYPE`

Retrieves the image type to provide when calling [`MclassDraw`](../../Reference/class/MclassDraw.md) or [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md) with [`M_DRAW_BEST_INDEX_IMAGE`](../../Reference/class/MclassDraw.md) or [`M_DRAW_BEST_INDEX_CONTOUR_IMAGE`](../../Reference/class/MclassDraw.md).

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the image type is 8-bit unsigned. |
| `M_UNSIGNED + 16` | Specifies that the image type is 16-bit unsigned. |

---

### `M_CLASSIFIER_PREDEFINED_TYPE`

Retrieves the type of the predefined classifier.

| Value | Description |
| --- | --- |
| `M_ADNET` | Specifies an ADNet classifier context for anomaly detection. |
| `M_CSNET_COLOR_XL` | Specifies an extra large CSNet classifier context that is for color images. |
| `M_CSNET_M` | Specifies a medium CSNet classifier context. |
| `M_CSNET_MONO_XL` | Specifies an extra large CSNet classifier context that is for monochrome images. |
| `M_CSNET_S` | Specifies a small CSNet classifier context. |
| `M_CSNET_XL` | Specifies an extra large CSNet classifier context. |
| `M_CUSTOM` | Specifies a custom classifier context.  This is a predefined classifier for image classification ([`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md)), segmentation ([`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md), or object detection ([`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md)). |
| `M_FCNET_COLOR_XL` | Specifies an extra large legacy FCNet classifier context that is for color images. |
| `M_FCNET_M` | Specifies a medium legacy FCNet classifier context. |
| `M_FCNET_MONO_XL` | Specifies an extra large legacy FCNet classifier context that is for monochrome images. |
| `M_FCNET_S` | Specifies a small legacy FCNet classifier context. |
| `M_FCNET_XL` | Specifies an extra large legacy FCNet classifier context. |
| `M_ICNET_COLOR_XL` | Specifies an extra large ICNet classifier context that is for color images. |
| `M_ICNET_M` | Specifies a medium ICNet classifier context. |
| `M_ICNET_MONO_XL` | Specifies an extra large ICNet classifier context that is for monochrome images. |
| `M_ICNET_S` | Specifies a small ICNet classifier context. |
| `M_ICNET_XL` | Specifies an extra large ICNet classifier context. |
| `M_ODNET` | Specifies an ODNet classifier context. |
| `M_USER_ONNX` | Specifies an ONNX classifier context. |

---

### `M_PREDICT_INFO`

Retrieves whether the entry contains results from the specified task ([`TaskType`](../../Reference/class/MclassGetResultEntry.md)).  For example, if your task is segmentation, you will get [`M_TRUE`](../../Reference/class/MclassGetResultEntry.md) if the entry contains segmentation results and [`M_FALSE`](../../Reference/class/MclassGetResultEntry.md) if it does not.  When using [`M_PREDICT_INFO`](../../Reference/class/MclassGetResultEntry.md), you must specify a specific task (you cannot set the [`TaskType`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_AUTO`](../../Reference/class/MclassGetResultEntry.md) or [`M_DEFAULT`](../../Reference/class/MclassGetResultEntry.md)). If you never called [`MclassTrain`](../../Reference/class/MclassTrain.md) or [`MclassPredict`](../../Reference/class/MclassPredict.md) with the specified dataset ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)), [`M_PREDICT_INFO`](../../Reference/class/MclassGetResultEntry.md) always returns [`M_FALSE`](../../Reference/class/MclassGetResultEntry.md).

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the entry does not contain results from the specified task ([`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md), [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md), [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md), or [`M_ANOMALY_DETECTION`](../../Reference/class/MclassGetResultEntry.md)). |
| `M_TRUE` | Specifies that the entry does contain results from the specified task ([`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md), [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md), [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md), or [`M_ANOMALY_DETECTION`](../../Reference/class/MclassGetResultEntry.md)). |

---

### `M_PREDICTION_SIZE_X`

Retrieves the number of class scores available for the entry, along the X-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of class scores available for the entry, along the X-direction. For image classification (which uses the entire entry image), [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) always returns 1. |

---

### `M_PREDICTION_SIZE_Y`

Retrieves the number of class scores available for the entry, along the Y-direction.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of class scores available for the entry, along the Y-direction. For image classification (which uses the entire entry image), [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md) always returns 1. |

---

### `M_RECEPTIVE_FIELD_OFFSET_X`

Retrieves the offset along the X-axis, from the top-left corner of the target image, needed to place the first class score at the center of its receptive field.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the receptive field offset along the X-axis. |

---

### `M_RECEPTIVE_FIELD_OFFSET_Y`

Retrieves the offset along the Y-axis, from the top-left corner of the target image, needed to place the first class score at the center of its receptive field.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the receptive field offset along the Y-axis. |

---

### `M_RECEPTIVE_FIELD_SIZE_X`

Retrieves the size of the receptive field along the X-axis.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the receptive field along the X-axis. |

---

### `M_RECEPTIVE_FIELD_SIZE_Y`

Retrieves the size of the receptive field along the Y-axis.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the size of the receptive field along the Y-axis. |

---

### `M_RECEPTIVE_FIELD_STRIDE_X`

Retrieves the stride spacing the receptive field centers in the target image along the X-axis.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the stride spacing the receptive fields along the X-axis. |

---

### `M_RECEPTIVE_FIELD_STRIDE_Y`

Retrieves the stride spacing the receptive field centers in the target image along the Y-axis.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the stride spacing the receptive fields along the Y-axis. |

### For retrieving general or class-specific entry results

To retrieve general or class-specific results from an entry in the images dataset ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)), the [`ResultType`](../../Reference/class/MclassGetResultEntry.md) parameter can be set to one of the following values. In this case, you can set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResultEntry.md) or a specific class. Unless otherwise specified, you can retrieve results for any task ([`TaskType`](../../Reference/class/MclassGetResultEntry.md)) or dataset (images or features).

---

### `M_NUMBER_OF_INSTANCES`

Retrieves the number of instances.  [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) is only available for [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md) tasks.

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the number of instances. |

---

### `M_NUMBER_OF_PREDICTIONS`

Retrieves the total number of class scores.  For general results, the total number of class scores in an images dataset corresponds to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md) * [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResultEntry.md). The total number of scores in a features dataset is equal to the number of classes.  For class-specific results, the total number of scores in an images dataset corresponds to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md). The total number of scores for a features dataset is one.  The class score data that gets returned is organized planar-wise. Specifically, the first batch of [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md) scores are for the first class, and the subsequent batches are for the remaining classes.

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the total number of class scores. |

---

### `M_PREDICTION_SCORES`

Retrieves the score of every class. Scores are returned in an array. The number of returned scores is retrievable with [`M_NUMBER_OF_PREDICTIONS`](../../Reference/class/MclassGetResultEntry.md).  For general results, the returned values are organized planar-wise in a 3D volume of size [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) *[`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md) * [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResultEntry.md). Note, each band contains the score of one given class. For example, the index in this returned array of the score of class _C_ at pixel (X, Y) is: `_Index_ = (_C_ * [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) *[`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md)) + (_Y_ * [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md)) + _X_`.  For class-specific results, the returned values are organized in a vector that corresponds to a 2D volume of size [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) *[`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md). For example, the index in this returned array of the score at location (X, Y) is: `_Index_ = (_Y_ * [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResultEntry.md)`+ _X_).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies all of the class scores. |

### For retrieving results for a specific class, a specific object detection instance, or all instances (images dataset)

To retrieve results from a specific class, a specific object detection instance, or all instances from an entry in an images dataset ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)), the [`ResultType`](../../Reference/class/MclassGetResultEntry.md) parameter can be set to one of the following values. In this case, you must set the [`LabelOrIndex`](../../Reference/class/MclassGetResultEntry.md) parameter to **M_CLASS_INDEX()**, **M_INSTANCE_INDEX()**, or [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResultEntry.md) and you must set the [`TaskType`](../../Reference/class/MclassGetResultEntry.md) parameter to [`M_OBJECT_DETECTION`](../../Reference/class/MclassGetResultEntry.md).

---

### `M_BOX_4_CORNERS`

Retrieves an array containing the coordinates of the four corners of the specified bounding boxes.  The 8 coordinates of the first box (b1) are grouped, followed by the 8 coordinates of the second box (b2), repeating this process for all specified boxes in the order b1x1, b1y1, b1x2, b1y2, b1x3, b1y3, b1x4, b1y4, b2x1, b2y1, b2x2, b2y2, b2x3, b2y3, b2x4, b2y4, etc.  *[Image: MclassBox4Corners.png]*  The size of the array will be equal to the number of specified instances ([`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md)) multiplied by 8.

---

### `M_CENTER_X`

Retrieves the X-coordinates of the centers of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the X-coordinates are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

---

### `M_CENTER_Y`

Retrieves the Y-coordinates for the centers of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the Y-coordinates are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

---

### `M_HEIGHT`

Retrieves the heights of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the heights are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). Only one height will be returned if you are retrieving results for a specific instance.

---

### `M_WIDTH`

Retrieves the widths of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the widths are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResultEntry.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

### Combination Constants — For determining the required number of elements in the array (array size)

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required number of elements in the array (array size).

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For determining whether results are available to be returned

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available to be returned.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

This [`ResultType`](../../Reference/class/MclassGetResultEntry.md) value is unavailable if your dataset results ([`DatasetContextClassId`](../../Reference/class/MclassGetResultEntry.md)) were produced using [`MclassPredict`](../../Reference/class/MclassPredict.md) with an ONNX classifier (that is, calling [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) must not return [`M_USER_ONNX`](../../Reference/class/MclassInquire.md)).

To calculate the center of the receptive field of each result returned for[`M_PREDICTION_SCORES`](../../Reference/class/MclassGetResultEntry.md) (_Y_,_X_) in the target image, use the following formula: `(_X_` * [`M_RECEPTIVE_FIELD_STRIDE_X`](../../Reference/class/MclassGetResultEntry.md) + [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResultEntry.md); _Y_ * [`M_RECEPTIVE_FIELD_STRIDE_Y`](../../Reference/class/MclassGetResultEntry.md) + [`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResultEntry.md)).

This is a predefined classifier for image classification ([`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md)).

This is a predefined classifier for segmentation ([`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md)).

This is a predefined classifier for image classification ([`M_CLASSIFICATION`](../../Reference/class/MclassGetResultEntry.md)) or segmentation ([`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md)).
