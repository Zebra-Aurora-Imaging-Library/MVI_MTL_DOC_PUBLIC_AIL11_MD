---
doctype: Reference
module: class
function: MclassGetResult
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassGetResult"
---

# MclassGetResult

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

> Get results from a training or prediction result buffer.

## Syntax

```c
void MclassGetResult(
    AIL_ID    ResultClassId,  //in
    AIL_INT64 LabelOrIndex,   //in
    AIL_INT64 ResultType,     //in
    void *    ResultArrayPtr  //out
)
```

## Description

This function retrieves results of the specified type from a training or prediction result buffer. Result buffers are allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md). You can either retrieve training results, which are available after calling [`MclassTrain`](../../Reference/class/MclassTrain.md), or you can retrieve prediction results, which are available after calling [`MclassPredict`](../../Reference/class/MclassPredict.md).

## Parameters

### `ResultClassId` *(in, AIL_ID)*

Specifies the identifier of the classification result buffer from which to retrieve results. You can get results from a CNN ([`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), object detection ([`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), segmentation ([`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)), anomaly detection ([`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) or tree ensemble ([`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)) training result buffer, or a CNN ([`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md)), object detection ([`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md)), segmentation ([`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md)), anomaly detection ([`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md)) tree ensemble ([`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)), or ONNX ([`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md)) prediction result buffer.

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies what to retrieve. The values in this table are available for all tasks unless otherwise specified. Set this parameter to one of the following values.

*For specifying what to retrieve*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLASS_INDEX` | Specifies the index of the class for which to get results. You can get the number of included classes in the result buffer using [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResult.md). |
| `M_INSTANCE_INDEX` | Specifies the index of the instance for which to get results. You can get the number of included instances in the result buffer using [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md).

This is only available for object detection result buffers. |
| `M_OUTPUT_INDEX` | Specifies the index of the output of an ONNX model for which to get results.

This is only available for ONNX result buffers. |
| `M_ALL_INSTANCES` | Specifies to retrieve results for all instances.

This is only available for object detection result buffers. |
| `M_GENERAL` *(default)* | Specifies to retrieve global results from the result buffer. |

### `ResultType` *(in, AIL_INT64)*

Specifies the type of result to retrieve.

### `ResultArrayPtr` *(out, *void)*

Specifies the address of the array in which to write results.

## Parameter Associations

### For training (CNN or segmentation) - retrieving general results

To retrieve general results from a CNN or segmentation training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_EPOCH_ACCURACY`

Retrieves an array containing the development dataset accuracy for each epoch.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to the [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the development dataset accuracy for each epoch, as a percentage. |

---

### `M_DEV_DATASET_EPOCH_ERROR_RATE`

Retrieves an array containing the development dataset error rate for each epoch.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to the [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the development dataset error rate for each epoch, as a percentage. |

---

### `M_TRAIN_DATASET_EPOCH_ACCURACY`

Retrieves an array containing the training dataset accuracy for each epoch.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to the [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the training dataset accuracy for each epoch, as a percentage. |

---

### `M_TRAIN_DATASET_EPOCH_ERROR_RATE`

Retrieves an array containing the training dataset error rate for each epoch.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to the [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the training dataset error rate for each epoch, as a percentage. |

### For training (CNN, object detection, segmentation, or anomaly detection) - retrieving general results

To retrieve general results from a CNN, object detection, segmentation, or anomaly detection training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_ERROR_ENTRIES`

Retrieves an array containing the AIL_UUID of each image in the development dataset that produced an error, at least once, during training.  For segmentation, UUIDs are also listed for entries that have invalid labels. A label can be invalid if Aurora Imaging Library failed to restore a mask, if the mask size does not match the size of the entry image, if not all the pixels in the image are labeled, or if there is an overlap of pixels between different classes.

---

### `M_DEV_DATASET_ERROR_ENTRIES_STATUS`

Retrieves the error status of each entry in the development dataset that produced an error. Use [`M_DEV_DATASET_ERROR_ENTRIES`](../../Reference/class/MclassGetResult.md) to get the UUID of the entries that produced an error.

| Value | Description |
| --- | --- |
| `M_FAILED_TO_RESTORE_GROUND_TRUTH_MASK` | Specifies that the ground truth mask entry failed to be restored from the file. This only applies to segmentation tasks. |
| `M_GROUND_TRUTH_MASK_DOES_NOT_EXIST` | Specifies that the ground truth mask entry does not exist in the dataset. This only applies to segmentation tasks. |
| `M_GROUND_TRUTH_MASK_INVALID_DIMENSIONS` | Specifies that the ground truth entry has invalid dimensions; the height/width of the entry does not match the size of the classifier source layer or the size of the specified target image. This only applies to segmentation tasks. |
| `M_IMAGE_FILE_INVALID` | Specifies that the image file associated with the source entry is not a valid image. |
| `M_IMAGE_FILE_NOT_FOUND` | Specifies that the image file associated with the source entry was not found. |
| `M_INVALID_BUFFER_SIZE` | Specifies that the entry has invalid dimensions; the height/width of the entry does not match the size of the classifier source layer or the size of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_SIZE_BAND` | Specifies that the entry has an invalid number of bands; the number of bands of the entry does not match the number of bands of the classifier source layer or the of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_TYPE` | Specifies that the entry has an invalid type; the entry type does not match the classifier source layer type or the of the specified target image type. |
| `M_OVERLAPPING_GROUND_TRUTH_PIXELS` | Specifies that the ground truth entry contains pixels with overlapping classes. This only applies to segmentation tasks. |
| `M_UNLABELED_GROUND_TRUTH_PIXELS` | Specifies that the ground truth entry contains unlabeled pixels. This only applies to segmentation tasks. |

---

### `M_TRAINABLE_DATASETS`

Retrieves the status of the datasets in train results to determine whether you can perform a complete training using this CNN, object detection, segmentation, or anomaly detection training result buffer with an [`M_NULL`](../../Reference/class/MclassTrain.md) identifier for both datasets of [`MclassTrain`](../../Reference/class/MclassTrain.md).

| Value | Description |
| --- | --- |
| `M_DATASETS_STATUS_INVALID` | Specifies that training cannot be done with these datasets. |
| `M_DATASETS_STATUS_MISSING_GT` | Specifies that training can be done if missing GT (ground truth) is supported. To support missing GT, enable [`M_SUPPORT_MISSING_GROUND_TRUTH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)). |
| `M_DATASETS_STATUS_VALID` | Specifies that training can be done with these datasets. |

---

### `M_TRAIN_DATASET_ERROR_ENTRIES`

Retrieves an array containing the AIL_UUID of each image in the training dataset that produced an error, at least once, during training.  For segmentation, UUIDs are also listed for entries that have invalid labels. A label can be invalid if Aurora Imaging Library failed to restore a mask, if the mask size does not match the size of the entry image, if not all the pixels in the image are labeled, or if there is an overlap of pixels between different classes.

---

### `M_TRAIN_DATASET_ERROR_ENTRIES_STATUS`

Retrieves the error status of each entry in the training dataset that produced an error. Use [`M_TRAIN_DATASET_ERROR_ENTRIES`](../../Reference/class/MclassGetResult.md) to get the UUID of the entries that produced an error.

| Value | Description |
| --- | --- |
| `M_FAILED_TO_RESTORE_GROUND_TRUTH_MASK` | Specifies that the ground truth mask entry failed to be restored from the file. This only applies to segmentation tasks. |
| `M_GROUND_TRUTH_MASK_DOES_NOT_EXIST` | Specifies that the ground truth mask entry does not exist in the dataset. This only applies to segmentation tasks. |
| `M_GROUND_TRUTH_MASK_INVALID_DIMENSIONS` | Specifies that the ground truth entry has invalid dimensions; the height/width of the entry does not match the size of the classifier source layer or the size of the specified target image. This only applies to segmentation tasks. |
| `M_IMAGE_FILE_INVALID` | Specifies that the image file associated with the source entry is not a valid image. |
| `M_IMAGE_FILE_NOT_FOUND` | Specifies that the image file associated with the source entry was not found. |
| `M_INVALID_BUFFER_SIZE` | Specifies that the entry has invalid dimensions; the height/width of the entry does not match the size of the classifier source layer or the size of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_SIZE_BAND` | Specifies that the entry has an invalid number of bands; the number of bands of the entry does not match the number of bands of the classifier source layer or the of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_TYPE` | Specifies that the entry has an invalid type; the entry type does not match the classifier source layer type or the of the specified target image type. |
| `M_OVERLAPPING_GROUND_TRUTH_PIXELS` | Specifies that the ground truth entry contains pixels with overlapping classes. This only applies to segmentation tasks. |
| `M_UNLABELED_GROUND_TRUTH_PIXELS` | Specifies that the ground truth entry contains unlabeled pixels. This only applies to segmentation tasks. |

### For training (CNN, object detection, or segmentation) - retrieving general results

To retrieve general results from a CNN, object detection, or segmentation training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_LAST_EPOCH_UPDATED_PARAMETERS`

Retrieves the index of the epoch where classifier weights were updated the last time.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the parameters were not updated. |
| `Value >= 0` | Specifies the index of the epoch. |

---

### `M_MINI_BATCH_PER_EPOCH`

Retrieves the number of mini-batches per epoch.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of mini-batches per epoch. |

---

### `M_TRAIN_DATASET_MINI_BATCH_LOSS`

Retrieves an array containing the loss error values for each mini-batch during training. You can use the loss to evaluate the lack of confidence (doubt) associated with the classification during training.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) * [`M_MINI_BATCH_PER_EPOCH`](../../Reference/class/MclassGetResult.md) ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the training dataset loss for each mini-batch. |

### For training (segmentation) - retrieving general results

To retrieve general results from a segmentation training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_EPOCH_IOU_MEAN`

Retrieves an array containing the mean IOU (Intersection over Union) metric, as a percentage, of the development dataset using the trained context after each epoch.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the mean IOU (Intersection over Union) metric of the development dataset, as a percentage. |

---

### `M_TRAIN_DATASET_EPOCH_IOU_MEAN`

Retrieves an array containing the mean IOU (Intersection over Union) metric, as a percentage, of the training dataset using the trained context after each epoch.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the mean IOU (Intersection over Union) metric of the training dataset, as a percentage. |

### For training (segmentation or object detection) - retrieving general results

To retrieve general results from a segmentation or object detection training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_EPOCH_LOSS`

Retrieves an array containing the loss metric of the development dataset after the last epoch.  Note that the required array size, which you can retrieve with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md), is equivalent to the [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) of the training context used.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 1.0` | Specifies the loss metric of the development dataset after each epoch. |

### For training (tree ensemble) - retrieving general results

To retrieve general results from a tree ensemble training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).  Note, bagging information is typically unreliable if your training dataset has augmented entries.

---

### `M_DEV_DATASET_ACCURACY_AFTER_EACH_TREE`

Retrieves an array containing the accuracies on the development dataset after adding each tree to the ensemble.

---

### `M_DEV_DATASET_ERROR_RATE_AFTER_EACH_TREE`

Retrieves an array containing the error rates after on the development dataset after adding each tree to the ensemble.

---

### `M_FEATURE_IMPORTANCE`

Retrieves an array containing the importance of each feature when training the classifier.  This result can be disabled if [`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md) is set to [`M_DISABLE`](../../Reference/class/MclassControl.md).

---

### `M_NUMBER_OF_ENTRIES_OUT_OF_BAG`

Retrieves the number of out-of-bag dataset entries.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of out-of-bag dataset entries. |

---

### `M_NUMBER_OF_FEATURES`

Retrieves the number of features.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of features. |

---

### `M_NUMBER_OF_TREES_TRAINED`

Retrieves the number of trained trees. This result is equivalent to the number of trees in the classifier.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of trained trees. |

---

### `M_OUT_OF_BAG_ACCURACY`

Retrieves the out-of-bag error accuracy.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the out-of-bag accuracy, as a percentage. |

---

### `M_OUT_OF_BAG_ACCURACY_AFTER_EACH_TREE`

Retrieves the array containing the accuracies on the out-of-bag set after adding each tree to the ensemble.

---

### `M_OUT_OF_BAG_CONFUSION_MATRIX`

Retrieves the confusion matrix obtained using the out-of-bag set.  This is a square matrix. Aurora Imaging Library returns the matrix values, row-by-row, as an array. The array size is equivalent to the square of the total amount of classes, which is the same [`M_OUT_OF_BAG_CONFUSION_MATRIX_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_OUT_OF_BAG_CONFUSION_MATRIX_SIZE_Y`](../../Reference/class/MclassGetResult.md), or [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md).

---

### `M_OUT_OF_BAG_CONFUSION_MATRIX_SIZE_X`

Retrieves the X-dimension of the confusion matrix. This value is calculated using the out-of-bag set.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the X-dimension of the confusion matrix. |

---

### `M_OUT_OF_BAG_CONFUSION_MATRIX_SIZE_Y`

Retrieves the Y-dimension of the confusion matrix. This value is calculated using the out-of-bag set.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the Y-dimension of the confusion matrix. |

---

### `M_OUT_OF_BAG_ERROR_RATE`

Retrieves the out-of-bag error rate.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the out-of-bag error rate. |

---

### `M_OUT_OF_BAG_ERROR_RATE_AFTER_EACH_TREE`

Retrieves the array containing the accuracies on the out-of-bag set after adding each tree to the ensemble.

---

### `M_PROXIMITY_MATRIX`

Retrieves the proximity matrix.  This is a square matrix. Aurora Imaging Library returns the matrix values, row-by-row, as an array. The array size is equivalent [`M_PROXIMITY_MATRIX_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_PROXIMITY_MATRIX_SIZE_Y`](../../Reference/class/MclassGetResult.md) or [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md).  In the proximity matrix, the entry in cell (_j_, _k_) is a measure of similarity (or distance) between the entries to which row _j_ and column _k_ correspond.  The proximity matrix result can be enabled or disabled using [`M_COMPUTE_PROXIMITY_MATRIX`](../../Reference/class/MclassControl.md) in the train context.

---

### `M_PROXIMITY_MATRIX_SIZE_X`

Retrieves the X-dimension of the proximity matrix.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the X-dimension of the proximity matrix. |

---

### `M_PROXIMITY_MATRIX_SIZE_Y`

Retrieves the Y-dimension of the proximity matrix.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the Y-dimension of the proximity matrix. |

---

### `M_SEED_VALUE`

Retrieves the seed used for the training dataset.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the seed used for the training dataset. |

---

### `M_TRAIN_DATASET_ACCURACY_AFTER_EACH_TREE`

Retrieves the array containing the accuracies on the training dataset after adding each tree to the classifier.

---

### `M_TRAIN_DATASET_ERROR_RATE_AFTER_EACH_TREE`

Retrieves the array containing the error rates on the training dataset after adding each tree to the classifier.

---

### `M_TREE_DEPTHS_ACHIEVED`

Retrieves the array containing the depth achieved by each tree in the classifier.

---

### `M_TREE_NUMBER_OF_LEAF_NODES_ACHIEVED`

Retrieves the array containing the number of leaf nodes for each tree in the classifier.

### For training (CNN, segmentation, or tree ensemble) - retrieving general results

To retrieve general results from a CNN, segmentation or tree ensemble training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_ACCURACY`

Retrieves the accuracy on the development dataset using the trained context.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy on the development dataset, as a percentage. |

---

### `M_DEV_DATASET_CONFUSION_MATRIX`

Retrieves the confusion matrix obtained using the development dataset.  This is a square matrix. Aurora Imaging Library returns the matrix values, row-by-row, as an array. The array size is equivalent to the square of the total amount of classes, which is the same [`M_DEV_DATASET_CONFUSION_MATRIX_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_DEV_DATASET_CONFUSION_MATRIX_SIZE_Y`](../../Reference/class/MclassGetResult.md), or [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_CONFUSION_MATRIX_SIZE_X`

Retrieves the X-dimension of the confusion matrix. This value is calculated using the development dataset.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the X-dimension of the confusion matrix. |

---

### `M_DEV_DATASET_CONFUSION_MATRIX_SIZE_Y`

Retrieves the Y-dimension of the confusion matrix. This value is calculated using the development dataset.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the Y-dimension of the confusion matrix. |

---

### `M_DEV_DATASET_ERROR_RATE`

Retrieves the error rate on the development dataset. This can be expressed as 100.0 - [`M_DEV_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy error rate on the development dataset, as a percentage. |

---

### `M_TRAIN_DATASET_ACCURACY`

Retrieves the accuracy on the training dataset using the trained context.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy on the training dataset, as a percentage. |

---

### `M_TRAIN_DATASET_CONFUSION_MATRIX`

Retrieves the confusion matrix obtained using the training dataset.  This is a square matrix. Aurora Imaging Library returns the matrix values, row-by-row, as an array. The array size is equivalent to the square of the total amount of classes, which is the same [`M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_Y`](../../Reference/class/MclassGetResult.md), or [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md).

---

### `M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_X`

Retrieves the X-dimension of the confusion matrix. This value is calculated using the training dataset.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the X-dimension of the confusion matrix. |

---

### `M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_Y`

Retrieves the Y-dimension of the confusion matrix. This value is calculated using the training dataset.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the Y-dimension of the confusion matrix. |

---

### `M_TRAIN_DATASET_ERROR_RATE`

Retrieves the error rate on the training dataset. This can be expressed as 100.0 - [`M_TRAIN_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md).

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy error rate on the training dataset, as a percentage. |

### For training (CNN, object detection, segmentation, anomaly detection or tree ensemble) - retrieving general results

To retrieve general results from a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_DEV_DATASET_USED_ENTRIES`

Retrieves an array containing the AIL_UUID of each image in the development dataset used during the training phase.

---

### `M_TRAIN_DATASET_USED_ENTRIES`

Retrieves an array containing the AIL_UUID of each image in the training dataset used during the training phase.

### For prediction (CNN, object detection, segmentation, or ONNX) - retrieving general results

To retrieve general results from any prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

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
| `M_CUSTOM` | Specifies a custom classifier context.  This is a predefined classifier for image classification, segmentation, or object detection. |
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
| `M_ODNET` | Specifies an ODNet classifier context.  This is a predefined classifier for object detection. |
| `M_USER_ONNX` | Specifies an ONNX classifier context. |

### For prediction (CNN, segmentation, or anomaly detection) - retrieving general results

To retrieve general results from a CNN, segmentation, or anomaly detection prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_PREDICTION_SIZE_X`

Retrieves the number of predictions the deep network has made according to the target image size.  For image classification, the map size will typically be 1x1 for a target image the same size as the network's source layer, meaning a single prediction for the whole target image. For a larger target image, the map size can be larger, meaning the classifier has convolved the larger target image and made many predictions doing so.  For segmentation, the network outputs a prediction for each pixel in the target image, so the map size will be the same size as the target image.  For anomaly detection, this shows the output size of the pixel level anomaly detection results.

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the number of entries in the dataset. |

---

### `M_PREDICTION_SIZE_Y`

Retrieves the number of predictions the deep network has made according to the target image size.  For image classification, the map size will typically be 1x1 for a target image the same size as the network's source layer, meaning a single prediction for the whole target image. For a larger target image, the map size can be larger, meaning the classifier has convolved the larger target image and made many predictions doing so.  For segmentation, the network outputs a prediction for each pixel in the target image, so the map size will be the same size as the target image.  For anomaly detection, this shows the output size of the pixel level anomaly detection results.

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the number of entries in the dataset. |

### For prediction (CNN or segmentation) - retrieving general results

To retrieve general results from a CNN or segmentation prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_BEST_INDEX_IMAGE_TYPE`

Retrieves the image type to provide when calling [`MclassDraw`](../../Reference/class/MclassDraw.md) with [`M_DRAW_BEST_INDEX_IMAGE`](../../Reference/class/MclassDraw.md) or [`M_DRAW_BEST_INDEX_CONTOUR_IMAGE`](../../Reference/class/MclassDraw.md).

| Value | Description |
| --- | --- |
| `M_UNSIGNED + 8` | Specifies that the image type is 8-bit unsigned. |
| `M_UNSIGNED + 16` | Specifies that the image type is 16-bit unsigned. |

---

### `M_RECEPTIVE_FIELD_OFFSET_X`

Retrieves the offset along the X-axis, from the top-left corner of the target image, needed to place the first class score at the center of its receptive field.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the classification map offset along the X-axis. |

---

### `M_RECEPTIVE_FIELD_OFFSET_Y`

Retrieves the offset along the Y-axis, from the top-left corner of the target image, needed to place the first class score at the center of its receptive field.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the classification map offset along the Y-axis. |

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
| `Value >= 0.0` | Specifies the classification map scale along the X-axis. |

---

### `M_RECEPTIVE_FIELD_STRIDE_Y`

Retrieves the stride spacing the receptive field centers in the target image along the Y-axis.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the classification map scale along the Y-axis. |

### For prediction (CNN, segmentation, or tree ensemble) - retrieving general results

To retrieve general results from a CNN, segmentation, or tree ensemble prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

---

### `M_NUMBER_OF_CLASSES`

Retrieves the number of classes.  For CNN or segmentation, this refers to the number of outputs in the network's last layer. For tree ensemble, this refers to the number of tree ensemble in the classifier.  This can also be used in a tree ensemble prediction with [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the number of entries in the dataset. |

### For prediction (CNN, object detection, segmentation, or tree ensemble) - retrieving general or specific results

To retrieve general results from a CNN, object detection, segmentation, or tree ensemble prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md).  To retrieve a class or instance specific result from an object detection result buffer, the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter can be set to [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResult.md), **M_CLASS_INDEX()**, or **M_INSTANCE_INDEX()**.

---

### `M_BEST_CLASS_INDEX`

Retrieves the index of the class with the highest score.  For image classification, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResult.md) retrieves one index.  For segmentation, the array (map) of values retrieved can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) multiplied by [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md). Each value is the index of the class having the best score for that X- Y-pixel location. The index of the class begins at 0.  For object detection, to retrieve the class index with the highest score for all instances, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResult.md). The array retrieved will have size equal to the number of instances ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to [`M_GENERAL`](../../Reference/class/MclassGetResult.md)) and will contain the class index with the highest score for each instance.  For object detection, to retrieve the class index with the highest score for all instances of a given class, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to **M_CLASS_INDEX()**. The array retrieved will have size equal to the number of instances ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to **M_CLASS_INDEX()**) and will contain the class index with the highest score for each instance of a given class.  For object detection, to retrieve the class index with the highest score for a specific instance, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to **M_INSTANCE_INDEX()**.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the index of the class with the highest score. |

---

### `M_BEST_CLASS_SCORE`

Retrieves the highest class score.  For image classification, [`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResult.md) retrieves one score.  For segmentation, the array (map) of values retrieved can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) multiplied by [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md). Each value is the best class score for that X- Y-pixel location.  For object detection, to retrieve the highest score for all instances, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResult.md). The array retrieved will have size equal to the number of instances ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to [`M_GENERAL`](../../Reference/class/MclassGetResult.md)) and will contain the highest score for each instance.  For object detection, to retrieve the highest score for all instances of a given class, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to **M_CLASS_INDEX()**. The array retrieved will have size equal to the number of instances ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md) and [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to **M_CLASS_INDEX()**) and will contain the highest score for each instance of a given class.  For object detection, to retrieve the highest score for a specific instance, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to **M_INSTANCE_INDEX()**.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the best class score. |

### For prediction (CNN, segmentation, or tree ensemble) - retrieving general or class-specific results

To retrieve general results from a CNN, segmentation, or tree ensemble prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md); to retrieve class-specific results from a CNN or segmentation prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter the index of a specific class (using **M_CLASS_INDEX()**). In these cases, the [`ResultType`](../../Reference/class/MclassGetResult.md) can be set to one of the following values. Note, you cannot receive class-specific results from a tree ensemble result buffer.

---

### `M_NUMBER_OF_PREDICTIONS`

Retrieves the total number of class scores.  For general results obtained from a CNN or segmentation predict result buffer, the total corresponds to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md) * [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResult.md) scores. The first batch of [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md) scores are for the first class and the subsequent batches are for the remaining classes. In other words, the data is organized planar-wise.  For general results obtained from a tree ensemble predict result buffer, the total corresponds to [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResult.md).  For class-specific results ([`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to **M_CLASS_INDEX()**) obtained from a CNN or segmentation predict result buffer, the total corresponds to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the total number of class scores. |

---

### `M_PREDICTION_SCORES`

Retrieves an array of all the class scores.  The number of returned values is [`M_NUMBER_OF_PREDICTIONS`](../../Reference/class/MclassGetResult.md).  For general results obtained from a CNN or segmentation prediction result buffer, the returned values are organized planar-wise in a 3d volume of size [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) *[`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md) * [`M_NUMBER_OF_CLASSES`](../../Reference/class/MclassGetResult.md). Note, each band contains the score of one given class. The index in this returned array of the score of class C at pixel X, Y is: index = (C * MapSizeX * MapSizeY) + (Y * MapSizeX) + X.  For general results obtained from a tree ensemble prediction result buffer, the number of class scores is equivalent to [`M_NUMBER_OF_PREDICTIONS`](../../Reference/class/MclassGetResult.md).  For class-specific results ([`LabelOrIndex`](../../Reference/class/MclassGetResult.md) set to **M_CLASS_INDEX()**) obtained from a CNN or segmentation prediction result buffer, the returned values are organized in a vector that corresponds to a 2D volume of size [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) *[`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md). The index of the score at location X, Y is: `index = (Y * [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md)`) + X.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies all of the class scores. |

### For prediction (object detection) - retrieving general results

To retrieve general results from an object detection prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to[`M_GENERAL`](../../Reference/class/MclassGetResult.md). In this case, the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values.

---

### `M_SCORE_THRESHOLD`

Retrieves the score threshold from the classifier context.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Retrieves the score threshold. |

### For prediction (object detection) - retrieving general or class-specific results

To retrieve general or class-specific results from an object detection prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to[`M_GENERAL`](../../Reference/class/MclassGetResult.md) or **M_CLASS_INDEX()**, respectively. In these cases, the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values.

---

### `M_NUMBER_OF_INSTANCES`

Retrieves the number of instances.

| Value | Description |
| --- | --- |
| `Value >= 0` | Retrieves the number of instances. |

### For prediction (object detection) - retrieving results for a specific class, a specific instance, or all instances

To retrieve results for a specific class, a specific instance, or all instances from an object detection prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to **M_CLASS_INDEX()**, **M_INSTANCE_INDEX()**, or [`M_ALL_INSTANCES`](../../Reference/class/MclassGetResult.md). In these cases, you can set the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter to one of the following values.

---

### `M_BOX_4_CORNERS`

Retrieves an array containing the coordinates of the four corners of the specified bounding boxes.  The 8 coordinates of the first box (b1) are grouped, followed by the 8 coordinates of the second box (b2), repeating this process for all specified boxes in the order b1x1, b1y1, b1x2, b1y2, b1x3, b1y3, b1x4, b1y4, b2x1, b2y1, b2x2, b2y2, b2x3, b2y3, b2x4, b2y4, etc.  *[Image: MclassBox4Corners.png]*  The size of the array will be equal to the number of specified instances ([`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md)) multiplied by 8.

---

### `M_CENTER_X`

Retrieves the X-coordinates of the centers of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the X-coordinates are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

---

### `M_CENTER_Y`

Retrieves the Y-coordinates for the centers of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the Y-coordinates are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

---

### `M_HEIGHT`

Retrieves the heights of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the heights are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md). Only one height will be returned if you are retrieving results for a specific instance.

---

### `M_WIDTH`

Retrieves the widths of the specified bounding boxes.  When retrieving results for all instances, or a specific class, the widths are returned in an array. The size of the array can be retrieved with [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetResult.md). Only one coordinate will be returned if you are retrieving results for a specific instance.

### For prediction (anomaly detection) - retrieving general results

To retrieve general results from an anomaly detection prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to[`M_GENERAL`](../../Reference/class/MclassGetResult.md). In this case, the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values.

---

### `M_IMAGE_PREDICTED_ANOMALOUS`

Retrieves whether or not the image is considered anomalous.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the image is normal. |
| `M_TRUE` | Specifies that the image is anomalous. |

---

### `M_IMAGE_SCORE`

Retrieves the anomaly score of the whole image.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the anomaly score of the image. |

---

### `M_MASK_IMAGE`

Retrieves whether or not each pixel in the image is considered anomalous.  The array must have a size of [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md).

---

### `M_PIXEL_SCORES`

Retrieves the anomaly score of each pixel in the image.  The array must have a size of [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetResult.md) x [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetResult.md).

### For prediction (ONNX) - retrieving general results

To retrieve general results from an ONNX prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to[`M_GENERAL`](../../Reference/class/MclassGetResult.md). In this case, the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values.

---

### `M_NUMBER_OF_OUTPUTS`

Retrieves the number of outputs of the ONNX model.

| Value | Description |
| --- | --- |
| `Value >= 1` | Retrieves the number of outputs. |

### For prediction (ONNX) - retrieving results for a specific output

To retrieve general results from an ONNX prediction result buffer, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to**M_OUTPUT_INDEX()**. In this case, the [`ResultType`](../../Reference/class/MclassGetResult.md) parameter can be set to one of the following values.

---

### `M_DATA_TYPE`

Retrieves the tensor element type.

| Value | Description |
| --- | --- |
| `M_FLOAT+16` | Specifies that the ONNX type is float16. This is not a basic C++ data type thus its representation is platform dependent. The recommended Aurora Imaging Library data type is AIL_UINT16. |
| `M_FLOAT+32` | Specifies that the ONNX type is float. The recommended Aurora Imaging Library data type is AIL_FLOAT. |
| `M_FLOAT+64` | Specifies that the ONNX type is double. The recommended Aurora Imaging Library data type is AIL_DOUBLE. |
| `M_SIGNED+8` | Specifies that the ONNX type is int8. The recommended Aurora Imaging Library data type is AIL_INT8. |
| `M_SIGNED+16` | Specifies that the ONNX type is int16. The recommended Aurora Imaging Library data type is AIL_INT16. |
| `M_SIGNED+32` | Specifies that the ONNX type is int32. The recommended Aurora Imaging Library data type is AIL_INT32. |
| `M_SIGNED+64` | Specifies that the ONNX type is int64. The recommended Aurora Imaging Library data type is AIL_INT64. |
| `M_UNSIGNED+8` | Specifies that the ONNX type is uint8. The recommended Aurora Imaging Library data type is AIL_UINT8. |
| `M_UNSIGNED+16` | Specifies that the ONNX type is uint8. The recommended Aurora Imaging Library data type is AIL_UINT8. |

---

### `M_OUTPUT_DATA`

Retrieves the output tensor data.  You can retrieve the number of elements in the tensor with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md). Apply the corresponding Aurora Imaging Library data type modifier to retrieve the output data in the appropriate format otherwise, elements are converted to AIL_DOUBLE which can be detrimental, especially for float16 and int64 ONNX data types.

---

### `M_OUTPUT_RAW`

Retrieves the output tensor raw data.  Retrieves the output tensor raw data. You can retrieve the size (in bytes) of the tensor with[`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md). This result must be cast using the [`M_TYPE_AIL_UINT8`](../../Reference/class/MclassGetResult.md) modifier.

---

### `M_OUTPUT_SHAPE`

Retrieves the output tensor shape array.  You can retrieve the number of elements in the array with [`M_NB_ELEMENTS`](../../Reference/class/MclassGetResult.md). The number of elements composing the shape equals the dimensionality (rank) of the tensor. The values of the shape correspond to the lengths of the dimensions of the tensor. The product of all lengths gives the total number of elements in the tensor.

| Value | Description |
| --- | --- |
| `Value >= 1` | Retrieves the output tensor shape array. |

### For training (CNN, object detection, segmentation, anomaly detection, or tree ensemble) or prediction (CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX) - retrieving the operation's resulting status

To retrieve the status of the training or prediction operation from a result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)), the [`ResultType`](../../Reference/class/MclassGetResult.md)can be set to the following value. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassGetResult.md) parameter to [`M_GENERAL`](../../Reference/class/MclassGetResult.md). Unless otherwise specified, the following values apply to a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer, or a CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX prediction result buffer.

---

### `M_STATUS`

Retrieves information regarding the state of the operation.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the operation completed successfully. |
| `M_CURRENTLY_PREDICTING` | Specifies that the prediction operation is currently ongoing. You can only get this status if you are retrieving it from another thread.  This only applies if you are retrieving results from a CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_CURRENTLY_TRAINING` | Specifies that the training operation is currently ongoing. You can only get this status if you are retrieving it from another thread.  This only applies if you are retrieving results from a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_INTERNAL_ERROR` | Specifies that an unexpected error occurred during the operation. |
| `M_INVALID_REGION_BUFFER` | Specifies that the target image had an invalid region buffer.  This only applies if you are retrieving results from a CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_NO_VALID_DEVSET_ENTRY` | Specifies that the target image had an invalid region buffer. |
| `M_NON_FINITE_VALUE_DETECTED` | Specifies that the training terminated because a non-finite value was detected when the network's parameters were last updated. These values include NaN (Not a Number) and INF (infinity) cases. Such invalid values are often caused by numerically unstable computations that occur during training due to a learning rate that is too high.  This only applies if you are retrieving results from a CNN object detection, or segmentation training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_NOT_ENOUGH_GPU_MEMORY` | Specifies that a memory allocation error occurred while training on the GPU.  This only applies if you are retrieving results from a CNN, object detection, segmentation, or anomaly detection training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_NOT_ENOUGH_MEMORY` | Specifies that a memory allocation error occurred during the operation. |
| `M_PREDICT_NOT_PERFORMED` | Specifies that the prediction operation was not performed. This is the initial status. This will be the status shown if [`MclassPredict`](../../Reference/class/MclassPredict.md) is terminated early by an Aurora Imaging Library error.  This only applies if you are retrieving results from a CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX prediction result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |
| `M_STOPPED_BY_REQUEST` | Specifies that the current execution of the operation was explicitly stopped ([`MclassControl`](../../Reference/class/MclassControl.md) with [`M_STOP_PREDICT`](../../Reference/class/MclassControl.md) or [`M_STOP_TRAIN`](../../Reference/class/MclassControl.md)). |
| `M_TIMEOUT_REACHED` | Specifies that the operation ended because the timeout limit was reached ([`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TIMEOUT`](../../Reference/class/MclassControl.md)). |
| `M_TRAINING_NOT_PERFORMED` | Specifies that the training operation was not performed. This is the initial status.  This only applies if you are retrieving results from a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer ([`ResultClassId`](../../Reference/class/MclassGetResult.md)). |

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

#### `M_TYPE_AIL_FLOAT`

Casts the requested results to an _AIL_FLOAT_.

#### `M_TYPE_AIL_ID`

Casts the requested results to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested results to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested results to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested results to an _AIL_INT64_.

#### `M_TYPE_AIL_UINT8`

Casts the requested results to an _AIL_UINT8_.

This is only available if [`M_COMPUTE_OUT_OF_BAG_RESULTS`](../../Reference/class/MclassControl.md) is set to[`M_ENABLE`](../../Reference/class/MclassControl.md) and [`M_BOOTSTRAP`](../../Reference/class/MclassControl.md) is not set to[`M_DISABLE`](../../Reference/class/MclassControl.md).

This is a predefined classifier for image classification.

This is a predefined classifier for segmentation.

This is a predefined classifier for image classification or segmentation.

To calculate the center of the receptive field of each result returned for[`M_PREDICTION_SCORES`](../../Reference/class/MclassGetResult.md) (_Y_,_X_) in the target image, use the following formula: `(_X_` * [`M_RECEPTIVE_FIELD_STRIDE_X`](../../Reference/class/MclassGetResult.md) + [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResult.md); _Y_ * [`M_RECEPTIVE_FIELD_STRIDE_Y`](../../Reference/class/MclassGetResult.md) + [`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResult.md)).

This value is unavailable if your dataset results were produced using [`MclassPredict`](../../Reference/class/MclassPredict.md) with an ONNX classifier (that is, calling [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassInquire.md) must not return [`M_USER_ONNX`](../../Reference/class/MclassInquire.md)).
