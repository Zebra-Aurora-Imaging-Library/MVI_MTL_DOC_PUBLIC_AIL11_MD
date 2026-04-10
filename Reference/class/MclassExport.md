---
doctype: Reference
module: class
function: MclassExport
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassExport"
---

# MclassExport

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Export data from a dataset context, training result buffer, or a tree ensemble classifier context to a single file or to multiple files in an Aurora Imaging Library folder structure.

## Syntax

```c
void MclassExport(
    AIL_CONST_TEXT_PTR FileNameOrFolderPath,  //in
    AIL_INT64          FileOrFolderFormat,    //in
    AIL_ID             ClassId,               //in
    AIL_INT64          Index,                 //in
    AIL_INT64          ExportType,            //in
    AIL_INT64          ControlFlag            //in
)
```

## Description

This function exports data from a dataset context, training result buffer, or a tree ensemble classifier context to a single file or to multiple files in an Aurora Imaging Library folder structure. A dataset can be exported to a CSV file. Image datasets can also be exported to multiple files in an Aurora Imaging Library folder structure. Training results are exported to TXT files. Tree ensemble classifiers (trees) are exported to DOT files.

You would typically use this function to export an entire images dataset, using [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) and [`M_COMPLETE`](../../Reference/class/MclassExport.md). The following is an example of the resulting Aurora Imaging Library folder structure after such an export. The folder names represent what Aurora Imaging Library uses; the file names are fabricated. Datasets for image classification do not have descriptor, segmentation, or detection data; exporting such datasets would not result in those folders and files.

|   |   |
| --- | --- |
| ```<br/>DstFolder¹<br/>├───Icons<br/>│ ├───Class1.mim<br/>│ ├───Class2.mim<br/>├───Images <br/>│ ├───Class1<br/>│ │ ├───Image1.mim<br/>│ │ └───Image2.mim<br/>│ ├───Class2<br/>│ │ ├───Image1.mim<br/>│ │ └───Image2.mim<br/>│ ├───Image1.mim<br/>│ └───Image2.mim<br/>├───Masks<br/>│ ├───GUID1.mim<br/>│ └───GUID2.mim<br/>├───Segmentations<br/>│ ├───GUID1_0.mbufi<br/>│ ├───GUID2_0.mbufi<br/>│ ├───GUID3_0.mbufi<br/>│ ├───GUID4_0.mbufi<br/>│ ├───GUID5_0.mbufi<br/>│ ├───GUID6_0.mbufi<br/>├───Anomalies<br/>│ ├───GUID1.mbufi<br/>│ ├───GUID2.mbufi<br/>│ ├───GUID3.mbufi<br/>│ ├───GUID4.mbufi<br/>│ ├───GUID5.mbufi<br/>│ ├───GUID6.mbufi<br/>├───authors.csv<br/>├───class_definitions.csv<br/>├───controls.csv<br/>├───descriptor_boxes.csv<br/>├───descriptor_masks.csv<br/>├───descriptor_polygons.csv<br/>├───entries.csv<br/>└───results_segmentations.csv<br/>└───results_anomaly_detection.csv<br/>└───results_detection.csv<br/>``` | ¹For more information about this folder structure, see [Importing data from a folder or CSV file](../../UserGuide/C48_ML_Datasets/S06_Importing_data_from_a_CSV_file_to_a_dataset_context.md). |

To import data, use [`MclassImport`](../../Reference/class/MclassImport.md).

## Parameters

### `FileNameOrFolderPath` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to export, or specifies the path to the folder in which to export multiple files in an Aurora Imaging Library folder structure. The function handles (internally) the opening and closing of the file. If you specify a file that already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which to interactively specify the file's drive, directory, and name. |
| `"FileName"` | Specifies the file's drive, directory, and name (for example, _"E:\Manual\MySweetSweetDataset.csv"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the file name string with `"remote:///"` (for example, `"remote:///E:\Manual\MyEvenSweeterDataset.dot"`). |

### `FileOrFolderFormat` *(in, AIL_INT64)*

Specifies the format of the file or folder in which to export. Set this parameter to one of the following values.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_FORMAT_CSV` | Specifies a CSV file format. This value is supported when exporting from a dataset context. |
| `M_FORMAT_DOT` | Specifies a DOT file format. This value is supported when exporting a tree from a tree ensemble training result buffer or a tree ensemble classifier context. |
| `M_FORMAT_TXT` | Specifies a TXT file format. This value is supported when exporting from a CNN, segmentation, or tree ensemble training result buffer. |
| `M_IMAGE_DATASET_FOLDER` | Specifies a path to a folder in which to create the Aurora Imaging Library folder structure for an images dataset.

Note, the export does not complete if two classes (or more) have the same name, or if the creation of the folders would exceed MAX_PATH. |
| `M_IMAGE_DATASET_FOLDER_ALLOW_RENAME` | Specifies a path to a folder in which to create the Aurora Imaging Library folder structure for an images dataset (allows the renaming of classes to make sure the export completes).

Note, [`M_IMAGE_DATASET_FOLDER_ALLOW_RENAME`](../../Reference/class/MclassExport.md), ignores the restrictions on [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) to make sure that the exports completes. It does so by renaming duplicate and reserved class names, and shortening the names of the folders that are too long. Using this operation might cause [`MclassImport`](../../Reference/class/MclassImport.md) to incorrectly import the dataset. |

### `ClassId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library Classification module object from which to export. Set this parameter to one of the following values.

*For specifying the Classification module object from which to export*

| Value | Description |
| --- | --- |
| `ContextDatasetId` | Specifies the identifier of a features or images dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md). |
| `ContextTreeEnsembleClassiferId` | Specifies the identifier of a tree ensemble classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_TREE_ENSEMBLE`](../../Reference/class/MclassAlloc.md). |
| `ResultBufTrainId` | Specifies the identifier of a CNN, object detection, segmentation, or tree ensemble training result buffer. This buffer is allocated using [`MclassAllocResult`](../../Reference/class/MclassAllocResult.md) with [`M_TRAIN_CNN_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_DET_RESULT`](../../Reference/class/MclassAllocResult.md), [`M_TRAIN_SEG_RESULT`](../../Reference/class/MclassAllocResult.md), or [`M_TRAIN_TREE_ENSEMBLE_RESULT`](../../Reference/class/MclassAllocResult.md). |

### `Index` *(in, AIL_INT64)*

Specifies the tree to export from a tree ensemble training result buffer or tree ensemble classifier context. To export a tree, you must set the [`ExportType`](../../Reference/class/MclassExport.md) parameter to [`M_TRAIN_TREE`](../../Reference/class/MclassExport.md). If you are not exporting a tree, you must set the [`Index`](../../Reference/class/MclassExport.md) parameter to [`M_DEFAULT`](../../Reference/class/MclassExport.md).

*For specifying the index of the tree to export*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that you are not exporting a tree. |
| `Value >= 0` | Specifies the index of the tree to export to a DOT file. |

### `ExportType` *(in, AIL_INT64)*

Specifies the type of export operation to perform. Available operations depend on the context or result buffer from which you are exporting.

*For specifying what to export from a dataset context*

| Value | Description |
| --- | --- |
| `M_AUTHORS` | Specifies to export the authors to a CSV file. |
| `M_CLASS_DEFINITIONS` | Specifies to export the class definitions to a CSV file. |
| `M_COMPLETE` | Specifies to export all the information of the dataset; this includes: entries (images and CSV), segmentation files (Aurora Imaging Library buffers), prediction results (CSV), descriptors (images and CSV), icons (images), class definitions (CSV), controls (CSV), and authors (CSV).

This is typically the most useful setting for image datasets. To use it, the [`FileOrFolderFormat`](../../Reference/class/MclassExport.md) parameter must be set to [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassExport.md) or [`M_IMAGE_DATASET_FOLDER_ALLOW_RENAME`](../../Reference/class/MclassExport.md).

> **Note:** You cannot specify [`M_COMPLETE`](../../Reference/class/MclassExport.md) with Distributed Aurora Imaging Library. |
| `M_ENTRIES` | Specifies to export the entries to a CSV file. |

*For specifying what to export from a training result buffer*

| Value | Description |
| --- | --- |
| `M_TRAIN_REPORT` | Specifies to export a train report to a TXT file.

If you are exporting from a CNN, object detection or segmentation training result buffer, the TXT file contains the CNN, object detection or segmentation training results of each epoch.

If you are exporting from a tree ensemble training result buffer, the TXT file contains results from the training. For example, the TXT file contains results such as feature importance, train accuracy, and number of trained trees. |

*For specifying what to export from a tree ensemble training result buffer or classifier context*

| Value | Description |
| --- | --- |
| `M_TRAIN_TREE` | Specifies to export the resulting tree to a DOT file. |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
