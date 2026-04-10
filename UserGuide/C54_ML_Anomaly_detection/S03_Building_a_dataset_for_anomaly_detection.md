---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Anomaly_detection
section: Building_a_dataset_for_anomaly_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Anomaly_detection / Building a dataset for anomaly detection"
---

# Building a dataset for anomaly detection

This section discusses specific options for building the images dataset needed for anomaly detection.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For an overview of the many considerations you should make when building a dataset, see [Datasets](../C48_ML_Datasets/ChapterInformation.md). It is recommended that you use Aurora Imaging CoPilot to create, label, modify, augment, and export your dataset interactively. |

## Populate the source dataset context

For anomaly detection, you must have both a training and a development image dataset; a testing dataset is optional. The training dataset is used to learn the representation of good (for example, defect-free) images. The development dataset is used to determine the threshold that differentiates the good images from the anomalous ones. The testing dataset is used to evaluate the trained classifier, adjust the threshold, and calculate statistics. Ideally, good images should typically be split among the datasets equally (1/3 in the development dataset, 1/3 in the testing dataset, and 1/3 in the training dataset).

The following table summarizes the use of valid (not anomalous) and invalid (anomalous) images in a dateset when performing anomaly detection.

|   |   |   |   |
| --- | --- | --- | --- |
| Images | Used in training | Used in testing to tweak/validate training (optional) | Used in prediction (deployment) |
| *[Image: MclassNotAnomalousThumbsUp.png]*<br/><br/>Not anomalous (Valid, good, acceptable) | Yes - many, no labeling required. | Yes - some, no labeling required. If possible, there should be as many good images in testing as there was in training. | Yes - there should be many and they should be similar to images used in training/testing. |
| *[Image: MclassAnomalousThumbsDown.png]*<br/><br/>Anomalous (Invalid, bad, unacceptable) | No - in fact, there must not be any. | Yes - a few, identified as anomalous. You should provide as many as possible, if you have them. | Yes - there should be few and, though they need not be similar to images used in testing, it can be difficult to determine performance on anomalous type images that were not tested. |

You must only specify good (non-anomalous) images in the development and training datasets. That is, entries in the datasets used for training (the development and training datasets) cannot have a class definition that has [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) set to [`M_TRUE`](../../Reference/class/MclassControl.md). This control is set to [`M_FALSE`](../../Reference/class/MclassControl.md) by default and can optionally be set to true for some entries in a test dataset to help adjust the trained threshold and calculate statistics after training is done. For more information, see [Adjusting the trained threshold and calculating statistics](S06_Adjusting_the_trained_threshold_and_calculating_statistics.md).

It is possible that the datasets for anomaly detection hold images representing many classes, often from datasets used for other machine learning tasks, such as image classification, segmentation, and object detection. Anomaly detection only checks for the [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) value, while all other machine learning tasks ignore [`M_ANOMALOUS`](../../Reference/class/MclassControl.md).

> **Note:** Since datasets for anomaly detection training must only contain non-anomalous images, it can seem like they misrepresent a real world scenario. However, this classifier is specifically designed to learn that one acceptable case, and to see anything else as unacceptable. In addition, it is expected that anomaly detection is typically used for cases where the vast majority of the images you get are in fact not anomalous (getting anomalous images should actually be rare). When providing non-anomalous training images, you must ensure that all varieties of non-anomalous images are included; for example, if you accept certain imperfections, then include images with those imperfections, in the proper ratio that they would occur in the real world. During testing, you can include anomalous images in the testing dataset, to validate the trained classifier can properly predict them.

## Splitting your dataset

You can call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a dataset into two smaller datasets. You can do this to create a training, development, and testing dataset out of your source dataset. If you want to use a testing dataset, you should first split a portion of the source dataset into a testing dataset by calling [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) with [`M_SPLIT_ANO_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) or [`M_SPLIT_ANO_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md). After isolating the testing dataset, you can then create the development and training datasets.

When using [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a dataset with anomalous entries (that is, the source dataset has classes with [`M_ANOMALOUS`](../../Reference/class/MclassControl.md) set to [`M_TRUE`](../../Reference/class/MclassControl.md)), all anomalous entries will be in the second destination dataset ([`DstSecondDatasetContextClassId`](../../Reference/class/MclassSplitDataset.md)). In this case, the specified percentage ([`Percentage`](../../Reference/class/MclassSplitDataset.md)) controls what percentage of good (non-anomalous) entries go into the first and second destination datasets. Note that datasets with anomalous entries cannot be used for training.

When training an anomaly detection classifier, you must explicitly specify both a development dataset and a training dataset (you cannot, for example, specify a single dataset when calling [`MclassTrain`](../../Reference/class/MclassTrain.md)). The development dataset must not contain data (image entries) that are in the training dataset. Also, only the training dataset can have augmented entries (the development dataset and the testing dataset must not). For more information about the different datasets, see [Different datasets and how they are split](../C48_ML_Datasets/S04_Different_datasets_and_how_they_are_split.md).

## Augmentation and other data preparations

Optionally, you can add augmented images to the training dataset, using [`MimAugment`](../../Reference/im/MimAugment.md). You can, for example, increase the entries in your training dataset by randomly rotating, translating, and blurring images.

As with any machine learning task, anomaly detection requires images that are the same size. It is therefore common practice to resize or crop images.

Note that, to resize, crop, or otherwise augment your images for anomaly detection, it is possible to use [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). Since there is no dedicated data preparation context for anomaly detection, you must specify a data preparation context that is for image classification, segmentation, or object detection. The resulting images in the destination dataset or buffer can then be used for anomaly detection. In this case, it is important to select the data preparation context that is consistent with your labeling, or you can get unwanted results. For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md) and [Augmentation](../C05_Specialized_image_processing/S11_Augmentation.md).

## Exporting and importing a dataset

Exporting and importing a dataset allows you to make it portable and organized. This will let you to easily reuse or combine your datasets. It is not uncommon to use preexisting datasets for anomaly detection, such as datasets already created for other machine learning tasks, like image classification or segmentation. For more information, see [Guidelines for managing an images dataset](../C48_ML_Datasets/S07_Guidelines_for_managing_an_images_dataset.md).

### Export

After you build a dataset, export it using [`MclassExport`](../../Reference/class/MclassExport.md). It is recommended that you export it with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) to create an organized folder than can be easily imported and reused.

### Import

You can use a folder or CSV file to define data for a dataset. To import it to a dataset context, use [`MclassImport`](../../Reference/class/MclassImport.md) and specify what to import (for example, [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassImport.md) or [`M_COMPLETE`](../../Reference/class/MclassImport.md)). For more information about importing data, see [Importing data from a folder or CSV file](../C48_ML_Datasets/S06_Importing_data_from_a_CSV_file_to_a_dataset_context.md).

You can also use [`MclassRestore`](../../Reference/class/MclassRestore.md) to restore a dataset that was previously saved to a file using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).
