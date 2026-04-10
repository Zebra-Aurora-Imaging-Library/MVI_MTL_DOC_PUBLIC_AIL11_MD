---
doctype: Reference
module: buf
function: MbufExportSequence
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf / MbufExportSequence"
---

# MbufExportSequence

> Export a sequence of image buffers to an AVI file.

## Syntax

```c
void MbufExportSequence(
    AIL_CONST_TEXT_PTR FileName,                  //in
    AIL_INT64          FileFormat,                //in
    const AIL_ID *     BufArrayPtrOrSystemIdPtr,  //in
    AIL_INT            NumOfIds,                  //in
    AIL_DOUBLE         FrameRate,                 //in
    AIL_INT64          ControlFlag                //in
)
```

## Description

This function exports a sequence of image buffers to an audio video interleave (AVI) file.

This function creates a new AVI file, or appends to an existing AVI file. If all your images have been acquired, call this function a single time ([`M_APPEND`](../../Reference/buf/MbufExportSequence.md) or [`M_DEFAULT`](../../Reference/buf/MbufExportSequence.md)) to automatically open (or create) the file, write to it, and then close the file. If your images are being acquired over time, call this function to open (or create) the file ([`M_OPEN`](../../Reference/buf/MbufExportSequence.md) or [`M_OPEN + M_APPEND`](../../Reference/buf/MbufExportSequence.md)), call the function again each time you want to write new images to the file ([`M_WRITE`](../../Reference/buf/MbufExportSequence.md)), and call the function once more to close the file ([`M_CLOSE`](../../Reference/buf/MbufExportSequence.md)).

## Parameters

### `FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the name and path of the AVI file.

*For the name and path of the AVI file*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).

To specify a file on a remote computer (under Distributed Aurora Imaging Library), prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`). |

### `FileFormat` *(in, AIL_INT64)*

Specifies the format of the file. This parameter can be set to one of the values below.

*For specifying the file format*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that Aurora Imaging Library automatically decides the appropriate format. |
| `M_AVI_AIL` | Specifies an AVI format used to hold image buffers in their Aurora Imaging Library format. Since this function saves images in the format in which they are sent, it can be used with any format. Also, since the images are saved "as is", no additional loss is introduced in the images. |
| `M_AVI_DIB` | Specifies an AVI format used to hold non-compressed DIB image buffers. If necessary, the image buffers will be converted to a non-compressed DIB format before exporting. |
| `M_AVI_MJPG` | Specifies an AVI format used to hold JPEG compressed sequences. When this format is specified, the image buffers must be in YUV16 packed format. In addition, image buffers must have a width that is a multiple of 16 pixels. For image buffers that have the [`M_JPEG_LOSSY`](../../Reference/buf/MbufAllocColor.md) compression type, the height must be a multiple of 8, and less than or equal to 240 pixels. For image buffers that have the [`M_JPEG_LOSSY_INTERLACED`](../../Reference/buf/MbufAllocColor.md) compression type, the height must be a multiple of 16 pixels, and greater than 240 pixels. If the image buffers are not already in these dimensions and format, Aurora Imaging Library will automatically convert them appropriately. |

### `BufArrayPtrOrSystemIdPtr` *(in, *AIL_ID)*

Specifies the image buffers to export or, when opening or closing ([`M_OPEN`](../../Reference/buf/MbufExportSequence.md), [`M_OPEN + M_APPEND`](../../Reference/buf/MbufExportSequence.md), or [`M_CLOSE`](../../Reference/buf/MbufExportSequence.md)) a file on a remote computer, an Aurora Imaging Library system allocated on the remote computer.

### `NumOfIds` *(in, AIL_INT)*

Specifies the number of Aurora Imaging Library identifiers passed to the [`BufArrayPtrOrSystemIdPtr`](../../Reference/buf/MbufExportSequence.md) parameter (image buffers or system identifiers).

### `FrameRate` *(in, AIL_DOUBLE)*

Specifies the frame rate (number of images/sec) of the sequence. The frame rate can be specified with any (or every) call to [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md), but it is only checked for validity when closing the AVI file (using [`M_CLOSE`](../../Reference/buf/MbufExportSequence.md), [`M_APPEND`](../../Reference/buf/MbufExportSequence.md), or [`M_DEFAULT`](../../Reference/buf/MbufExportSequence.md)). The last call that specifies a valid frame rate will be used.

*For specifying the frame rate*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies to not set or change the frame rate. |
| `Value > 0` | Specifies the frame rate, in number of frames/sec. |

### `ControlFlag` *(in, AIL_INT64)*

Specifies whether to write, overwrite, or append the image buffers to the AVI file.

## Parameter Associations

### For specifying how to export the sequence

---

### `M_DEFAULT`

Opens the AVI file, overwriting it in the process. The file is opened, written into, and then closed. If no file exists, one is created.

---

### `M_APPEND`

Appends the image buffers to the file without overwriting it. The file is opened, the specified images are appended, and then the file is closed. If no file exists, one is created.

---

### `M_CLOSE`

Closes the AVI file.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the file to be closed is on a local computer. In this case, the file name must not be prefixed with "remote:///". |
| `Remote system identifier` | Specifies the address of a variable containing the identifier of a remote system that is on the same remote computer as the AVI file. In this case, the file name must be prefixed with "remote:///". |

---

### `M_OPEN`

Opens the AVI file, overwriting it in the process. Opens the AVI file for writing, and sets the pointer to the beginning of the file. If no file exists, one will be created.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the file to be opened is on a local computer. In this case, the file name must not be prefixed with "remote:///". |
| `Remote system identifier` | Specifies the address of a variable containing the identifier of a remote system that is on the same remote computer as the AVI file. In this case, the file name must be prefixed with "remote:///". |

---

### `M_OPEN + M_APPEND`

Opens the AVI file, and sets the pointer to the end of the file without overwriting the existing images. New images are appended to the file. If no file exists, one is created.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies that the file to be opened is on a local computer. In this case, the file name must not be prefixed with "remote:///". |
| `Remote system identifier` | Specifies the address of a variable containing the identifier of a remote system that is on the same remote computer as the AVI file. In this case, the file name must be prefixed with "remote:///". |

---

### `M_WRITE`

Writes the specified number of images in the file starting from the current file pointer position. After the write operation, the file pointer is left at the end of the file, ready for the next [`M_WRITE`](../../Reference/buf/MbufExportSequence.md) operation.

## Remarks

> Note that during development and at runtime, compression support, particularly for [`M_AVI_MJPG`](../../Reference/buf/MbufExportSequence.md) format, requires the presence of an Aurora Imaging Library license that grants access to the compression/decompression package. This access is only granted by default with the development license dongle for the full version of Aurora Imaging Library. In other cases, you must purchase access to this package separately.
