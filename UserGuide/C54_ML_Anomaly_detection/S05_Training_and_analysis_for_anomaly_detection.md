---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Training_and_analysis_for_anomaly_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Training and analysis for anomaly detection"
---

# Training and analysis for anomaly detection

When you call [`MclassTrain`](../../Reference/class/MclassTrain.md), the classifier context will be trained with the settings specified in the training context, and trained on the data in the training and development datasets. The results from training will be stored in the classification result buffer. These must all be specified when you call [`MclassTrain`](../../Reference/class/MclassTrain.md).

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on the training process, and training analysis, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md). |

## Monitoring the training process

You can save time and improve the training process by using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) to hook a function to a training event, such as [`M_SAMPLE_ADDED`](../../Reference/class/MclassHookFunction.md).

While your classifier is training, you can get the information from the events that caused the hook-handler function to execute by using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). Training can take a long time, and you can use hook functions to monitor the training process.

For most machine learning tasks, you should typically expect to monitor the training process for proper convergence, and to make modifications to the process. You might need to abort or restart it, if required. Anomaly detection, however, does not converge in the same way. The sample loss is essentially guaranteed to go down with each sample added. It is likely that you probably want a sample's fixed size that is large enough for the sample loss to almost plateau (note, however, that it will not actually plateau).

## Results

After the training process is done, you can retrieve training results using [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with the training result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced. For example, you can retrieve which images in the development dataset ([`M_DEV_DATASET_USED_ENTRIES`](../../Reference/class/MclassGetResult.md)) and training dataset ([`M_TRAIN_DATASET_USED_ENTRIES`](../../Reference/class/MclassGetResult.md)) were used during training. To retrieve the status of the training, use [`M_STATUS`](../../Reference/class/MclassGetResult.md).

To evaluate whether your trained classifier is actually able to produce proper prediction results, you must predict with it. This is typically done on your test dataset and, if the prediction results are not what you expect (such as, images or pixels incorrectly predicted as anomalous/non-anomalous), you can adjust the score threshold ([`M_SCORE_THRESHOLD`](../../Reference/class/MclassGetResult.md)) and predict again. In addition to evaluating prediction results (for example, [`M_IMAGE_PREDICTED_ANOMALOUS`](../../Reference/class/MclassGetResult.md) and [`M_MASK_IMAGE`](../../Reference/class/MclassGetResult.md)), you can also draw results using [`MclassDraw`](../../Reference/class/MclassDraw.md) to visually inspect them (for example, [`M_DRAW_ANOMALY_HEATMAP`](../../Reference/class/MclassDraw.md) and [`M_DRAW_ANOMALY_MASK`](../../Reference/class/MclassDraw.md)).

As previously discussed, you can use [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) and [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) to determine if there is a threshold you can set that meets the performance requirements. For more information, see [Evaluate your trained classifier](S02_Steps_to_perform_anomaly_detection.md).
