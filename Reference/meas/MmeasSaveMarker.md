---
doctype: Reference
module: meas
function: MmeasSaveMarker
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / meas / MmeasSaveMarker"
---

# MmeasSaveMarker

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

> Save a marker to disk.

## Syntax

```c
void MmeasSaveMarker(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             MarkerId,    //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves an allocated marker to disk, including all of its current characteristics. The marker and its characteristics can later be restored, using [`MmeasRestoreMarker`](../../Reference/meas/MmeasRestoreMarker.md) or [`MmeasStream`](../../Reference/meas/MmeasStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the marker. It is recommended that you use the MRK file extension for easier use with other Zebra Aurora software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For displaying results in an interactive dialog box*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, marker files have an MRK extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `MarkerId` *(in, AIL_ID)*

Specifies the identifier of the marker to save.

### `ControlFlag` *(in, AIL_INT64)*

Reserved for future expansion, and must be set to `M_DEFAULT`.
