---
doctype: Reference
module: class
function: MclassGetHookInfo
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassGetHookInfo"
---

# MclassGetHookInfo

> Get information about a hooked classification object event.

## Syntax

```c
AIL_INT MclassGetHookInfo(
    AIL_ID    EventId,    //in
    AIL_INT64 InfoType,   //in
    void *    UserVarPtr  //out
)
```

## Description

This function retrieves information about the event that caused the hook-handler function to be called. This function should only be called within the scope of a classification object hook-handler function call (see [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md)).

## Parameters

### `EventId` *(in, AIL_ID)*

Specifies the classification object event identifier received by the hook-handler function. See [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) for more information. The event will be of the type passed to the [`HookType`](../../Reference/class/MclassHookFunction.md) parameter of the hook-handler function.

### `InfoType` *(in, AIL_INT64)*

Specifies the type of information to get.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Note that when getting parameter values, the [`UserVarPtr`](../../Reference/class/MclassGetHookInfo.md) parameter should be of the same data type as value of the selected parameter.

## Parameter Associations

### For getting information about a hooked classification object event

---

### `An M_DATASETS_PREPARED event ID`

Specifies that the event that called the hook-handler function was due to an [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md) event type on a CNN, object detection, segmentation, or anomaly detection training context object.

#### `M_RESULT_ID`

Retrieves the identifier of the result buffer.  If the hook-handler function was called due to an [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md) event type when training, you can pass this result buffer identifier to [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md) to get the prepared training ([`M_PREPARED_TRAIN_DATASET`](../../Reference/class/MclassCopyResult.md)) and development ([`M_PREPARED_DEV_DATASET`](../../Reference/class/MclassCopyResult.md)) datasets.

---

### `An M_EPOCH_TRAINED event ID`

Specifies that the event that called the hook-handler function was due to an [`M_EPOCH_TRAINED`](../../Reference/class/MclassHookFunction.md) event type. Unless otherwise specified, this applies to a CNN, object detection or segmentation training context object.

#### `M_DEV_DATASET_ACCURACY`

Retrieves the accuracy on the development dataset using the trained context obtained at the end of the epoch.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy on the development dataset, as a percentage. |

#### `M_DEV_DATASET_CONFUSION_MATRIX`

Retrieves an array that represents the confusion matrix calculated using the development dataset. The confusion matrix contains information about how many entries were correctly and incorrectly classified during training in the development dataset.  > **Note:** Note, this value is not available for object detection.

#### `M_DEV_DATASET_CONFUSION_MATRIX_SIZE_X`

Retrieves the X-dimension (number of rows) of the confusion matrix calculated using the development dataset.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of rows in the confusion matrix. |

#### `M_DEV_DATASET_CONFUSION_MATRIX_SIZE_Y`

Retrieves the Y-dimension (number of columns) of the confusion matrix calculated using the development dataset.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of columns in the confusion matrix. |

#### `M_DEV_DATASET_EPOCH_ERROR_ENTRIES`

Retrieves the array containing the AIL_UUIDs of each image in the development dataset that produced an error.  For segmentation, UUIDs are also listed for entries that have invalid labels. A label can be invalid if Aurora Imaging Library failed to restore a mask, if the mask size does not match the size of the entry image, if not all the pixels in the image are labeled, or if there is an overlap of pixels between different classes.

#### `M_DEV_DATASET_ERROR_RATE`

Retrieves the error rate on the development dataset using the trained context obtained at the end of the epoch. The error rate is a measurement of how frequently the classifier incorrectly classifies images in the dataset.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the error on the development dataset, as a percentage. |

#### `M_DEV_DATASET_ID`

Retrieves the identifier of the development dataset context.

#### `M_DEV_DATASET_IOU_MEAN`

Retrieves the mean IOU (Intersection over Union) metric of the development dataset after the last epoch. The IOU compares the area and location of the predicted region to the ground truth region.  > **Note:** Note, this value is only available for segmentation.

#### `M_DEV_DATASET_LOSS`

Retrieves the loss metric of the development dataset after the last epoch. You can use the loss to evaluate the lack of confidence (doubt) associated with the training.  > **Note:** Note, this value is not available for CNN.

#### `M_DEV_DATASET_USED_ENTRIES`

Retrieves the array that contains the AIL_UUID of each image in the development dataset used during training.

#### `M_EPOCH_INDEX`

Retrieves the index of the last epoch. An epoch is one complete pass of the training dataset.

#### `M_MINI_BATCH_PER_EPOCH`

Retrieves the number of mini-batches per epoch. A mini-batch is a subset of the dataset. The weights of the classifier are updated after each mini-batch iteration.

#### `M_RESULT_ID`

Retrieves the identifier of the result buffer.

#### `M_TRAIN_CONTEXT_ID`

Retrieves the identifier of the training context.

#### `M_TRAIN_DATASET_ACCURACY`

Retrieves the accuracy of the training dataset using the trained context obtained at the end of the epoch.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the accuracy of the training dataset, as a percentage. |

#### `M_TRAIN_DATASET_CONFUSION_MATRIX`

Retrieves an array that represents the confusion matrix calculated using the training dataset. The confusion matrix contains information about how many entries were correctly and incorrectly classified during training in the development dataset.  > **Note:** Note, this value is not available for object detection.

#### `M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_X`

Retrieves the X-dimension (number of rows) of the confusion matrix calculated using the training dataset.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of rows in the confusion matrix. |

#### `M_TRAIN_DATASET_CONFUSION_MATRIX_SIZE_Y`

Retrieves the Y-dimension (number of columns) of the confusion matrix calculated using the training dataset.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `Value >= 1` | Specifies the number of columns in the confusion matrix. |

#### `M_TRAIN_DATASET_EPOCH_ERROR_ENTRIES`

Retrieves the array containing the AIL_UUIDs of each image in the training dataset that produced an error.  For segmentation, UUIDs are also listed for entries that have invalid labels. A label can be invalid if Aurora Imaging Library failed to restore a mask, if the mask size does not match the size of the entry image, if not all the pixels in the image are labeled, or if there is an overlap of pixels between different classes.

#### `M_TRAIN_DATASET_ERROR_RATE`

Retrieves the error rate of the training dataset using the trained context obtained at the end of the epoch.  > **Note:** Note, this value is not available for object detection.

| Value | Description |
| --- | --- |
| `0.0 <= Value <= 100.0` | Specifies the error of the training dataset, as a percentage. |

#### `M_TRAIN_DATASET_ID`

Retrieves the identifier of the training dataset. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_TRAIN_DATASET_IOU_MEAN`

Retrieves the mean IOU (Intersection over Union) metric of the training dataset after the last epoch.  > **Note:** Note, this value is only available for segmentation.

#### `M_TRAIN_DATASET_USED_ENTRIES`

Retrieves the array that contains the AIL_UUID of each image in the training dataset used during the training phase.

#### `M_TRAINED_PARAMETERS_UPDATED`

Retrieves whether the current CNN, object detection or segmentation classifier parameters are up to date.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the current parameters are not up to date. |
| `M_TRUE` | Specifies that the current parameters are up to date. |

---

### `An M_MINI_BATCH_TRAINED event ID`

Specifies that the event that called the hook-handler function was due to an [`M_MINI_BATCH_TRAINED`](../../Reference/class/MclassHookFunction.md) event type on a CNN, object detection, segmentation, or anomaly detection training context object.

#### `M_DEV_DATASET_ID`

Retrieves the identifier of the development dataset context. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_EPOCH_INDEX`

Retrieves the index of the last epoch.

#### `M_MINI_BATCH_INDEX`

Retrieves the index of the last mini-batch.

#### `M_MINI_BATCH_LOSS`

Retrieves the loss at the end of training a mini-batch. You can use the loss to evaluate the lack of confidence (doubt) associated with the training.

#### `M_MINI_BATCH_PER_EPOCH`

Retrieves the number of mini-batches per epoch.

#### `M_RESULT_ID`

Retrieves the identifier of the result buffer.

#### `M_TRAIN_CONTEXT_ID`

Retrieves the identifier of the training context. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_TRAIN_DATASET_ID`

Retrieves the identifier of the training dataset. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

---

### `An M_PREDICT_ENTRY event ID`

Specifies that the event that called the hook-handler function was due to an [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) event type on a CNN, object detection classifier, segmentation, anomaly detection or tree ensemble context object.

#### `M_BEST_CLASS_INDEX`

Retrieves the index of the class with the highest score, for the last entry being predicted.  For image or feature classification, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetHookInfo.md) retrieves one index.  For segmentation, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetHookInfo.md) retrieves an array of values (indices) that can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetHookInfo.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetHookInfo.md). Each value is the index of the best class score for that X- Y-pixel location. The index of the class begins at 0.  For object detection, [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetHookInfo.md) retrieves an array of values representing the index of the best class score for each detected instance. The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

#### `M_BOX_4_CORNERS`

Retrieves an array containing the coordinates of the four corners of the specified bounding boxes.  The 8 coordinates of the first box (b1) are grouped, followed by the 8 coordinates of the second box (b2), repeating this process for all specified boxes in the order b1x1, b1y1, b1x2, b1y2, b1x3, b1y3, b1x4, b1y4, b2x1, b2y1, b2x2, b2y2, b2x3, b2y3, b2x4, b2y4, etc.  *[Image: MclassBox4Corners.png]*  The size of the array will be equal to the number of specified instances ([`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md)) multiplied by 8.

#### `M_CENTER_X`

Retrieves an array of the X-coordinates of the centers of the specified bounding boxes.  The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

#### `M_CENTER_Y`

Retrieves an array of the Y-coordinates of the centers of the specified bounding boxes.  The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

#### `M_ENTRY_KEY`

Retrieves the key of the last entry being predicted.

#### `M_HEIGHT`

Retrieves an array containing the heights of the specified bounding boxes.  The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

#### `M_INPUT_DATASET_ID`

Retrieves the identifier of the dataset whose entries get predicted ([`MclassPredict`](../../Reference/class/MclassPredict.md) with [`TargetAilObjectId`](../../Reference/class/MclassPredict.md)).

#### `M_NUMBER_OF_ENTRIES_IN_ERROR`

Retrieves the current number of entries that could not be predicted.

#### `M_NUMBER_OF_INSTANCES`

Retrieves the current number of instances in the predicted entry for object detection.

#### `M_NUMBER_OF_PREDICTED_ENTRIES`

Retrieves the current number of entries that were predicted.

#### `M_OUTPUT_DATASET_ID`

Retrieves the identifier of the dataset in which the entries and their predictions are written ([`MclassPredict`](../../Reference/class/MclassPredict.md) with [`DatasetContextOrResultClassId`](../../Reference/class/MclassPredict.md)).

#### `M_PREDICT_CONTEXT_ID`

Retrieves the identifier of the trained classifier context used to predict dataset entries ([`MclassPredict`](../../Reference/class/MclassPredict.md) with [`ContextClassId`](../../Reference/class/MclassPredict.md)).

#### `M_PREDICT_SCORE`

Retrieves the highest class score of the last entry being predicted.  For image or feature classification, [`M_PREDICT_SCORE`](../../Reference/class/MclassGetHookInfo.md) retrieves one score.  For segmentation, [`M_PREDICT_SCORE`](../../Reference/class/MclassGetHookInfo.md) retrieves an array of values (scores) that can be arranged in a 2D image, where the size is equal to [`M_PREDICTION_SIZE_X`](../../Reference/class/MclassGetHookInfo.md) * [`M_PREDICTION_SIZE_Y`](../../Reference/class/MclassGetHookInfo.md). Each value is the highest score for that X- Y- pixel location.  For object detection, [`M_PREDICT_SCORE`](../../Reference/class/MclassGetHookInfo.md), retrieves an array of values representing the highest class score for each detected instance. The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

#### `M_PREDICT_SCORE_AVERAGE`

Retrieves the average prediction score of the predicted entries. This is not available for segmentation.

#### `M_PREDICT_SCORE_MAX`

Retrieves the maximum prediction score of the predicted entries. This is not available for segmentation.

#### `M_PREDICT_SCORE_MIN`

Retrieves the minimum prediction score of predicted entries. This is not available for segmentation.

#### `M_PREDICTION_SIZE_X`

Retrieves the number of class scores, along the X-direction. This is only available for segmentation.

#### `M_PREDICTION_SIZE_Y`

Retrieves the number of class scores, along the Y-direction. This is only available for segmentation.

#### `M_STATUS`

Retrieves the status of the last predicted entry.  [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) event types can occur with CNN, segmentation, object detection, anomaly detection, or tree ensemble classifiers.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the prediction completed. |
| `M_IMAGE_FILE_INVALID` | Specifies that the image file associated with the source entry is not a valid image. |
| `M_IMAGE_FILE_NOT_FOUND` | Specifies that the image file associated with the source entry was not found. |
| `M_INTERNAL_ERROR` | Specifies that an internal error occurred during the current entry predict. |
| `M_INVALID_BUFFER_SIZE` | Specifies that the entry has invalid dimensions; the height/width of the entry does not match the size of the classifier source layer or the size of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_SIZE_BAND` | Specifies that the entry has an invalid number of bands; the number of bands of the entry does not match the number of bands of the classifier source layer or the of the specified target image. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a segmentation classifier. |
| `M_INVALID_BUFFER_TYPE` | Specifies that the entry has an invalid type; the entry type does not match the classifier source layer type or the of the specified target image type. |
| `M_INVALID_NUMBER_OF_FEATURES` | Specifies that the entry has an invalid number of features. This applies to [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a tree ensemble classifier. |
| `M_INVALID_REGION_BUFFER` | Specifies that the entry has no valid region of interest (ROI). |

#### `M_TASK_TYPE`

Retrieves the type of task performed by the [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) event.

| Value | Description |
| --- | --- |
| `M_ANOMALY_DETECTION` | Specifies that the prediction was for an anomaly detection task type. |
| `M_CLASSIFICATION` | Specifies that the prediction was for a classification task type. |
| `M_OBJECT_DETECTION` | Specifies that the prediction was for an object detection task type. |
| `M_SEGMENTATION` | Specifies that the prediction was for a segmentation task type. |

#### `M_WIDTH`

Retrieves an array containing the widths of the specified bounding boxes for object detection.  The size of the array is equal to [`M_NUMBER_OF_INSTANCES`](../../Reference/class/MclassGetHookInfo.md).

---

### `An M_PREPARE_ENTRY_POST event ID`

Specifies that the event that called the hook-handler function was due to an [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) event type on an object detection, CNN, or segmentation data preparation context object.

#### `M_DST_DATASET_ID`

Retrieves the identifier of the destination dataset.

#### `M_DST_ENTRIES_START_INDEX`

Get the destination entry start index for the last mini-batch of prepared images. That batch extends from the start index up to the end of the destination dataset.

#### `M_NUMBER_OF_PREPARED_SRC_ENTRIES`

Retrieves the number of entries from the source dataset that have been prepared so far. You will always get a value greater than or equal to 0.

#### `M_SRC_DATASET_ID`

Retrieves the identifier of the source dataset that is specified by the source entry index ([`M_SRC_ENTRY_INDEX`](../../Reference/class/MclassGetHookInfo.md)).

#### `M_SRC_ENTRY_INDEX`

Retrieves the source entry index of the entry that was prepared. You will always get a value greater than or equal to 0.

#### `M_STATUS`

Retrieves the status of the last prepared entry.  [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) event types can occur with object detection, CNN, or segmentation data preparation contexts.

| Value | Description |
| --- | --- |
| `M_COMPLETE` | Specifies that the entry was prepared successfully. |
| `M_FAILED_TO_SAVE_IMAGE` | Specifies that the prepared image could not be saved at the required destination. |
| `M_FLOAT_IMAGE_NOT_NORMALIZED` | Specifies that float images must be normalized between 0 and 1. |
| `M_IMAGE_FILE_INVALID` | Specifies that the image file associated with the source entry exists but is not restorable. |
| `M_IMAGE_FILE_NOT_FOUND` | Specifies that the image file associated with the source entry was not found. |
| `M_INVALID_AUG_OP_FOR_1_BAND_BUFFER` | Specifies that not all augmentation operations were valid for 1 band buffers and a 1 band buffer was given. |
| `M_INVALID_AUG_OP_FOR_1_BIT_BUFFER` | Specifies that not all augmentation operations were valid for 1 bit buffers and a 1 bit buffer was given. |
| `M_INVALID_AUG_OP_FOR_BUFFER_SIZE_BAND` | Specifies that not all augmentation operations were valid for multi-band buffers with 2 bands or more than 3 bands and that such a buffer was given. This applies to [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) with a CNN or segmentation data preparation context. |
| `M_INVALID_BUFFER_SIGN_FOR_AUG` | Specifies that the augmentation required an unsigned image buffer and a signed buffer was given. |
| `M_INVALID_BUFFER_SIZE_BAND` | Specifies that a buffer with an invalid size band was given; specifically, multi-band buffers with 2 bands or more than 3 bands are not supported. This applies to [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) with an object detection data preparation context. |
| `M_INVALID_CENTER` | Specifies that the given center, for the [`M_CENTER_X`](../../Reference/class/MclassControl.md) and [`M_CENTER_Y`](../../Reference/class/MclassControl.md) controls, is outside the source image. |
| `M_MASK_FILE_NOT_FOUND` | Specifies that one of the mask files associated with the source entry was not found. This applies to [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) with a segmentation data preparation context. |
| `M_RESIZED_IMAGE_TOO_SMALL` | Specifies that the size mode and resize scale factor resulted in a destination image too small to create; this can only occur when no classifier was passed. |
| `M_SOURCE_TOO_SMALL_FOR_DERICHE_OP` | Specifies that the source image is too small for Deriche operations; it must be at least 3 pixels. |

---

### `An M_PREPARE_ENTRY_PRE event ID`

Specifies that the event that called the hook-handler function was due to an [`M_PREPARE_ENTRY_PRE`](../../Reference/class/MclassHookFunction.md) event type on a CNN or segmentation data preparation context object.

#### `M_IMAGE_ID`

Retrieves the buffer identifier of the restored source entry about to be augmented. Use your algorithm (for example the Blob Analysis module) on this buffer identifier to find the center that you want to set ([`M_CENTER_X`](../../Reference/class/MclassSetHookInfo.md) and [`M_CENTER_Y`](../../Reference/class/MclassSetHookInfo.md)).

#### `M_SRC_DATASET_ID`

Retrieves the identifier of the source dataset that is specified by the source entry index ([`M_SRC_ENTRY_INDEX`](../../Reference/class/MclassGetHookInfo.md)).

#### `M_SRC_ENTRY_INDEX`

Retrieve the source entry index of the entry that was prepared. You will always get a value greater than or equal to 0.

---

### `An M_SAMPLE_ADDED event ID`

Specifies that the event that called the hook-handler function was due to an [`M_SAMPLE_ADDED`](../../Reference/class/MclassHookFunction.md)event type on an anomaly detection training context object.

#### `M_CURRENT_SAMPLE_INDEX`

Retrieves the index of the newly added sample.

#### `M_CURRENT_SAMPLE_LOSS`

Retrieves the loss of the newly added sample.

#### `M_ITERATION_INDEX`

Retrieves the index of the current iteration.

#### `M_NUMBER_OF_ITERATIONS`

Retrieves the total number of iterations.

#### `M_RESULT_ID`

Retrieves the identifier of the training result buffer.

---

### `An M_TREE_TRAINED event ID`

Specifies that the event that called the hook-handler function was due to an [`M_TREE_TRAINED`](../../Reference/class/MclassHookFunction.md) event type on a tree ensemble training context object.

#### `M_DEV_DATASET_ID`

Retrieves the identifier of the development dataset context. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_RESULT_ID`

Retrieves the identifier of the result buffer.

#### `M_TRAIN_CONTEXT_ID`

Retrieves the identifier of the training context. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_TRAIN_DATASET_ID`

Retrieves the identifier of the training dataset. This is unavailable for [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md).

#### `M_TREE_DEPTH_ACHIEVED`

Retrieves the tree depth of the current tree. The current tree is equivalent to the last tree added to the tree ensemble.

#### `M_TREE_INDEX`

Retrieves the index of the tree being trained.

#### `M_TREE_NUMBER_OF_LEAF_NODES_ACHIEVED`

Retrieves the number of leaf nodes of the current tree. The current tree is equivalent to the last tree added to the tree ensemble.

---

### `An M_WRITE_NEW_FILE event ID`

Specifies that the event that called the hook-handler function was due to an [`M_WRITE_NEW_FILE`](../../Reference/class/MclassHookFunction.md) event type on a CNN, segmentation, or anomaly detection dataset context object.

#### `M_NEW_FILE_PATH`

Retrieves the path of the most recently created file on disk.

| Value | Description |
| --- | --- |
| `"NewFilePath"` | Specifies the path. |

### Combination Constants — For determining the required number of elements in the array (array size)

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required number of elements in the array (array size).

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### Combination Constants — For getting the size of a string

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the size of a string.

#### `M_STRING_SIZE`

Retrieves the number of characters in the string. This number accounts for every character, including the terminating null character.

### Combination Constants — For determining whether event results are available to be returned

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an event result is available to be returned.

#### `M_AVAILABLE`

Retrieves whether the requested information is available for retrieval.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the requested information is not available. |
| `M_TRUE` | Specifies that the requested information is available. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is `M_NULL` if successful. If the operation fails, a non-null (!`M_NULL`) value is returned.
