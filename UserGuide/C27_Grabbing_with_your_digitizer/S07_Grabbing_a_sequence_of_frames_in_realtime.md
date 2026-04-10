---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Grabbing_a_sequence_of_frames_in_realtime
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Grabbing a sequence of frames in realtime"
---

# Grabbing a sequence of frames in real-time

To grab a sequence of frames in real-time, simply use successive, asynchronous calls to [`MdigGrab`](../../Reference/dig/MdigGrab.md):

> **Code example:** [userguide.grabbing_with_your_digitizer.optimizing_application_performance_when_grabbing](userguide.grabbing_with_your_digitizer.optimizing_application_performance_when_grabbing)

Alternatively, you can make a single call to [`MdigProcess`](../../Reference/dig/MdigProcess.md). For more information, see [Multiple buffering](S08_Grabbing_and_processing.md).

In either case, you must allocate buffers to store the frames of the sequence. After you have grabbed a sequence, you can use the [`MbufExportSequence`](../../Reference/buf/MbufExportSequence.md) function to export the sequence of image buffers (compressed or uncompressed 8-bit) to an AVI file. When exporting, you must specify the number of buffers and the frame rate (number of images/second) of the sequence. Note, the Aurora Imaging Library identifiers of the image buffers to export must be kept in an array.

Use the [`MbufImportSequence`](../../Reference/buf/MbufImportSequence.md) to import a sequence of images from an AVI file into separate image buffers. You can import images in all formats supported by Aurora Imaging Library. You can also choose to import the sequence into automatically allocated buffers or previously allocated buffers.
