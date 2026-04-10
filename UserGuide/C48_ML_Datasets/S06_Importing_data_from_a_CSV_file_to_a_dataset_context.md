---
doctype: UserGuide
part: "Machine learning fundamentals"
chapter: ML_Datasets
section: Importing_data_from_a_CSV_file_to_a_dataset_context
module_tag: class
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Machine learning fundamentals / Datasets / Importing data from a CSV file to a dataset context"
---

# Importing data from a folder or CSV file

You can use a folder or CSV (comma-separated value) file to define data for a dataset (images or features), such as authors, class definitions, and entries. You can then add that data to a dataset context, using [`MclassImport`](../../Reference/class/MclassImport.md), and specify what to import (for example, [`M_COMPLETE`](../../Reference/class/MclassImport.md), [`M_ENTRIES`](../../Reference/class/MclassImport.md), [`M_AUTHORS`](../../Reference/class/MclassImport.md), and [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassImport.md)).

You will typically be importing data that was previously exported from Aurora Imaging Library or Aurora Imaging CoPilot. To export a dataset (for example, to a CSV file), call [`MclassExport`](../../Reference/class/MclassExport.md).

## Folder

In general, the most useful way to import data to a dataset is to do a complete import ([`M_COMPLETE`](../../Reference/class/MclassImport.md)) from a folder ([`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassImport.md)).

This imports the entire content of that folder, including subfolders, into the dataset. This includes authors (CSV), class definitions (CSV), controls (CSV), entries (images and CSV), descriptors (images and CSV), segmentation files (Aurora Imaging Library buffers to hold the pixel data), prediction results (CSV), and icons (images).

As indicated, the folder from which you are importing must contain the proper content, in the proper format (for example, CSV), and the structure of that folder, and its subfolders, must be coherent to Aurora Imaging Library. Typically, the folder from which you are importing was previously a complete export from a dataset ([`MclassExport`](../../Reference/class/MclassExport.md)). The following is an example of the resulting Aurora Imaging Library folder structure after such an export. The folder names represent what Aurora Imaging Library uses; the file names are fabricated. Datasets for image classification and anomaly detection do not have descriptor, segmentation, or detection data; exporting such datasets would not result in those folders and files.

|   |   |
| --- | --- |
| ```<br/>DstFolder<br/>├───Icons¹<br/>│ ├───Class1.mim<br/>│ ├───Class2.mim<br/>├───Images <br/>│ ├───Class1²<br/>│ │ ├───Image1.mim<br/>│ │ └───Image2.mim<br/>│ ├───Class2<br/>│ │ ├───Image1.mim<br/>│ │ └───Image2.mim<br/>│ ├───Image1.mim³<br/>│ └───Image2.mim<br/>├───Masks⁴<br/>│ ├───GUID1.mim<br/>│ └───GUID2.mim<br/>├───Segmentations⁵<br/>│ ├───GUID1_0.mbufi<br/>│ ├───GUID2_0.mbufi<br/>│ ├───GUID3_0.mbufi<br/>├───Anomalies⁶<br/>│ ├───GUID1_0.mbufi<br/>│ ├───GUID2_0.mbufi<br/>│ ├───GUID3_0.mbufi<br/>├───authors.csv<br/>├───class_definitions.csv<br/>├───controls.csv<br/>├───descriptor_boxes.csv<br/>├───descriptor_masks.csv<br/>├───descriptor_polygons.csv<br/>├───entries.csv<br/>└───results_segmentations.csv<br/>└───results_detection.csv<br/>└───results_anomaly_detection.csv<br/>``` | ¹This folder's content can be drawn using [`M_DRAW_CLASS_ICON`](../../Reference/class/MclassDraw.md). ²Sub-folders at this level contain images labeled, with respect to image classification, for each class. Anomaly detection follows the same convention as image classification, with only 2 possible classes. That is, the vast majority of your images should be non-anomalous (one type of class), while some images used for testing can be anomalous (another type of class). ³Images labeled for segmentation and object detection are under the _Images_ folder. ⁴Contains the segmentation ground truth masks. ⁵Contains the segmentation prediction score tensors (this can be seen as a type of image with the scores for all pixels for each class). ⁶Contains the anomaly detection prediction score tensors (this can be seen as a type of image with the scores for all anomalous/non-anomalous pixels, if you are performing pixel-level anomaly detection). |

## CSV

When importing data from a CSV ([`MclassImport`](../../Reference/class/MclassImport.md)), note the following:

- Import authors and class definitions first, and then import entries, as entries require authors and class definitions (with the exception of anomaly detection).
- If you only import entries, Aurora Imaging Library automatically creates class definitions and authors according to the information in the entries. If you import class definitions or authors afterward, Aurora Imaging Library appends them to the already existing ones.
- If Aurora Imaging Library encounters non-existing class definitions or authors when importing entries, they are automatically added to the dataset using default values.

When calling [`MclassImport`](../../Reference/class/MclassImport.md), you must specify what to import (CSV files); this includes:

- _authors.csv_.
- _class_definitions.csv_.
- _controls.csv_.
- _descriptor_boxes.csv_.
- _descriptor_masks.csv_.
- _descriptor_polygons.csv_.
- _entries.csv_.
- _results_segmentations.csv_.
- _results_detection.csv_.

You can only specifically import the files _authors.csv_, _class_definitions.csv_, and _entries.csv_ (using [`M_FORMAT_CSV`](../../Reference/class/MclassImport.md) with [`M_AUTHORS`](../../Reference/class/MclassImport.md), [`M_CLASS_DEFINITIONS`](../../Reference/class/MclassImport.md), or [`M_ENTRIES`](../../Reference/class/MclassImport.md)). Other CSVs, such as _controls.csv_, _descriptor_boxes.csv_, and _descriptor_masks.csv_ are imported from a folder ([`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassImport.md)) using [`M_COMPLETE`](../../Reference/class/MclassImport.md). In this case, all CVSs are completely imported (including authors, class definitions, and entries).

> **Note:** Although it is possible to directly modify CSV files, it is not recommended.

### General CSV file format

To import data from a CSV file, it must adhere to formatting requirements and contain the necessary headers and corresponding data (this can depend on what you are importing). The following is an example of valid content in a CSV file from which you can import entries ([`M_ENTRIES`](../../Reference/class/MclassImport.md)).

```
Key,FilePath,AuthorName,AugmentationSource,RegionType,ClassIdxGroundTruth,UserString
87fbb05a-b078-4389-8ad2-2a2f9707c8f4,E:\Images\Class1\0001.mim,Zebra,NOT_AUGMENTED,WholeImage,0,Any useful 
g78e04qe-rtol-0xds-zegp-lgjzrw5t0wkv,E:\Images\Class1\0002.mim,Zebra,NOT_AUGMENTED,WholeImage,0,meta information 
hqrwrwmk-mqbo-n9gq-wpt3-5z6fqciqtida,E:\Images\Class1\0003.mim,Zebra,NOT_AUGMENTED,WholeImage,0,can be written here 
k605lq7g-bbek-53x4-pgx1-3hcx9276ikf7,E:\Images\Class1\0004.mim,Zebra,NOT_AUGMENTED,WholeImage,0,in the UserString field.
sy69s7xr-9wkl-v0ym-f8s8-a4zlcry1t7ly,E:\Images\Class2\0001.mim,Zebra,NOT_AUGMENTED,WholeImage,1,Lines that end with a comma
fmz46ymy-08mo-lgq5-gy90-wh6sbade9mmz,E:\Images\Class2\0002.mim,Zebra,NOT_AUGMENTED,WholeImage,1,indicate that there is no
l2g9mx20-9yv5-dpgz-kusk-wl6byok8095j,E:\Images\Class2\0003.mim,Zebra,NOT_AUGMENTED,WholeImage,1,meta information for that entry.
q345ay0a-dkho-05m9-42vk-ob64dgajplh8,E:\Images\Class2\0004.mim,Zebra,NOT_AUGMENTED,WholeImage,1,
j6ebgfi2-vjom-7rb4-6uqx-6apc6t1n2jz1,E:\Images\Class2\Augmented\0001a.mim,Zebra,4,WholeImage,1,augmented version of the image from the entry index 4.
```

Regardless of the data you import, CSV files must contain:

- Header fields (cells) on the first line that are separated by a comma. You do not need to list the headers in any particular order.
- One or more lines below the header containing the information fields corresponding to the headers. You must separate the information fields with a comma and order them according to the order of the header fields. You can leave information fields empty but cannot omit the comma between fields. Each line below the header is one entry in a dataset.
  If you have N header fields, you should have N fields on every line below the header, and every line should have N-1 commas.
- A header called 'Key'. You can leave the corresponding key information in the lines below this header empty, or you can specify a unique number. In either case, Aurora Imaging Library generates a UUID for each key field.
  Key information depends on the data you are importing. When importing entries, the key corresponds to the entry key, when importing authors or class definitions, the key corresponds to the author key and the class key, respectively.

Typically, CSV headers are PascalCase terms that correspond to an entry setting in [`MclassControlEntry`](../../Reference/class/MclassControlEntry.md). For example, the header 'AugmentationSource' corresponds to the [`M_AUGMENTATION_SOURCE`](../../Reference/class/MclassControlEntry.md) setting.

> **Note:** The information presented here indicates how CSV files are generally set up. For details on the exact information (such as headers) required for import, see [`MclassImport`](../../Reference/class/MclassImport.md) in the Aurora Imaging Library Reference.

### Headers for authors

To import authors ([`M_AUTHORS`](../../Reference/class/MclassImport.md)), use the headers below (and provide the corresponding lines of information after the header) in the CSV file:

| Header name | Required or optional | Related Aurora Imaging Library setting | Images or features dataset |
| --- | --- | --- | --- |
|  |
| Key | Required | [`M_AUTHOR_KEY`](../../Reference/class/MclassInquire.md) ([`MclassInquire`](../../Reference/class/MclassInquire.md)) | Both |
| AuthorName | Required | [`M_AUTHOR_NAME`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Both |

The following is an example of the content in a CSV file that you can use to import authors:

```
Key,AuthorName
0,SM
1,FX
2,VC
,JR
4,
```

### Headers for class definitions

To import class definitions ([`M_CLASS_DEFINITIONS`](../../Reference/class/MclassImport.md)), use the headers below (and provide the corresponding lines of information after the header) in the CSV file.

| Header name | Required or optional | Related Aurora Imaging Library setting | Images or features dataset |
| --- | --- | --- | --- |
|  |
| Key | Required | [`M_CLASS_KEY`](../../Reference/class/MclassInquire.md) ([`MclassInquire`](../../Reference/class/MclassInquire.md)) | Both |
| Name | Required | [`M_CLASS_NAME`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) | Both |
| Color_R | Optional | The [`M_RGB888()`](../../Reference/class/MclassControl.md)parameter of the [`M_RGB888()`](../../Reference/class/MclassControl.md) macro ([`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) | Both |
| Color_G | Optional | The [`M_RGB888()`](../../Reference/class/MclassControl.md)parameter of the [`M_RGB888()`](../../Reference/class/MclassControl.md) macro ([`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) | Both |
| Color_B | Optional | The [`M_RGB888()`](../../Reference/class/MclassControl.md)parameter of the [`M_RGB888()`](../../Reference/class/MclassControl.md) macro ([`MclassControl`](../../Reference/class/MclassControl.md) with [`M_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md)) | Both |
| Weight | Optional | [`M_CLASS_WEIGHT`](../../Reference/class/MclassControl.md) ([`MclassControl`](../../Reference/class/MclassControl.md)) | Both |

The following is an example of the content in a CSV file that you can use to import class definitions:

```
Key,Name,Color_R,Color_G,Color_G,Weight
0,Apples,,,,
1,Oranges,,,,
```

If you do not specify the optional headers, you need not specify the empty fields below; for example:

```
Key,Name
0,Apples
1,Oranges
```

### Headers for entries

To import entries ([`M_ENTRIES`](../../Reference/class/MclassImport.md)), use the headers below (and provide the corresponding lines of information after the headers) in the CSV file.

| Header name | Required or optional | Related Aurora Imaging Library setting | Images or features dataset |
| --- | --- | --- | --- |
|  |
| Key | Required | [`M_ENTRY_KEY`](../../Reference/class/MclassInquireEntry.md) ([`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md)) | Both |
| FilePath | Required | [`M_ENTRY_IMAGE_PATH`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Images dataset only |
| AuthorName | Optional | [`M_AUTHOR_NAME`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Both |
| AugmentationSource¹ | Optional | [`M_AUGMENTATION_SOURCE`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Both |
| RegionType² | Optional | [`M_REGION_TYPE`](../../Reference/class/MclassInquireEntry.md) ([`MclassInquireEntry`](../../Reference/class/MclassInquireEntry.md)) | Images dataset only |
| Data_...³ | Optional | [`M_RAW_DATA`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Features dataset only |
| ClassIdxGroundTruth_...³ | Required | [`M_CLASS_INDEX_GROUND_TRUTH`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Both |
| ClassIdxPredicted_...³ | Optional | [`M_BEST_CLASS_INDEX`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Both |
| ClassScorePredicted_...³ | Optional | [`M_BEST_CLASS_SCORE`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Both |
| UserString | Optional | [`M_ENTRY_USER_STRING`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Both |
| EntryWeight | Optional | [`M_ENTRY_WEIGHT`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Features dataset only |
| UserConfidence | Optional | [`M_USER_CONFIDENCE`](../../Reference/class/MclassControlEntry.md) ([`MclassControlEntry`](../../Reference/class/MclassControlEntry.md)) | Images dataset only |
| PredefinedNetworkType | Optional | [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldSizeX | Optional | [`M_RECEPTIVE_FIELD_SIZE_X`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldSizeY | Optional | [`M_RECEPTIVE_FIELD_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldOffsetX | Optional | [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldOffsetY | Optional | [`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldAsymetricOffsetX | Optional | [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldAsymetricOffsetY | Optional | [`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldStrideX | Optional | [`M_RECEPTIVE_FIELD_STRIDE_X`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ReceptiveFieldStrideY | Optional | [`M_RECEPTIVE_FIELD_STRIDE_Y`](../../Reference/class/MclassGetResultEntry.md) ([`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md)) | Images dataset only |
| ¹If an entry is not augmented, set the corresponding information field to 'NOT_AUGMENTED'. |
| ²If an entry uses the whole image, set the corresponding information field to 'WholeImage' |
| ³This header represents an array of values. You must replace the ellipsis with an integer, starting at 0 for the first array value, and increasing by 1 for each subsequent array value, such as: 'Data_0,Data_1,Data_2'. |

The following is an example of the content in a CSV file that you can use to import class entries that have 3 values for the header field named 'Data_...':

```
Key,FilePath,AuthorName,AugmentationSource,Data_0,Data_1,Data_2,ClassIdxGroundTruth
0,E:\Images\Class1\0000.mim,SC,NOT_AUGMENTED,99,66,87,0
1,E:\Images\Class1\0001.mim,SM,NOT_AUGMENTED,33,29,30,1
2,E:\Images\Class1\0002.mim,FX,NOT_AUGMENTED,4,19,77,2
```

When listing the header fields for an array of values, ensure that you separate them with commas and you do not skip a number in the sequence of integer suffixes; for example, listing 'Data_0' and 'Data_2' will cause a "missing field" error.
