---
doctype: Reference
module: class
function: MclassImport
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / class / MclassImport"
---

# MclassImport

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

> Import data to a dataset or classifier context.

## Syntax

```c
void MclassImport(
    AIL_CONST_TEXT_PTR FileNameOrFolderPath,          //in
    AIL_INT64          FileOrPathFormat,              //in
    AIL_ID             DatasetOrClassifierContextId,  //in
    AIL_INT64          Index,                         //in
    AIL_INT64          ImportType,                    //in
    AIL_INT64          ControlFlag                    //in
)
```

## Description

This function imports data from a CSV file or folder to a dataset context (images or features), or imports data from an ONNX file to an ONNX classifier context. Aurora Imaging Library appends imported data from a CSV file or folder to any existing data in the dataset. To export data from a dataset, use [`MclassExport`](../../Reference/class/MclassExport.md).

## Parameters

### `FileNameOrFolderPath` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of a file, or specifies a folder, from which to import. When specifying the name and path of a file, the function handles (internally) the opening and closing of that file. Also, for easier use with other Aurora Imaging software products, the specified file name must use the CSV or ONNX file extension. A CSV file represents a common format that holds comma separated values. An ONNX file is a platform-neutral and language-neutral open source file format for describing machine learning models.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens a dialog box from which to interactively specify the file's drive, directory, and name. |
| `"FileNameOrFolderPath"` | Specifies the required information for the file or folder from which to import.

For a file, you must specify its drive, directory, and name (for example, _"E:\Manual\MySweetSweetDataset.csv"_ or _"E:\Manual\MySweetSweetClassifierContext.onnx"_). For a folder, you must specify its drive and directory (for example,_ "E:\Manual\MySweetDatasetFolder"_).

To specify a dataset file on a remote computer (under Distributed Aurora Imaging Library), prefix the file name string with `"remote:///"` (for example, `"remote:///E:\Manual\\MyEvenSweeterDataset.csv"`). |

### `FileOrPathFormat` *(in, AIL_INT64)*

Specifies the format of the file or path from which to import. Set this parameter to one of the following values.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_FORMAT_CSV` | Specifies a CSV file format. In this case, set the [`DatasetOrClassifierContextId`](../../Reference/class/MclassImport.md) parameter to the identifier of an images or features dataset.

When importing from a CSV file, note the following:

- Field values with commas must delimit them within double quotation marks.
- If an entry refers to a non-existing class label, the entry is added to the dataset with default initial values.
- When no BOM (Byte-Order-Marker) is encoded in the file, the encoding character UTF-8 is used.

The CSV file must adhere to formatting requirements and contain the required data; this can depend on what you are importing, as specified by the [`ImportType`](../../Reference/class/MclassImport.md) parameter. |
| `M_IMAGE_DATASET_FOLDER` | Specifies a path to an existing folder. In this case, set the [`DatasetOrClassifierContextId`](../../Reference/class/MclassImport.md) parameter to the identifier of an images dataset.

When importing from a folder, note the following:

- You should follow the Aurora Imaging Library folder structure, as described in [`MclassExport`](../../Reference/class/MclassExport.md).
- If the dataset you are importing was exported with [`M_IMAGE_DATASET_FOLDER_ALLOW_RENAME`](../../Reference/class/MclassExport.md), Aurora Imaging Library cannot guarantee that the import will successfully restore the exported dataset.
- Aurora Imaging Library might modify the root path ([`M_ROOT_PATH`](../../Reference/class/MclassControl.md)) in the dataset, if one is specified, to maintain the dataset's coherence. For example, dataset paths should all be either relative or absolute. |
| `M_ONNX_FILE` | Specifies an ONNX file format. In this case, set the [`DatasetOrClassifierContextId`](../../Reference/class/MclassImport.md) parameter to the identifier of an ONNX classifier context.

When importing an ONNX file, note the following:

- The supported ONNX file format version (ir_version) is from 3 to 8, inclusive. The supported ONNX operator set version is from 7 to 15, inclusive.
- The ONNX model must have a single operator set domain. This domain must be either "ai.onnx" or an empty string ("").
- The network must have one input.
  - The input type must be a dense tensor, that is, an array where the elements are all sequential starting at index 0.
    - The tensor element type must be either float32, uint8, uint16, int8, int16 or int32.
    - The tensor dimensionality (rank) must range from 1 to 4. Aurora Imaging Library follows the NCHW convention, that is "Number in batch" x "Channels" x "Height" x "Width" or, in Aurora Imaging Library terminology, "size batch" x "size band" x "size Y" x "size X". Aurora Imaging Library will automatically assign values of 1 to the missing higher (leftmost) dimensions for inputs of lower dimensionality. For instance, given an input tensor with 2 dimensions, Aurora Imaging Library will assign values of 1 to N and C and define HW based on the tensor.
      - Each dimension must be expressed either with an integer value greater than 0 or using a dimension variable string.
      - Aurora Imaging Library only supports 1 as the integer value of dimension N.
      - Dimension variables are allowed at import time but must be defined by the user with [`MclassControl`](../../Reference/class/MclassControl.md) with [`M_TARGET_IMAGE_SIZE_X`](../../Reference/class/MclassControl.md),[`M_TARGET_IMAGE_SIZE_Y`](../../Reference/class/MclassControl.md), [`M_TARGET_IMAGE_SIZE_BAND`](../../Reference/class/MclassControl.md) calls prior to calling [`MclassPreprocess`](../../Reference/class/MclassPreprocess.md).
      - Dimension variables for the size batch (N) are internally set to 1.
- The network must have at least one output.
  - All output type must be dense tensor.
    - The tensor element types must be either float16, float32, float64, uint8, uint16, int8, int16, int32 or int64.
    - The tensors must have at least one dimension.
      - Each dimension must be expressed either with an integral value greater than 0 or using a dimension variable string.
  - Outputs are filled at [`MclassPredict`](../../Reference/class/MclassPredict.md) and are retrieved using [`MclassGetResult`](../../Reference/class/MclassGetResult.md).
- The graph of the model must be free of cycles.
- The ONNX model must pass the ONNX function `onnx::checker::check_model()` without error. |

### `DatasetOrClassifierContextId` *(in, AIL_ID)*

Specifies the identifier of the dataset or classifier context in which to import. Set this parameter to one of the following values.

*For specifying the dataset or classifier context in which to import*

| Value | Description |
| --- | --- |
| `ContextClassifierId` | Specifies the identifier of an ONNX classifier context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_CLASSIFIER_ONNX`](../../Reference/class/MclassAlloc.md). |
| `ContextDatasetId` | Specifies the identifier of a features or images dataset context. This context is allocated using [`MclassAlloc`](../../Reference/class/MclassAlloc.md) with [`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md) or [`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md). |

### `Index` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `ImportType` *(in, AIL_INT64)*

Specifies the type of import operation to perform.

*For specifying the type of the file that you can import*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to import an ONNX file. |
| `M_AUTHORS` | Specifies to import authors from a CSV file. In this case, use the headers below (and provide the corresponding lines of information after the header) in the CSV file.

```
Key,AuthorName
```

Both of these fields are required. |
| `M_CLASS_DEFINITIONS` | Specifies to import class definitions from a CSV file. In this case, use the headers below (and provide the corresponding lines of information after the header) in the CSV file.

```
Key,Name,Color_R,Color_G,Color_B,Weight,Icon,Anomalous
```

The 'Key' and 'Name' fields are required; the others are optional. |
| `M_COMPLETE` | Specifies to import the entire folder into the dataset. This includes authors (CSV), class definitions (CSV), controls (CSV), entries (images and CSV), descriptors (images and CSV), segmentation files (Aurora Imaging Library buffers), prediction results (CSV), and icons (images).

To use it, the [`FileOrPathFormat`](../../Reference/class/MclassImport.md) parameter must be set to [`M_IMAGE_DATASET_FOLDER`](../../Reference/class/MclassImport.md). In this case, use the headers below (and provide the corresponding lines of information after the headers) in the CSV file.

- Headers for _controls.csv_ files.
  ```
  ControlType,ControlValue
  ```
  Note:
  - Currently supported control types (and associated control values) are [`M_NO_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md), [`M_NO_REGION_PIXEL_CLASS`](../../Reference/class/MclassControl.md), and [`M_DONT_CARE_CLASS_DRAW_COLOR`](../../Reference/class/MclassControl.md). If you are importing into a newly allocated dataset, the control values for these control types (as specified in your folder) are imported into the dataset; otherwise, the control values (as specified in your folder) must be the same as those in the dataset you are importing into.
- Headers for _descriptor_boxes.csv_ files.
  ```
  Key,FilePath,RegionIndex,ClassIdxGroundTruth,AuthorName,BoxCoords
  ```
  Note:
  - When importing a dataset for object detection, this file is used to import information about the bounding boxes.
  - FilePath and AuthorName are optional.
  - BoxCoords is in the format "xmin, ymin, xmax, ymax" (each box is a line in the CSV).
- Headers for _descriptor_masks.csv_ files.
  ```
  Key,FilePath,RegionIndex,ClassIdxGroundTruth,AuthorName,MaskPath
  ```
  Note:
  - FilePath and AuthorName are optional.
  - MaskPath is the path to the mask image file, relative to the folder being imported.
- Headers for _descriptor_polygons.csv_ files.
  ```
  Key,FilePath,RegionIndex,ClassIdxGroundTruth,AuthorName,PolygonCoords
  ```
  Note:
  - FilePath and AuthorName are optional.
  - PolygonCoords is in the format "x1, y1, x2, y2, ..., xN, yN" (each polygon is a line in the CSV).
- Headers for _results_detection.csv_ files.
  ```
  Key,FilePath,ClassScorePredicted,ClassIdxPredicted,BoxCoords
  ```
  Note:
  - FilePath is optional.
  - BoxCoords is in the format "xmin, ymin, xmax, ymax" (each box is a line in the CSV).
- Headers for _results_anomaly_detection.csv_ files.
  ```
  Key,FilePath,AnomalyScoresPath,ImageAnomalyScore,Threshold,SizeX,SizeY,ClassifierPredefinedType
  ```
  Note:
  - AnomalyScoresPath is the reletive path to the anomaly scores.
  - SizeX and SizeY are the dimensions of the anomaly scores map.
- Headers for _results_segmentation.csv_ files.
  ```
  Key,FilePath,SegmentationPath,SizeX,SizeY,NumClasses,PredefinedNetworkType,ReceptiveFieldSizeX,ReceptiveFieldSizeY,ReceptiveFieldOffsetX,
  ReceptiveFieldOffsetY,ReceptiveFieldAsymetricOffsetX,ReceptiveFieldAsymetricOffsetY,ReceptiveFieldStrideX,ReceptiveFieldStrideY
  ```
  Note:
  - Headers must be on one line; illustrated headers are on multiple lines for display purposes.
  - FilePath is optional.
  - SegmentationPath is the relative path to the segmentation scores.
  - SizeX, SizeY and NumClasses are the dimensions of the segmentation scores map (saved under SegmentationPath).
  - PredefinedNetworkType is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md).
  - ReceptiveFieldSizeX/Y is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_SIZE_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md).
  - The combination of ReceptiveFieldOffsetX/Y and ReceptiveFieldAsymetricOffsetX/Y represents the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResultEntry.md).
  - ReceptiveFieldStrideX/Y is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_STRIDE_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_STRIDE_Y`](../../Reference/class/MclassGetResultEntry.md).

> **Note:** You cannot specify [`M_COMPLETE`](../../Reference/class/MclassImport.md) with Distributed Aurora Imaging Library. |
| `M_ENTRIES` | Specifies to import the entries from a CSV file. In this case, use the headers below (and provide the corresponding lines of information after the headers) in the CSV file.

- Headers for an entries file of an images dataset ([`M_DATASET_IMAGES`](../../Reference/class/MclassAlloc.md)); for example, _DatasetEntries_images.csv_.
  ```
  Key,FilePath,AuthorName,AugmentationSource,RegionType,ClassIdxGroundTruth_...,ClassIdxPredicted_...,ClassScorePredicted_...,UserString,UserConfidence,PredefinedNetworkType,
  ReceptiveFieldSizeX,ReceptiveFieldSizeY,ReceptiveFieldOffsetX,ReceptiveFieldOffsetY,ReceptiveFieldAsymetricOffsetX,ReceptiveFieldAsymetricOffsetY,ReceptiveFieldStrideX,ReceptiveFieldStrideY
  ```
  Note:
  - Headers must be on one line; illustrated headers are on multiple lines for display purposes.
  - PredefinedNetworkType is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_CLASSIFIER_PREDEFINED_TYPE`](../../Reference/class/MclassGetResultEntry.md).
  - ReceptiveFieldSizeX/Y is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_SIZE_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_SIZE_Y`](../../Reference/class/MclassGetResultEntry.md).
  - The combination of ReceptiveFieldOffsetX/Y and ReceptiveFieldAsymetricOffsetX/Y represents the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_OFFSET_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_OFFSET_Y`](../../Reference/class/MclassGetResultEntry.md).
  - ReceptiveFieldStrideX/Y is the result returned by calling [`MclassGetResultEntry`](../../Reference/class/MclassGetResultEntry.md) with [`M_SEGMENTATION`](../../Reference/class/MclassGetResultEntry.md) and [`M_RECEPTIVE_FIELD_STRIDE_X`](../../Reference/class/MclassGetResultEntry.md)/[`M_RECEPTIVE_FIELD_STRIDE_Y`](../../Reference/class/MclassGetResultEntry.md).
- Headers for an entries file of a features dataset ([`M_DATASET_FEATURES`](../../Reference/class/MclassAlloc.md)); for example, _DatasetEntries_features.csv_.
  ```
  Key,FilePath,AuthorName,AugmentationSource,Data_...,ClassIdxGroundTruth_...,ClassIdxPredicted_...,ClassScorePredicted_...,UserString,EntryWeight
  ```

For entries from either and images or features dataset, the 'Key', 'FilePath', and 'ClassIdxGroundTruth_...' fields are required; the others are optional.

The fields with an underscore and an ellipsis, such as 'Data_...', represent an array of values. You must replace the ellipsis with an integer, starting at 0 for the first array value, and increasing by 1 for each subsequent array value; for example:

```
Key,FilePath,AuthorName,AugmentationSource,Data_0,Data_1,Data_2,ClassIdxGroundTruth
0,E:\Images\Class1\0001.mim,Zebra,NOT_AUGMENTED,99,66,77,1
``` |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
