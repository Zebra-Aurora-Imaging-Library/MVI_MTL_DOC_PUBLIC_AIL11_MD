---
doctype: UserGuide
part: "2D related information"
chapter: Sequence
section: Compressing_and_decompressing_a_sequence_of_images
module_tag: seq
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / Sequence / Compressing and decompressing a sequence of images"
---

# Compressing and decompressing a sequence of images

The Sequence module allows you to compress and decompress sequences of images using the H.264 standard. To compress a sequence, you must allocate a sequence context using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md)with [`M_SEQ_COMPRESS`](../../Reference/seq/MseqAlloc.md); to decompress a sequence, you must allocate a sequence context using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md)with [`M_SEQ_DECOMPRESS`](../../Reference/seq/MseqAlloc.md). When allocating the sequence context, you must also specify whether the compression and decompression will be performed in hardware or software. By default, Aurora Imaging Library will select the most appropriate option for your computer. If your processor supports Intel's Quick Sync Video (QSV), the compression and decompression will be hardware accelerated. If your processor does not have a QSV engine, the Sequence module will perform the compression or decompression in software.

> **Note:** H.264 video encoding is optimized for Intel CPUs and can be subject to performance and stability issues when used with other CPUs.

H.264 compression algorithms achieve high compression ratios by eliminating redundant information between frames. In many videos, changes from one frame to the next are relatively minor. Objects present in one frame are often present in the next. Even if they have moved, the pixels forming the objects themselves are the same. Therefore, it is a waste to allocate space for these unchanging objects in every frame. H.264 achieves compression by dividing the frames into groups, where the first frame of the group, known as the I-frame, contains all the data necessary to regenerate the original image that it represents. The other frames in the group are composed of B- and P-frames, which are defined relative to I-frames or other B- and P- frames. H.264 compression further segments frames into macroblocks, and only the differences in macroblocks between frames are recorded in the B- and P-frames.

## Inputs and outputs of an H.264 compression or decompression

Both a sequence compression and a sequence decompression operation take a single input and produce a single output.

The compression operation outputs an elementary video stream composed of fully defined I-frames, and referential P- and B-frames. The data of these frames can be retrieved individually in raw format from the internal output buffer using a hook function and/or saved as a sequence in raw format in an H.264 elementary video data file ([`M_FILE_FORMAT_H264`](../../Reference/seq/MseqDefine.md)). The frames can also be saved as a sequence wrapped in an AVI ([`M_FILE_FORMAT_AVI`](../../Reference/seq/MseqDefine.md)) or MP4 ([`M_FILE_FORMAT_MP4`](../../Reference/seq/MseqDefine.md)) video container file. Viewing the H.264 file (in either its raw form or in a video container format) on a computer requires the use of an H.264 compliant-third party media player such as VLC.

The decompression operation outputs an elementary video stream composed entirely of fully defined uncompressed images. You can access each image individually using a hook function or save them in an AVI file or buffer list.

## Controlling an H.264 compression

Once you have allocated your sequence context for a compression operation, you can customize the compression operation using [`MseqControl`](../../Reference/seq/MseqControl.md). For example, to fit the requirements of your application, you can select the appropriate:

- Frame rate of the elementary video stream.
- Resolution of the I-frames.
- Bit rate of the elementary video stream.
- Compression level.
- Compression profile.
- Compression priority.
- Group-of-pictures size.

### Frame rate

An elementary video stream is composed of a sequence of images called frames. The number of frames per second of video is referred to as the frame rate. In some applications, a low frame rate will suffice. For example, a slideshow with an image being displayed every five seconds will have a frame rate of 0.2 frames per second (frames/sec). In other applications such as movies, a higher frame rate is required to give viewers the illusion of movement. Increasing the frame rate requires a higher bit rate to maintain the stream's quality since more information is being output per second. You can set the frame rate using [`MseqControl`](../../Reference/seq/MseqControl.md)with[`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqControl.md).

If you are using [`M_FILE`](../../Reference/seq/MseqDefine.md) destination types, and are uncertain if the processor can encode at the specified frame rate during a live grab, you can use [`MseqControl`](../../Reference/seq/MseqControl.md) with [`M_STREAM_FRAME_RATE_MODE`](../../Reference/seq/MseqControl.md) set to [`M_VARIABLE`](../../Reference/seq/MseqControl.md) to have the average encoding frame rate written in the header of the file as the decoding frame rate. This will allow playback at more natural speeds. For example, if you are grabbing at a rate of 40 frames/sec but the processor is encoding at a rate of 20 frames/sec, 20 frames will be missed per second. If you playback the compressed sequence at a rate of 40 frames/sec, the 20 frames that were encoded will be played back unnaturally fast (that is, in the first half-second). Using [`M_STREAM_FRAME_RATE_MODE`](../../Reference/seq/MseqControl.md) set to [`M_VARIABLE`](../../Reference/seq/MseqControl.md), would set the playback rate to 20 frames/sec so that the 20 frames would be played back at a more natural speed (even if there are frames missing).

### Resolution

You can set the size of the images to compress into the elementary video stream by specifying a sample image buffer. Passing a sample image buffer is useful because it improves the performance of the sequence operation, by initializing the engine before the first image is fed. Set the resolution of your images by passing a sample image buffer using [`MseqControl`](../../Reference/seq/MseqControl.md) with [`M_BUFFER_SAMPLE`](../../Reference/seq/MseqControl.md). Note that this operation only saves information about the image and does not save the image itself.

### Bit rate

The bit rate is the amount of data, in kbits, used to represent one second of the elementary video stream. You can determine the total size of the elementary stream in kbits by multiplying its average bit rate by its duration in seconds.

You can choose either a constant or variable bit rate. If you choose a constant bit rate using [`MseqControl`](../../Reference/seq/MseqControl.md)with[`M_STREAM_BIT_RATE_MODE`](../../Reference/seq/MseqControl.md)set to[`M_CONSTANT`](../../Reference/seq/MseqControl.md), the same number of bits will be used to represent each second of video. In this case, you must specify the bit rate value using [`M_STREAM_BIT_RATE`](../../Reference/seq/MseqControl.md).

If you choose a variable bit rate using [`M_VARIABLE`](../../Reference/seq/MseqControl.md), the bit rate will fluctuate and a different number of bits will be used to represent each second of the elementary video stream. In this case, you must specify the target average bit rate using [`M_STREAM_BIT_RATE`](../../Reference/seq/MseqControl.md).

It is usually more efficient to use a variable bit rate instead of a constant bit rate because some sections of an elementary video stream are generally more compressible than others. For example, a scene with a lot of movement is less compressible than a scene that is mainly stationary. With a variable bit rate, more bits will be allocated to the less compressible sections of the video, and less bits will be allocated to the more compressible sections of the video. When using a variable bit rate, you must also specify the maximum possible bit rate the elementary video stream can have using [`M_STREAM_BIT_RATE_MAX`](../../Reference/seq/MseqControl.md). This will limit the bit rates of less compressible sections of video. This is important because if the bit rate exceeds the available bandwidth of an Internet or network connection, the video will lag. [`M_STREAM_BIT_RATE_MAX`](../../Reference/seq/MseqControl.md)is ignored if you use a constant bit rate.

> **Note:** The maximum bit rate of your elementary video stream should not exceed the maximum bit rate allowed by the specified compression level and profile. For information on appropriate bit rate values, see [Guidelines for setting H.264 compression controls](S05_Compressing_and_decompressing_a_sequence_of_images.md).

### Compression level

H.264 compression divides each frame into groups of 256 pixels (16x16) called macroblocks. The compression level limits the number of macroblocks per second, which restricts the possible frame rate and resolution that you can specify for your elementary video stream. The compression level also limits the bit rate that you can specify. Setting the compression level prevents devices from attempting to playback streams that would require too much processing power and memory to decode. You can set the compression level using [`MseqControl`](../../Reference/seq/MseqControl.md)with [`M_STREAM_LEVEL`](../../Reference/seq/MseqControl.md). For information on setting the correct compression level, see [Guidelines for setting H.264 compression controls](S05_Compressing_and_decompressing_a_sequence_of_images.md).

### Compression profile

The compression profile determines the allowed complexity of the compression operation. For example, a high profile ([`M_PROFILE_HIGH`](../../Reference/seq/MseqControl.md)) will allow the compression operation to make use of the more sophisticated features of the H.264 standard. This will improve the compression ratio, but more processing power will be required to decode the elementary video stream. It might be necessary to chose a lower profile if the stream might be played back on devices with limited processing power. To set the profile, use [`MseqControl`](../../Reference/seq/MseqControl.md)with [`M_STREAM_PROFILE`](../../Reference/seq/MseqControl.md).

### Compression priority

The compression priority determines whether more importance is placed on reducing the time it takes to compress the sequence or maximizing the quality of the resulting elementary video stream. You can set the compression priority using [`MseqControl`](../../Reference/seq/MseqControl.md)with[`M_STREAM_QUALITY`](../../Reference/seq/MseqControl.md).

### Group-of-pictures size

A group of pictures contains one I-frame followed by several P- and B- frames. I-frames contain all the data necessary to regenerate the original image that it represents. In contrast, P- and B-frames contain only the changes between consecutive images. Therefore, I-frames are more accurate, but less compressible. Reducing the group-of-pictures size reduces the time it takes to recover from a decrease in quality caused by a dropped frame, but it will also increase the required bit rate. In most cases, the default size of the group of pictures ([`MseqControl`](../../Reference/seq/MseqControl.md)with [`M_STREAM_GROUP_OF_PICTURE_SIZE`](../../Reference/seq/MseqControl.md)) is appropriate. However, if you want more control over your compression, you can specify a custom group-of-pictures size.

## Guidelines for setting H.264 compression controls

The frame rate, bit rate, compression profile, and compression level of your H.264 elementary video stream are interdependent and specifying a certain value for one of these settings can restrict the range of acceptable values for the other settings, as well as supported image resolutions. For example, as previously discussed, the compression level will limit the maximum possible bit rate of your stream. If you specify a bit rate not supported by the compression level, or specify any set of incompatible settings, an error will be generated or your settings will be internally modified, potentially producing unintended results. Therefore, you should take note of the following guidelines when setting up your H.264 compression.

> **Note:** If you set [`M_SETTING_AUTO_ADJUSTMENT`](../../Reference/seq/MseqControl.md) to [`M_DISABLE`](../../Reference/seq/MseqControl.md), using [`MseqControl`](../../Reference/seq/MseqControl.md), the Sequence module will not automatically adjust incompatible settings; instead, it will generate an error. If you set [`M_SETTING_AUTO_ADJUSTMENT`](../../Reference/seq/MseqControl.md) to [`M_ENABLE`](../../Reference/seq/MseqControl.md), the Sequence module will automatically adjust incompatible settings. In this case, you can inquire the values used internally using [`MseqInquire`](../../Reference/seq/MseqInquire.md) with the [`M_EFFECTIVE_VALUE`](../../Reference/seq/MseqInquire.md) combination constant.

In general, you should first specify the required frame rate for your elementary video stream ([`M_STREAM_FRAME_RATE`](../../Reference/seq/MseqControl.md)) and, if possible, the resolution of your images ([`M_BUFFER_SAMPLE`](../../Reference/seq/MseqControl.md)). With these settings, you can calculate the number of macroblocks per second it will contain, using the following equation:

`_MacroblocksPerSecond_ = `ceil`(_Width_/16) * `ceil`(_Height_/16) * _FrameRate_`

Once you calculate the number of macroblocks per second, refer to the table below to choose an appropriate compression level. For example, if your resolution is 320×240 pixels and your frame rate is 36.0 frames/sec, your stream will contain 10,800 macroblocks per second, and you must set your compression level to 1.3 or higher. Next, determine the compression profile that should be used by the compression operation based on how computationally intensive you want the decoding process to be. You can then refer to your compression level to specify a bit rate for your elementary video stream.

Although the compression level specifies the maximum bit rate allowed at a given frame rate and resolution, it is often possible to achieve high quality with a lower bit rate. If the quality of the resulting stream is not satisfactory, you can raise the bit rate or lower the resolution and/or frame rate of your stream. Finally, you can choose an appropriate compression priority, and if necessary, a group-of-pictures size.

The following table indicates the maximum bit rate and number of macroblocks per second for each H.264 compression level. Sample resolution and frame rate combinations are provided for each compression level.

|  |
||
|  |
| 1 | 1,485 | 64 | 80 | - 128×96@30.9.<br/>- 176×144@15.0. |
| 1b | 1,485 | 128 | 160 | - 128×96@30.9.<br/>- 176×144@15.0. |
| 1.1 | 3,000 | 192 | 240 | - 176×144@30.3.<br/>- 320×240@10.0.<br/>- 352×288@7.5. |
| 1.2 | 6,000 | 384 | 480 | - 320×240@20.0.<br/>- 352×288@15.2. |
| 1.3 | 11,880 | 768 | 960 | - 320×240@36.0.<br/>- 352×288@30.0. |
| 2 | 11,880 | 2,000 | 2,500 | - 320×240@36.0.<br/>- 352×288@30.0. |
| 2.1 | 19,800 | 4,000 | 5,000 | - 352×480@30.0. |
| 2.2 | 20,250 | 4,000 | 5,000 | - 352×480@30.7.<br/>- 720×576@12.5. |
| 3 | 40,500 | 10,000 | 12,500 | - 352×480@61.4.<br/>- 720×576@25.0. |
| 3.1 | 108,000 | 14,000 | 17,500 | - 720×576@66.7.<br/>- 1280×720@30.0. |
| 3.2 | 216,000 | 20,000 | 25,000 | - 1,280×720@60.0.<br/>- 1,280×1,024@42.2. |
| 4 | 245,760 | 20,000 | 25,000 | - 1,280×720@68.3.<br/>- 1,920×1,080@30.1. |
| 4.1 | 245,760 | 50,000 | 62,500 | - 1,280×720@68.3.<br/>- 1,920×1,080@30.1. |
| 4.2 | 522,240 | 50,000 | 62,500 | - 1,280×720@145.1.<br/>- 1,920×1,080@64.0.<br/>- 2,048×1,080@60.0. |
| 5 | 589,824 | 135,000 | 168,750 | - 1,920×1,080@72.3.<br/>- 2,048×1,080@67.8. |
| 5.1 | 983,040 | 240,000 | 300,000 | - 1,920×1,080@120.5.<br/>- 4,096×2,304@26.7. |
