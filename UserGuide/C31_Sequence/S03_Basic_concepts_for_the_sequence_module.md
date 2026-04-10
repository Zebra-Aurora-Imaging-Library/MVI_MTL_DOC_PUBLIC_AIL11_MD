---
doctype: UserGuide
part: "2D related information"
chapter: Sequence
section: Basic_concepts_for_the_sequence_module
module_tag: seq
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / Sequence / Basic concepts for the sequence module"
---

# Basic concepts for the Aurora Imaging Library Sequence module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Sequence module are:

- **Destination**. The location in which the output of the specified operation is saved.
- **Elementary video stream**. A video composed of a sequence of frames containing no audio component.
- **Video container format**. A format that can contain an elementary video stream, as well as other elements such as audio and subtitles. AVI and MP4 are video container formats.
- **Frame**. One of the successive images forming a video.
- **H.264 compression**. An advanced compression standard that uses predictive frames to maximize compression efficiency.
- **I-frame**. A frame containing all the data necessary to decode it independently of other frames. I-frames are the least compressible H.264 frames.
- **Operation**. An action performed on a certain number of inputs and having a certain number of outputs. The H.264 compression and decompression operations each have one input and one output.
- **Predictive frame (P-frame) and bi-predictive frame (B-frame)**. A frame contained within an elementary video stream that cannot be indepently decoded into a full image. It must refer to other frames in the stream during playback to be properly decoded. P- and B-frames are more compressible than I-frames.
- **Source**. The location from which the input of the operation is taken.
