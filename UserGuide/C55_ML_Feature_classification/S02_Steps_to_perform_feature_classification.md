---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Steps_to_perform_feature_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Steps to perform feature classification"
---

# Steps to perform feature classification

The following steps provide a basic methodology to perform feature classification with the Aurora Imaging Library Classification module.

1. [Perform all required allocations](S02_Steps_to_perform_feature_classification.md).
2. [Build and populate your dataset](S02_Steps_to_perform_feature_classification.md).
3. [Train your classifier context](S02_Steps_to_perform_feature_classification.md).
4. [Predict with your classifier](S02_Steps_to_perform_feature_classification.md).
5. If necessary, [Save your classification contexts](S02_Steps_to_perform_feature_classification.md).
6. [Free your allocated objects](S02_Steps_to_perform_feature_classification.md).

## Perform all required allocations

These allocations are required to perform feature classification. In many cases, some of these allocations will be imported or restored from previous work, using [`MclassImport`](../../Reference/class/MclassImport.md) or [`MclassRestore`](../../Reference/class/MclassRestore.md).

1. Allocate a tree ensemble classifier context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). Note, [`MclassTrain`](../../Reference/class/MclassTrain.md) can automatically allocate a classifier context if none is specified.
2. Allocate a features dataset context to hold all of your data (source dataset), using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md). Alternatively, you can restore an features dataset context, using [`MclassRestore`](../../Reference/class/MclassRestore.md), or you can import data into a features dataset context, using [`MclassImport`](../../Reference/class/MclassImport.md).
3. Allocate a tree ensemble training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context.
4. Allocate a tree ensemble training result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).
5. Allocate a tree ensemble result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md).

Object allocation for feature classification is summarized in the table below:

| *[Image: MclassObject.png]* | Aurora Imaging Library allocation |
| --- | --- |
| Classifier context | [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md) |
| Specific predefined classifier context | None |
| Dataset context | [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) |
| Data preparation context | None |
| Training context | [`M_TRAIN_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md) |
| Training result buffer | [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md) |
| Prediction result buffer | [`M_PREDICT_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md) |

## Build and populate your dataset

If you restore or import a dataset that is fully built, you can skip this step. Otherwise, follow the steps below to build and populate the source dataset.

1. Add class definitions to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ADD`](../../Reference/class/MclassControl.md). Note, the number of classes with which to categorize your data is a key decision to make when performing feature classification.
   Optionally, you can specify settings to help manage class definitions, using [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can assign a color ([`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) and an icon image ([`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md)) to class definitions. This allows you to draw and visually identify them with [`MclassDraw`](../../Reference/class/MclassDraw.md).
2. Add entries to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_ENTRY_ADD`](../../Reference/class/MclassControl.md).
   Entries in a features dataset require feature data (numerical values in an array). To specify the entry's feature data, use [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_RAW_DATA`](../../Reference/class/MclassControlEntry.md).
3. For each entry, specify the class definition (ground truth) that is represented, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md). This step is known as labeling your data.
   After you build a dataset, you can export it using [`MclassExport`](../../Reference/class/MclassExport.md) so you can train a classifier with it at a later time. You can use [`MclassImport`](../../Reference/class/MclassImport.md) to import previously defined and exported datasets.
   > **Note:** You must label your data before training (labeled data is a prerequisite to using the module). The quality, quantity, and proportionality of correctly labeled data is critical to building a good dataset, and developing a properly trained classifier.
4. Optionally, create a development and testing dataset out of your source dataset using [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md). A development dataset is not required for the training of a tree ensemble classifier, and typically only the training dataset is used.
5. After you build a dataset, you can export it using [`MclassExport`](../../Reference/class/MclassExport.md) so you can train a classifier with it at a later time. You can use [`MclassImport`](../../Reference/class/MclassImport.md) to import previously defined and exported datasets.

## Train your classifier context

To train your classifier context on your datasets, perform the following:

1. Modify training settings, using [`MclassControl`](../../Reference/class/MclassControl.md) and [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md).
2. Optionally, hook functions to training events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).
3. Preprocess the training context, using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
4. Perform the training operation, using [`MclassTrain`](../../Reference/class/MclassTrain.md).
5. Optionally, get information about training events that caused the hook-handler function to execute, using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). If results indicate that the current training operation will be unsuccessful, stop the training, and modify your training settings and if necessary your dataset, and re-train.
6. Optionally, get training results, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). As indicated in the previous step, if results indicate an unsuccessful training, modify your training settings and if necessary your dataset, and re-train.
7. Copy the classification result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md). Once copied, the classifier context is considered trained.
8. If your training results are unsatisfactory, adjust training settings, contexts, and datasets as required, and call [`MclassTrain`](../../Reference/class/MclassTrain.md) with the trained classifier context.

## Predict with your classifier

To predict with the trained classifier context, perform the following:

1. Preprocess the trained classifier context, using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
2. Perform the prediction operation with the trained classifier context and the target data that you want to classify, using [`MclassPredict`](../../Reference/class/MclassPredict.md).
   Optionally, you can perform the prediction operation with a dataset as your target. In this case, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).
   If you are predicting with a dataset (for example, a test dataset) to evaluate the performance of your trained classifier, and the predicted classes are not what you expect, when compared to the actual classes in the dataset (the ground truth), you can adjust your training setup, and continue the training process, to improve your classifier's performance. To help you understand how you can improve your classifier, you can calculate and retrieve statistical information about it. For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).
3. Retrieve the required results from the prediction result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md).

## Save your classification contexts

If necessary, save your classification contexts, using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).

## Free your allocated objects

Free all your allocated objects, using [`MclassFree`](../../Reference/class/MclassFree.md), unless [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified during allocation.
