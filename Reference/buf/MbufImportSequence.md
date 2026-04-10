---
doctype: Reference
module: buf
function: MbufImportSequence
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufImportSequence"
---

# MbufImportSequence

> Import a sequence of images from an AVI file into separate image buffers.

## Syntax

```c
void MbufImportSequence(
    AIL_CONST_TEXT_PTR FileName,        //in
    AIL_INT64          FileFormat,      //in
    AIL_INT64          Operation,       //in
    AIL_ID             SystemId,        //in
    AIL_ID *           BufArrayPtr,     //in-out
    AIL_INT            StartImage,      //in
    AIL_INT            NumberOfImages,  //in
    AIL_INT64          ControlFlag      //in
)
```

## Description

This function imports a sequence of images from an AVI file into separate image buffers. [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md) can automatically allocate the necessary buffers or you can use previously allocated buffers. In the latter case, the [`BufArrayPtr`](../../Reference/buf/MbufImportSequence.md) parameter should point to an array containing the buffer identifiers. In the former case, [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md) will write the identifiers of the new buffers into the array pointed to by [`BufArrayPtr`](../../Reference/buf/MbufImportSequence.md).

Rather than importing all the required images at once, you can also use this function to only open the AVI file for reading. Then, call this function as many times as required to import different sets of images from the file. Once you have finished importing images from the file, you must call this function again to close the file.

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the AVI file. This function handles the opening and/or closing of the file depending on the [`ControlFlag`](../../Reference/buf/MbufImportSequence.md) parameter setting.

*For specifying the name and path of the AVI file*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `FileFormat` *(in, AIL_INT64)*

Specifies the format of the file. This parameter can be set to one of the values listed below.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically determines the file format. |
| `M_AVI_AIL` | Specifies an AVI format containing images in their Aurora Imaging Library format. |
| `M_AVI_DIB` | Specifies an AVI format containing non-compressed images. |
| `M_AVI_MJPG` | Specifies an AVI format containing compressed images. |

### `Operation` *(in, AIL_INT64)*

Specifies whether to import the specified sequence of images into automatically allocated buffers or previously allocated buffers. If not importing images, this parameter should be set to `M_NULL`.

*For importing images*

| Value | Description |
| --- | --- |
| `M_LOAD` | Imports the sequence into previously allocated buffers. |
| `M_RESTORE` | Imports the sequence into automatically allocated buffers. |

### `SystemId` *(in, AIL_ID)*

Specifies the system on which to allocate the buffers for an [`M_RESTORE`](../../Reference/buf/MbufImportSequence.md) operation. In other cases, this parameter should be set to `M_NULL`.

*For specifying the system*

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### `BufArrayPtr` *(in-out, *AIL_ID)*

Specifies the address of the array containing the buffer identifiers for an [`M_LOAD`](../../Reference/buf/MbufImportSequence.md) operation, or the address of the array in which to store the new buffer identifiers for an [`M_RESTORE`](../../Reference/buf/MbufImportSequence.md) operation. If not importing images, this parameter should be set to `M_NULL`.

### `StartImage` *(in, AIL_INT)*

Specifies the frame number of the image from which to begin importing. If not importing images, this parameter should be set to `M_NULL`.

*For specifying the frame number of the image from which to begin importing*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to import the image at the current read position. |
| `Value >= 0` | Specifies the frame number of the image from which to begin importing. Note that the frame number of the first image in the sequence is zero. |

### `NumberOfImages` *(in, AIL_INT)*

Specifies the number of images, starting at [`StartImage`](../../Reference/buf/MbufImportSequence.md), to import. If not importing images, this parameter should be set to `M_NULL`.

### `ControlFlag` *(in, AIL_INT64)*

Specifies the function's control flag. This parameter can be set to one of the following:

*For specifying the function's control flag*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Opens the AVI file, imports the specified images, and then closes the file. |
| `M_CLOSE` | Closes the AVI file, and (re)sets the pointer position to the first image. No images are imported. |
| `M_OPEN` | Opens the AVI file for reading, and sets the pointer to the first image. No images are imported. |
| `M_READ` | Imports the specified images from the AVI file, starting at the specified [`StartImage`](../../Reference/buf/MbufImportSequence.md) position. After the importing the images, the file pointer is left at the position of the next image, ready for the next [`M_READ`](../../Reference/buf/MbufImportSequence.md) operation. |

## Remarks

> When a "remote" path is specified (the [`FileName`](../../Reference/buf/MbufImportSequence.md) parameter begins with "remote:///"), the [`SystemId`](../../Reference/buf/MbufImportSequence.md) parameter must point to the remote system when the AVI file is being opened or closed ([`M_OPEN`](../../Reference/buf/MbufImportSequence.md) or [`M_CLOSE`](../../Reference/buf/MbufImportSequence.md)). The buffers that are receiving the images are allocated on this same remote system.

> Note that during development and at runtime, compression support, particularly for [`M_AVI_MJPG`](../../Reference/buf/MbufImportSequence.md) format and [`M_IMAGE`](../../Reference/buf/MbufAllocColor.md)+[`M_COMPRESS`](../../Reference/buf/MbufAllocColor.md) buffer type, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
