---
doctype: BoardSpecificNotes
part: ""
chapter: Clarity_UHD
section: Using_Clarity_UHD
module_tag: null
product: Aurora-Imaging-Library-board-specific-notes
version: 1100
path: "BoardSpecificNotes /  / clarity-UHD-u36 / Using Clarity UHD"
---

# Using Zebra Clarity UHD

To use any Zebra Clarity UHD, you must allocate it as an Aurora Imaging Library Clarity UHD system using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`M_SYSTEM_CLARITY_UHD`](../../Reference/sys/MsysAlloc.md). This allocation opens communications with your Zebra Clarity UHD and allows Aurora Imaging Library to use its resources. You can allocate an Aurora Imaging Library Clarity UHD system for your board in multiple processes (executables). However, multiple processes cannot allocate a digitizer for the same acquisition path simultaneously.

Zebra Clarity UHD supports the following grab buffer pixel formats:

| Aurora Imaging Library grab buffer format | Description |
| --- | --- |
| 1 band 8-bit | Monochrome 8-bit |
| [`M_YUV16`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | YUV 4:2:2 8-bit |
| [`M_YUV24`](../../Reference/buf/MbufAllocColor.md) + [`M_PLANAR`](../../Reference/buf/MbufAllocColor.md) | YUV 4:4:4 8-bit planar |
| RGB 3 band 8-bit | RGB 8-bit planar |
| [`M_BGR32`](../../Reference/buf/MbufAllocColor.md) + [`M_PACKED`](../../Reference/buf/MbufAllocColor.md) | BGR32 8-bit packed buffer |

You can also grab into a dynamic buffer. For more information, see [Grabbing capabilities](S08_Additional_Clarity_UHD_notes_User_guide.md).

If Zebra Clarity UHD has been purchased with the H.264 encoder option, you can use the Aurora Imaging Library Sequence module to perform on-board H.264 video compression or decompression. To compress a sequence, you must allocate a sequence context using[`MseqAlloc`](../../Reference/seq/MseqAlloc.md)with [`M_SEQ_COMPRESS`](../../Reference/seq/MseqAlloc.md); to decompress a sequence, you must allocate a sequence context using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md)with [`M_SEQ_DECOMPRESS`](../../Reference/seq/MseqAlloc.md). For more information on H.264 video compression and decompression, see [Inputs and outputs of an H.264 compression or decompression](../../UserGuide/C31_Sequence/S05_Compressing_and_decompressing_a_sequence_of_images.md).
