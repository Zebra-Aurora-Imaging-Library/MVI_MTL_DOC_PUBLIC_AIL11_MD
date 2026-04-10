---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Steps_to_perform_anomaly_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Steps to perform anomaly detection"
---

# Steps to perform anomaly detection

The following steps provide a basic methodology to perform anomaly detection with the Aurora Imaging Library Classification module:

1. [Perform all required allocations](S02_Steps_to_perform_anomaly_detection.md).
2. [Build and populate your dataset](S02_Steps_to_perform_anomaly_detection.md).
3. [Train your classifier context](S02_Steps_to_perform_anomaly_detection.md).
4. [Evaluate your trained classifier](S02_Steps_to_perform_anomaly_detection.md) (this includes [adjusting the threshold and calculating statistics](S02_Steps_to_perform_anomaly_detection.md)).
5. [Predict with your classifier](S02_Steps_to_perform_anomaly_detection.md).
6. If necessary, [save your classification contexts](S02_Steps_to_perform_anomaly_detection.md).
7. [Free your allocated objects](S02_Steps_to_perform_anomaly_detection.md).

## Perform all required allocations

These allocations are required to perform anomaly detection. In many cases, some of these allocations will be imported or restored from previous work, using [`MclassImport`](../../Reference/class/MclassImport.md) or [`MclassRestore`](../../Reference/class/MclassRestore.md).

1. Allocate an anomaly detection classifier context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md) and [`M_ADNET`](../../Reference/class/MclassAlloc.md). Note, [`MclassTrain`](../../Reference/class/MclassTrain.md) can automatically allocate a classifier context if none is specified.
2. Allocate an images dataset context to hold all of your data (source dataset), using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md). Alternatively, you can restore an images dataset context, using [`MclassRestore`](../../Reference/class/MclassRestore.md), or you can import data into an images dataset context, using [`MclassImport`](../../Reference/class/MclassImport.md).
3. Allocate an anomaly detection training context, using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md). A training context holds the settings with which to train a classifier context.
4. Allocate an anomaly detection training result buffer to hold training results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md).
5. Allocate an anomaly detection result buffer to hold the prediction results, using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md).

Object allocation for anomaly detection is summarized in the table below:

| *[Image: MclassObject.png]* | Aurora Imaging Library allocation |
| --- | --- |
| Classifier context | [`M_CLASSIFIER_ANO_PREDEFINED`](../../Reference/class/MclassAlloc.md) |
| Specific predefined classifier context | [`M_ADNET`](../../Reference/class/MclassAlloc.md) |
| Dataset context | [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md) |
| Data preparation context | None¹ |
| Training context | [`M_TRAIN_ANO`](../../Reference/class/MclassAlloc.md) |
| Training result buffer | [`M_TRAIN_ANO_RESULT`](../../Reference/class/MclassAllocResult.md) |
| Prediction result buffer | [`M_PREDICT_ANO_RESULT`](../../Reference/class/MclassAllocResult.md) |
| ¹There is no dedicated data preparation for anomaly detection. However it can be possible to use [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md), or [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) to prepare your data. In this case, it is important to select the data preparation context that is consistent with your labeling. |

## Build and populate your dataset

It is recommended that you build and manage your datasets interactively, using Aurora Imaging CoPilot. For example, Aurora Imaging CoPilot lets you interactively create, label, modify, import, and export datasets. When using Aurora Imaging CoPilot, ensure that you have installed all related Aurora Imaging Library updates. For more information, see [Requirements, recommendations, and troubleshooting](../C47_ML_with_the_MIL_Classification_module/S06_Requirements_recommendations_and_troubleshooting.md).

If you restore or import a dataset that is fully built, you can skip this step. If you are not importing a dataset, or are importing a dataset that you want to adjust, refer to the steps below:

1. Add class definitions to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ADD`](../../Reference/class/MclassControl.md). By default, every dataset entry is considered good (that is, not anomalous). Note that no class definitions are required for training and any entry not assigned to a class is considered good.
   To specify class definitions as anomalous, call [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_INDEX()`](../../Reference/class/MclassControl.md) and [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) set to [`M_TRUE`](../../Reference/class/MclassControl.md). If you specify anomalous classes in datasets that are used during training, you must not reference any of these classes or you will get an error; for example, during training, you must ensure that no entry image, nor any of the image regions in any of the entries, are assigned to a class index for which you set [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) to [`M_TRUE`](../../Reference/class/MclassControl.md). Note, you can actually assign anomalous classes in datasets you are using to validate your trained classifier with a prediction.
2. Add entries to the source images dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_ENTRY_ADD`](../../Reference/class/MclassControl.md).
   Entries in an images dataset require image data (paths to where images are stored). To specify the location from which to get an entry's image data, use [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md).
3. Optionally, specify the class definition (ground truth) that is represented for each entry, using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md). Also, you can optionally pixel-level label your test dataset, so you can compute pixel-level statistics on it ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)).
   If you do not specify a class definition, the training assumes that the corresponding image is not anomalous. Pre-existing datasets, such as those created for image classification, segmentation, and object detection can be used without having to alter them, or only specifying a class index as anomalous (for the test dataset).
   This step is known as labeling your data. For anomaly detection, this type of labeling is not required for training and, even when it is done (for testing), it is typically simpler than other machine learning tasks, since you would only need to explicitly label the anomalous entries (which is optional).
   > **Note:** You must properly set up your data before training. The quality and quantity of accurate data that is representative of the problem you are trying to solve is critical to building a good dataset. For anomaly detection, it is essential that every image in your development and training dataset is a good image. Specifying one or more anomalous images during training can result in an improperly trained classifier. Since anomaly detection considers every image is good by default, be especially careful to not inadvertently include anomalous training data.
4. Split your source dataset into the training, development, and testing datasets. You can split datasets manually, using [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md). Anomaly detection requires an explicit training and development dataset (that is, you cannot pass a single dataset to [`MclassTrain`](../../Reference/class/MclassTrain.md)).
   When splitting a dataset with anomalous entries, all anomalous entries will be in the second destination dataset ([`DstSecondDatasetContextClassId`](../../Reference/class/MclassSplitDataset.md)). In this case, the specified percentage ([`Percentage`](../../Reference/class/MclassSplitDataset.md)) controls what percentage of good (non-anomalous) entries go into the first and second destination datasets.
5. Optionally, prepare your data by cropping or resizing images, and adding augmented images to a dataset. As with other machine learning tasks, all images must be the same size for anomaly detection. Using smaller images, provided they have enough details for an accurate analysis, can result in faster training and prediction (inference), and can reduce your memory requirements for training. To resize, crop, or augment your images, you can use [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md).
   Note that there is no dedicated data preparation for anomaly detection. However it is possible to use [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md), [`M_PREPARE_IMAGES_SEG`](../../Reference/class/MclassAlloc.md), or [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md) to prepare your data with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) (or Aurora Imaging CoPilot). In this case, it is important to select the data preparation context that is consistent with your labeling. After calling [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md), the resulting images in the destination dataset or buffer can be used for anomaly detection.
6. After you build a dataset, you can export it using [`MclassExport`](../../Reference/class/MclassExport.md) so you can train a classifier with it at a later time. It is recommended that you export it with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md). You can use [`MclassImport`](../../Reference/class/MclassImport.md) to import previously defined and exported datasets (for example, from a folder or a CSV file). If you have exported your dataset with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md), then you should import it with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) as well.
   > **Note:** It is recommended that you familiarize yourself with how dataset information (such as images) is stored on disk, and how you can ensure that information is well organized and portable. For more information, see [Guidelines for managing an images dataset](../C48_ML_Datasets/S07_Guidelines_for_managing_an_images_dataset.md).

## Train your classifier context

To train your classifier context on your datasets, perform the following:

1. Modify training settings, using [`MclassControl`](../../Reference/class/MclassControl.md).
2. Optionally, hook functions to training events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).
3. Preprocess the training context, using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
4. Perform the training operation, using [`MclassTrain`](../../Reference/class/MclassTrain.md).
5. Optionally, get information about training events that caused the hook-handler function to execute, using [`MclassGetHookInfo`](../../Reference/class/MclassGetHookInfo.md). If results indicate that the current training operation will be unsuccessful, stop the training, and modify your training settings and if necessary your dataset, and re-train.
6. Get the status result of your training ([`M_STATUS`](../../Reference/class/MclassGetResult.md)), as well as other training results, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md).
   For other types of training tasks, you would typically evaluate your training results; if you are unsatisfied with them (for example, the classifier performed poorly on the development dataset), you would modify your training settings, and if necessary your dataset, and re-train. For anomaly detection, this type of analysis is done with the trained classifier and [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md), as explained later.
7. Copy the trained classifier context from the train result buffer that [`MclassTrain`](../../Reference/class/MclassTrain.md) produced into a classifier context, using [`MclassCopyResult`](../../Reference/class/MclassCopyResult.md). Once copied, the classifier context is considered trained.

## Evaluate your trained classifier

To evaluate your trained classifier, perform the following:

1. Call [`MclassPredict`](../../Reference/class/MclassPredict.md) with the test dataset (which must contain anomalous entries) and the trained classifier. If you get the prediction results you expect (for example, all anomalous images/pixels are predicted and with good scores), and there are no good images/pixels incorrectly predicted as anomalous, it is possible to deploy.
2. If your results are unsatisfactory, you can adjust the trained threshold, by calling [`MclassControl`](../../Reference/class/MclassControl.md) with the identifier of the trained classifier context, and lowering or increasing the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) setting. Lowering the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) value can allow you to detect more anomalous cases, but at the potential cost of incorrectly detecting good/non-anomalous images as anomalous ones. Increasing the [`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md) value can decrease the likelihood of incorrectly detecting good/non-anomalous cases as anomalous, but at the potential cost of missing actual anomalous ones. For more information, see [Adjusting the trained threshold and calculating statistics](S06_Adjusting_the_trained_threshold_and_calculating_statistics.md).
3. Once you modified the trained threshold, call [`MclassPredict`](../../Reference/class/MclassPredict.md) again with the test dataset and the trained classifier. You can repeat this process of adjusting the threshold and predicting on the test dataset until you get the results you expect (at which point, you can deploy).
4. If you cannot get the prediction results you expect from your threshold adjustments, you can use [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) (along with [`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md)) to determine if there is a threshold you can set that meets the performance requirements.
   [`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md) performs a high level statistical analysis that can help guide your understanding of why you are not getting the training results you want, and help you find the most optimal threshold value to set (that is, you can retrieve calculated statistics across different thresholds to find the best one). For more information, see [Threshold and statistics](S02_Steps_to_perform_anomaly_detection.md).
   If you are unable to find a satisfactory threshold once you complete this analysis, you will likely have to adjust your training. For example, you might need to add more images to your training and development datasets, and retrain. You can also try adjusting the [`M_SAMPLES_FIXED_SIZE`](../../Reference/class/MclassControl.md) control value (increasing this value can improve results for more complex cases/images).

### Threshold and statistics

To help determine how different an image can be, before it is considered anomalous, Aurora Imaging Library automatically establishes a threshold for the trained classifier, which you can modify (fine tune) to help fit your application's needs. The threshold level is based on score, and by increasing or decreasing it (for example, with a test dataset), you can influence which images are or are not considered anomalous.

Fine tuning the threshold is done post training, and is done in such a way that it results in preferable statistics. Since you can retrieve ([`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md)) the calculated statistics ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)) across different thresholds ([`M_SCORE_THRESHOLD`](../../Reference/class/MclassControl.md)), you can use this methodology to find the ideal threshold that meets your requirements. Note that the default threshold has been selected to minimize false positives (good images predicted as anomalous) as the anomalous detection task is typically performed in scenarios where anomalous images are difficult to acquire.

If you find that, after adjusting the threshold and calculating statistics, the classifier still keeps detecting many false positives, you will likely need more (and more diverse) training images. Similarly, if the classifier keeps detecting many false negatives (anomalous images being predicted as good), there is likely a resolution issue with the images; for example, the defects are too small or you potentially have bad images in your training or development dataset. A high number of false positives or false negatives requires updating your datasets and retraining.

Calculating statistics helps you evaluate the performance of a trained classifier and using those statistics to fine tune the threshold is typically done when the threshold that was automatically established during training is not working as well as you require. You must have some anomalous images (in the test dataset) to realistically adjust the threshold and calculate statistics. Calculating statistics can also shed some light on other improvements that you can make at training.

For more information about how to fine tune the threshold and calculate statistics, see [Adjusting the trained threshold and calculating statistics](S06_Adjusting_the_trained_threshold_and_calculating_statistics.md).

## Predict with your classifier

To predict with the trained classifier context, perform the following:

1. Preprocess the trained classifier context, using [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
2. Perform the prediction operation with the trained classifier context and the target data that you want to classify (for example, target images), using [`MclassPredict`](../../Reference/class/MclassPredict.md).
   Optionally, you can perform the prediction operation with a dataset as your target. In this case, you can hook functions to prediction events, using [`MclassHookFunction`](../../Reference/class/MclassHookFunction.md).
   As previously discussed, if you are predicting with a dataset (for example, a test dataset) to evaluate the performance of your trained classifier, and the predicted classes are not what you expect, when compared to what is labeled/not labeled anomalous in the dataset (the test dataset should have some anomalous images to properly evaluate the trained classifier), you can adjust your training setup, and continue the training process, to improve your classifier's performance. To help you understand how you can improve your classifier, you can calculate and retrieve statistical information about it. For more information, see [Calculating and retrieving statistics on a trained classifier](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).
3. Retrieve the required results from the prediction result buffer, using [`MclassGetResult`](../../Reference/class/MclassGetResult.md). You can also draw prediction results, using [`MclassDraw`](../../Reference/class/MclassDraw.md).

## Save your classification contexts

If necessary, save your classification contexts, using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).

## Free your allocated objects

Free all your allocated objects, using [`MclassFree`](../../Reference/class/MclassFree.md), unless [`M_UNIQUE_ID`](../../Reference/class/MclassAlloc.md) was specified during allocation.
