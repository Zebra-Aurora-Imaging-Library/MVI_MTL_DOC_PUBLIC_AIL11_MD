---
doctype: Reference
module: edge
function: MedgeSave
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / edge / MedgeSave"
---

# MedgeSave

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

> Save an Edge Finder context to a file, or save edge chains and/or edge approximations from an Edge Finder result buffer to a CAD (Computer-Aided Design) file.

## Syntax

```c
void MedgeSave(
    AIL_CONST_TEXT_PTR FileName,           //in
    AIL_ID             ContextOrResultId,  //in
    AIL_INT64          ControlFlag         //in
)
```

## Description

This function saves all the information about the previously allocated Edge Finder context to disk. This information can be reloaded, using [`MedgeRestore`](../../Reference/edge/MedgeRestore.md) or [`MedgeStream`](../../Reference/edge/MedgeStream.md). However, any associated camera calibration contexts are not saved.

This function also saves calculated edges (edge chains and/or edge approximations) included in a previously calculated Edge Finder result buffer to disk, in a standard DXF format CAD file. Note that edge chains and edge approximations are saved in the DXF file in separate layers.

Note that the Edge Finder context or the calculated edges are saved in real-world units if there is a camera calibration context associated with them. Otherwise, they are saved in pixel units.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the file in which to save the Edge Finder context, or the destination CAD file. For easier use with other Aurora Imaging Library software products, when saving an Edge Finder context to a file, use the MEF file extension, and when saving calculated edges from an Edge Result buffer to a CAD file, use the DXF file extension. The function internally handles the opening and closing of this file. If this file already exists, it will be overwritten.

*For specifying the file name and path*

| Value | Description |
| --- | --- |
| `M_INTERACTIVE` | Opens the **File Save As** dialog box from which you can interactively specify the drive, directory, and name of the file. |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). Typically, Edge Finder contexts have an MEF extension. Edge Finder result buffers have a DXF extension.

To save the file on the remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `ContextOrResultId` *(in, AIL_ID)*

Specifies either the identifier of the Edge Finder context to save, or the identifier of the Edge Finder result buffer from which to save to a CAD file.

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to save edge chains or edge approximations to the DXF CAD file for Edge Finder result buffers. For Edge Finder contexts, this parameter must be set to `M_DEFAULT`.

*For Edge Finder result buffers*

| Value | Description |
| --- | --- |
| `M_CHAIN` | Specifies that edge chains will be saved. |
| `M_CHAIN_APPROXIMATION` | Specifies that edge approximations will be saved. |
