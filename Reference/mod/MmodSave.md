---
doctype: Reference
module: mod
function: MmodSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / mod / MmodSave"
---

# MmodSave

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

> Save a Model Finder context to a file.

## Syntax

```c
void MmodSave(
    AIL_CONST_TEXT_PTR FileName,    //in
    AIL_ID             ContextId,   //in
    AIL_INT64          ControlFlag  //in
)
```

## Description

This function saves all the information about the previously allocated Model Finder context to disk, including all of the individual models' current settings. This information can be reloaded, using [`MmodRestore`](../../Reference/mod/MmodRestore.md) or [`MmodStream`](../../Reference/mod/MmodStream.md). However, preprocessing and all drawing control type settings are not saved.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the Model Finder context; it is recommended that you use the MMF file extension for easier use with other Aurora Imaging Library software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, Model Finder context files have an MMF extension.

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the Model Finder context to save.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to save, with the Model Finder context, the camera calibration contexts associated with the models.

*For specifying whether to save the camera calibration contexts associated with the models*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the camera calibration contexts are not saved. |
| `M_WITH_CALIBRATION` | Specifies that the camera calibration contexts are saved. The camera calibration information is saved in the same file as the Model Finder context. Note that the camera calibration contexts associated with synthetic models are not saved. |
