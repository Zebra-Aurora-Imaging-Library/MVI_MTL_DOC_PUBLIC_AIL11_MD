---
doctype: Reference
module: class
function: MclassHookFunction
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassHookFunction"
---

# MclassHookFunction

> Hook a function to an event that occurs using classification objects.

## Syntax

```c
void MclassHookFunction(
    AIL_ID                      ClassId,         //in
    AIL_INT64                   HookType,        //in
    AIL_CLASS_HOOK_FUNCTION_PTR HookHandlerPtr,  //in
    void *                      UserDataPtr      //in-out
)
```

## Description

This function allows you to attach or detach a user-defined function to a classification object event (for example, during the execution of [`MclassTrain`](../../Reference/class/MclassTrain.md) or [`MclassPredict`](../../Reference/class/MclassPredict.md)). Once a hook-handler function is defined and hooked to an event, it is automatically called when the event occurs.

You can hook more than one function to an event by making separate calls to [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) for each function that you want to hook. Aurora Imaging Library automatically chains and keeps an internal list of all these hooked functions. When a function is hooked, this new function is added to the end of the list. When the event happens, all user-defined functions in the list will be executed in the same order that they were hooked to the event. You can also remove any function from the list; in this case, Aurora Imaging Library preserves the order of the remaining functions in the list.

You can obtain information concerning the event, using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md), and take appropriate action before returning control to the application.

## Parameters

### `ClassId` *(in, AIL_ID)*

Specifies the identifier of the classification object onto which to attach a hook-handler. Set this parameter to one of the following values.

*For specifying the identifier of the classification object*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of a predefined CNN, predefined object detection, predefined segmentation, predefined anomaly detection, or tree ensemble classifier context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_CNN_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_DET_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_SEG_PREDEFINED`](../../Reference/class/MclassAlloc.md), [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md) or [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `ContextDataPreparationId` | Specifies the identifier of a data preparation context. This context must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) or [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md). |
| `ContextDatasetId` | Specifies the identifier of an images or features dataset context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). |
| `ContextTrainId` | Specifies the identifier of a CNN, object detection, segmentation, anomaly detection, or tree ensemble training context. These contexts must have been previously allocated on the required system using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_CNN`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_DET`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_SEG`](../../Reference/class/MclassAlloc.md), [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md) or [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |

### `HookType` *(in, AIL_INT64)*

Specifies the event type. Set this parameter to one of the following values.

*For specifying the event type*

| Value | Description |
| --- | --- |
| `M_DATASETS_PREPARED` | Calls the hook-handler function when the training and development datasets are prepared and available in the training results.

You can use [`M_DATASETS_PREPARED`](../../Reference/class/MclassHookFunction.md) with a training context when training a CNN, object detection, segmentation, or anomaly detection classifier ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_EPOCH_TRAINED` | Calls the hook-handler function each time the training process is completed for an epoch. This is typically used to plot a graph of the training process.

You can use [`M_EPOCH_TRAINED`](../../Reference/class/MclassHookFunction.md) with a training context when training a CNN, object detection, or segmentation classifier ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_MINI_BATCH_TRAINED` | Calls the hook-handler function each time the training process is completed for a given mini-batch within the current epoch.

You can use [`M_MINI_BATCH_TRAINED`](../../Reference/class/MclassHookFunction.md) with a training context when training a CNN, object detection, segmentation, or anomaly detection classifier ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_PREDICT_ENTRY` | Calls the hook-handler function each time the prediction is completed for a dataset entry.

You can use [`M_PREDICT_ENTRY`](../../Reference/class/MclassHookFunction.md) with a classifier context when performing a prediction with a trained CNN, object detection, segmentation, anomaly detection, or tree ensemble classifier and an images or features dataset ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_PREPARE_ENTRY_POST` | Calls the hook-handler function after each entry is prepared. This lets you track the progress of the data preparation, and how close it is to completing. The data preparation operation is done when the number of prepared entries is equal to the number of entries in the source dataset. It is possible that some source images (entries) are not used; to know which, access the status of the previous preparation.

You can use [`M_PREPARE_ENTRY_POST`](../../Reference/class/MclassHookFunction.md) when preparing data with a data preparation context ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_PREPARE_ENTRY_PRE` | Calls the hook-handler function before each entry is prepared. Set this hook type when you want to use a custom algorithm to determine the center around which to crop your images. That is, you are given the identifier of the image buffer, which you can then use in a customized way to select the center of the image, and then set that center with [`MclassSetHookInfo`](../../Reference/class/MclassSetHookInfo.md).

You can use [`M_PREPARE_ENTRY_PRE`](../../Reference/class/MclassHookFunction.md) when preparing data with a data preparation context ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_SAMPLE_ADDED` | Calls the hook-handler function each time the training process is completed for a given sample.

You can use [`M_SAMPLE_ADDED`](../../Reference/class/MclassHookFunction.md) with a training context when training an anomaly detection classifier ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_TREE_TRAINED` | Calls the hook-handler function each time the training process is completed for a tree.

You can use [`M_TREE_TRAINED`](../../Reference/class/MclassHookFunction.md) with a training context when training a tree ensemble classifier ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |
| `M_WRITE_NEW_FILE` | Calls the hook-handler function each time a file is written to disk when certain Aurora Imaging Library functions are called (for example, [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md) and [`MclassPredict`](../../Reference/class/MclassPredict.md) with a segmentation classifier context).

You can use [`M_WRITE_NEW_FILE`](../../Reference/class/MclassHookFunction.md) with an images dataset context ([`ClassId`](../../Reference/class/MclassHookFunction.md)). |

*For the HookType parameter*

| Value | Description |
| --- | --- |
| `M_UNHOOK` | Unhooks the specified function if hooked to the specified event. When you use [`M_UNHOOK`](../../Reference/class/MclassHookFunction.md), you must provide the same values for all the parameters as when you originally hooked the function. |

### `HookHandlerPtr` *(in, AIL_CLASS_HOOK_FUNCTION_PTR)*

Specifies the address of the function that should be called when an event occurs.

### `UserDataPtr` *(in-out, *void)*

Specifies the address of the user data that you want to make available to the hook-handler function. This address is passed to the hook-handler function, through its [`UserDataPtr`](../../Reference/class/MclassHookFunction.md) parameter, when the specified event occurs. Set this parameter to `M_NULL` if user data is not required.
