---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Segmentation
section: Training_and_analysis_for_segmentation
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Coarse_segmentation / Training and analysis for segmentation"
---

# Training and analysis for segmentation

When you call [`MclassTrain`](../../Reference/class/MclassTrain.md), the classifier context will be trained with the settings specified in the training context, and trained on the data in the training and development datasets. The results from training will be stored in the classification result buffer. These must all be specified when you call [`MclassTrain`](../../Reference/class/MclassTrain.md).

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For more information on the training process, and training analysis, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md). |

## Hook functions

You can save time and improve the training process by using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) to hook a function to a training event. Call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md) to get information about the event that caused the hook-handler function to be called. Training can take a long time, and you can use hook functions to monitor the training process.

It is recommended that you call a hook function at the end of each mini-batch ([`M_MINI_BATCH_TRAINED`](../../Reference/class/MclassHookFunction.md)), and also at the end of each epoch ([`M_EPOCH_TRAINED`](../../Reference/class/MclassHookFunction.md)), to ensure that training is developing in the correct direction.

While your classifier is training, you can get the information from the events that caused the hook-handler function to execute by using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). Typically, you should expect to monitor the training process for proper convergence, and to make modifications to the process. You might need to abort or restart it, if required.

## Results

After training is completed, retrieve your training results by calling [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with the training result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced. You will often get results related to accuracy, such as the accuracy of the training dataset ([`M_TRAIN_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md)) and the development dataset ([`M_DEV_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md)). You can also get training results from a specific entry in either the training or development dataset by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md). To do this, you must first copy the dataset results from the result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces to a dataset using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md).

If your training results are not as you expected (for example, if the classifier is clearly performing poorly by over-fitting to the development dataset), then you can make adjustments and train again.

Copy the classification result buffer into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md) with [`M_TRAINED_CLASSIFIER`](../../Reference/class/MclassCopyResult.md). Once copied, the classifier context is considered trained. If necessary, you can always continue to adjust training settings and contexts, and call [`MclassTrain`](../../Reference/class/MclassTrain.md) with the trained classifier context.

> **Note:** In addition to retrieving results from a training (or prediction) result buffer using [`MclassGetResult`](../../Reference/class/MclassGetResult.md), you can retrieve results from a statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md). Although some results are similar (such as confusion matrix results), [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) provides more information, is specialized for your task, and helps you better gauge how well the trained classifier performs at prediction (information that you can use to re-train in a better way). To get results from a statistics classification result buffer, you must first perform a prediction on a dataset with the trained classifier, and then perform the statistics calculation, using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

## Training analysis

Analyzing your training results is critical to building a well-performing classifier.

### IOU

IOU (Intersection Over Union) is a metric for evaluating the performance a segmentation classifier. It compares the area and location of the predicted region to the ground truth region, relying on two concepts:

- Intersection is the area of overlap between the predicted and ground truth regions.
- Union is the total area covered by the combined predicted and ground truth regions.

Intersection and union are demonstrated in the following image:

*[Image: MclassIOUExample.png]*

The IOU is equal to the intersection divided by the union. In Aurora Imaging Library, this value is given as a percentage from 0 (the two regions have no overlap) to 100 (the two regions have complete overlap and are the same size). Ideally, the IOU should be as close to 100 as possible.

You can call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) to get the mean IOU after each epoch for the training and development datasets. The mean IOU can also be retrieved with [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md).

### Confusion matrix

For segmentation, the confusion matrix tells you how many pixels were correctly and incorrectly classified during training. It will include all pixels that are assigned to a region that is not [`M_NO_CLASS`](../../Reference/class/MclassControl.md) or [`M_DONT_CARE_CLASS`](../../Reference/class/MclassControl.md). The confusion matrix is available by calling [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_TRAIN_DATASET_CONFUSION_MATRIX`](../../Reference/class/MclassGetResult.md) or [`M_DEV_DATASET_CONFUSION_MATRIX`](../../Reference/class/MclassGetResult.md).

You can use the confusion matrix to identify classes that are poorly predicted in your datasets. The IOU can also be calculated explicitly from a confusion matrix.
