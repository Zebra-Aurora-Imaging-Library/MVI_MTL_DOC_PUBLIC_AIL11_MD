---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Object_detection
section: Building_a_dataset_for_object_detection
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Object_detection / Building a dataset for object detection"
---

# Building a dataset for object detection

This section discusses specific options for building a dataset for object detection.

|   |   |
| --- | --- |
| *[Image: MclassIconNote.png]* | For an overview of the many considerations you should make when building a dataset, see [Datasets](../C48_ML_Datasets/ChapterInformation.md). It is recommended that you use Aurora Imaging CoPilot to create, label, modify, augment, and export your dataset interactively. |

## Add class definitions and entries to the source dataset context

After allocating your images dataset context, you must add class definitions and entries to it. Entries in an images dataset for object detection typically have multiple rectangular regions, which are smaller than the target image being predicted. Each region identifies a class.

*[Image: MclassObjectDetectionBuildingDataset.png]*

Although you can use an images dataset for image classification, segmentation, and object detection simultaneously, it is generally recommended to have a dataset that is exclusively for one task or the other.

The process for adding class definitions for object detection are the same as for segmentation. For more information, see [Building a dataset for segmentation](../C52_ML_Segmentation/S03_Building_a_dataset_for_segmentation.md).

## Add regions

To add regions to an entry in an images dataset context, call [`MclassEntryAddRegion`](../../Reference/class/MclassEntryAddRegion.md). Each time you call this function, one or more regions (classes) are added to the specified entry. Entry regions must properly and proportionally represent all the different classes.

The number and type of regions that are added is determined by the specified region's descriptor. Typically, you would add regions by specifying rectangles in a graphics list ([`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassEntryAddRegion.md)).

Each added region must have a ground truth (class) assigned to it (just like each entry image for image classification has a ground truth). For example, when using box regions ([`M_DESCRIPTOR_TYPE_BOX`](../../Reference/class/MclassEntryAddRegion.md)), the pixels in the entry image that correspond to the areas within the bounding box are considered part of the ground truth established for this region, which you must set with the [`ClassIndexGroundTruth`](../../Reference/class/MclassEntryAddRegion.md) parameter.

The table below illustrates the components of an image dataset for object detection.

|   |   |   |   |
| --- | --- | --- | --- |
| Entry | Image | Region | Label |
| Entry 0 | Image 0 | Region 0 | Class 0 |
| Region 1 | Class 1 |
| Region 2 | Class 2 |
| Entry 1 | Image 1 | Region 0 | Class 0 |
| Region 1 | Class 1 |
| Entry 2 | Image 2 | Region 0 | Class 0 |
| Region 1 | Class 2 |

The following code snippet shows how to add rectangular entry regions to a dataset for object detection.

> **Code example:** [userguide.classification.adding_entry_regions](userguide.classification.adding_entry_regions)

You can control and inquire about regions, by calling [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) and [`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md). To draw region information, call [`MclassDrawEntry`](../../Reference/class/MclassDrawEntry.md).

When you are finished adding your data, you should ensure that all of the images in your source dataset are the same size. This is a requirement for training an object detection classifier. To do this, call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) and specify your source dataset context. To specify how to resize images, use the [`M_SIZE_MODE`](../../Reference/class/MclassControl.md) and [`M_RESIZE_SCALE_FACTOR`](../../Reference/class/MclassControl.md) controls. Resizing can be performed automatically or with explicit image width and height values. Note, using smaller images will allow for faster training.

## Splitting your dataset

You can call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a dataset into two smaller datasets. You can do this to create a training, development, or testing dataset out of your source dataset. If you want to use a testing dataset, then you should first split a portion of the source dataset into a testing dataset by calling [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) with [`M_SPLIT_DET_CONTEXT_DEFAULT`](../../Reference/class/MclassSplitDataset.md) or [`M_SPLIT_DET_CONTEXT_FIXED_SEED`](../../Reference/class/MclassSplitDataset.md). For more information about splitting different datasets, see [Different datasets and how they are split](../C48_ML_Datasets/S04_Different_datasets_and_how_they_are_split.md).

## Augmentation and other data preparations

Optionally, prepare your data and add augmented images to a dataset using [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md). You can call [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) with an images dataset, or an individual image. You should not use [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) on a dataset if you are letting [`MclassTrain`](../../Reference/class/MclassTrain.md) split the dataset, since Aurora Imaging Library will automatically augment your data with the internally defined data preparation context. You can call [`MclassControl`](../../Reference/class/MclassControl.md) with the identifier of the internal data preparation context (which you can inquire with [`M_PREPARE_DATA_CONTEXT_ID`](../../Reference/class/MclassInquire.md)) and specify the required data preparation controls (for example, [`M_PRESET_CROP`](../../Reference/class/MclassControl.md) and [`M_PRESET_NOISE_SALT_PEPPER`](../../Reference/class/MclassControl.md)). These preparations will be applied when you call [`MclassTrain`](../../Reference/class/MclassTrain.md). For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

### Data preparation settings

[`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) requires that you allocate a data preparation context by calling [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_PREPARE_IMAGES_DET`](../../Reference/class/MclassAlloc.md). The data preparation context holds the settings with which to modify the specified source (the images in a dataset or an individual image). Preprocess the data preparation context by calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md). You must also specify the location in which prepared images will be stored by setting [`M_PREPARED_DATA_FOLDER`](../../Reference/class/MclassControl.md). For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

### Augmenting images

Some augmentations will also affect the regions and labels of the image. For example, if you rotate an image, the bounding boxes are transformed to always be parallel with the image borders. This is done automatically during augmentation, by creating a new bounding box that completely surrounds the corners of the rotated bounding box. The rotation and translation augmentations shown below reveal new entries.

*[Image: MclassDetAugmentTranslateExample.png]*

Augmented entries, and the entries used to augment them, must only be in the training dataset. Augmented entries in the development dataset can cause errors. For more information, see [Data augmentation and other data preparations](../C48_ML_Datasets/S05_Data_augmentation_and_other_data_preparations.md).

## Exporting and importing a dataset

Exporting and importing your datasets allows you to make your datasets portable and organized. This will allow you to easily reuse or combine your datasets. For more information, see [Guidelines for managing an images dataset](../C48_ML_Datasets/S07_Guidelines_for_managing_an_images_dataset.md).

### Export

After you build a dataset, export it using [`MclassExport`](../../Reference/class/MclassExport.md). It is recommended that you export it with [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) to create an organized folder than can be easily imported and reused.

### Import

You can use a folder or CSV file to define data for a dataset. To import it to a dataset context, use [`MclassImport`](../../Reference/class/MclassImport.md) and specify what to import (for example, [`M_COMPLETE`](../../Reference/class/MclassImport.md), [`M_ENTRIES`](../../Reference/class/MclassImport.md), [`M_AUTHORS`](../../Reference/class/MclassImport.md), and [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassImport.md)). For more information about importing data, see [Importing data from a folder or CSV file](../C48_ML_Datasets/S06_Importing_data_from_a_CSV_file_to_a_dataset_context.md).

You can also use [`MclassRestore`](../../Reference/class/MclassRestore.md) to restore a dataset that was previously saved to a file using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).
