---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Image_classification
section: Building_a_dataset_for_image_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Image_classification / Building a dataset for image classification"
---

# Building a dataset for image classification

This section discusses specific options for building a dataset for image classification.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For an overview of the many considerations you should make when building a dataset, see [Datasets](../C48_ML_Datasets/ChapterInformation.md). It is recommended that you use Aurora Imaging CoPilot to create, label, modify, augment, and export your dataset interactively. |

## Populate the source dataset context

Add class definitions to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ADD`](../../Reference/class/MclassControl.md). These are the classes that will be represented in your dataset.

Optionally, you can specify settings to help manage class definitions, using [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can assign a color ([`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) and an icon image ([`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md)) to class definitions. This allows you to draw and visually identify them with [`MclassDraw`](../../Reference/class/MclassDraw.md).

Add entries to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_ENTRY_ADD`](../../Reference/class/MclassControl.md). Every entry must refer to an image and must identify its ground truth (the class label that identifies that image). To specify the location from which to get an image entry's data, use [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md). Specify the class label for each entry using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md).

The table below illustrates the components of an image dataset for image classification.

|   |   |   |
| --- | --- | --- |
| Entry | Image | Label |
| Entry 0 | Image 0 | Class 0 |
| Entry 1 | Image 1 | Class 1 |
| Entry 2 | Image 2 | Class 2 |

Typically, the dataset holds images representing many classes. Datasets can also hold images representing just two classes. Such datasets are usually used for image classification related to a boolean type of validation; that is, to determine whether or not an image is acceptable (or, in other words, whether an image is good or bad). If this is the case, you might consider performing [anomaly detection](../C54_ML_Anomaly_detection/ChapterInformation.md).

When you are finished adding your data, you should ensure that all of the images in your source dataset are the same size. This is a requirement for training a CNN classifier. To do this, call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) and specify your source dataset context. To specify how to resize images, use the [`M_SIZE_MODE`](../../Reference/class/MclassControl.md) and [`M_RESIZE_SCALE_FACTOR`](../../Reference/class/MclassControl.md) controls. Resizing can be performed automatically or with explicit image width and height values. Note, using smaller images will allow for faster training.

## Splitting your dataset

You can call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a dataset into two smaller datasets. You can do this to create a training, development, or testing dataset out of your source dataset. If you want to use a testing dataset, you should first split a portion of the source dataset into a testing dataset by calling [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) with [`M_SPLIT_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) or [`M_SPLIT_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md). For more information about the different datasets, see [Different datasets and how they are split](../C48_ML_Datasets/S04_Different_datasets_and_how_they_are_split.md).

## Augmentation and other data preparations

Optionally, prepare your data and add augmented images to a dataset using [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). You can call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with an images dataset, or an individual image. You should not use [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) on a dataset if you are letting [`MclassTrain`](../../Reference/class/MclassTrain.md) split the dataset, since Aurora Imaging Library will automatically augment your data with the internally defined data preparation context. You can call [`MclassControl`](../../Reference/class/MclassControl.md) with the identifier of the internal data preparation context (which you can inquire with [`M_PREPARE_DATA_CONTEXT_ID`](../../Reference/class/MclassInquire.md)) and specify the required data preparation controls (for example, [`M_PRESET_CROP`](../../Reference/class/MclassControl.md) and [`M_PRESET_NOISE_SALT_PEPPER`](../../Reference/class/MclassControl.md)). These preparations will be applied when you call [`MclassTrain`](../../Reference/class/MclassTrain.md). For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

### Data preparation settings

[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) requires that you allocate a data preparation context by calling [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_CNN`](../../Reference/class/MclassAlloc.md). The data preparation context holds the settings with which to modify the specified source (the images in a dataset or an individual image). Preprocess the data preparation context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md). You must also specify the location in which prepared images will be stored by setting [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md). For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

### Augmenting images

To prepare your data with specific, preset augmentations, you can call [`MclassControl`](../../Reference/class/MclassControl.md) and enable the [`M_PRESET...`](../../Reference/class/MclassControl.md) augmentation operation setting. Alternatively, you can access additional types of augmentation operations using the data preparation context's internal augmentation context. This context is automatically managed by Aurora Imaging Library (you need not allocate it or free it) and is an internal version of the image processing context for augmentation (that is, [`MimAlloc`](../../Reference/im/MimAlloc.md) with [`M_AUGMENTATION_CONTEXT`](../../Reference/im/MimAlloc.md)).

Augmented entries, and the entries used to augment them, must only be in the training dataset. Augmented entries in the development dataset can cause errors. For more information on augmentation, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

## Exporting and importing a dataset

Exporting and importing your datasets allows you to make your datasets portable and organized. This will allow you to easily reuse or combine your datasets. For more information, see [Guidelines for managing an images dataset](../C48_ML_Datasets/S07_Guidelines_for_managing_an_images_dataset.md).

### Export

After you build a dataset, export it using [`MclassExport`](../../Reference/class/MclassExport.md). It is recommended that you export it with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) to create an organized folder than can be easily imported and reused.

### Import

You can use a folder or CSV file to define data for a dataset. To import it to a dataset context, use [`MclassImport`](../../Reference/class/MclassImport.md) and specify what to import (for example, [`M_COMPLETE`](../../Reference/class/MclassImport.md), [`M_ENTRIES`](../../Reference/class/MclassImport.md), [`M_AUTHORS`](../../Reference/class/MclassImport.md), and [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassImport.md)). For more information about importing data, see [Importing data from a folder or CSV file](../C48_ML_Datasets/S06_Importing_data_from_a_CSV_file_to_a_dataset_context.md).

You can also use [`MclassRestore`](../../Reference/class/MclassRestore.md) to restore a dataset that was previously saved to a file using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).
