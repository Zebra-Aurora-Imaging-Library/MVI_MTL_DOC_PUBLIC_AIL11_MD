---
doctype: Reference
module: str
function: MstrExpert
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / str / MstrExpert"
---

# MstrExpert

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

> Makes a diagnosis of the String Reader context and reports configuration problems.

## Syntax

```c
void MstrExpert(
    AIL_ID         ContextId,             //in
    const AIL_ID * ImageArrayPtr,         //in
    const void *   TargetStringArrayPtr,  //in
    AIL_INT        NbImages,              //in
    AIL_ID         ResultId,              //out
    AIL_INT64      ControlFlag            //in
)
```

## Description

This function analyzes the consistency of a String Reader context using a specified image and a target string. Retrieve results using [`MstrGetResult`](../../Reference/str/MstrGetResult.md); this function returns error and/or warning codes for problems found. Use [`MstrInquire`](../../Reference/str/MstrInquire.md) to retrieve the meaning associated with these codes.

When the diagnosis of the String Reader context does not cause a general error (see [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_GENERAL`](../../Reference/str/MstrGetResult.md)), the context is ready to be used to read the image provided to [`MstrExpert`](../../Reference/str/MstrExpert.md). You can call [`MstrExpert`](../../Reference/str/MstrExpert.md) multiple times to find and correct problems in the context.

You can limit the verify operation to a region of the image buffer, using a rectangular ROI set with [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md).

Note that this function is only available for a font-based context.

## Parameters

### `ContextId` *(in, AIL_ID)*

Specifies the identifier of the String Reader context. The String Reader context must have been previously allocated on the required system using [`MstrAlloc`](../../Reference/str/MstrAlloc.md).

### `ImageArrayPtr` *(in, *AIL_ID)*

Specifies the identifier of the image to analyze. Note that you cannot provide more than one image.

### `TargetStringArrayPtr` *(in, *void)*

Specifies the identifier of the string to locate. This string represents the expected strings to locate in each image analyzed using [`MstrRead`](../../Reference/str/MstrRead.md). The string array must be of the right type for the encoding scheme selected ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_ENCODING`](../../Reference/str/MstrControl.md)). Only one string is supported.

### `NbImages` *(in, AIL_INT)*

Specifies the number of images to analyze. This parameter must be set to 1.

### `ResultId` *(out, AIL_ID)*

Specifies the identifier of the String Reader result buffer in which to save the results of [`MstrExpert`](../../Reference/str/MstrExpert.md).

### `ControlFlag` *(in, AIL_INT64)*

Specifies the type of analysis to perform with [`MstrExpert`](../../Reference/str/MstrExpert.md).

*For specifying the type of analysis to perform*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to report errors and warnings. |
| `M_ERRORS` | Specifies to report errors. |
| `M_WARNINGS` | Specifies to report warnings. |
