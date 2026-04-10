---
doctype: Reference
module: cal
function: McalSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / cal / McalSave"
---

# McalSave

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

> Save a camera or 3D draw calibration context to a file.

## Syntax

```c
void McalSave(
    AIL_CONST_TEXT_PTR FileName,       //in
    AIL_ID             CalibrationId,  //in
    AIL_INT64          ControlFlag     //in
)
```

## Description

This function saves a camera or 3D draw calibration context to a file. This information can be reloaded, using [`McalRestore`](../../Reference/cal/McalRestore.md) or [`McalStream`](../../Reference/cal/McalStream.md).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the camera or 3D draw calibration context. It is recommended that you use the MCA file extension for easier use with other Aurora Imaging software products. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, camera and 3D draw calibration context files have an MCA extension.

To save the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `CalibrationId` *(in, AIL_ID)*

Specifies the camera or 3D draw calibration context to save.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter must be set to the following value:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Sets the function's control flag to the default. |
