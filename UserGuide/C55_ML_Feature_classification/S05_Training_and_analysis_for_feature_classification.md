---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Training_and_analysis_for_feature_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Training and analysis for feature classification"
---

# Training and analysis for feature classification

Once you have established your training related settings, your classifier can be trained. For more information on the training process, and training analysis, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

## Hook functions

You can save time and improve the training process by using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md) to hook a function to a training event. Call [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md) to get information about the event that caused the hook-handler function to be called. Training can take a long time, and you can use hook functions to monitor the training process.

## Train your classifier

When you call [`MclassTrain`](../../Reference/class/MclassTrain.md), the classifier context will be trained with the settings specified in the training context, and trained using the data in the training dataset and development dataset, if one is specified. The results from training will be stored in the classification result buffer. These must all be specified when you call [`MclassTrain`](../../Reference/class/MclassTrain.md). If you are not using a development dataset, set the [`DevDatasetContextClassId`](../../Reference/class/MclassTrain.md) to [`M_NULL`](../../Reference/class/MclassTrain.md). Aurora Imaging Library will not automatically split your dataset into a development dataset when training a tree ensemble classifier. This must be done explicitly with [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md).

While your classifier is training, you can get the information from the events that caused the hook-handler function to execute by using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). Typically, you should expect to monitor the training process for proper convergence, and to make modifications to the process. You might need to abort or restart it, if required.

## Results

After training is completed, retrieve your training results by calling [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with the training result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced. You will often get results related to accuracy, such as the accuracy of the training dataset ([`M_TRAIN_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md)), the development dataset ([`M_DEV_DATASET_ACCURACY`](../../Reference/class/MclassGetResult.md)) and the out-of-bag set ([`M_OUT_OF_BAG_ACCURACY`](../../Reference/class/MclassGetResult.md)). The out-of-bag accuracy represents the generalizability of the classifier on data not seen during training. You can also get training results from a specific entry in the training, development, or out-of-bag dataset by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md). To do this, you must first copy the dataset results from the result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produces to a dataset using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md).

If your training results are not as you expected (for example, if the classifier is clearly performing poorly by over-fitting to the development dataset), then you can make adjustments and train again.

Copy the classification result buffer into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md) with [`M_TRAINED_CLASSIFIER`](../../Reference/class/MclassCopyResult.md). Once copied, the classifier context is considered trained. If necessary, you can always continue to adjust training settings and contexts, and call [`MclassTrain`](../../Reference/class/MclassTrain.md) with the trained classifier context.

> **Note:** In addition to retrieving results from a training (or prediction) result buffer using [`MclassGetResult`](../../Reference/class/MclassGetResult.md), you can retrieve results from a statistics classification result buffer, using [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md). Although some results are similar (such as confusion matrix results), [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md) provides more information, is specialized for your task, and helps you better gauge how well the trained classifier performs at prediction (information that you can use to re-train in a better way). To get results from a statistics classification result buffer, you must first perform a prediction on a dataset with the trained classifier, and then perform the statistics calculation, using [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md). For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

## Training analysis

Analyzing your training results is critical to building a well-performing classifier. Based on the result information, you can modify your dataset and set of features and retrain, which can help improve the tree ensemble classifier's performance and accuracy.

### Confusion matrix

A confusion matrix tells you information about how many entries were correctly and incorrectly classified during training in the training or development dataset. It can be used to identify weaknesses in your classifier, particularly when dealing with an unbalanced dataset.

To retrieve the confusion matrix, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_TRAIN_DATASET_CONFUSION_MATRIX`](../../Reference/class/MclassGetResult.md), [`M_DEV_DATASET_CONFUSION_MATRIX`](../../Reference/class/MclassGetResult.md), or [`M_OUT_OF_BAG_CONFUSION_MATRIX`](../../Reference/class/MclassGetResult.md). Confusion matrix results are also available from a statistics classification result buffer ([`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md)); for more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

### Feature importance

To retrieve the importance of features, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_FEATURE_IMPORTANCE`](../../Reference/class/MclassGetResult.md).

### Proximity matrix

To retrieve the proximity measure matrix, call [`MclassGetResult`](../../Reference/class/MclassGetResult.md) with [`M_PROXIMITY_MATRIX`](../../Reference/class/MclassGetResult.md).
