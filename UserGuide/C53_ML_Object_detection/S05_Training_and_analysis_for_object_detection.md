---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Object_detection
section: Training_and_analysis_for_object_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Object_detection / Training and analysis for object detection"
---

# Training and analysis for object detection

When you call [`MclassTrain`](../../Reference/class/MclassTrain.md), the classifier context will be trained with the settings specified in the training context, and trained on the data in the training and development datasets. The results from training will be stored in the classification result buffer. These must all be specified when you call [`MclassTrain`](../../Reference/class/MclassTrain.md).

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on the training process, and training analysis, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md). |

## Monitoring the training process

You can save time and improve the training process by using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) to hook a function to a training event. Call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md) to get information about the event that caused the hook-handler function to be called. Training can take a long time, and you can use hook functions to monitor the training process.

It is recommended that you call a hook function at the end of each mini-batch ([`M_MINI_BATCH_TRAINED`](../../Reference/class/MclassHookFunction.md)), and also at the end of each epoch ([`M_EPOCH_TRAINED`](../../Reference/class/MclassHookFunction.md)), to ensure that training is developing in the correct direction.

While your classifier is training, you can get the information from the events that caused the hook-handler function to execute by using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). Typically, you should expect to monitor the training process for proper convergence, and to make modifications to the process. You might need to abort or restart it, if required. You can monitor the loss metric of the development dataset during training by using [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_DEV_DATASET_EPOCH_LOSS`](../../Reference/class/MclassGetResult.md). You can also monitor the loss metric of the training dataset using, [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_TRAIN_DATASET_MINI_BATCH_LOSS`](../../Reference/class/MclassGetResult.md).

> **Note:** In addition to retrieving results from a training (or prediction) result buffer using [`MclassGetResult`](../../Reference/class/MclassGetResult.md), you can retrieve results from a statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md). Although some results are similar (such as confusion matrix results), [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) provides more information, is specialized for your task, and helps you better gauge how well the trained classifier performs at prediction (information that you can use to re-train in a better way). To get results from a statistics classification result buffer, you must first perform a prediction on a dataset with the trained classifier, and then perform the statistics calculation, using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).
