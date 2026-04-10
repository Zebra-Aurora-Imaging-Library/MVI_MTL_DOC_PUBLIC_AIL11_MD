---
doctype: UserGuide
part: "Machine learning tasks"
chapter: ML_Feature_classification
section: Building_a_dataset_for_feature_classification
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning tasks / ML_Feature_classification / Building a dataset for feature classification"
---

# Building a dataset for feature classification

For an overview of the many considerations you should make when building a dataset, see [Datasets](../C48_ML_Datasets/ChapterInformation.md).

## Create your dataset context

Allocate a dataset context to hold all of your image data (source dataset), using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md).

## Populate the source dataset context

Add class definitions to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_ADD`](../../Reference/class/MclassControl.md). These are the classes that will be represented in your dataset.

Optionally, you can specify settings to help manage class definitions, using [`MclassControl`](../../Reference/class/MclassControl.md). For example, you can assign a color ([`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) and an icon image ([`M_CLASS_ICON_ID`](../../Reference/class/MclassControl.md)) to class definitions. This allows you to draw and visually identify them with [`MclassDraw`](../../Reference/class/MclassDraw.md).

Add entries to the source dataset, using [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_ENTRY_ADD`](../../Reference/class/MclassControl.md). Each entry must refer to a set of features (an array of values) and must identify its ground truth (the class label that identifies the entry). Specify a set of features for an entry using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_RAW_DATA`](../../Reference/class/MclassControlEntry.md). Although there is no realistic limit on the number of feature values that you can provide, or what they represent, every entry must have the same number of feature values, and they must be in the same order. For example, if entry 0 specifies three feature values, and the first is length, the second is width, and the third is height, then every entry must specify those three values in that order. While the feature values might trace back back to an image, the image itself is not included in a features dataset.

Set the class label for each entry using [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md) with [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md).

The table below illustrates the components of a features dataset.

| Entry | Features | Label |
| --- | --- | --- |
| Entry 0 | Feature 0 (length), Feature 1 (width), Feature 2 (height) | Class 0 |
| Entry 1 | Feature 0 (length), Feature 1 (width), Feature 2 (height) | Class 1 |
| Entry 2 | Feature 0 (length), Feature 1 (width), Feature 2 (height) | Class 0 |

## Splitting your dataset

When training for feature classification, the development dataset is optional. Typically, only the training dataset is used. Aurora Imaging Library uses bootstrap aggregating to train such classifiers and, inherent to this process, is a bagging technique, which randomly selects the dataset entries with which to train (in-the-bag) and the dataset entries with which to regulate the training (out-of-bag). For more information, see [Tree ensemble classifier](../C47_ML_with_the_MIL_Classification_module/S04_Classifiers_in_general.md).

You can call [`MclassSplitDataset`](../../Reference/class/MclassSplitDataset.md) to split a portion of your dataset into the development and testing datasets prior to training if you want to use them.

## Augmentation and other data preparations

Augmentation and preparation with [`MclassPrepareData`](../../Reference/class/MclassPrepareData.md) is not supported for a features dataset. An entry in a features dataset can still be augmented by other means.

For example, if one of the features (numerical values) in a dataset entry refers to the width of a blob, you can add entries that are a certain percentage bigger or smaller. Those additional entries should be considered augmented. Additionally, entries in a features dataset could have been added from a blob analysis that you performed on images. If some of those images were augmented, then the corresponding features should be considered augmented also.

Note that you should only allow entries with augmented features in the training dataset.

## Exporting and importing a dataset

Exporting and importing your datasets allows you to make your datasets portable and organized. This will allow you to easily reuse or combine your datasets. For more information about importing data, see [Importing data from a folder or CSV file](../C48_ML_Datasets/S06_Importing_data_from_a_CSV_file_to_a_dataset_context.md).

### Export

After you build a dataset, export it using [`MclassExport`](../../Reference/class/MclassExport.md). It is recommended that you export it with [`M_FORMAT_CSV`](../../Reference/class/MclassExport.md). To export the entire dataset, call the function three times with [`M_AUTHORS`](../../Reference/class/MclassExport.md), [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassExport.md), and [`M_ENTRIES`](../../Reference/class/MclassExport.md).

### Import

You can import a features dataset from a CSV file into a dataset context by calling [`MclassImport`](../../Reference/class/MclassImport.md) and specifying what to import (for example, [`M_AUTHORS`](../../Reference/class/MclassExport.md), [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassExport.md), and [`M_ENTRIES`](../../Reference/class/MclassExport.md)). You will typically be importing data that was previously exported from Aurora Imaging Library.

You can also use [`MclassRestore`](../../Reference/class/MclassRestore.md) to restore a dataset that was previously saved to a file using [`MclassSave`](../../Reference/class/MclassSave.md) or [`MclassStream`](../../Reference/class/MclassStream.md).
