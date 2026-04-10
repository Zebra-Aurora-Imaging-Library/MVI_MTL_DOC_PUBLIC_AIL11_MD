---
doctype: Reference
module: dlocr
function: MdlocrImport
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrImport"
---

# MdlocrImport

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

> Import a Deep Learning OCR dataset from disk.

## Syntax

```c
void MdlocrImport(
    AIL_CONST_TEXT_PTR FolderPath,      //in
    AIL_INT64          DatasetFormat,   //in
    AIL_INT64          Operation,       //in
    AIL_ID             DlocrDatasetId,  //in
    AIL_INT64          ControlFlag      //in
)
```

## Description

This function imports a Deep Learning OCR dataset from disk. This function reads the specified JSON file describing the dataset and checks that all images can be restored. To export data from a dataset, use [`MdlocrExport`](../../Reference/dlocr/MdlocrExport.md).

## Parameters

### `FolderPath` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of a file from which to import. When specifying the name and path of a file, the function handles (internally) the opening and closing of that file. Also, for easier use with other Aurora Imaging software products, the specified file name must use the JSON file extension.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `"JsonPath"` | Specifies the required information for the file from which to import.

You must specify the file's drive, directory, and name (for example, _"E:\Manual\MySweetSweetDataset.json"_).

To specify a dataset file on a remote computer (under Distributed Aurora Imaging Library), prefix the file name string with `"remote:///"` (for example, `"remote:///E:\Manual\\MyEvenSweeterDataset.json"`). |

### `DatasetFormat` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `Operation` *(in, AIL_INT64)*

Specifies the type of import operation to perform.

*For specifying the type of the file that you can import*

| Value | Description |
| --- | --- |
| `M_APPEND` | Specifies to append entries to the existing entries in the dataset. |
| `M_RESTORE` | Specifies to overwrite any existing entries in the dataset. |

### `DlocrDatasetId` *(in, AIL_ID)*

Specifies the identifier of the dataset in which to import. Set this parameter to one of the following values.

*For specifying the dataset in which to import*

| Value | Description |
| --- | --- |
| `DlocrDatasetId` | Specifies the identifier of a Deep Learning OCR dataset context. This dataset must have been previously allocated using[`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
