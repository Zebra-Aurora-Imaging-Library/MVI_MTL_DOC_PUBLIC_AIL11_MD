---
doctype: Reference
module: seq
function: MseqControl
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqControl"
---

# MseqControl

| Board | Supported |
| --- | --- |
| Host System | Partial |
| V4L2 | Partial |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Partial |
| GevIQ | Partial |
| GigE Vision | Partial |
| Indio | No |
| Iris GTX | Partial |
| Radient eV-CL | Partial |
| Rapixo CL | Partial |
| Rapixo CoF | Partial |
| Rapixo CXP | Partial |
| USB3 Vision | Partial |

> Control a sequence context or an individual source or destination setting.

## Syntax

```c
void MseqControl(
    AIL_ID     ContextSeqId,   //in
    AIL_INT    SequenceIndex,  //in
    AIL_INT64  ControlType,    //in
    AIL_DOUBLE ControlValue    //in
)
```

## Description

This function allows you to control a sequence context or an individual source or destination setting.

You cannot modify a setting while the sequence processing session is in progress ([`MseqProcess`](../../Reference/seq/MseqProcess.md)).

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context to control. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `SequenceIndex` *(in, AIL_INT)*

Specifies that the sequence context, the source of an input, or a destination of an output is controlled. Set this parameter to one of the following values:

*For specifying a context, or an individual source or destination*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SEQ_INPUT` | Specifies an input of the sequence operation, whose source must be controlled. |
| `M_SEQ_OUTPUT` | Specifies an output of the sequence operation, whose destination must be controlled. An output can have multiple destinations. |
| `M_ALL` | Controls all the sources and destinations in the sequence context. |
| `M_CONTEXT` *(default)* | Controls a setting of the specified sequence context. |

*For specifying the index of the destination of the output to control*

| Value | Description |
| --- | --- |
| `M_SEQ_DEST` | Specifies the destination of the output to control. |

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to control.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the value needed for the setting.

## Parameter Associations

### For specifying the H.264 compression settings

The following [`ControlType`](../../Reference/seq/MseqControl.md) and corresponding [`ControlValue`](../../Reference/seq/MseqControl.md) parameter settings are used to control the H.264 compression operation settings for the sequence context ([`M_CONTEXT`](../../Reference/seq/MseqControl.md)).

---

### `M_BUFFER_SAMPLE`

Sets the size of the images contained in the elementary video stream equal to the size of a sample image buffer.

| Value | Description |
| --- | --- |
| `Buffer identifier` | Specifies the identifier of the sample image buffer. |

---

### `M_SETTING_AUTO_ADJUSTMENT`

Sets whether to automatically adjust incompatible operation settings.  Note that even with automatically adjusted settings, the processor might not be able to attain the specified settings, if the resolution of the images is very large or you are feeding the images too quickly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not automatically adjust incompatible settings. An error will be generated instead. |
| `M_ENABLE` | Specifies to automatically adjust incompatible settings. You can inquire the values used internally, using [`MseqInquire`](../../Reference/seq/MseqInquire.md) with the [`M_EFFECTIVE_VALUE`](../../Reference/seq/MseqInquire.md) combination constant. |

---

### `M_STREAM_BIT_RATE`

Sets the elementary video stream bit rate. The bit rate is the amount of data used to represent one second of the elementary video stream.  When used with a variable bit rate, this value sets the target average bit rate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the bit rate, in Kbits/sec. |

---

### `M_STREAM_BIT_RATE_MAX`

Sets the maximum bit rate that the elementary video stream can have.  When used with a constant bit rate, this value is ignored.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` *(default)* | Specifies the maximum bit rate, in Kbits/sec. |

---

### `M_STREAM_BIT_RATE_MODE`

Sets the type of bit rate the elementary video stream will have.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONSTANT` | Specifies that the bit rate will remain constant throughout the duration of the elementary video stream. You can specify the stream's bit rate using[`M_STREAM_BIT_RATE`](../../Reference/seq/MseqControl.md). |
| `M_VARIABLE` *(default)* | Specifies that the bit rate will fluctuate depending on how compressible sections of the elementary video stream are. You can specify a target average bit rate using [`M_STREAM_BIT_RATE`](../../Reference/seq/MseqControl.md). You can specify the maximum bit rate that the elementary video stream can have using [`M_STREAM_BIT_RATE_MAX`](../../Reference/seq/MseqControl.md). |

---

### `M_STREAM_FRAME_RATE`

Sets the elementary video stream's frame rate. Increasing the frame rate requires a higher bit rate to maintain the stream's quality since more information is being output per second.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the elementary video stream's frame rate, in frames/sec. |

---

### `M_STREAM_FRAME_RATE_MODE`

Sets how the decoding frame rate, written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types, is established.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CONSTANT` *(default)* | Specifies that the decoding frame rate written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types is the same as the frame rate specified with [`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqControl.md). |
| `M_VARIABLE` | Specifies that the decoding frame rate written in the header of [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types is the average encoding frame rate, if the processor could not encode at the specified frame rate; otherwise, the frame rate specified with [`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqControl.md) will be written.  Setting this value should be done if you are uncertain if the processor can encode at the specified frame rate during a live grab. If you are grabbing at a rate of 40 frames/sec, for example, but the processor is encoding at a rate of 20 frames/sec, 20 frames will be missed per second. If you playback the compressed sequence at a rate of 40 frames/sec, the 20 frames that were encoded will be played back unnaturally fast (that is, in the first half-second). By setting this value, the playback rate would be set to 20 frames/sec so that the 20 frames would be played back at a more natural speed (even if there are frames missing). |

---

### `M_STREAM_GROUP_OF_PICTURE_SIZE`

Sets the group-of-pictures size. A group of pictures is defined as the number of P- or B-frames that follow each I-frame. I-frames contain all the data necessary to regenerate the original image that it represents. In contrast, P- and B-frames contain only the changes between consecutive images. Therefore, I-frames are more accurate, but less compressible. Reducing the group-of-pictures size reduces the time it takes to recover from a decrease in quality caused by a dropped frame, but it will also increase the required bit rate.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the group-of-pictures size, in number of frames/group.  Note that specifying a value of 0 will cause the sequence to be composed solely of I-frames (nearly lossless compression). |

---

### `M_STREAM_LEVEL`

Sets the H.264 compression level used by the compression operation. The compression level limits the number of macroblocks per second, which restricts the possible frame rates and bit rates for your elementary video stream. The compression level also limits the image resolution that you can specify. Setting the compression level prevents devices from attempting to playback streams that would require too much processing power and memory to decode. Higher compression levels increase the maximum number of macroblocks per second.  Choose the compression level by calculating the maximum number of macroblocks per second. For information regarding setting the correct compression level, see [Guidelines for setting H.264 compression controls](../../UserGuide/C31_Sequence/S05_Compressing_and_decompressing_a_sequence_of_images.md).

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
| `M_LEVEL_5_2` | Specifies the 5.2 compression level. **[Clarity UHD]** |

---

### `M_STREAM_PROFILE`

Sets the H.264 compression profile. The compression profile determines the allowed complexity of the compression operation. As the complexity is increased, the video quality for a given bit rate will be higher, but more processing power will be required to compress and decompress the elementary video stream.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PROFILE_BASELINE` | Specifies to use the baseline profile. The compression operation will be performed using a low level of complexity. This profile is commonly used for video conferencing and for devices with limited processing power. |
| `M_PROFILE_HIGH` *(default)* | Specifies to use the high profile. The compression operation will be performed using a high level of complexity. This profile is commonly used in high-definition applications (such as HDTV). |
| `M_PROFILE_MAIN` | Specifies to use the main profile. The compression operation will be performed using a mid-level of complexity. |

---

### `M_STREAM_QUALITY`

Sets the H.264 compression operation speed/quality priority.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 100` *(default)* | Specifies the speed/quality priority. 0 sets the highest priority to encoding speed and 100 sets the highest priority to quality. |
