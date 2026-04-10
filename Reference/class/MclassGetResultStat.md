---
doctype: Reference
module: class
function: MclassGetResultStat
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassGetResultStat"
---

# MclassGetResultStat

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

> Get results from a statistics classification result buffer.

## Syntax

```c
void MclassGetResultStat(
    AIL_ID     StatResultClassId,  //in
    AIL_INT64  LabelOrIndex1,      //in
    AIL_INT64  LabelOrIndex2,      //in
    AIL_DOUBLE ScoreThreshold,     //in
    AIL_INT64  ResultType,         //in
    void *     ResultArrayPtr      //out
)
```

## Description

This function retrieves results of the specified type from a statistics classification result buffer. Results are available after calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md).

## Parameters

### `StatResultClassId` *(in, AIL_ID)*

Specifies the identifier of the statistics classification result buffer from which to retrieve results. The result buffer must have been previously allocated on the system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

### `LabelOrIndex1` *(in, AIL_INT64)*

Specifies the first index for which to retrieve results. Supported indices depend on the statistics classification result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)) and the result type ([`ResultType`](../../Reference/class/MclassGetResultStat.md)). For details about the index to set, see the description of the result type in the parameter associations section below.

*For specifying the first index for which to retrieve results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLASS_INDEX` | Specifies the index of the class for which to get results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an image classification, object detection, or segmentation statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_INSTANCE_INDEX` | Specifies the index of the instance (in a specific entry) for which to get results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_REGION_INDEX` | Specifies the index of the region (in a specific entry) for which to get results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_ALL_INSTANCES` | Specifies to retrieve results for all instances (in a specific entry). This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_ALL_REGIONS` | Specifies to retrieve results for all regions (in a specific entry). This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_ANOMALOUS` | Specifies to get results related to anomalous entries only. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an anomaly detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_GENERAL` *(default)* | Specifies to get general results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from any classification statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_NO_CLASS` | Specifies to get results related to entries not assigned to a class. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_NON_ANOMALOUS` | Specifies to get results related to non-anomalous entries only. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an anomaly detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |

### `LabelOrIndex2` *(in, AIL_INT64)*

Specifies the second index for which to retrieve results. Supported indices depend on the statistics classification result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)) and the result type ([`ResultType`](../../Reference/class/MclassGetResultStat.md)). For details about the index to set, see the description of the result type in the parameter associations section below.

*For specifying the second index for which to retrieve results*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLASS_INDEX` | Specifies the index of the class for which to get results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an image classification, object detection, or segmentation statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_ANOMALOUS` | Specifies to get results related to anomalous entries only. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an anomaly detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_GENERAL` *(default)* | Specifies to get general results. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from any classification statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_NO_CLASS` | Specifies to get results related entries not assigned to a class. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an object detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |
| `M_NON_ANOMALOUS` | Specifies to get results related to non-anomalous entries only. This is supported for specific result types ([`ResultType`](../../Reference/class/MclassGetResultStat.md)) retrieved from an anomaly detection statistics result buffer ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md)). |

### `ScoreThreshold` *(in, AIL_DOUBLE)*

Specifies the score threshold, when retrieving result types that use this information. When retrieving result types that do not use information, set this parameter to [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md). When applicable, only results with scores greater than or equal to the specified threshold are returned. For details about when and how the score threshold applies, see the description of the result type in the parameter associations section below.

*For specifying score threshold, when retrieving result types that use this information*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the score threshold does not apply. |
| `M_ALL` | Specifies to sequentially retrieve results from all score thresholds in an array, when applicable. Score thresholds can be adjusted using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_SCORE_THRESHOLD_STEP`](../../Reference/class/MclassControl.md). To retrieve the score threshold values, call [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) with [`M_SCORE_THRESHOLDS`](../../Reference/class/MclassGetResultStat.md). |
| `0.0 <= Value <= 100.0` | Specifies the score threshold. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results.

## Parameter Associations

### For any statistics result buffer

To retrieve results for any statistics result buffer, set the [`ResultType`](../../Reference/class/MclassGetResultStat.md) parameter to one of the following values.

---

### `M_ACCURACY`

Retrieves the accuracy of a single class versus other classes according to the specified score threshold.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy. |

---

### `M_ACCURACY_MACRO`

Retrieves the macro accuracy of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro accuracy. |

---

### `M_ACCURACY_OVERALL`

Retrieves the overall accuracy of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the overall accuracy. |

---

### `M_ACCURACY_WEIGHTED`

Retrieves the weighted accuracy of the entire confusion matrix, computed by taking the accuracy for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted accuracy. |

---

### `M_AUC_ROC`

Retrieves the area under the curve of the receiver operating characteristics (recall as a function of false positive rate) for a single class vs other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve area under the curve value (can occur if either all of the ground instances are of the given class, or none of them are). |
| `0.0 <= Value <= 100.0` | Specifies the area under the curve. |

---

### `M_AUC_ROC_MACRO`

Retrieves the average area under the curve of the receiver operating characteristics of all classes, giving the same importance to all classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve area under the curve This can occur if one of the class-index specific [`M_AUC_ROC`](../../Reference/class/MclassGetResultStat.md) result is [`M_INVALID`](../../Reference/class/MclassGetResultStat.md). |
| `0.0 <= Value <= 100.0` | Specifies the area under the curve. |

---

### `M_AUC_ROC_WEIGHTED`

Retrieves the weighted area under the curve of the receiver operating characteristics of all classes, computed by taking the area under the curve for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve area under the curve This can occur if one of the class-index specific [`M_AUC_ROC`](../../Reference/class/MclassGetResultStat.md) result is [`M_INVALID`](../../Reference/class/MclassGetResultStat.md). |
| `0.0 <= Value <= 100.0` | Specifies the area under the curve. |

---

### `M_AVERAGE_PRECISION`

Retrieves an approximation of the area under the precision-recall curve for a single class versus other classes. It consists of the averaged interpolated precision across all selected score thresholds for the selected IOU threshold.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), object detection ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the average precision. |

---

### `M_AVERAGE_PRECISION_MACRO`

Retrieves the mean average precision of all classes, giving the same importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the mean average precision. |

---

### `M_AVERAGE_PRECISION_WEIGHTED`

Retrieves the weighted mean average precision of all classes, computed by taking the mean average precision for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted mean average precision. |

---

### `M_CONFUSION_MATRIX`

Retrieves, in row major order, the full NxN confusion matrix across all classes, using best class index to populate the matrix. Rows of the confusion matrix are associated to ground truths classes and columns to predicted classes.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Any classification statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | 0.0 &lt;= Value &lt;= 100.0 |  For object detection, an extra row/column is added at the end for the no class type. The [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter setting is also supported as a criterion to populate the matrix. For anomaly detection, the confusion matrix is 2x2, for [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) in the first row/column and [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) in the second row/column.  [`M_CONFUSION_MATRIX`](../../Reference/class/MclassGetResultStat.md) can also return, in row major order, the 2x2 1 vs other classes confusion matrix, depending on [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) parameter setting. To do this, for feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), object detection ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result IDs, you must set the [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md)parameter to **M_CLASS_INDEX()**; for an anomaly detection statistics result ID ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), you must set the [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) parameter to [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md).  In these cases, you can use the [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter setting as a criterion on the selected class to populate the matrix. Note that for anomaly detection, [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) and [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) are specified to get that class versus the other.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the confusion matrix values. |

---

### `M_CONFUSION_MATRIX_COLUMN`

Retrieves the same value as the main confusion matrix result type, except for a single column of the confusion matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the confusion matrix values. |

---

### `M_CONFUSION_MATRIX_ELEMENT`

Retrieves the same value as the main confusion matrix result type, except for a single element of the confusion matrix. The [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) and [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) parameters specify row and column, respectively.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the confusion matrix element. |

---

### `M_CONFUSION_MATRIX_PERCENTAGE`

Retrieves the same type of result as [`M_CONFUSION_MATRIX`](../../Reference/class/MclassGetResultStat.md), with the values in each row normalized to sum up to 100%.  Class indices with no ground truth instances will have the elements of their corresponding row set to[`M_INVALID`](../../Reference/class/MclassGetResultStat.md).  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Any classification statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | 0.0 &lt;= Value &lt;= 100.0 |

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies an invalid result. |
| `0.0 <= Value <= 100.0` | Specifies the percentage. |

---

### `M_CONFUSION_MATRIX_ROW`

Retrieves the same value as the confusion matrix result type, except for a single row of the confusion matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the confusion matrix values. |

---

### `M_CONFUSION_MATRIX_USED_ENTRIES`

Retrieves the dataset entry keys composing the confusion matrix.

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_CONFUSION_MATRIX_USED_ENTRIES_COLUMN`

Retrieves the corresponding column of the confusion matrix of entries, representing the key of the entries in the dataset which constitute the corresponding column in the multi-class matrix.

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_CONFUSION_MATRIX_USED_ENTRIES_ELEMENT`

Retrieves the corresponding element of the confusion matrix of entries, representing the key of the entries in the dataset which constitute the corresponding element in the multi-class matrix. The [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) and [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) parameters specify row and column, respectively.

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_CONFUSION_MATRIX_USED_ENTRIES_ROW`

Retrieves the corresponding row of the confusion matrix of entries, representing the key of the entries in the dataset which constitute the corresponding row in the multi-class matrix.

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_ERROR_ENTRIES`

Retrieves the dataset entry keys that were not processed because of an error. To retrieve the reported cause of the error, use [`M_ERROR_ENTRIES_STATUS`](../../Reference/class/MclassGetResultStat.md).

---

### `M_ERROR_ENTRIES_STATUS`

Retrieves the error status of each entry in error. To retrieve the key of the entries in error, use [`M_ERROR_ENTRIES`](../../Reference/class/MclassGetResultStat.md).

| Value | Description |
| --- | --- |
| `M_FAILED_TO_RESTORE_GROUND_TRUTH_MASK` | Specifies that the ground truth mask could not be restored. |
| `M_FAILED_TO_RESTORE_PREDICTION_MASK` | Specifies that the prediction mask could not be restored. |
| `M_FAILED_TO_RESTORE_PREDICTION_SCORE_FILE` | Specifies that the prediction score file could not be restored. |
| `M_FCNET_SEGMENTATION_NOT_SUPPORTED` | Specifies that segmentation with FCNET (legacy classifier) is not supported. |
| `M_GROUND_TRUTH_MASK_DOES_NOT_EXIST` | Specifies that the ground truth mask does not exist. |
| `M_GROUND_TRUTH_MASK_INVALID_DIMENSIONS` | Specifies that the dimensions of the ground truth mask are invalid. |
| `M_IMAGE_FILE_INVALID` | Specifies that the image file exists but cannot be restored. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred during the operation. |
| `M_MULTIPLE_GROUND_TRUTHS_NOT_SUPPORTED` | Specifies that multiple ground truths are not supported. |
| `M_NO_GROUND_TRUTH` | Specifies that there is no ground truth. |
| `M_NO_PREDICT_INFO` | Specifies that there is no prediction information. |
| `M_NUMBER_OF_CLASSES_MISMATCH` | Specifies that there is a mismatch in the number of classes. |
| `M_PREDICTION_MASK_DOES_NOT_EXIST` | Specifies that the prediction mask does not exist. |
| `M_PREDICTION_MASK_INVALID_DIMENSIONS` | Specifies that the dimensions of the prediction mask are invalid. |
| `M_SCORE_FILE_DOES_NOT_EXIST` | Specifies that the score files does not exist. |
| `M_SCORE_FILE_INVALID_DIMENSIONS` | Specifies that the dimensions of the score file are invalid. |
| `M_UNLABELED_GROUND_TRUTH_PIXELS` | Specifies that ground truth pixels are unlabeled. |

---

### `M_ERROR_RATE`

Retrieves the error rate of a single class versus other classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the error rate. |

---

### `M_ERROR_RATE_MACRO`

Retrieves the macro error rate of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro error rate. |

---

### `M_ERROR_RATE_OVERALL`

Retrieves the overall error rate of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the overall error rate. |

---

### `M_ERROR_RATE_WEIGHTED`

Retrieves the weighted error rate of the entire confusion matrix, computed by taking the error rate for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted error rate. |

---

### `M_F1SCORE`

Retrieves the F1-score of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the F1-score result (occurs when TP, FP and FN are 0 with respect to the given class index). |
| `0.0 <= Value <= 100.0` | Specifies the F1-score. |

---

### `M_F1SCORE_MACRO`

Retrieves the macro F1-score of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro F1-score. |

---

### `M_F1SCORE_MICRO`

Retrieves the micro F1-score of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro F1-score. |

---

### `M_F1SCORE_WEIGHTED`

Retrieves the weighted F1-score of the entire confusion matrix, computed by taking the F1-score for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted F1-score. |

---

### `M_FALSE_NEGATIVES`

Retrieves the number of false negatives out of a confusion matrix for a 1 versus other classes, using the [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter as the criterion on the selected class to populate the matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the false negatives. |

---

### `M_FALSE_POSITIVES`

Retrieves the number of false positives out of a confusion matrix for a 1 versus other classes, using the [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter as the criterion on the selected class to populate the matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the false positives. |

---

### `M_FDR`

Retrieves the False Discovery Rate (FDR) of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the FDR result (occurs when none of the predictions are of the given class; that is, there is no TP/FP). |
| `0.0 <= Value <= 100.0` | Specifies the FDR. |

---

### `M_FDR_MACRO`

Retrieves the macro false discovery rate of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro false discovery rate. |

---

### `M_FDR_MICRO`

Retrieves the micro false discovery rate of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro false discovery rate. |

---

### `M_FDR_WEIGHTED`

Retrieves the weighted false discovery rate of the entire confusion matrix, computed by taking the false discovery rate for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted false discovery rate. |

---

### `M_FNR`

Retrieves the False Negative Rate (FNR) of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the FNR result (occurs when none of the ground truth instances are of the given class; that is, there is no TP/FN). |
| `0.0 <= Value <= 100.0` | Specifies the FNR. |

---

### `M_FNR_MACRO`

Retrieves the macro false negative rate of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro false negative rate. |

---

### `M_FNR_MICRO`

Retrieves the micro false negative rate of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro false negative rate. |

---

### `M_FNR_WEIGHTED`

Retrieves the weighted false negative rate of the entire confusion matrix, computed by taking the false negative rate for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted false negative rate. |

---

### `M_FPR`

Retrieves the False Positive Rate (FPR) of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the FPR result (occurs when all ground truth instances are of the given class; that is, there is no FP/TN). |
| `0.0 <= Value <= 100.0` | Specifies the FPR. |

---

### `M_FPR_MACRO`

Retrieves the macro false positive rate of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro false positive rate. |

---

### `M_FPR_MICRO`

Retrieves the micro false positive rate of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro false positive rate. |

---

### `M_FPR_WEIGHTED`

Retrieves the weighted false positive rate of the entire confusion matrix, computed by taking the false positive rate for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted false positive rate. |

---

### `M_NUMBER_OF_CLASSES`

Retrieves the number of class definitions processed from the dataset.  > **Note:** This result is not supported for anomaly detection ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the number of classes. |

---

### `M_PRECISION`

Retrieves the precision of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the precision result (occurs when none of the predictions are of the given class; that is, there is no TP/FP). |
| `0.0 <= Value <= 100.0` | Specifies the precision. |

---

### `M_PRECISION_MACRO`

Retrieves the macro precision of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro precision. |

---

### `M_PRECISION_MICRO`

Retrieves the micro precision of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro precision. |

---

### `M_PRECISION_WEIGHTED`

Retrieves the weighted precision of the entire confusion matrix, computed by taking the precision for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted precision. |

---

### `M_RECALL`

Retrieves the recall value of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the recall value (occurs when none of the ground truth instances are of the given class; that is, there is no TP/FN). |
| `0.0 <= Value <= 100.0` | Specifies the recall. |

---

### `M_RECALL_MACRO`

Retrieves the macro recall of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro recall. |

---

### `M_RECALL_MICRO`

Retrieves the micro recall of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro recall. |

---

### `M_RECALL_WEIGHTED`

Retrieves the weighted recall of the entire confusion matrix, computed by taking the recall for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted recall. |

---

### `M_SCORE_HISTOGRAM`

Retrieves the number of occurrences of the given class associated with each score threshold. Number of elements is the same as for [`M_SCORE_THRESHOLDS`](../../Reference/class/MclassGetResultStat.md). The ith bin is filled with score occurrences in the range [threshold_i, threshold_i+1).That is, greater or equal to threshold_i but lower than threshold_i+1.  For object detection, [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) is supported and returns the number of occurrences of all classes combined.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_CLASS_INDEX()** or [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of occurrences of the given class associated with each score threshold. |

---

### `M_SCORE_HISTOGRAM_OTHER_CLASSES`

Retrieves the number of occurrences of all classes other than the given class associated with each score threshold. Number of elements is the same as for [`M_SCORE_THRESHOLDS`](../../Reference/class/MclassGetResultStat.md). The ith bin is filled with score occurrences in the range [threshold_i, threshold_i+1).That is, greater or equal to threshold_i but lower than threshold_i+1. This does not apply to object detection.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of occurrences of all classes other than the given class associated with each score threshold. |

---

### `M_SCORE_HISTOGRAM_USED_ENTRIES`

Retrieves the array of entries contributed to occurrences of the given class associated with the specified score threshold. Number of elements is the number of occurrences. It is filled with entry keys of the occurrences in the range [threshold_value, threshold_value+1). That is, greater or equal to threshold_value but lower than threshold_value+1.  For object detection, [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) is supported and returns the number of occurrences of all classes combined.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` | | Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_CLASS_INDEX()** or [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` | | Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` |

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_SCORE_HISTOGRAM_USED_ENTRIES_OTHER_CLASSES`

Retrieves the array of entry keys of the occurrences of all classes other than the given class associated with the specified score threshold. Number of elements is the number of corresponding entries. It is filled with entry keys of the occurrences in the range [threshold_value, threshold_value +1). That is, greater or equal to threshold_value but lower than threshold_value+1. This does not apply to object detection.

| Value | Description |
| --- | --- |
| `Value` | Specifies the UUID. |

---

### `M_SCORE_THRESHOLDS`

Retrieves the score thresholds used in the thresholding analysis. The same set of thresholds is applied to every class. Each score returned is a value between 0.0 and 100.0, inclusively.  For tasks predicting probability distribution over all classes (feature classification, image classification, and segmentation), thresholding analysis is done using a "one class versus the rest" strategy. Thus, a threshold selected for a given class must be applied to scores associated to that class. Occurrences with scores greater or equal to the threshold are considered to be part of the class.  One the other hand, the score predicted for object detection is a confidence score not associated with any specific class. In this case, only the detections with score greater or equal to the threshold are considered relevant.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the score thresholds. |

---

### `M_STATUS`

Retrieves the status of the statistics calculation.

| Value | Description |
| --- | --- |
| `M_CALCULATE_NOT_PERFORMED` | Specifies that the calculate operation was not performed. |
| `M_COMPLETE` | Specifies that the statistics calculation completed successfully. |
| `M_CURRENTLY_CALCULATING` | Specifies that the calculate operation is currently ongoing. You can only get this status if you are retrieving it from another thread. |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred during the operation. |
| `M_STOPPED_BY_REQUEST` | Specifies that the current execution of the operation was explicitly stopped by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_STOP_CALCULATE`](../../Reference/class/MclassControl.md). |
| `M_TIMEOUT_REACHED` | Specifies that the operation ended because the timeout limit was reached. The timeout is specified by calling [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TIMEOUT`](../../Reference/class/MclassControl.md). |
| `M_ZERO_USED_ENTRIES` | Specifies that no entries were used because they are all in error. |

---

### `M_TNR`

Retrieves the true negative rate of a single class versus other classes.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that you cannot retrieve the true negative rate result (occurs when all ground truth instances are of the given class; that is, there is no FP/TN). |
| `0.0 <= Value <= 100.0` | Specifies the true negative rate. |

---

### `M_TNR_MACRO`

Retrieves the macro true negative rate of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro true negative rate. |

---

### `M_TNR_MICRO`

Retrieves the micro true negative rate of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies micro macro true negative rate. |

---

### `M_TNR_WEIGHTED`

Retrieves the weighted true negative rate of the entire confusion matrix, computed by taking the true negative rate for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted true negative rate. |

---

### `M_TRUE_NEGATIVES`

Retrieves the number of true negatives out of a confusion matrix for a 1 versus other classes, using the [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter as the criterion on the selected class to populate the matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the true negatives. |

---

### `M_TRUE_POSITIVES`

Retrieves the number of true positives out of a confusion matrix for a 1 versus other classes, using the [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameter as the criterion on the selected class to populate the matrix.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the true positives. |

### For anomaly detection (pixel-level) or segmentation statistics result buffers

To retrieve results for anomaly detection (pixel-level) or segmentation statistics result buffers, set the [`ResultType`](../../Reference/class/MclassGetResultStat.md) parameter to one of the following values.

---

### `M_IOU`

Retrieves the Intersection-over-Union (IoU) metric of a single class vs other classes.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Pixel-level anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0`, [`M_ALL`](../../Reference/class/MclassGetResultStat.md), or [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) | | Segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0`, [`M_ALL`](../../Reference/class/MclassGetResultStat.md), or [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the IoU. |

---

### `M_IOU_MACRO`

Retrieves the macro Intersection-over-Union (IoU) of the entire confusion matrix, giving equal importance to all classes.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the macro IoU. |

---

### `M_IOU_MICRO`

Retrieves the micro Intersection-over-Union (IoU) of the entire confusion matrix, giving equal importance to all samples.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the micro IoU. |

---

### `M_IOU_WEIGHTED`

Retrieves the weighted Intersection-over-Union (IoU) of the entire confusion matrix, computed by taking the IoU for each class and then averaging them, weighted by the number of true instances for each class.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the weighted IoU. |

### For object detection (single-entry) statistics result buffers

To retrieve results for object detection (single-entry) statistics result buffers, set the [`ResultType`](../../Reference/class/MclassGetResultStat.md) parameter to one of the following values. To calculate single-entry statistic results for object detection, you must specify a specific dataset entry index or key ([`EntryIndex`](../../Reference/class/MclassStatCalculate.md) or [`EntryKey`](../../Reference/class/MclassStatCalculate.md)) when calling [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md).

---

### `M_MATCHED_INSTANCE`

Retrieves the index of the detection instance matched to the ground truth region. This is only possible for bounding box regions ([`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) with [`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassEntryAddRegion.md)).  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Single-entry object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_REGION_INDEX()** or [`M_ALL_REGIONS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the region is not matched to any instance. |
| `M_REGION_IGNORED` | Specifies that the region's [`M_REGION_USE`](../../Reference/class/MclassControlEntry.md) control is set to [`M_IGNORE`](../../Reference/class/MclassControlEntry.md). |
| `M_UNSUPPORTED_DESCRIPTOR_TYPE` | Specifies that the region is not a bounding box. |
| `Value >= 0` | Specifies the index. |

---

### `M_MATCHED_REGION`

Retrieves the index of the ground truth region matched to the detection instance.  To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.  |   |   |   |   | | --- | --- | --- | --- | | [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) | | Single-entry object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_INSTANCE_INDEX()** or [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the region is not matched. |
| `Value >= 1` | Specifies the index. |

---

### `M_NUMBER_OF_INSTANCES`

Retrieves the number of detection instances in the entry.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of detection instances. |

---

### `M_NUMBER_OF_REGIONS`

Retrieves the number of regions in the entry.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies number of regions. |

### Combination Constants — For determining whether results are available

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is available.

#### `M_AVAILABLE`

Retrieves whether the requested result type is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested result type is not available. |
| `M_TRUE` | Specifies that the requested result type is available. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested results to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested results to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Any classification statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Anomaly detection ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)), feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Any classification statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |
| Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` or [`M_ALL`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Single-entry object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Pixel-level anomaly detection ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |
| Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_CLASS_INDEX()** or [`M_NO_CLASS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` |
| Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | **M_CLASS_INDEX()** | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |
| Object detection statistics result ID ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) | **M_CLASS_INDEX()** or [`M_NO_CLASS`](../../Reference/class/MclassGetResultStat.md) | **M_CLASS_INDEX()** or [`M_NO_CLASS`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0` |
| Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), or segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |
| Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

To retrieve this result, set the [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) parameters to the following.

|   |   |   |   |
| --- | --- | --- | --- |
| [`StatResultClassId`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md) | [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) | [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md) |
| Feature classification ([`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), image classification ([`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), segmentation ([`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)), or object detection ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)) statistics result ID | **M_CLASS_INDEX()** | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0`, [`M_ALL`](../../Reference/class/MclassGetResultStat.md), or [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |
| Anomaly detection statistics result ID ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) | [`M_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) or [`M_NON_ANOMALOUS`](../../Reference/class/MclassGetResultStat.md) | [`M_GENERAL`](../../Reference/class/MclassGetResultStat.md) | `0.0 &lt;= Value &lt;= 100.0`, [`M_ALL`](../../Reference/class/MclassGetResultStat.md), or [`M_DEFAULT`](../../Reference/class/MclassGetResultStat.md) |

> **Note:** For more information about the confusion matrix information that is returned, and about the related parameters ([`StatResultClassId`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex1`](../../Reference/class/MclassGetResultStat.md), [`LabelOrIndex2`](../../Reference/class/MclassGetResultStat.md) and [`ScoreThreshold`](../../Reference/class/MclassGetResultStat.md)), see the description of [`M_CONFUSION_MATRIX`](../../Reference/class/MclassGetResultStat.md).

> **Note:** This result is not supported for object detection ([`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)).
