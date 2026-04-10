---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Different_datasets_and_how_they_are_split
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Different datasets and how they are split"
---

# Different datasets and how they are split

The Aurora Imaging Library Classification module makes use of the following datasets:

- Source dataset (sometimes known as the working dataset).
- Training dataset.
- Development dataset (not needed and usually not used for feature classification).
- Testing dataset.

What these datasets are, and how they established (split) and used, is described below.

## Source dataset

Your source dataset should hold all the data with which to train your classifier. This dataset typically includes data that you explicitly collect, and can also included imported data. It is important that this dataset contains only unique images (no duplicates) and does not contain augmented images.

Given enough time, the training accuracy of a sufficiently large classifier that uses a single source dataset eventually converges to an accuracy of 100% (0% error). However, this does not mean that the training is successful. Several sources of errors can limit and improperly bias the performance of a classifier that you trained using a single dataset to the point where, if you use the trained classifier with similar but different data (images), it will almost surely fail.

Even though it is recommended that you specify a single source dataset when supported for your task, internally, Aurora Imaging Library splits that dataset into the training dataset and the development dataset, as is typically required by the theoretical principles of deep learning classification. In so doing, the classifier not only learns to classify a specific set of images (the training dataset), but also learns the general principles with which to classify the images so you can use it to successfully classify images with which it did not train (the development dataset). You can also have a [testing dataset](S04_Different_datasets_and_how_they_are_split.md) and use it with [`MclassPredict`](../../Reference/class/MclassPredict.md) to serve as a quarantined final check for any trained classifier. For anomaly detection, the testing dataset is the only place you would specify anomalous images.

### Splitting data

You can call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a dataset into 2 smaller datasets. You can do this to create a training, development, or testing dataset out of your source dataset. Alternatively, and if supported for your machine learning task, you can specify a single dataset and let [`MclassTrain`](../../Reference/class/MclassTrain.md) split it into the training and development datasets, as per the related split control settings, such as [`M_SPLIT_PERCENTAGE`](../../Reference/class/MclassControl.md) and [`M_SPLIT_SEED_MODE`](../../Reference/class/MclassControl.md) (this is not done for feature classification, since only the training dataset is needed). If you want to use a testing dataset, then you should first split a portion of the source dataset into a testing dataset by calling [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md).

Keep in mind that, regardless of how you create your dataset contexts, the data in each of them must come from the final imaging setup (for example, you should capture all training images in all datasets using the same kind of resolution and illumination conditions, which should in turn use the same kind of resolution and illumination conditions at prediction time).

> **Note:** Only the training dataset can contain augmented data; the development dataset and the testing dataset must not contain augmentations. You must manually split your dataset if you want to apply custom augmentations to your training dataset. For more information, see [Data augmentation and other data preparations](S05_Data_augmentation_and_other_data_preparations.md).

## Training dataset

The training dataset context holds the entries that update the internal parameters (weights) of the classifier.

In general, it is recommended that the training dataset holds about 70% of your source data, unless otherwise specified. If your source dataset is significantly large, you can increase this percentage to 80% or 90%. As previously discussed, when using tree ensemble classifiers, your training dataset typically holds 100% of your data (unless you want to use some for the testing dataset).

## Development dataset

The development dataset is generally used to evaluate the performance of the classifier on data that is not involved in establishing the classifier's internal parameters (weights). This is done at the end of each epoch, if your machine learning task uses epochs.

Once training is complete, and depending on the machine learning task, an analysis of the training metrics can indicate several actions to take before training once more to obtain better results. In some cases, you can analyze training metrics at the end of an epoch to decide whether to continue or abort training. For example, for segmentation, you could use results ([`MclassGetResult`](../../Reference/class/MclassGetResult.md)) related to intersection over union (such as [`M_DEV_DATASET_EPOCH_IOU_MEAN`](../../Reference/class/MclassGetResult.md) and [`M_TRAIN_DATASET_EPOCH_IOU_MEAN`](../../Reference/class/MclassGetResult.md)), and for object detection, you could use results related to bounding box coordinates (such as [`M_BOX_4_CORNERS`](../../Reference/class/MclassGetResult.md)). For more information, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

In general, it is recommended that the development dataset holds about 10% to 30% of your source data. The development dataset and the testing dataset, if you have one, typically have the same percentage. For example, if your training dataset has 70% of your data, then the development dataset and testing dataset would each have 15%. For anomaly detection, it is recommended to split your source data equally among your datasets; for example, the training dataset, the development dataset, and the testing dataset would each have 33% of the source data.

## Testing dataset

The testing dataset contains labeled data that resembles the training data, but was completely unseen by the training process. You can use it to help ensure that your classifier is free from any bias it might have inadvertently learned, and as a way to validate the final performance of the trained classifier. Testing data is initially separated from the source dataset to populate a testing dataset.

To use the testing dataset, you must pass it to [`MclassPredict`](../../Reference/class/MclassPredict.md), and compare the predicted class to the ground truth class. The accuracy of prediction results from a properly trained classifier should be overwhelmingly similar to the accuracy of training results. In general, the testing dataset is about the same size as the development dataset.

To help confirm a successful training from predicting on the test dataset, and to understand why training was not as successful as you wanted, you can also calculate classification statistics ([`MclassStatCalculate`](../../Reference/class/MclassStatCalculate.md)) and retrieve advanced results from a statistics classification result buffer ([`MclassGetResultStat`](../../Reference/class/MclassGetResultStat.md)). For example, you can get the false discovery rate of a single class versus other classes ([`M_FDR`](../../Reference/class/MclassGetResultStat.md)), and the false negative rate of a single class versus other classes ([`M_FNR`](../../Reference/class/MclassGetResultStat.md)). Advanced statistics are available for all machine learning tasks.

For more information about understanding the state of a trained classifier, see [Analysis, adjustment, and additional settings](../C49_ML_Training/S04_Analysis_adjustment_and_additional_settings.md).

## An example of distributing data among datasets

The following is an example of how data is typically distributed among datasets containing image data (for image classification, segmentation, or object detection). Note, 30% of the data in class A, B, and C is split among the development and testing datasets (this amounts to 15% for each class in each of these datasets). Anomaly detection is similar, with the exception that there is only 1 class (non-anomalous) for the training and development datasets.

*[Image: MclassTheoreticalDatasetOrganization.png]*
