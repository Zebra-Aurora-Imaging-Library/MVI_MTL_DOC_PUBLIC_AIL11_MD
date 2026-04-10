---
doctype: Reference
module: seq
function: MseqInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqInquire"
---

# MseqInquire

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

> Inquire information about a sequence context or an individual source or destination setting.

## Syntax

```c
AIL_INT MseqInquire(
    AIL_ID    ContextSeqId,   //in
    AIL_INT   SequenceIndex,  //in
    AIL_INT64 InquireType,    //in
    void *    UserVarPtr      //out
)
```

## Description

This function inquires information about a sequence context or an individual source or destination setting.

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the sequence context about which to inquire information. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `SequenceIndex` *(in, AIL_INT)*

Specifies that the sequence context, the source of an input, or a destination of an output is inquired. Set this parameter to one of the following values:

*For specifying a context, or an individual source or destination*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SEQ_INPUT` | Specifies an input of the sequence operation, whose source must be inquired. |
| `M_SEQ_OUTPUT` | Specifies an output of the sequence operation, whose destination must be inquired. An output can have multiple destinations. |
| `M_CONTEXT` *(default)* | Inquires information about the specified sequence context. |

*For specifying the index of the destination of the output to inquire*

| Value | Description |
| --- | --- |
| `M_SEQ_DEST` | Specifies the destination of the output to inquire. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MseqInquire`](../../Reference/seq/MseqInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`, except when the specified [`InquireType`](../../Reference/seq/MseqInquire.md) requires the [`UserVarPtr`](../../Reference/seq/MseqInquire.md) parameter to be set to the address of an _AIL_INT_,_AIL_DOUBLE_, or _AIL_TEXT_CHAR_. In this case, you must set this parameter to the address of an _AIL_INT_,_AIL_DOUBLE_, or _AIL_TEXT_CHAR_, respectively.

## Parameter Associations

### For retrieving information about the H.264 compression operation settings

The following inquire types allow you to inquire the H.264 compression settings for the sequence context ([`M_CONTEXT`](../../Reference/seq/MseqInquire.md)).

---

### `M_CODEC_TYPE`

Inquires the type of codec used for the compression or decompression operation.

| Value | Description |
| --- | --- |
| `M_QSV + M_HARDWARE` | Specifies to perform the operation using hardware acceleration. |
| `M_QSV + M_SOFTWARE` | Specifies to perform the operation in software. |

---

### `M_SETTING_AUTO_ADJUSTMENT`

Inquires whether incompatible operation settings are automatically adjusted.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not automatically adjust incompatible settings. |
| `M_ENABLE` | Specifies to automatically adjust incompatible settings. |

---

### `M_SIZE_X`

Inquires the width of the elementary video stream.

| Value | Description |
| --- | --- |
| `Value` | Specifies the width, in pixels. |

---

### `M_SIZE_Y`

Inquires the height of the elementary video stream.

| Value | Description |
| --- | --- |
| `Value` | Specifies the height, in pixels. |

---

### `M_STREAM_BIT_RATE`

Inquires the elementary video stream bit rate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the bit rate, in Kbits/sec. |

---

### `M_STREAM_BIT_RATE_MAX`

Inquires the maximum bit rate that the elementary video stream can have.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum bit rate, in Kbits/sec. |

---

### `M_STREAM_BIT_RATE_MODE`

Inquires the type of bit rate the elementary video stream will have.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONSTANT` | Specifies that the bit rate will remain constant throughout the duration of the elementary video stream. |
| `M_VARIABLE` *(default)* | Specifies that the bit rate will fluctuate depending on how compressible sections of the elementary video stream are. |

---

### `M_STREAM_FRAME_RATE`

Inquires the frame rate of the elementary video stream.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the elementary video stream's frame rate, in frames/sec. |

---

### `M_STREAM_FRAME_RATE_MODE`

Inquires how the decoding frame rate, written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types, is established.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONSTANT` *(default)* | Specifies that the decoding frame rate written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types is the same as the frame rate specified with [`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqInquire.md). |
| `M_VARIABLE` | Specifies that the decoding frame rate written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types is the average encoding frame rate, if the processor could not encode at the specified frame rate; otherwise, the frame rate specified with [`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqInquire.md) will be written. |

---

### `M_STREAM_GROUP_OF_PICTURE_SIZE`

Inquires the group-of-pictures size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the group-of-pictures size, in number of frames/group. |

---

### `M_STREAM_LEVEL`

Inquires the H.264 compression level used by the compression operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_LEVEL_1` | Specifies the 1 compression level. |
| `M_LEVEL_1_1` | Specifies the 1.1 compression level. |
| `M_LEVEL_1_2` | Specifies the 1.2 compression level. |
| `M_LEVEL_1_3` | Specifies the 1.3 compression level. |
| `M_LEVEL_1B` | Specifies the 1B compression level. |
| `M_LEVEL_2` | Specifies the 2 compression level. |
| `M_LEVEL_2_1` | Specifies the 2.1 compression level. |
| `M_LEVEL_2_2` | Specifies the 2.2 compression level. |
| `M_LEVEL_3` | Specifies the 3 compression level. |
| `M_LEVEL_3_1` | Specifies the 3.1 compression level. |
| `M_LEVEL_3_2` | Specifies the 3.2 compression level. |
| `M_LEVEL_4` | Specifies the 4 compression level. |
| `M_LEVEL_4_1` | Specifies the 4.1 compression level. |
| `M_LEVEL_4_2` *(default)* | Specifies the 4.2 compression level. |
| `M_LEVEL_5` | Specifies the 5 compression level. |
| `M_LEVEL_5_1` | Specifies the 5.1 compression level. |
| `M_LEVEL_5_2` | Specifies the 5.2 compression level. |

---

### `M_STREAM_PROFILE`

Inquires the H.264 compression profile.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE_BASELINE` | Specifies to use the baseline profile. |
| `M_PROFILE_HIGH` *(default)* | Specifies to use the high profile. |
| `M_PROFILE_MAIN` | Specifies to use the main profile. |

---

### `M_STREAM_QUALITY`

Inquires the H.264 compression operation priority.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the speed/quality priority. |

### Combination Constants — For inquiring the actual values used

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the actual values used if [`MseqControl`](../../Reference/seq/MseqControl.md) with [`M_SETTING_AUTO_ADJUSTMENT`](../../Reference/seq/MseqControl.md) was set to [`M_ENABLE`](../../Reference/seq/MseqControl.md).

| Value | Description |
| --- | --- |
| `M_EFFECTIVE_VALUE` | Inquires the actual value used by the compression operation. |

### For retrieving information about general individual source or destination settings

You can inquire information about the following settings for any type of individual source (**M_SEQ_INPUT()**) or destination (**M_SEQ_OUTPUT()**+**M_SEQ_DEST()**).

---

### `M_TYPE`

Inquires the type of source or destination at the specified index.

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the drive, directory, and name of the file (for example, _"C:\mydirectory\myfile"_). |
| `M_FILE_FORMAT_AVI` | Specifies that the file is in AVI format. |
| `M_FILE_FORMAT_H264` | Specifies that the file is in H.264 elementary video data format. |
| `M_FILE_FORMAT_MP4` | Specifies that the file is in MP4 format. |

### For retrieving information about individual file (

You can inquire information about the following settings for individual file sources or destinations (**M_SEQ_INPUT()** or **M_SEQ_OUTPUT()**+**M_SEQ_DEST()** defined as [`M_FILE`](../../Reference/seq/MseqDefine.md)).

---

### `M_FILE_FORMAT`

Inquires the format of the file at the specified index.

| Value | Description |
| --- | --- |
| `M_FILE_FORMAT_AVI` | Specifies that the file is in AVI format. |
| `M_FILE_FORMAT_H264` | Specifies that the file is in H.264 elementary video data format. |
| `M_FILE_FORMAT_MP4` | Specifies that the file is in MP4 format. |

---

### `M_STREAM_FILE_NAME`

Inquires the name of the file at the specified index.

| Value | Description |
| --- | --- |
| `Value` | Specifies the file name. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
