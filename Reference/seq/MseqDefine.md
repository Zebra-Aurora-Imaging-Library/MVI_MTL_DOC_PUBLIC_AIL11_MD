---
doctype: Reference
module: seq
function: MseqDefine
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqDefine"
---

# MseqDefine

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

> Define, or redefine, a source or destination of an input or output of the sequence operation, respectively.

## Syntax

```c
void MseqDefine(
    AIL_ID       ContextSeqId,   //in
    AIL_INT      SequenceIndex,  //in
    AIL_INT64    SequenceType,   //in
    const void * Param1Ptr,      //in
    AIL_DOUBLE   Param2          //in
)
```

## Description

This function defines, or redefines, the source of an input or a destination of an output of the sequence operation of the specified sequence context. The source of an input specifies from where to obtain the data to be operated on by the sequence processing operation, while the destinations of an output specify where to store data resulting from the sequence processing operation.

You can define multiple destinations for each output of the operation, but only one source for each input of the operation. The number of inputs and outputs are operation dependent. Both a sequence compression and a sequence decompression take only one input and have only one output.

If you specify an index that has already been defined, that index will be associated with the new information. If you do not want to use a destination at a specified index, redefine the destination as [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md). If you do not want to use a source at a specified index, redefine the source as [`M_USER_FEED`](../../Reference/seq/MseqDefine.md).

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `SequenceIndex` *(in, AIL_INT)*

Specifies whether to define, or redefine, the source of an input or one of the destinations of an output for the sequence operation of the specified sequence context.

*For specifying an input or output of the sequence operation at a given index*

| Value | Description |
| --- | --- |
| `M_SEQ_INPUT` | Specifies an input of the sequence operation, whose source must be defined or redefined. |
| `M_SEQ_OUTPUT` | Specifies an output of the sequence operation, whose destination must be defined or redefined. An output can have multiple destinations. |

*For specifying the index of the destination of the output to define or redefine*

| Value | Description |
| --- | --- |
| `M_SEQ_DEST` | Specifies the destination of the output, to define or redefine. |

### `SequenceType` *(in, AIL_INT64)*

Specifies the type of source or destination to define.

### `Param1Ptr` *(in, *void)*

Specifies an attribute of the source or destination to define. Its definition is dependent on the sequence type chosen.

### `Param2` *(in, AIL_DOUBLE)*

Specifies an attribute of the source or destination to define. Its definition is dependent on the sequence type chosen.

## Parameter Associations

### For specifying the source or destination settings

To specify the settings of the source or destination, set the [`Param1Ptr`](../../Reference/seq/MseqDefine.md) and [`Param2`](../../Reference/seq/MseqDefine.md)parameters to the following values. Note that any unused parameters should be set to [`M_NULL`](../../Reference/seq/MseqDefine.md).

---

### `M_BUFFER_LIST`

Defines the source of the input, or destination of the output, of the sequence operation as a buffer list.  > **Note:** A buffer list cannot be used as the source of the input for a decompression operation or as the destination of the output for a compression operation

---

### `M_FILE`

Defines the source of the input, or destination of the output, of the sequence operation as a file.

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_).  If the sequence context is allocated on a remote system (under Distributed Aurora Imaging Library), you must specify a file on the same remote computer. To do so, prefix the specified file name string with `"remote:///"` (for example, `"remote:///C:\mydirectory\myfile"`); otherwise, an error will occur. |
| `M_FILE_FORMAT_AVI` | Specifies that the file is in AVI format. |
| `M_FILE_FORMAT_H264` | Specifies that the file is in H.264 elementary video data format.  > **Note:** An H.264 elementary video file cannot be used as a destination for a decompression operation. |
| `M_FILE_FORMAT_MP4` | Specifies that the file is in MP4 format. |

---

### `M_USER_FEED`

Defines the source of the input of the sequence operation as a manual image feed, where the images are passed individually to the sequence operation. To perform the feed, call [`MseqFeed`](../../Reference/seq/MseqFeed.md)after you start the sequence processing operation using [`MseqProcess`](../../Reference/seq/MseqProcess.md). By default, the source of each input of the operation is an image feed.

---

### `M_USER_HOOK`

Defines the destination of the output as the internal output buffer of the operation; if this is the only destination, you should use a user-defined hook function to retrieve the data before it is overwritten by the next frame of resulting data ([`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md) with [`M_FRAME_END`](../../Reference/seq/MseqHookFunction.md)). In the hook function, use [`MseqGetHookInfo`](../../Reference/seq/MseqGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/seq/MseqGetHookInfo.md)+[`M_BUFFER_ID`](../../Reference/seq/MseqGetHookInfo.md) to retrieve the identifier of the internal output buffer. By default, the destination of each output is set to [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md).  You can use hook functions to retrieve data about the internal output buffer, regardless of whether [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md) is specified.
