---
doctype: Reference
module: dlocr
function: MdlocrExport
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / dlocr / MdlocrExport"
---

# MdlocrExport

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

> Export a Deep Learning OCR dataset to disk.

## Syntax

```c
void MdlocrExport(
    AIL_CONST_TEXT_PTR FolderPath,      //in
    AIL_INT64          DatasetFormat,   //in
    AIL_ID             DlocrDatasetId,  //in
    AIL_INT64          ControlFlag      //in
)
```

## Description

This function exports a Deep Learning OCR dataset to disk. This function creates a copy of all of the images in the dataset and creates a JSON file containing information about the dataset in the same folder. To import data from a JSON file, use [`MdlocrImport`](../../Reference/dlocr/MdlocrImport.md).

## Parameters

### `FolderPath` *(in, AIL_CONST_TEXT_PTR)*

Specifies the path of the folder in which to export the dataset. The function handles (internally) the copying of files to the folder. The destination folder should exist and be empty.

*For specifying the folder path*

| Value | Description |
| --- | --- |
| `"FolderPath"` | Specifies the path of the folder in which to export the dataset.

You must specify the folder's drive and directory (for example, _"E:\mydirectory\"_).

To specify a dataset folder on a remote computer (under Distributed Aurora Imaging Library), prefix the folder path string with `"remote:///"` (for example, `"remote:///E:\mydirectory\"`). |

### `DatasetFormat` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.

### `DlocrDatasetId` *(in, AIL_ID)*

Specifies the identifier of the dataset to export. Set this parameter to one of the following values.

*For specifying the dataset to export*

| Value | Description |
| --- | --- |
| `DlocrDatasetId` | Specifies the identifier of a Deep Learning OCR dataset. This dataset must have been previously allocated using[`MdlocrAllocDataset`](../../Reference/dlocr/MdlocrAllocDataset.md) and contain entries added using [`MdlocrAddImageToDataset`](../../Reference/dlocr/MdlocrAddImageToDataset.md). |

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion and must be set to `M_DEFAULT`.
