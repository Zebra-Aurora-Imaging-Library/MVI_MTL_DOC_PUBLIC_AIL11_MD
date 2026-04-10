---
doctype: Reference
module: class
function: MclassControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassControl"
---

# MclassControl

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

> Control a classifier context, dataset context, class definition, data preparation context, training context, training result buffer, or prediction result buffer.

## Syntax

```c
void MclassControl(
    AIL_ID     ContextOrResultClassId,  //out
    AIL_INT64  LabelOrIndex,            //in
    AIL_INT64  ControlType,             //in
    AIL_DOUBLE ControlValue             //in
)
```

## Description

This function allows you to control a setting of a classifier context (or a class definition therein), a dataset context (or a class definition therein), a data preparation context (for image data), a training context, a training result buffer, or a prediction result buffer. You can typically inquire these settings, using [`MclassInquire`](../../Reference/class/MclassInquire.md). These settings can affect the execution of [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), [`MclassTrain`](../../Reference/class/MclassTrain.md), or [`MclassPredict`](../../Reference/class/MclassPredict.md).

## Parameters

### `ContextOrResultClassId` *(out, AIL_ID)*

Specifies the identifier of a context or result buffer to control. Set this parameter to one of the following values.

*For specifying the context or result buffer*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX classifier context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md), or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md). |
| `ContextDataPreparationId` | Specifies the identifier of a data preparation context (for image data). This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md). |
| `ContextDatasetId` | Specifies the identifier of an images or features dataset context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). |
| `ContextTrainId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble training context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md), or [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `ResultPredictId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX prediction result buffer. These buffers must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_PREDICT_ONNX_RESULT`](../../Reference/class/MclassAllocResult.md). |
| `ResultTrainId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble training result buffer. These buffers must have been previously allocated on the required system using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |

### `LabelOrIndex` *(in, AIL_INT64)*

Specifies what to control. Set this parameter to one of the following values.

*For specifying what to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTHOR_INDEX` | Specifies to control an author in a dataset context. |
| `M_CLASS_INDEX` | Specifies to control a class definition in a classifier context or a dataset context. |
| `M_CONTEXT` *(default)* | Specifies to control a global setting of a classifier context, dataset context, or data preparation context (for image data). |
| `M_DEFAULT_SOURCE_LAYER` | Specifies to control the input (initial) layer of the classifier in a classification context (for example, the source layer of the ONNX classifier). In this case, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a predefined CNN, object detection, segmentation, anomaly detection, or an ONNX classifier. |
| `M_GENERAL` | Specifies to control a general setting of a classification result buffer. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For a classifier context (segmentation or ONNX)

To control a global setting of a predefined segmentation or ONNX classifier context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a predefined segmentation or ONNX classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_TARGET_IMAGE_SIZE_X`

Sets the X-size of the target image with which to call [`MclassPredict`](../../Reference/class/MclassPredict.md), when using CSNet ([`M_CSNET_...`](../../Reference/class/MclassAlloc.md)) predefined segmentation classifiers, FCNet ([`M_FCNET_...`](../../Reference/class/MclassInquire.md)) predefined legacy classifiers, or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) ONNX classifiers.  By default, Aurora Imaging Library assumes the target image size is the same as the default source layer size, which you can retrieve, using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).  [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) must both be set to [`M_DEFAULT`](../../Reference/class/MclassControl.md) (or 0), or they must both be greater than 0.  For CSNets, any target image size is allowed. Typically, cropping is the only valid method for predicting on different sized images. Resized images should not be used for prediction unless your classifier was trained using similarly resized images.  For legacy FCNet classifiers, the increment by which you can increase the dimensions of the target image depends on the size of the classifier's source layer and step. The following calculations indicate how to establish these dimensions:  - `[`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) = [`M_SIZE_X`](../../Reference/class/MclassInquire.md) + _m_ * [`M_STEP_X`](../../Reference/class/MclassInquire.md)`, where _m_ is an integer that is greater than or equal to 0. - `[`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) = [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) + _n_ * [`M_STEP_Y`](../../Reference/class/MclassInquire.md)`, where _n_ is an integer that is greater than or equal to 0.  For ONNX classifiers, [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) must be set to a value greater than 0 if the X-size of the network is defined by the target image X-size; that is, if inquiring [`M_SIZE_X`](../../Reference/class/MclassControl.md) returns [`M_DEFINED_BY_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassInquire.md). Otherwise, [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) must be set to 0 ([`M_DEFAULT`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. By default, Aurora Imaging Library assumes the target image size is the same as the default source layer size. |
| `Value >= 0` | Specifies the X-size. |

---

### `M_TARGET_IMAGE_SIZE_Y`

Sets the Y-size of the target image with which to call [`MclassPredict`](../../Reference/class/MclassPredict.md), when using CSNet ([`M_CSNET_...`](../../Reference/class/MclassAlloc.md)) predefined segmentation classifiers, FCNet ([`M_FCNET_...`](../../Reference/class/MclassInquire.md)) predefined legacy classifiers, or [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md) ONNX classifiers.  By default, Aurora Imaging Library assumes the target image size is the same as the default source layer size, which you can retrieve, using [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_SIZE_X`](../../Reference/class/MclassInquire.md) and [`M_SIZE_Y`](../../Reference/class/MclassInquire.md).  [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) must both be set to [`M_DEFAULT`](../../Reference/class/MclassControl.md) (or 0), or they must both be greater than 0.  For CSNets, any target image size is allowed. Typically, cropping is the only valid method for predicting on different sized images. Resized images should not be used for prediction unless your classifier was trained using similarly resized images.  For legacy FCNet classifiers, the increment by which you can increase the dimensions of the target image depends on the size of the classifier's source layer and step. The following calculations indicate how to establish these dimensions:  - `[`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md) = [`M_SIZE_X`](../../Reference/class/MclassInquire.md) + _m_ * [`M_STEP_X`](../../Reference/class/MclassInquire.md)`, where _m_ is an integer that is greater than or equal to 0. - `[`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) = [`M_SIZE_Y`](../../Reference/class/MclassInquire.md) + _n_ * [`M_STEP_Y`](../../Reference/class/MclassInquire.md)`, where _n_ is an integer that is greater than or equal to 0.  For ONNX classifiers, [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) must be set to a value greater than 0 if the Y-size of the network is defined by the target image Y-size; that is, if inquiring [`M_SIZE_Y`](../../Reference/class/MclassControl.md) returns [`M_DEFINED_BY_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassInquire.md). Otherwise, [`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md) must be set to 0 ([`M_DEFAULT`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. By default, Aurora Imaging Library assumes the target image size is the same as the default source layer size. |
| `Value >= 0` | Specifies the Y-size. |

### For a classifier context (ONNX)

To control a global setting of an ONNX classifier context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of an ONNX classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_TARGET_IMAGE_SIZE_BAND`

Sets the number of bands of the target image with which to call [`MclassPredict`](../../Reference/class/MclassPredict.md).  [`M_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassControl.md) must be set to a value greater than 0 if the number of input bands of the network is defined by the target image; that is, if inquiring [`M_SIZE_BAND`](../../Reference/class/MclassInquire.md) returns [`M_DEFINED_BY_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassInquire.md). Otherwise, [`M_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassControl.md) must be set to 0 ([`M_DEFAULT`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. By default, Aurora Imaging Library assumes the number of bands of the target image is the same as the number of bands of the default source layer. |
| `Value >= 0` | Specifies the number of bands. |

### For a classifier context (object detection)

To control a global setting of a predefined object detection classifier context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a predefined object detection classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_NMS_IOU_THRESHOLD`

Sets the intersection over union (IOU) threshold value used by the non-maximum suppression (NMS) algorithm to determine if a given box proposal is already covered by an existing box proposal. Aurora Imaging Library proposes many overlapping boxes that must be filtered. Of the two overlapping proposals, the one with the lowest detection score will be discarded. The higher the IOU threshold value, the more similarity is required between the boxes before one is discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the IOU threshold. |

---

### `M_NMS_POST_K`

Sets the maximum number of detections to keep after applying the non-maximum suppression (NMS) algorithm to the detection results specified by [`M_NMS_TOP_K`](../../Reference/class/MclassControl.md). The rest of the detections will be discarded. Note, this must be less than or equal to the top K number of detection results ([`M_NMS_TOP_K`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of detections to keep. |

---

### `M_NMS_TOP_K`

Sets the top K number of detection results (in terms of score) that will have the non-maximum suppression (NMS) algorithm applied. A lower value reduces the computation complexity of the algorithm. Note, this must be greater than or equal to the maximum number of detections to keep after applying the NMS algorithm ([`M_NMS_POST_K`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of top detection results. |

### For a classifier context (object detection or anomaly detection)

To control a global setting of a predefined object detection classifier context or a predefined anomaly detection classifier context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a predefined object detection classifier context or a predefined anomaly detection, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_SCORE_THRESHOLD`

Sets the minimum score threshold for a successful detection result. For object detection, only scores above this threshold are kept. For anomaly detection, images or pixels above this score are considered anomalous.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the score threshold. |

### For a classifier context (CNN, object detection, segmentation, or anomaly detection)

To control a global setting of a classifier context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a predefined CNN, predefined object detection, predefined segmentation, or predefined anomaly detection classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_PREDICT_ENGINE`

Sets the predict engine to use during [`MclassPredict`](../../Reference/class/MclassPredict.md). You can get predict engine information by calling[`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREDICT_ENGINE_...`](../../Reference/class/MclassInquire.md). This setting does not apply to statistics classification contexts.  You must have installed the appropriate [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md) to perform GPU accelerated prediction using CUDA or OpenVINO.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default predict engine that was set using the **Aurora Imaging Configurator** utility. |
| `0 <= Value < M_NUMBER_OF_PREDICT_ENGINES` | Specifies the index of the predict engine to use. To inquire a description of the predict engine associated with an index, call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_PREDICT_ENGINE_DESCRIPTION`](../../Reference/class/MclassInquire.md). |

### For a dataset context (images)

To control a global setting of a dataset context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a dataset context. Unless otherwise specified, the following values apply to an images dataset context and you should set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_STOP_PREPARE`

Stops the current execution of [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) on a dataset. This can only be done from another thread of higher priority, or from a user-defined classification hook function. If you stop the data preparation, [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) does not return any images in its destination folder.  Aurora Imaging Library applies[`M_STOP_PREPARE`](../../Reference/class/MclassControl.md) on the destination dataset specified with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). [`M_STOP_PREPARE`](../../Reference/class/MclassControl.md) does not apply when you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For a dataset context (images or features)

To control a global setting of a dataset context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a dataset context. Unless otherwise specified, the following values apply to an images or features dataset context and you should set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_ACTIVE_AUTHOR_INDEX`

Sets the author that is automatically added to labeling operations performed on a dataset context. Aurora Imaging Library uses the current user unless you specify a different one.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTHOR_INDEX` | Specifies the index identifying the author that is automatically defined for each dataset entry.  You can only specify the index of an existing author. Use [`M_AUTHOR_ADD`](../../Reference/class/MclassControl.md) to add a new author. |
| `M_CURRENT_USER` *(default)* | Specifies the system's current username as the author that is automatically defined for each dataset entry. If the system's current user name does not exist in the dataset, it is added at the end. Note that, in this case, authors are considered the same if their respective names are the same (regardless of their index). |

---

### `M_ACTIVE_AUTHOR_UPDATE`

Sets the behavior of the entry's author after a modification was performed on the entry. When disabled, changing an entry will never change its author's name.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to keep the existing author's information. |
| `M_ENABLE` *(default)* | Specifies that when an entry's information (an entry's region information) is modified, its author is automatically modified according to [`M_ACTIVE_AUTHOR_INDEX`](../../Reference/class/MclassControl.md). |

---

### `M_ANOMALY_DETECTION_FOLDER`

Sets the path at which to save the anomaly detection scores. When working with relative paths, you must set the anomaly detection folder to be under the root path.

| Value | Description |
| --- | --- |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md).  If this path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"SegmentationFilePath"` | Specifies the anomaly detection file path. For example, you can specify the root path as, _AIL_TEXT("E:\\Manual\AnomalyDetection\Good")_. |

---

### `M_AUTHOR_ADD`

Adds a new author to a dataset context, according to the specified name. Added authors are given an index and a key (UUID). Once you add an author, you can refer to it in a dataset's entries.

| Value | Description |
| --- | --- |
| `"NewAuthorName"` | Specifies the name of the author to add. |

---

### `M_AUTHOR_DELETE`

Deletes authors from a dataset context, according to the specified index.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTHOR_INDEX` | Specifies the authors to delete from the dataset context.  Note that you cannot delete the active author specified with [`M_ACTIVE_AUTHOR_INDEX`](../../Reference/class/MclassControl.md). |
| `M_ALL` *(default)* | Specifies to delete all explicitly added authors. The active author ([`M_ACTIVE_AUTHOR_INDEX`](../../Reference/class/MclassControl.md)) is not deleted. |

---

### `M_AUTHOR_DELETE_BY_KEY`

Deletes an author from a dataset context, according to the specified key (UUID).

| Value | Description |
| --- | --- |
| `AIL_UUID Value` | Specifies the key of the author to delete. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).  To specify an _AIL_UUID_ value when using a C compiler, you must call the _AIL_UUID_ version of this function (MclassControlAilUuid). When using a C++ or other compiler, [`MclassControl`](../../Reference/class/MclassControl.md) internally calls the AIL_UUID version of this function.  | AIL_UUID version of MclassControl | | --- | | void MclassControlAilUuid | (AIL_ID ContextOrResultClassId, AIL_INT64 LabelOrIndex, AIL_INT64 ControlType, AIL_UUID ControlValue) | |

---

### `M_AUTHOR_NAME`

Sets the name of an author in the dataset context. You must set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to **M_AUTHOR_INDEX()**.

| Value | Description |
| --- | --- |
| `"AuthorName"` | Specifies the name of an author. This name replaces the current author's name. |

---

### `M_CONSOLIDATE_ENTRIES_INTO_FOLDER`

Copies and reorganizes a dataset according to a specified destination directory. The structure of the directory follows the same one as [`MclassExport`](../../Reference/class/MclassExport.md) without the icons and CSV files. In general, this means that within the destination folder, images of the dataset will be copied to the _Images_ folder by class. If the dataset holds segmentation data, mask regions will be copied in the _Masks_ folder and segmentation scores will be copied in the _Segmentation_ folder. The paths of all entries, masks, and segmentation files are updated, as well as the _Mask_ ([`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md)), _Segmentation_ ([`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassControl.md)), and [`M_ANOMALY_DETECTION_FOLDER`](../../Reference/class/MclassControl.md) folders. Source files are not affected by this control type.  It is recommended that you specify an empty destination folder each time you use [`M_CONSOLIDATE_ENTRIES_INTO_FOLDER`](../../Reference/class/MclassControl.md). Class folders that already exist in the destination are not recreated. Existing files within that folder that have conflicting names are overwritten. Existing files without conflicting names remain unaffected. Entries without a ground truth are placed in the _Images_ folder.  Characters that are illegal in Windows file paths are replaced by the underscore character regardless of the operating system used. The maximum size of the file path is 259 characters. If this maximum is exceeded, Aurora Imaging Library attempts to shorten the folder class names.

| Value | Description |
| --- | --- |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md).  If this path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"Path\\ToDstFolder"` | Specifies the file path. For example, you can specify _"C:\\ILoveZebra\\Dest"_. Note, relative paths are supported; paths must not end with '\\'. |

---

### `M_DONT_CARE_CLASS_DRAW_COLOR`

Sets the color given for all regions where the value for [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) has been set to [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControlEntry.md).  The effect of this value can be seen in [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md) with [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md) + [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) where any pixels assigned to [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControlEntry.md) will be given this color.  The effect of this value can also be seen in[`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) with[`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md) where any pixels of this color in the input image buffer will be added as a region with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) set to [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControlEntry.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_COLOR_DONT_CARE_CLASS`](../../Reference/class/MclassControl.md). |
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

---

### `M_ENTRY_ADD`

Adds a new entry to a dataset context. A new entry is added after the last one. For every entry added, a key (_AIL_UUID_) is generated to uniquely identify that entry.  For an images dataset, this automatically adds a full frame region that you can refer to using index 0.  You can import data, such as entries, from a CSV file to a dataset context, using [`MclassImport`](../../Reference/class/MclassImport.md). You can also use Aurora Imaging CoPilot to interactively create, modify, and export datasets and their entries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_ENTRY_DELETE`

Deletes an entry from a dataset context, according to the specified index.

| Value | Description |
| --- | --- |
| `0 <= Value <= M_NUMBER_OF_ENTRIES` *(default)* | Specifies the index of the entry to delete. |

---

### `M_ENTRY_DELETE_ALL`

Deletes all entries in a dataset context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_ENTRY_DELETE_BY_KEY`

Deletes an entry from a dataset context, according to the specified key (UUID).

| Value | Description |
| --- | --- |
| `AIL_UUID Value` | Specifies the key of the entry to delete. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).  To specify an _AIL_UUID_ value when using a C compiler, you must call the _AIL_UUID_ version of this function (MclassControlAilUuid). When using a C++ or other compiler, [`MclassControl`](../../Reference/class/MclassControl.md) internally calls the AIL_UUID version of this function.  | AIL_UUID version of MclassControl | | --- | | void MclassControlAilUuid | (AIL_ID ContextOrResultClassId, AIL_INT64 LabelOrIndex, AIL_INT64 ControlType, AIL_UUID ControlValue) | |

---

### `M_MAKE_FILE_PATHS_ABSOLUTE`

Makes the file path of every entry in a dataset context into an absolute path. This is done by concatenating the current root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md) value) to the relative path ([`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassInquireEntry.md), [`M_REGION_MASK_PATH`](../../Reference/class/MclassInquireEntry.md), [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md), [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md) [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md), [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassControl.md), and [`M_ANOMALY_DETECTION_FOLDER`](../../Reference/class/MclassControl.md)) of all entries. This results in every entry having a complete path, and the root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md)) being replaced with an empty string (_AIL_TEXT("")_).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_MAKE_FILE_PATHS_RELATIVE`

Makes the file path of every entry in a dataset context into a relative path. This is done by parsing the absolute file path ([`M_ENTRY_IMAGE_PATH_ABS`](../../Reference/class/MclassInquireEntry.md), [`M_SEGMENTATION_PATH_ABS`](../../Reference/class/MclassInquireEntry.md), [`M_ANOMALY_DETECTION_PATH_ABS`](../../Reference/class/MclassInquireEntry.md), and [`M_REGION_MASK_PATH_ABS`](../../Reference/class/MclassInquireEntry.md)) of all entries to find a common root path. If a common root path is found, the file paths of all entries are modified to use relative paths, and the root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md)) is set to the newly found common root path. In addition, if [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md), [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassControl.md), and [`M_ANOMALY_DETECTION_FOLDER`](../../Reference/class/MclassControl.md) are set, those folders' paths are also involved in the search for the common root path.  Note, if [`MobjInquire`](../../Reference/obj/MobjInquire.md) with [`M_OBJECT_FILE_FOLDER`](../../Reference/obj/MobjInquire.md) returns a non-empty string value, and the common path has a part that is the same as the object's file folder, Aurora Imaging Library will substitute this value with "///DATASET/". If [`M_OBJECT_FILE_FOLDER`](../../Reference/obj/MobjInquire.md) returns an empty string, Aurora Imaging Library uses the common root path that was found.  For example, if the file paths of two dataset entries are_A:\work\examples\ProcessingRocks\Classification\Image1.mim_ and _A:\work\examples\ProcessingRocks\Classification\Image2.mim_, Aurora Imaging Library would establish the common path as _A:\work\examples\ProcessingRocks\Classification_. In this case, if [`M_OBJECT_FILE_FOLDER`](../../Reference/obj/MobjInquire.md) returns _A:\work\examples\ProcessingRocks\Classification_, Aurora Imaging Library would set the root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md)) to _///DATASET///_. However, if [`M_OBJECT_FILE_FOLDER`](../../Reference/obj/MobjInquire.md) returns an empty string, Aurora Imaging Library would set the root path to _A:\work\examples\ProcessingRocks\Classification_.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_NO_CLASS_DRAW_COLOR`

Sets the color used for pixels that are either not assigned to any region or that you would like to not be assigned to any region.  The effect of this value can be seen in [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md) with [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md) + [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) where any unlabeled pixels will be given this color if [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to[`M_NO_CLASS`](../../Reference/class/MclassControl.md).  The effect of this value can also be seen in[`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) with[`M_GROUND_TRUTH_IMAGE_COLOR`](../../Reference/class/MclassEntryAddRegion.md) where any pixels of this color in the input image buffer will not be added as a region.

| Value | Description |
| --- | --- |
| *(see [`M_DONT_CARE_CLASS_DRAW_COLOR`](Reference/class/MclassControl.md))* |  |
| `M_DEFAULT` |  |
| `M_COLOR_NO_CLASS` *(default)* | Specifies the color (16, 0, 0). |

---

### `M_NO_REGION_PIXEL_CLASS`

Sets the class of any pixels that are not associated with any regions and therefore do not have an assigned value for [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md). This control lets you determine whether you want to interpret these region-less values as having no class or being a part of a specific class.  This control can be useful for interpreting unlabeled pixels as part of a specific class. For example, if you have two classes, one being the object you are interested in and the other being the background class, you have the option of only labeling the objects that you are interested in and then setting the [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) to be the index of your background class. In this case, during training, all pixels without a region will be interpreted as having the same ground truth index as the background class.  [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md)is associated with the UUID of the class at the specified index at the time that it is set. If the corresponding class is later deleted, [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) will default to [`M_NO_CLASS`](../../Reference/class/MclassControl.md).  The effect of this value can be seen in [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md) with [`M_GROUND_TRUTH_IMAGE`](../../Reference/class/MclassDrawEntry.md) + [`M_PSEUDO_COLOR`](../../Reference/class/MclassDrawEntry.md) where any pixels that do not have a label will be given the [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) of the ground truth index specified by value of [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md).  To change the color of the pixels assigned to [`M_NO_CLASS`](../../Reference/class/MclassControl.md), use the [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md) control.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DONT_CARE_CLASS` | Specifies that all pixels not assigned to a region will not contribute to training loss. |
| `M_NO_CLASS` *(default)* | Specifies that no class index is used for pixels not assigned to a region. |
| `0 <= Value < M_NUMBER_OF_CLASSES` | Specifies the index of the class that is used for pixels not assigned to a region. |

---

### `M_REGION_MASKS_FOLDER`

Sets the path at which to save the region masks as they are added by [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md). When working with relative paths, you must set the region masks folder to be under the root path.

| Value | Description |
| --- | --- |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md).  If this path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"RegionMaskFilePath"` | Specifies the region mask file path. For example, you can specify the root path as, _AIL_TEXT("E:\\Manual\RegionMasks\Good")_. |

---

### `M_ROOT_PATH`

Sets the root path that is used for the file path of a dataset's entry images, region masks, and segmentation scores. The file path is set, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md), [`M_REGION_MASK_PATH`](../../Reference/class/MclassControlEntry.md), [`M_SEGMENTATION_PATH`](../../Reference/class/MclassInquireEntry.md), and [`M_ANOMALY_DETECTION_PATH`](../../Reference/class/MclassInquireEntry.md). Note, this is also related to (affects) [`M_REGION_MASKS_FOLDER`](../../Reference/class/MclassControl.md), [`M_SEGMENTATION_FOLDER`](../../Reference/class/MclassControl.md), and [`M_ANOMALY_DETECTION_FOLDER`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `""` | Specifies an empty path. This indicates that all entries' file paths are absolute paths. |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md).  If this path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"RootFilePath"` | Specifies the root file path. For example, you can specify the root path as, _AIL_TEXT("E:\\Manual\LabeledIt\Good")_. |

---

### `M_SEGMENTATION_FOLDER`

Sets the path at which to save the segmentation scores. When working with relative paths, you must set the segmentation folder to be under the root path.

| Value | Description |
| --- | --- |
| `"///DATASET///"` | Specifies the folder path from where the dataset context is restored, using [`MclassRestore`](../../Reference/class/MclassRestore.md).  If this path does not exist, Aurora Imaging Library uses the path in which your application's executable file is located. |
| `"SegmentationFilePath"` | Specifies the segmentation file path. For example, you can specify the root path as, _AIL_TEXT("E:\\Manual\Segmentation\Good")_. |

### For a classifier or dataset context

To control a classifier or dataset context, set the [`ControlType`](../../Reference/class/MclassControl.md) parameter to one of the values below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a classifier or dataset context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md), unless otherwise specified.  Note, these values apply to a classifier context or an images or a features dataset context, unless otherwise specified.

---

### `M_CLASS_ADD`

Adds a new class definition to a classifier or dataset context, with the specified name. Aurora Imaging Library assigns an index to the new class definition such that the index is 1 greater than the highest existing class definition index.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to add a new class definition with no explicitly defined name. Aurora Imaging Library defines a default name, in a "Class ##" format, where "##" is the index value. |
| `"NewClassName"` | Specifies the name of the class definition to add. |

---

### `M_CLASS_DELETE`

Deletes a class definition from a classifier or dataset context, according to the specified index. If an entry in a dataset context references this deleted class definition, that reference is also deleted. Dataset entries without class definitions will cause an error when you call [`MclassTrain`](../../Reference/class/MclassTrain.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLASS_INDEX` | Specifies a class definition to delete. If a region (other than region 0) refers to the specified class to delete, that region is removed. |
| `M_ALL` *(default)* | Specifies to delete all class definitions. All regions other than region 0 will be removed. |

---

### `M_CLASS_DELETE_BY_KEY`

Deletes a class definition from a classifier or dataset context, according to the specified key (UUID). If an entry in a dataset context references this deleted class definition, that reference is also deleted. Dataset entries without class definitions will cause an error when you call [`MclassTrain`](../../Reference/class/MclassTrain.md).

| Value | Description |
| --- | --- |
| `AIL_UUID Value` | Specifies the key of the class definition to delete. The key is defined as an Aurora Imaging Library universal unique identifier (UUID).  To specify an _AIL_UUID_ value when using a C compiler, you must call the _AIL_UUID_ version of this function (MclassControlAilUuid). When using a C++ or other compiler, [`MclassControl`](../../Reference/class/MclassControl.md) internally calls the AIL_UUID version of this function.  If a region (other than region 0) is referred to the specified class, the region is removed.  | AIL_UUID version of MclassControl | | --- | | void MclassControlAilUuid | (AIL_ID ContextOrResultClassId, AIL_INT64 LabelOrIndex, AIL_INT64 ControlType, AIL_UUID ControlValue) | |

### For a class definition in a classifier or dataset context (for CNN, object detection, segmentation, anomaly detection, tree ensemble, or ONNX)

To control a class definition, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to the following values. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a classifier context (predefined CNN, object detection, predefined segmentation, predefined anomaly detection, tree ensemble, or ONNX) or a dataset context (images or features), and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to **M_CLASS_INDEX()**.  Note, class definitions are added to a dataset context, using [`M_CLASS_ADD`](../../Reference/class/MclassControl.md).

---

### `M_ANOMALOUS`

Sets whether the specified class is considered anomalous.  > **Note:** Note, the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter must be the identifier of an anomaly detection classifier context or images dataset used for anomaly detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FALSE` *(default)* | Specifies that images containing this class, either at the image or pixel level, will not be considered anomalous unless it also contains a class with [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) set to [`M_TRUE`](../../Reference/class/MclassControl.md). |
| `M_TRUE` | Specifies that images containing this class, either at the image or pixel level, will be considered anomalous. |

---

### `M_CLASS_DRAW_COLOR`

Sets the color assigned to a class definition. This color is used as a visual label and helps to identify class definitions. Draw operations use the class definition color.

| Value | Description |
| --- | --- |
| *(see [`M_DONT_CARE_CLASS_DRAW_COLOR`](Reference/class/MclassControl.md))* |  |

---

### `M_CLASS_ICON_ID`

Sets the class definition's sample image. This image helps visually identify the class definition. This does not affect calculations.  When you call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md), Aurora Imaging Library copies the image to the context and assigns it a new identifier. Aurora Imaging Library returns this new identifier, if you inquire [`M_CLASS_ICON_ID`](../../Reference/class/MclassInquire.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the image associated with the class definition is freed.  Note that if the specified class definition does not have an associated image, this setting has no effect. |
| `Image identifier` | Specifies the identifier of the image buffer to use for the class definition's sample image. |

---

### `M_CLASS_NAME`

Sets the name of a class definition that is currently in the dataset context. This name replaces the current class definition's name.

| Value | Description |
| --- | --- |
| `"ClassDefinitionName"` | Specifies the name of a class definition. |

---

### `M_CLASS_WEIGHT`

Sets the class definition's weight. In this case, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a features dataset context, an images dataset context being used for segmentation, or a tree ensemble classifier context.  The specified weight can only have an effect if [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the weight. |

### For a classifier context's source layer (ONNX)

To control the source layer of the classifier in an ONNX classifier context, the[`ControlType`](../../Reference/class/MclassControl.md) parameter can be set to one of the following values. In this case, set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_DEFAULT_SOURCE_LAYER`](../../Reference/class/MclassControl.md).

---

### `M_BAND_ORDER`

Sets the order of the input bands of the network. This corresponds to the order in which the network interprets the bands of the images that were used in the training phase. If the bands in the target image ([`MclassPredict`](../../Reference/class/MclassPredict.md)) do not follow this order, Aurora Imaging Library internally compensates by temporarily copying the bands to be ordered as the network expects.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BGR` | Specifies that the bands are ordered according to the BGR (Blue, Green, Red) convention. |
| `M_RGB` *(default)* | Specifies that the bands are ordered according to the RGB (Red, Green, Blue) convention. |

### For a data preparation context (images)

To control a global setting of a data preparation context (for image data), the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a data preparation context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md). These values affect the execution of [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).  Data preparation values related to augmentation ([`M_AUGMENT_...`](../../Reference/class/MclassControl.md)) only have an effect if augmentations are enabled. This can be done by calling [`MclassControl`](../../Reference/class/MclassControl.md) and enabling preset augmentations ([`M_PRESET...`](../../Reference/class/MclassControl.md)). You can also enable other types of augmentation operations ([`M_AUG_..._OP`](../../Reference/im/MimControl.md)) by calling [`MimControl`](../../Reference/im/MimControl.md) with the identifier of the data preparation context's internal augmentation context, which you can inquire using [`M_AUGMENT_CONTEXT_ID`](../../Reference/class/MclassInquire.md).

---

### `M_AUGMENT_BALANCING`

Sets a value that represents the proportion of augmented entries that will go towards class balancing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 80.0. |
| `0.0 <= Value <= 100.0` | Specifies the proportion of augmented entries that will go towards class balancing.  For example, if this is set to 80.0, then 80 percent of the augmented entries specified by [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md) will go towards balancing the dataset whereas the last 20 percent (mainly for image classification) will go evenly to all classes such that they can all have some augmentation.  [`M_AUGMENT_BALANCING`](../../Reference/class/MclassControl.md) only applies when calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with datasets. If you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with image buffers, [`M_AUGMENT_BALANCING`](../../Reference/class/MclassControl.md) is ignored. |

---

### `M_AUGMENT_NUMBER_ABSOLUTE`

Sets the number of augmented entries that Aurora Imaging Library should add to the destination dataset. [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md) only applies when calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with datasets. If you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with image buffers, [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md) is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 0. |
| `Value >= 0` | Specifies the number of augmented entries that should be added to the destination dataset. If you do not specify any augmentations, either with [`M_PRESET_...`](../../Reference/class/MclassControl.md) or with the internal augmentation context ([`M_AUGMENT_CONTEXT_ID`](../../Reference/class/MclassInquire.md)), no images are added to the destination dataset.  The actual number of augmented entries added to the destination dataset will be less than the augment number if there are any problems with the images in the source dataset. To check that all images were augmented properly you can check the [`M_STATUS`](../../Reference/class/MclassGetResult.md) hook info in the [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) hook. |

---

### `M_AUGMENT_NUMBER_FACTOR`

Sets the ratio with which to establish the number of augmentations to add to the destination dataset. [`M_AUGMENT_NUMBER_FACTOR`](../../Reference/class/MclassControl.md) only applies when calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with datasets. If you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with image buffers, [`M_AUGMENT_NUMBER_FACTOR`](../../Reference/class/MclassControl.md) is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 3.0. If you do not specify any augmentations, either with [`M_PRESET_...`](../../Reference/class/MclassControl.md) or with the internal augmentation context ([`M_AUGMENT_CONTEXT_ID`](../../Reference/class/MclassInquire.md)), no images are added to the destination dataset. |
| `Value >= 0.0` | Specifies the ratio. Aurora Imaging Library applies the ratio value to the source dataset to establish the number of augmentations in the destination dataset. For example, setting [`M_AUGMENT_NUMBER_FACTOR`](../../Reference/class/MclassControl.md) to 3.0 indicates a 3:1 ratio between augmented images (destination dataset) and original images (source dataset). Specifying 0.0 results in no augmented entries being added. |

---

### `M_AUGMENT_NUMBER_MODE`

Sets the mode with which to establish the number of augmentations that should be added to the destination dataset when calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies that the number of augmentations is established with [`M_AUGMENT_NUMBER_ABSOLUTE`](../../Reference/class/MclassControl.md). |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically establishes the number of augmentations.  When augmenting a dataset, [`M_AUTO`](../../Reference/class/MclassControl.md) is the same as [`M_FACTOR`](../../Reference/class/MclassControl.md). When augmenting an image buffer, [`M_AUTO`](../../Reference/class/MclassControl.md) is the same as [`M_DISABLE`](../../Reference/class/MclassControl.md). |
| `M_DISABLE` | Specifies that no augmentations are performed. This does not affect other data preparations (these are still performed). |
| `M_FACTOR` | Specifies that the number of augmentations is established with [`M_AUGMENT_NUMBER_FACTOR`](../../Reference/class/MclassControl.md). |

---

### `M_CENTER_MODE`

Sets the mode with which to establish the center of the destination image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the center of the destination image is taken to be the center of the source image.  If a hook-handler function was called due to an [`M_PREPARE_ENTRY_PRE`](../../Reference/class/MclassHookFunction.md) event, the user-defined centers will be over written by the centers specified by [`MclassSetHookInfo`](../../Reference/class/MclassSetHookInfo.md) with [`M_CENTER_X`](../../Reference/class/MclassSetHookInfo.md) and [`M_CENTER_Y`](../../Reference/class/MclassSetHookInfo.md). |
| `M_USER_DEFINED` | Specifies the center for cropping is taken to be the user-defined [`M_CENTER_X`](../../Reference/class/MclassControl.md) and [`M_CENTER_Y`](../../Reference/class/MclassControl.md).  If a hook-handler function was called due to an [`M_PREPARE_ENTRY_PRE`](../../Reference/class/MclassHookFunction.md) event, the user-defined centers will be over written by the centers specified by [`MclassSetHookInfo`](../../Reference/class/MclassSetHookInfo.md) with [`M_CENTER_X`](../../Reference/class/MclassSetHookInfo.md) and [`M_CENTER_Y`](../../Reference/class/MclassSetHookInfo.md).  This mode requires that the [`M_CENTER_X`](../../Reference/class/MclassControl.md) and [`M_CENTER_Y`](../../Reference/class/MclassControl.md) be selected such that in combination with the size the cropped image does not exceed the boundaries of the source image. |

---

### `M_CENTER_X`

Sets the X-coordinate of the center point used by [`M_CENTER_MODE`](../../Reference/class/MclassControl.md). [`M_CENTER_X`](../../Reference/class/MclassControl.md)only has an effect if [`M_CENTER_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the X-coordinate. |

---

### `M_CENTER_Y`

Sets the Y-coordinate of the center point used by [`M_CENTER_MODE`](../../Reference/class/MclassControl.md). [`M_CENTER_Y`](../../Reference/class/MclassControl.md)only has an effect if [`M_CENTER_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the Y-coordinate. |

---

### `M_DESTINATION_FOLDER_MODE`

Sets the mode Aurora Imaging Library uses to manage the destination folder in which it saves the prepared images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CREATE` *(default)* | Specifies that the data preparation process creates a unique _prepare_ destination folder. Each time you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), prepared images are written to this unique folder. Previously created folders are not overwritten.  Aurora Imaging Library creates the unique _prepare_ folders under the folder specified with [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md); for example, _M_PREPARED_DATA_FOLDER/prepare1_, _M_PREPARED_DATA_FOLDER/prepare2_, _M_PREPARED_DATA_FOLDER/prepare..._, _M_PREPARED_DATA_FOLDER/preparex_. Note, Aurora Imaging Library uses the first available folder by following this hierarchy. |
| `M_OVERWRITE` | Specifies that the data preparation process uses the same _prepare_ destination folder. Each time you call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), prepared images are written to this folder.  Aurora Imaging Library creates the _prepare_ folder under the folder specified with [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md); that is, _M_PREPARED_DATA_FOLDER/prepare_.  When using overwrite mode, you cannot make concurrent calls to [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). |

---

### `M_PREPARE_ORIGINAL_IMAGES`

Sets the mode with which to add source entries to the destination dataset, when cropping/resizing images with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). By default, no cropping or resizing is done; in this case, [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) adds (copies) the original (source) images as-is.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the source dataset entries, for cropped/resized images, are added to the destination dataset, if they were not previously added. In this case, multiple calls with the same destination dataset will result in the cropped/resized images only being present in the destination dataset once. |
| `M_DISABLE` | Specifies that the source dataset entries, for cropped/resized images, are not added to the destination dataset. |
| `M_ENABLE` | Specifies that the source entries, for cropped/resized images, are added to the destination dataset, regardless of whether they were previously added. This can result in repeated entries/images. |

---

### `M_PREPARED_DATA_FOLDER`

Sets the path to the destination folder in which to save the prepared dataset.

| Value | Description |
| --- | --- |
| `""` | Specifies an empty path. This implies the destination folder is the default destination folder (_C:\Users\Public\Documents\Classification_ on Windows or _/var/lib/aurora_imaging/Classification_ in the user's home directory on Linux). |
| `"DestinationFolder"` | Specifies the destination folder where the prepared dataset will be saved. |

---

### `M_PRESET_ASPECT_RATIO`

Sets whether to enable the aspect ratio augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the aspect ratio augmentation operation. |
| `M_ENABLE` | Specifies to enable the aspect ratio augmentation operation with preset values. |

---

### `M_PRESET_CROP`

Sets whether to enable the crop augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the crop augmentation operation. |
| `M_ENABLE` | Specifies to enable the crop augmentation operation with preset values. |

---

### `M_PRESET_FLIP`

Sets whether to enable the flip augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the flip augmentation operation. |
| `M_ENABLE` | Specifies to enable the flip augmentation operation with preset values. |

---

### `M_PRESET_GAMMA`

Sets whether to enable the gamma augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the gamma augmentation operation. |
| `M_ENABLE` | Specifies to enable the gamma augmentation operation with preset values. |

---

### `M_PRESET_HUE_OFFSET`

Sets whether to enable the hue offset augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the hue offset augmentation operation. |
| `M_ENABLE` | Specifies to enable the hue offset augmentation operation with preset values. |

---

### `M_PRESET_INTENSITY_ADD`

Sets whether to enable the intensity add augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the intensity add augmentation operation. |
| `M_ENABLE` | Specifies to enable the intensity add augmentation operation with preset values. |

---

### `M_PRESET_INTENSITY_MULTIPLY`

Sets whether to enable the intensity multiply augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the intensity multiply augmentation operation. |
| `M_ENABLE` | Specifies to enable the intensity multiply augmentation operation with preset values. |

---

### `M_PRESET_LIGHTING_DIRECTIONAL`

Sets whether to enable the directional lighting augmentation operation. Modifying an image's directional lighting can be seen as a type of add-ramp operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the directional lighting augmentation operation. |
| `M_ENABLE` | Specifies to enable the directional lighting augmentation operation with preset values. |

---

### `M_PRESET_NOISE_GAUSSIAN_ADDITIVE`

Sets whether to enable the Gaussian additive noise augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the Gaussian additive noise augmentation operation. |
| `M_ENABLE` | Specifies to enable the Gaussian additive noise augmentation operation with preset values. |

---

### `M_PRESET_NOISE_SALT_PEPPER`

Sets whether to enable the salt and pepper augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the salt and pepper noise augmentation operation. |
| `M_ENABLE` | Specifies to enable the salt and pepper noise augmentation operation with preset values. |

---

### `M_PRESET_REDUCE`

Sets whether to enable the reduce augmentation operation. The reduce operation creates an image with the mean values and puts the reduced image at a random location with the regions translated accordingly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the reduce augmentation operation. |
| `M_ENABLE` | Specifies to enable the reduce augmentation operation with preset values. |

---

### `M_PRESET_ROTATION`

Sets whether to enable the rotation augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the rotation augmentation operation. |
| `M_ENABLE` | Specifies to enable the rotation augmentation operation with preset values. |

---

### `M_PRESET_SATURATION_GAIN`

Sets whether to enable the saturation gain augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the saturation gain augmentation operation. |
| `M_ENABLE` | Specifies to enable the saturation gain augmentation operation with preset values. |

---

### `M_PRESET_SCALE`

Sets whether to enable the scale augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the scale augmentation operation. |
| `M_ENABLE` | Specifies to enable the scale augmentation operation with preset values. |

---

### `M_PRESET_SMOOTH_GAUSSIAN`

Sets whether to enable the smooth Gaussian augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the smooth Gaussian augmentation operation. |
| `M_ENABLE` | Specifies to enable the smooth Gaussian augmentation operation with preset values. |

---

### `M_PRESET_TRANSLATION`

Sets whether to enable the translation augmentation operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the translation augmentation operation. |
| `M_ENABLE` | Specifies to enable the translation augmentation operation with preset values. |

---

### `M_RESIZE_SCALE_FACTOR`

Sets the resize scale factor to apply between the contents of the source and destination images. While the control affects the contents of an image by cropping or adding black borders, it does not affect the size x/y of the destination image. Black borders will be ignored during training.  For example, a resize scale factor of 0.5 will result in the contents of the destination images being half the size of the source images, with the remainder of the destination image filled with a black border.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FILL_DESTINATION_BILINEAR` | Specifies to make all entries scale to fill the destination image size x/y determined by [`M_SIZE_MODE`](../../Reference/class/MclassControl.md), [`M_SIZE_X`](../../Reference/class/MclassInquire.md), [`M_SIZE_Y`](../../Reference/class/MclassControl.md) and classifier passed in the [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) call using bilinear interpolation. This mode is meant for the case where you would like to resize all images to a common size x/y without cropping or adding black borders. When in this mode, [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md), [`M_CENTER_X`](../../Reference/class/MclassControl.md), and [`M_CENTER_Y`](../../Reference/class/MclassInquire.md) are all ignored and not used even if they are set. |
| `M_FILL_DESTINATION_NEAREST_NEIGHBOR` | Specifies to make all entries scale to fill the destination image size x/y determined by [`M_SIZE_MODE`](../../Reference/class/MclassControl.md), [`M_SIZE_X`](../../Reference/class/MclassInquire.md), [`M_SIZE_Y`](../../Reference/class/MclassControl.md) and classifier passed in the [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) call using nearest neighbor interpolation. This mode is meant for the case where you would like to resize all images to a common size x/y without cropping or adding black borders. When in this mode, [`M_CENTER_MODE`](../../Reference/class/MclassInquire.md), [`M_CENTER_X`](../../Reference/class/MclassControl.md), and [`M_CENTER_Y`](../../Reference/class/MclassInquire.md) are all ignored and not used even if they are set. |
| `0.0 < Value <= 16.0` *(default)* | Specifies the factor by which to resize. If the factor is set to 1.0 (default), there is no resizing. |

---

### `M_SIZE_MODE`

Sets the mode with which to establish the size of the destination image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the size of the destination image will be the same as the source image if no classifier was given. If a trained classifier is given, then the size of the destination image will be the same as the source layer. If an untrained classifier is given, then the size of the destination image will be the same as the source image size.  Note, if you are using a legacy FCNet classifier, and it is untrained, it will try to adapt the source image size to the nearest classifier formula size. When selecting a size based on the classifier formula, the one selected will be the nearest one when rounding down. In this case, the size of the destination will always be less than or equal to the size of the source image and never larger. |
| `M_USER_DEFINED` | Specifies the size of the destination image will be the user-defined [`M_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_SIZE_Y`](../../Reference/class/MclassControl.md). Both [`M_SIZE_X`](../../Reference/class/MclassControl.md) and [`M_SIZE_Y`](../../Reference/class/MclassControl.md) must be specified in this mode. When [`M_SIZE_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md), the classifier ([`ClassifierContextClassId`](../../Reference/class/MclassPrepareData.md)) in [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) must not be specified. |

---

### `M_SIZE_X`

Sets the width for the destination image. To use this control, [`M_SIZE_MODE`](../../Reference/class/MclassControl.md) must be set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md) and the classifier ([`ClassifierContextClassId`](../../Reference/class/MclassPrepareData.md)) in [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) must not be specified.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the width. |

---

### `M_SIZE_Y`

Sets the height for the destination image. To use this control, [`M_SIZE_MODE`](../../Reference/class/MclassControl.md) must be set to [`M_USER_DEFINED`](../../Reference/class/MclassControl.md) and the classifier ([`ClassifierContextClassId`](../../Reference/class/MclassPrepareData.md)) in [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) must not be specified.

| Value | Description |
| --- | --- |
| `Value > 0.0` | Specifies the height. |

### For a data preparation context (images) or a training context (anomaly detection or tree ensemble)

To control a global setting of a data preparation context (for image data) or a training context (anomaly detection or tree ensemble), the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. Set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a data preparation context (for image data) or the identifier of a training context (anomaly detection or tree ensemble), and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_SEED_MODE`

Sets the mode with which to initialize the seed used for anomaly detection and tree ensemble training or image data augmentation.  Aurora Imaging Library uses the seed value to generate the random numbers needed to train an anomaly detection context or a tree ensemble. Specifically, Aurora Imaging Library uses randomness to:  - Select the entries for each training set (if you do not disable [`M_BOOTSTRAP`](../../Reference/class/MclassControl.md)). - Select the features to consider when splitting a node. To control the maximum number of features considered, use [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) and [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassControl.md). - To shuffle the values of a feature for all entries of the development set (or out-of-bag set), when calculating the feature importance ([`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md)) using permutation ([`M_PERMUTATION`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default mode with which to initialize the seed.  Same as [`M_USER_DEFINED`](../../Reference/class/MclassControl.md) for training contexts.  Same as[`M_AUTO`](../../Reference/class/MclassControl.md) for data preparation contexts. |
| `M_AUTO` | Specifies that the seed is chosen at random, and that Aurora Imaging Library generates and uses a new seed each time you call[`MclassTrain`](../../Reference/class/MclassTrain.md). To retrieve the used seed, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_GENERAL`](../../Reference/class/MclassControl.md) and [`M_SEED_VALUE`](../../Reference/class/MclassControl.md). If you retrain with the same seed, you can reproduce the same training (Aurora Imaging Library uses the same random values). |
| `M_USER_DEFINED` | Specifies the user-defined seed set with [`M_SEED_VALUE`](../../Reference/class/MclassControl.md). |

---

### `M_SEED_VALUE`

Sets the seed value that Aurora Imaging Library uses to generate random numbers. These random numbers are used to train a tree ensemble or to generate the random augmentations required by image data preparation.  Aurora Imaging Library ignores this explicitly specified seed value if [`M_SEED_MODE`](../../Reference/class/MclassControl.md) is set to [`M_AUTO`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the seed value. |

### For a statistics classification, classifier, or training context (CNN, object detection, segmentation, anomaly detection, or tree ensemble)

To control a global setting of a statistics classification context, a classifier context, or a training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values, unless otherwise specified. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a statistics classification context, a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or tree ensemble training or classifier context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_TIMEOUT`

Sets the maximum calculation time, in msec. If you specify a training context, the timeout applies to [`MclassTrain`](../../Reference/class/MclassTrain.md). If you specify a classifier context, the timeout applies to [`MclassPredict`](../../Reference/class/MclassPredict.md). If you specify a statistics classification context, the timeout applies to [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of time. |
| `Value > 0.0` | Specifies the maximum time, in msec. |

### For a training context (CNN, object detection, segmentation, anomaly detection)

To control a global setting of a CNN, object detection, segmentation, or anomaly detection training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, object detection, segmentation, or anomaly detection training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_MINI_BATCH_SIZE`

Sets the size of each mini-batch. Aurora Imaging Library uses mini-batches to split the training dataset to help make the training process more efficient.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 32. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 4. For anomaly detection ([`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md)), the default is 1. |
| `Value > 0` | Specifies the size. |

---

### `M_TRAIN_ENGINE`

Sets the engine (processing unit) that Aurora Imaging Library uses to perform the training process.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies the engine automatically. Typically, Aurora Imaging Library uses the best available GPU. |
| `M_CPU` | Specifies the CPU. |
| `M_GPU` | Specifies the GPU.  You must have installed the appropriate [Aurora Imaging Library add-ons and updates](../../UserGuide/C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md) to train on your GPU. |

### For a training context (CNN, object detection or segmentation)

To control a global setting of a CNN, object detection or segmentation training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, object detection or segmentation training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_FOCAL_LOSS_GAMMA`

Sets a focusing value for the focal loss when training. This only has an effect when [`M_LOSS_FUNCTION_TYPE`](../../Reference/class/MclassControl.md) is set to [`M_FOCAL_LOSS`](../../Reference/class/MclassControl.md). This is only available for classification and segmentation training contexts ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md) and [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the focusing value. Greater values reduce the relative loss for well-classified images while putting more focus on hard to classify images. |

---

### `M_INITIAL_LEARNING_RATE`

Sets the initial learning rate for training. The learning rate determines the degree to which the classifier's weights are modified at each mini-batch during training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 0.005. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 0.001. |
| `Value > 0.0` | Specifies the learning rate. |

---

### `M_LEARNING_RATE_DECAY`

Sets the learning rate decay for training. The learning rate decay determines how fast the learning rate decreases during training. This can help prevent excessive weights.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), the default is 0.1. For segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)) and object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), the default is 0.05. |
| `Value > 0.0` | Specifies the learning rate decay. |

---

### `M_LOSS_FUNCTION_TYPE`

Sets the type of loss function for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically establishes the type of loss function to use. For image classification, Aurora Imaging Library uses [`M_CROSS_ENTROPY`](../../Reference/class/MclassControl.md). For object detection, Aurora Imaging Library uses [`M_DET_LOSS`](../../Reference/class/MclassControl.md). For segmentation, Aurora Imaging Library uses [`M_FOCAL_LOSS`](../../Reference/class/MclassControl.md). |
| `M_CROSS_ENTROPY` | Specifies a cross entropy type of loss. This is only available for classification and segmentation training contexts ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md) and [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)). |
| `M_DET_LOSS` | Specifies a detection type of loss. This is only available for object detection training contexts ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)). |
| `M_DICE_LOSS` | Specifies a dice type of loss. This is only available for segmentation training contexts ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)). |
| `M_FOCAL_LOSS` | Specifies a focal type of loss. To set a focusing value for the focal loss, use [`M_FOCAL_LOSS_GAMMA`](../../Reference/class/MclassControl.md). This is only available for classification and segmentation training contexts ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md) and [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)). |

---

### `M_MAX_EPOCH`

Sets the maximum number of epochs. An epoch refers to one complete cycle through the data that the classifier must learn during training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the maximum number. |

---

### `M_RESET_TRAINING_VALUES`

Sets the value of all training mode controls (hyperparameters) automatically, according to the specified training mode.  Note, only a complete training is performed for object detection and anomaly detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_COMPLETE` *(default)* | Specifies a complete training mode. Typically, this is for completely restarting the training of a CNN, object detection or segmentation classifier context, or for training a CNN, object detection or segmentation classifier context that is not trained. A compete training is also performed for anomaly detection, though this value cannot be explicitly specified.  The training mode controls, and what [`M_COMPLETE`](../../Reference/class/MclassControl.md) automatically sets them to, are listed below. Unless otherwise specified, values apply for image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)), object detection ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), anomaly detection ([`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md)), and segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)).  |   |   | | --- | --- | | [`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) | 0.005 (image classification) or 0.001 (object detection and segmentation) | | [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md) | 0.1 (image classification) or 0.05 (object detection and segmentation) | | [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) | 60 | | [`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md) | 32 (image classification), 4 (object detection and segmentation), or 1 (anomaly detection) | | [`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md) | [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md) (image classification and segmentation) or [`M_FACTOR_DECAY`](../../Reference/class/MclassControl.md) (object detection) | | For anomaly detection, a complete training is always performed (though [`M_COMPLETE`](../../Reference/class/MclassControl.md) cannot be explicitly set). This complete training also affects [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md), [`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md), and [`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md). Refer to these values for details. |  > **Note:** When using [`M_COMPLETE`](../../Reference/class/MclassControl.md), the classifier architecture adapts to certain qualities of the training dataset (image X- and Y-size, image bands, and the number of classes). All of the classifier's weights (learnable parameters) are randomly initialized prior to a complete training. |
| `M_FINE_TUNING` | Specifies a fine tuning training mode. Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must fine tune with new data. If you specify [`M_FINE_TUNING`](../../Reference/class/MclassControl.md) with a classifier context that was not sufficiently trained, you will get an error.  The training mode controls, and what [`M_FINE_TUNING`](../../Reference/class/MclassControl.md) automatically sets them to, are listed below. Unless otherwise specified, values apply for image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)) and segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)).  |   |   | | --- | --- | | [`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) | 0.001 | | [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md) | 0.1 (image classification) or 0.05 (segmentation) | | [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) | 60 | | [`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md) | 32 (image classification) or 4 (segmentation) | | [`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md) | [`M_DECAY`](../../Reference/class/MclassControl.md) |  > **Note:** When using [`M_FINE_TUNING`](../../Reference/class/MclassControl.md), the classifier architecture is unchangeable. Therefore, the classifier's weights must be complete, and all the related qualities of the training dataset (image X- and Y-size, image bands, and the number of classes) must comply to the classifier's architecture. A fine-tuning training starts by using the classifier's current weights (learnable parameters). [`M_FINE_TUNING`](../../Reference/class/MclassControl.md) is not supported for object detection. |
| `M_TRANSFER_LEARNING` | Specifies a transfer learning mode. Typically, this is for a CNN or segmentation classifier context that was already trained on a specific classification problem, and that you must train on a similar (but new) problem. If you specify [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md) with a context that was not sufficiently trained, you will get an error.  The training mode controls, and what [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md) automatically sets them to, are listed below. Unless otherwise specified, values apply for image classification ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)) and segmentation ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)).  |   |   | | --- | --- | | [`M_INITIAL_LEARNING_RATE`](../../Reference/class/MclassControl.md) | 0.005 (image classification) or 0.001 (segmentation) | | [`M_LEARNING_RATE_DECAY`](../../Reference/class/MclassControl.md) | 0.1 (image classification) or 0.05 (segmentation) | | [`M_MAX_EPOCH`](../../Reference/class/MclassControl.md) | 60 | | [`M_MINI_BATCH_SIZE`](../../Reference/class/MclassControl.md) | 32 (image classification) or 4 (segmentation) | | [`M_SCHEDULER_TYPE`](../../Reference/class/MclassControl.md) | [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md) |  > **Note:** When using [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md), the classifier architecture adapts to certain qualities of the training dataset (image X- and Y-size, and the number of classes). During training, some of the classifier's weights (learnable parameters) are randomly initialized and affected by the training (those related to classifying features), while others remain unaffected by the training (those related to extracting features). [`M_TRANSFER_LEARNING`](../../Reference/class/MclassControl.md) is not supported for object detection. |

---

### `M_SCHEDULER_TYPE`

Sets the schedule with which to adjust the learning rate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. For image classification and segmentation, the default is [`M_CYCLICAL_DECAY`](../../Reference/class/MclassControl.md).  For object detection, the default is [`M_FACTOR_DECAY`](../../Reference/class/MclassControl.md). |
| `M_CYCLICAL_DECAY` | Specifies to decay the learning rate at an internally established cyclical schedule. |
| `M_DECAY` | Specifies to decay the learning rate, as weights are updated. |
| `M_FACTOR_DECAY` | Specifies to decay the learning rate by a factor at every specified epoch. |

---

### `M_SPLIT_PERCENTAGE`

Sets the percentage value, for the training dataset, used by [`MclassTrain`](../../Reference/class/MclassTrain.md) when splitting a single dataset into a training and development dataset. For example, if you call [`MclassTrain`](../../Reference/class/MclassTrain.md) with a single dataset, and you set [`M_SPLIT_PERCENTAGE`](../../Reference/class/MclassControl.md) to 80 percent ([`M_DEFAULT`](../../Reference/class/MclassControl.md)), the training dataset will contain 80 percent of the entries in the single dataset, and the development dataset will contain 20 percent of the entries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the percentage. |

---

### `M_SPLIT_SEED_MODE`

Sets the split seed mode to use when splitting a single dataset into a training and development dataset. For example, this can be set to randomly assign each entry to the training set or development set using [`M_RANDOM`](../../Reference/class/MclassControl.md) or, a fixed selection process using [`M_FIXED`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_RANDOM`](../../Reference/class/MclassControl.md). |
| `M_FIXED` | Specifies to use the [`M_SPLIT_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for image classification (CNN) or feature classification (tree ensemble), or the[`M_SPLIT_SEG_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for segmentation. |
| `M_RANDOM` | Specifies to use the [`M_SPLIT_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for image classification (CNN) or feature classification (tree ensemble), or the[`M_SPLIT_SEG_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) predefined split classification context, for segmentation. |

---

### `M_TRAIN_DESTINATION_FOLDER`

Sets the path to the destination folder in which to save training results.  For image classification training ([`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md)) and object detection training ([`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md)), this specifies the location of the prepared images that will be stored on disk. For segmentation training ([`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md)), this determines the destination for both the segmentation scores and the prepared images that will be stored on disk.

| Value | Description |
| --- | --- |
| `""` | Specifies an empty path. This indicates that you are using the default path, which you can inquire using [`M_TRAIN_DESTINATION_FOLDER`](../../Reference/class/MclassControl.md) + [`M_DEFAULT_PATH`](../../Reference/class/MclassInquire.md). |
| `"TrainingResultsPath"` | Specifies the training results file path. For example, you can specify the root path as, _AIL_TEXT("E:\\Manual\TrainingResults\Good")_. |

### For a training context (CNN, segmentation, or feature classification)

To set a global setting of a CNN, segmentation, or tree ensemble training context, set the [`ControlType`](../../Reference/class/MclassControl.md) parameter to the value below. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, segmentation, or tree ensemble training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_SUPPORT_MISSING_GROUND_TRUTH`

Sets whether the training process allows the training and development datasets to contain entries that do not have a ground truth. An entry's ground truth is specified using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md).  For segmentation, when [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md) is set to [`M_NO_CLASS`](../../Reference/class/MclassControl.md), entries without a descriptor based region ([`M_DESCRIPTOR_BASED`](../../Reference/class/MclassInquireEntry.md)) are considered not to have a ground truth.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that entries without a ground truth are not allowed. In this case, if you call [`MclassTrain`](../../Reference/class/MclassTrain.md), and one or more entries in the training or development datasets do not have a ground truth, you will get an error. |
| `M_ENABLE` | Specifies that entries without a ground truth are allowed. In this case, if you call [`MclassTrain`](../../Reference/class/MclassTrain.md), and one or more entries in the training or development datasets do not have a ground truth, Aurora Imaging Library ignores them. |

### For a training context (segmentation)

To control a global setting of a segmentation training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a segmentation training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_CLASS_WEIGHT_STRENGTH`

Sets the class weight strength when training with an inverse class frequency weight mode ([`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassControl.md)). This setting has no effect when training with other weight modes.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the proportion of weighting to assign to [`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassControl.md). Reducing this value reduces the emphasis on the less represented classes. |

---

### `M_FEATURE_SIZE_X`

Sets the width (X-size) of the smallest feature to segment. This acts as a lower bound to the X-size of the class regions that the classifier expects during training. This control might be useful if the minimum feature size is larger than the minimum receptive field of the classifier. The default feature size ([`M_MIN`](../../Reference/class/MclassControl.md)) is typically the best possible setting. This has no effect when fine tuning ([`M_FINE_TUNING`](../../Reference/class/MclassControl.md)).  If images have been resized during data preparation (either by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) or an internal data preparation context), this value must be set relative to the final prepared images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MIN` *(default)* | Specifies the smallest size possible. |
| `Value > 0` | Specifies that the classifier will adjust to encompass features of the size you set, provided they are larger than the minimum possible feature size. |

---

### `M_FEATURE_SIZE_Y`

Sets the height (Y-size) of the smallest feature to segment. This acts as a lower bound to the Y-size of the class regions that the classifier expects during training. This control might be useful if the minimum feature size is larger than the minimum receptive field of the classifier. The default feature size ([`M_MIN`](../../Reference/class/MclassControl.md)) is typically the best possible setting. This has no effect when fine tuning ([`M_FINE_TUNING`](../../Reference/class/MclassControl.md)).  If images have been resized during data preparation (either by [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) or an internal data preparation context), this value must be set relative to the final prepared images.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_MIN` | Specifies the smallest size possible. |
| `M_SAME` *(default)* | Specifies to use the same value as [`M_FEATURE_SIZE_X`](../../Reference/class/MclassControl.md). |
| `Value > 0` | Specifies that the classifier will adjust to encompass features of the size you set, provided they are larger than the minimum possible feature size. |

---

### `M_USE_MASK_DESCRIPTORS`

Sets whether to use mask descriptors for training. You must enable [`M_USE_MASK_DESCRIPTORS`](../../Reference/class/MclassControl.md) if you disable[`M_USE_POLYGON_DESCRIPTORS`](../../Reference/class/MclassControl.md) (both can be enabled, but both cannot be disabled).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use mask descriptors for training. |
| `M_ENABLE` *(default)* | Specifies to use mask descriptors for training. |

---

### `M_USE_POLYGON_DESCRIPTORS`

Sets whether to use polygon descriptors for training. You must enable [`M_USE_POLYGON_DESCRIPTORS`](../../Reference/class/MclassControl.md) if you disable[`M_USE_MASK_DESCRIPTORS`](../../Reference/class/MclassControl.md) (both can be enabled, but both cannot be disabled).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use polygon descriptors for training. |
| `M_ENABLE` *(default)* | Specifies to use polygon descriptors for training. |

### For a training context (anomaly detection)

To control a global setting of an anomaly detection training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of an anomaly detection training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_MINI_BATCHES_PER_SAMPLING`

Sets how many mini-batches are loaded into memory before sampling when [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md) is set to [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the number of mini-batches. |

---

### `M_SAMPLES_FIXED_SIZE`

Sets the number of samples learned by the classifier. The higher the number, the bigger the size of the network (bigger networks can have a better/higher capacity to handle more complex cases).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the number of samples. |

---

### `M_TRAIN_MODE`

Sets the mode in which the samples are selected.  By default, Aurora Imaging Library tries to perform a global type of training ([`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md)), but this can lead to running out of GPU memory. In this case, it is recommended to set [`M_TRAIN_MODE`](../../Reference/class/MclassControl.md) to [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md), and then increase [`M_MINI_BATCHES_PER_SAMPLING`](../../Reference/class/MclassControl.md) until you maximize GPU usage.  Note that both modes result in similar accuracy.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically select the mode in which the samples are selected. This is the same as [`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md). |
| `M_GLOBAL_SAMPLING` | Specifies a global selection mode.  This mode loads the whole training set into memory and then samples it. [`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md) is memory intensive but faster than [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md) since the sampling occurs only once. |
| `M_ITERATIVE_SAMPLING` | Specifies an iterative selection mode.  This mode partitions the training set and samples each partition iteratively. [`M_ITERATIVE_SAMPLING`](../../Reference/class/MclassControl.md) uses less memory than [`M_GLOBAL_SAMPLING`](../../Reference/class/MclassControl.md) but takes longer since the sampling process is repeated at each iteration. |

### For a training context (segmentation or tree ensemble)

To control a global setting of a segmentation or tree ensemble training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a segmentation or tree ensemble training context, unless otherwise specified, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_CLASS_WEIGHT_MODE`

Sets how to establish the weight of a class definition.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically sets how the weight is established. For segmentation training contexts, if the loss function type is [`M_FOCAL_LOSS`](../../Reference/class/MclassControl.md) or [`M_CROSS_ENTROPY`](../../Reference/class/MclassControl.md), [`M_AUTO`](../../Reference/class/MclassControl.md) is the same as[`M_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassControl.md). If the loss function type is [`M_DICE_LOSS`](../../Reference/class/MclassControl.md), [`M_AUTO`](../../Reference/class/MclassControl.md)is the same as [`M_SQUARED_INVERSE_CLASS_FREQUENCY`](../../Reference/class/MclassControl.md). For image classification and tree ensemble training contexts, [`M_AUTO`](../../Reference/class/MclassControl.md)is the same as [`M_NONE`](../../Reference/class/MclassControl.md). |
| `M_BALANCE` | Specifies a balanced weight. This setting is only for tree ensemble. |
| `M_INVERSE_CLASS_FREQUENCY` | Specifies to use weight factors equal to the inverse of each class frequency. You can further adjust the weight factors with [`M_CLASS_WEIGHT_STRENGTH`](../../Reference/class/MclassControl.md).  This setting is only for segmentation. |
| `M_NONE` | Specifies no weight. |
| `M_SQUARED_INVERSE_CLASS_FREQUENCY` | Specifies to use weight factors equal to the squared inverse of each class frequency. You can further adjust the weight factors with [`M_CLASS_WEIGHT_STRENGTH`](../../Reference/class/MclassControl.md).  This setting is only for segmentation. |
| `M_USER_DEFINED` | Specifies to use the weight defined with [`M_CLASS_WEIGHT`](../../Reference/class/MclassControl.md). |

### For a training context (tree ensemble)

To control a global setting of a tree ensemble training context, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a tree ensemble training context, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_BOOTSTRAP`

Sets whether to use bootstrap aggregation for the training dataset entries to produce multiple and separate training sets when building trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use bootstrap aggregation. |
| `M_WITH_REPLACEMENT` *(default)* | Specifies to use bootstrap aggregation, and allow replacement (chosen entries in a training set can be chosen again). |
| `M_WITHOUT_REPLACEMENT` | Specifies to use bootstrap aggregation, and not allow replacement (chosen entries in a training set cannot be chosen again). |

---

### `M_COMPUTE_OUT_OF_BAG_RESULTS`

Sets whether to use out-of-bag dataset entries to estimate the generalization accuracy during training.  Note, bagging information is typically unreliable if your training dataset has augmented entries.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies not to use out-of-bag entries. |
| `M_ENABLE` *(default)* | Specifies to use out-of-bag entries. |

---

### `M_COMPUTE_PROXIMITY_MATRIX`

Sets whether to calculate the proximity measure matrix when building trees.  This is a normalized `_N_ x _N_` matrix, where _N_ is the number of entries. The intersection value between pairs of entries in the matrix defines their proximity, which refers to the average number of trees that occupy the same terminal node for those entries. For example, if entries `i` and _j_ are in the same terminal node, their proximity increases.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to calculate the proximity measure matrix when building trees. |
| `M_ENABLE` | Specifies to calculate the proximity measure matrix when building trees. |

---

### `M_FEATURE_IMPORTANCE_MODE`

Sets how to establish the importance of the features.  Every entry in a features dataset refers to a set of features. For example, every entry can refer to a set of blob features, such as area, perimeter, and Feret diameter. Specific values for these features are specified, for each entry, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_RAW_DATA`](../../Reference/class/MclassControlEntry.md).  By enabling a feature importance mode, Aurora Imaging Library establishes, during training, whether certain features are more important than others in determining the class to which the input data belongs. For example, the perimeter feature can end up being very important to establishing a successful classification, while the area feature can end up being almost irrelevant.  To retrieve the resulting importance of features after training, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_FEATURE_IMPORTANCE`](../../Reference/class/MclassGetResult.md). You can then modify your set of features and retrain, which can help improve the tree ensemble classifier's performance and accuracy.  Note, [`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md) does not directly affect training. It allows you to retrieve which features are more important; you can then use this information to modify your dataset (for example, you can specify only the important features) and retrain.  In general, the fastest modes with which to establish the feature importance are [`M_MEAN_DECREASE_IMPURITY`](../../Reference/class/MclassControl.md), [`M_PERMUTATION`](../../Reference/class/MclassControl.md), and [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md), while the modes with which to most accurately establish the feature importance are [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md), [`M_PERMUTATION`](../../Reference/class/MclassControl.md), and [`M_MEAN_DECREASE_IMPURITY`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to disable the feature importance mode. In this case, you cannot retrieve any information about the feature importance. |
| `M_DROP_COLUMN` | Specifies that the feature importance is based on the drop column of features. In this case, the more the elimination of a feature affects accuracy, the more importance that feature is given. To specify the set with which to calculate this type of feature importance, use [`M_FEATURE_IMPORTANCE_SET`](../../Reference/class/MclassControl.md). To use [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md), you must train with a development dataset or compute out-of-bag results (that is, enable [`M_COMPUTE_OUT_OF_BAG_RESULTS`](../../Reference/class/MclassControl.md)). |
| `M_MEAN_DECREASE_IMPURITY` *(default)* | Specifies that the feature importance is based on a decreasing impurity process. In this case, the more a feature affects a proper node splitting, the more important it is. Proper splitting means that the two sets resulting from splitting the node are significantly purer (better) than the set that was input to the node. |
| `M_PERMUTATION` | Specifies that the feature importance is based on feature permutation. In this case, the more the shuffling of a feature affects accuracy, the more importance that feature is given. To specify the set with which to calculate this type of feature importance, use [`M_FEATURE_IMPORTANCE_SET`](../../Reference/class/MclassControl.md). To use [`M_PERMUTATION`](../../Reference/class/MclassControl.md), you must train with a development dataset or compute out-of-bag results (that is, enable [`M_COMPUTE_OUT_OF_BAG_RESULTS`](../../Reference/class/MclassControl.md)). |

---

### `M_FEATURE_IMPORTANCE_SET`

Sets how to establish the group of features with which to determine feature importance when using drop column or permutation. [`M_FEATURE_IMPORTANCE_SET`](../../Reference/class/MclassControl.md) only has an effect if [`M_FEATURE_IMPORTANCE_MODE`](../../Reference/class/MclassControl.md) is set to [`M_DROP_COLUMN`](../../Reference/class/MclassControl.md) or [`M_PERMUTATION`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to automatically establish the feature set to use. If available, Aurora Imaging Library uses [`M_DEV_DATASET`](../../Reference/class/MclassControl.md); otherwise, Aurora Imaging Library uses [`M_OUT_OF_BAG`](../../Reference/class/MclassControl.md). |
| `M_DEV_DATASET` | Specifies to use the development dataset. |
| `M_OUT_OF_BAG` | Specifies to use the out-of-bag set. In this case, you must not disable [`M_BOOTSTRAP`](../../Reference/class/MclassControl.md).  Note, bagging information is typically unreliable if your data is augmented. |

---

### `M_MIN_IMPURITY_DECREASE`

Sets a decrease of impurity requirement for splitting a node. That is, Aurora Imaging Library will split a node if it induces a decrease of impurity greater than or equal to the specified value.  The weighted impurity decrease equation is as follows: `_N_t_ / _N_ * (_impurity_ - _N_t_R_ / _N_t_ * _right_impurity_ - _N_t_L_ / _N_t_ * _left_impurity_)`, where _N_ is the total number of dataset entries,_ N_t_ is the number of dataset entries related to the current node, _N_t_L_ is the number of dataset entries related to the left child, and _N_t_R_ is the number of dataset entries related to the right child. _N_, _N_t_, _N_t_R_, and _N_t_L_ all refer to the weighted sum, if weights (entry weights and class weights) are used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the impurity value. |

---

### `M_MIN_NUMBER_OF_ENTRIES_LEAF`

Sets the minimum number of training dataset entries, or a percentage of the minimum number of related dataset entries, that Aurora Imaging Library uses to identify a leaf node. Aurora Imaging Library uses this value to establish [`M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < MinNumPercent <= 100.0` | Specifies the minimum number of entries, as a percentage. You should only use a percentage when [`M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_PERCENTAGE`](../../Reference/class/MclassControl.md).  If you specify a percentage value that results in the minimum number of entries equaling 0, Aurora Imaging Library uses 1 as the minimum number; that is, if `Round((Value/100) * _NumberEntriesInTrainDataset_)`== 0, then _MinNumberEntriesLeaf_ = 1. |
| `Value >= 1` *(default)* | Specifies the minimum number of entries. You should only use this value when [`M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_VALUE`](../../Reference/class/MclassControl.md). |

---

### `M_MIN_NUMBER_OF_ENTRIES_LEAF_MODE`

Sets how to establish the minimum number of related dataset entries required to be at a leaf node. A split point at any depth will only be considered if it leaves at least the minimum number of entries (leaf training entries) in each of the left and right branches.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_LEAF`](../../Reference/class/MclassControl.md). |
| `M_USER_DEFINED_VALUE` *(default)* | Specifies to use the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_LEAF`](../../Reference/class/MclassControl.md). |

---

### `M_MIN_NUMBER_OF_ENTRIES_SPLIT`

Sets the minimum number of training dataset entries, or a percentage of the minimum number of related dataset entries, that Aurora Imaging Library uses to determine how to best split the internal nodes of the trees. Aurora Imaging Library uses this value to establish [`M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 < MinNumPercent <= 100.0` | Specifies the minimum number of entries, as a percentage. This only has an effect if [`M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_PERCENTAGE`](../../Reference/class/MclassControl.md).  If you specify a percentage value that results in the minimum number of entries equaling 0, Aurora Imaging Library uses 2 as the minimum number; that is, if `Round((Value/100) * _NumberEntriesInTrainDataset_)` == 0, then _MinNumberEntriesSplit_ = 2. |
| `Value >= 1` *(default)* | Specifies the minimum number of entries. This only has an effect if [`M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_VALUE`](../../Reference/class/MclassControl.md). |

---

### `M_MIN_NUMBER_OF_ENTRIES_SPLIT_MODE`

Sets how to establish the minimum number of related dataset entries required to split an internal node.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_SPLIT`](../../Reference/class/MclassControl.md). |
| `M_USER_DEFINED_VALUE` *(default)* | Specifies to use the minimum number of entries defined with [`M_MIN_NUMBER_OF_ENTRIES_SPLIT`](../../Reference/class/MclassControl.md). |

---

### `M_MIN_WEIGHT_FRACTION_LEAF`

Sets the minimum weighted fraction of the sum total of weights (of all the dataset entries) required to be at a leaf node. Entries have equal weight when [`M_CLASS_WEIGHT_MODE`](../../Reference/class/MclassControl.md) is set to [`M_NONE`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 0.5` *(default)* | Specifies the minimum weighted fraction. |

---

### `M_NODE_MAX_NUMBER_OF_FEATURES`

Sets the maximum number of features in the training dataset, or a percentage of the maximum number of features, that Aurora Imaging Library can use to determine how to best split the nodes of the trees. [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassControl.md) only has an effect if [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_VALUE`](../../Reference/class/MclassControl.md) or [`M_USER_DEFINED_PERCENTAGE`](../../Reference/class/MclassControl.md).  If [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) is set to [`M_LOG2`](../../Reference/class/MclassControl.md), and the number of features in the dataset is 1, Aurora Imaging Library trains with 1 feature, even though the calculation results in 0; that is, `log(1) = 0`.  If [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) is equal to [`M_USER_DEFINED_VALUE`](../../Reference/class/MclassControl.md), and [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassControl.md) is higher than the actual number of features in the training dataset, Aurora Imaging Library uses all the features in the dataset.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies no maximum. |
| `0.0 <= MaxNumPercent <= 100.0` | Specifies the percentage with which to determine the maximum number of features. This only has an effect if [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_PERCENTAGE`](../../Reference/class/MclassControl.md).  If you specify a percentage value that results in the maximum number of features equaling 0, Aurora Imaging Library uses 1 feature as the maximum number; that is, if `Round((Value/100)*_NumberOfFeaturesInTrainDataset_)`== 0, then _MaxNumberOfFeatures_ = 1 (see [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md)). |
| `Value >= 1` | Specifies the value with which to determine the maximum number of features. This only has an effect if [`M_NODE_MAX_NUMBER_OF_FEATURES_MODE`](../../Reference/class/MclassControl.md) is set to [`M_USER_DEFINED_VALUE`](../../Reference/class/MclassControl.md).  If you specify a maximum value that is greater than the total number of features available, Aurora Imaging Library uses the total number of features available. |

---

### `M_NODE_MAX_NUMBER_OF_FEATURES_MODE`

Sets how to establish the maximum number of features that Aurora Imaging Library uses to determine how to best split the nodes of the trees.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` | Specifies that all available features are used. |
| `M_LOG2` | Specifies to use a base 2 logarithm, determined from the total number of features. This can be described as: [`M_LOG2`](../../Reference/class/MclassControl.md) = `Round(Log2(_NumberOfFeatures_))`. |
| `M_SQUARE_ROOT` *(default)* | Specifies to use a square root, determined from the total number of features. This can be described as: [`M_SQUARE_ROOT`](../../Reference/class/MclassControl.md) = `Round(Log2(_NumberOfFeatures_))`. |
| `M_USER_DEFINED_PERCENTAGE` | Specifies to use a percentage, determined from the total number of features, where the percentage is set by [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassControl.md). This can be described as: [`M_USER_DEFINED_PERCENTAGE`](../../Reference/class/MclassControl.md) = `Round(_NumberOfFeatures_* _MaxNumberOfFeatures_/100)`. |
| `M_USER_DEFINED_VALUE` | Specifies to use the maximum number of features defined with [`M_NODE_MAX_NUMBER_OF_FEATURES`](../../Reference/class/MclassControl.md). |

---

### `M_NUMBER_OF_TREES`

Sets the number of trees in the tree ensemble train context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 1` *(default)* | Specifies the amount of trees in the tree ensemble. |

---

### `M_SPLIT_QUALITY_TYPE`

Sets how to measure the quality of a node split.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ENTROPY` | Specifies that Aurora Imaging Library uses an Entropy based function to measure the quality of a node split. |
| `M_GINI` *(default)* | Specifies that Aurora Imaging Library uses a Gini based function to measure the quality of a node split. |

---

### `M_TREE_MAX_DEPTH`

Sets the maximum depth of the trees in the tree ensemble.  The depth of a tree ([`M_TREE_MAX_DEPTH`](../../Reference/class/MclassControl.md)) is the depth of its deepest node. The depth of a node is the number of edges (connectors) from that node to the tree's root node. A root node has a depth of 0.  To retrieve the actual depth achieved after training, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_TREE_DEPTHS_ACHIEVED`](../../Reference/class/MclassGetResult.md). The depth achieved is always less than or equal to the maximum tree depth ([`M_TREE_MAX_DEPTH`](../../Reference/class/MclassControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies no maximum depth. |
| `Value >= 0` | Specifies the maximum depth. |

---

### `M_TREE_MAX_NUMBER_OF_LEAF_NODES`

Sets the maximum number of terminal nodes in the tree.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies no maximum. |
| `Value >= 1` | Specifies the maximum number of terminal nodes in the tree. |

### For a training result buffer (CNN, object detection, segmentation, or tree ensemble)

To control a general setting of a training result buffer, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In this case, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, object detection, segmentation, or tree ensemble training result buffer, unless otherwise specified, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_GENERAL`](../../Reference/class/MclassControl.md).

---

### `M_STOP_TRAIN`

Stops the current execution of [`MclassTrain`](../../Reference/class/MclassTrain.md). This can only be done from another thread of higher priority, or from a user-defined classification hook function.  If you stop training a CNN, object detection or segmentation classifier context, and 1 or more epochs are completed, results are typically available, except those related to development accuracy, training accuracy, development error rate, and training error rate. If you stop training a tree ensemble classifier context, and 1 or more trees are completed, all results are available.  If 1 or more epochs are completed, Aurora Imaging Library saves the trained classifier results that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces up to the point that you stop training. You can copy these results into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_UPDATE_TRAINED_PARAMS`

Updates the internal and hidden parameters (weights) of the classifier.  This only applies when training a CNN or segmentation classifier, which you must specify with the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NOW` *(default)* | Specifies to immediately update the training parameters network. |

### For a prediction result buffer (CNN, object detection, segmentation, tree ensemble, or ONNX) or a dataset (images or features)

To control a general setting of a prediction result buffer, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, object detection, segmentation, tree ensemble, or ONNX prediction result buffer, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_GENERAL`](../../Reference/class/MclassControl.md). Note, it is possible to specify [`M_STOP_PREDICT`](../../Reference/class/MclassControl.md) when calling [`MclassPredict`](../../Reference/class/MclassPredict.md) with a target dataset; this requires setting the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of the output dataset (images or features), and setting the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

---

### `M_STOP_PREDICT`

Stops the current execution of [`MclassPredict`](../../Reference/class/MclassPredict.md). This can only be done from another thread of higher priority, or from a user-defined classification hook function. If you stop the prediction, Aurora Imaging Library does not return any results.  Note, to stop the prediction of a target dataset, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of the output dataset (produced by [`MclassPredict`](../../Reference/class/MclassPredict.md)), and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For a prediction result buffer (object detection)

To control a general setting of a prediction result buffer, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of an object detection prediction result buffer, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_GENERAL`](../../Reference/class/MclassControl.md).

---

### `M_RESULT_OUTPUT_UNITS`

Sets which units and coordinate system to use for prediction results.  This setting only affects positional results within the Aurora Imaging Library Classification module.  > **Note:** Note, this setting can be changed at any time to get results in the required units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies to use world units if the target image is associated with a camera calibration context; otherwise, specifies to use pixel units. |
| `M_PIXEL` | Specifies to use pixel units with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to use world units with respect to the relative coordinate system. Using [`MclassGetResult`](../../Reference/class/MclassGetResult.md) will generate an error if the target image is not associated with a calibration context. |

### For a prediction or training result buffer (CNN, object detection, segmentation, tree ensemble, or ONNX)

To control a general setting of a prediction or training result buffer, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. Unless otherwise specified, set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a CNN, object detection, segmentation, or tree ensemble prediction or training result buffer, or an ONNX prediction result buffer, and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_GENERAL`](../../Reference/class/MclassControl.md).

---

### `M_RESET`

Removes all results from the classification result buffer.  Note that this does not delete the result buffer identifier, as is the case with [`MclassFree`](../../Reference/class/MclassFree.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

### For statistics classification

To control statistics classification, the [`ControlType`](../../Reference/class/MclassControl.md) and corresponding [`ControlValue`](../../Reference/class/MclassControl.md) parameter settings can be set to one of the following values. In addition, you can also specify [`M_TIMEOUT`](../../Reference/class/MclassControl.md).

---

### `M_MATCH_IOU_THRESHOLD`

Sets the IOU threshold for an object detection stat context, based on which predicted boxes are compared to their matched ground truth boxes. By comparing the IOU of each matched pair, the boxes are classified on the confusion matrix.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the IOU threshold. |

---

### `M_MODE`

Sets whether to calculate statistics for anomaly detection on an image level (default) or a pixel level. On an image level, the problem is an image classification one, whereas on the pixel level it is a segmentation problem.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMAGE_LEVEL` *(default)* | Specifies to calculate statistics for anomaly detection on an image level. This takes into account [`M_WHOLE_IMAGE`](../../Reference/class/MclassInquireEntry.md) regions to identify anomalous entries. For a given entry, if its [`M_WHOLE_IMAGE`](../../Reference/class/MclassInquireEntry.md) region has no [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md), it is considered non anomalous unless there exist at least one anomalous [`M_DESCRIPTOR_BASED`](../../Reference/class/MclassInquireEntry.md) region. |
| `M_PIXEL_LEVEL` | Specifies to calculate statistics for anomaly detection on a pixel level. This only takes into account [`M_DESCRIPTOR_TYPE_POLYGON`](../../Reference/class/MclassEntryAddRegion.md) and [`M_DESCRIPTOR_TYPE_MASK`](../../Reference/class/MclassEntryAddRegion.md) regions of the entry to identify anomalous pixels. |

---

### `M_SCORE_THRESHOLD_STEP`

Sets the step increment of threshold values from 0.0 to 100.0. Last threshold might be less than 100.0 if the step setting leads past it.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the step increment for threshold. |

---

### `M_STOP_CALCULATE`

Stops the current execution of [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). This can only be done from another thread of higher priority, or from a user-defined classification hook function. You cannot call [`MclassInquire`](../../Reference/class/MclassInquire.md) with [`M_STOP_CALCULATE`](../../Reference/class/MclassControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

This value only applies to an images dataset context.

You can automatically modify this setting, if you modify [`M_RESET_TRAINING_VALUES`](../../Reference/class/MclassControl.md).

This operation is not supported for binary (1-bit) images.

This operation is not supported for binary (1-bit) and grayscale (1-band) images.

> **Note:** To use this control, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a statistics classification context for object detection ([`M_STAT_DET`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

> **Note:** To use this control, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of a statistics classification context for anomaly detection ([`M_STAT_ANO`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

> **Note:** To use this control, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of any statistics classification context ([`M_STAT_ANO`](../../Reference/class/MclassAlloc.md), [`M_STAT_CNN`](../../Reference/class/MclassAlloc.md), [`M_STAT_DET`](../../Reference/class/MclassAlloc.md), [`M_STAT_SEG`](../../Reference/class/MclassAlloc.md), or [`M_STAT_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_CONTEXT`](../../Reference/class/MclassControl.md).

> **Note:** To use this control, you must set the [`ContextOrResultClassId`](../../Reference/class/MclassControl.md) parameter to the identifier of any statistics classification result buffer ([`M_STAT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_STAT_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_STAT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md)) and set the [`LabelOrIndex`](../../Reference/class/MclassControl.md) parameter to [`M_GENERAL`](../../Reference/class/MclassControl.md).
