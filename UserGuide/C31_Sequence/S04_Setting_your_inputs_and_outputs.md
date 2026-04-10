---
doctype: UserGuide
part: "2D related information"
chapter: Sequence
section: Setting_your_inputs_and_outputs
module_tag: seq
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / Sequence / Setting your inputs and outputs"
---

# Setting your inputs and outputs

Once you have allocated your sequence context and specified the operation to perform, you must set the sources and destinations of the inputs and outputs of your operation, respectively, using [`MseqDefine`](../../Reference/seq/MseqDefine.md). The specified operation is characterized by a certain number of inputs and a certain number of outputs. Each input can only take one source, but each output can be saved in multiple destinations.

To define the source of an input, choose the source of which input to define using [`M_SEQ_INPUT()`](../../Reference/seq/MseqDefine.md)and then select the type of its source. To define a destination of an output, use [`M_SEQ_OUTPUT()`](../../Reference/seq/MseqDefine.md)+[`M_SEQ_DEST()`](../../Reference/seq/MseqDefine.md) to select which destinations of an output to set and then select the type of this destination; each output can have 32 destinations. The source of the input can be a file ([`M_FILE`](../../Reference/seq/MseqDefine.md)), a buffer list ([`M_BUFFER_LIST`](../../Reference/seq/MseqDefine.md)), or a manual image feed, where the images are passed individually ([`M_USER_FEED`](../../Reference/seq/MseqDefine.md)). The destination(s) of an output can be files, buffer lists, or the data can be retrieved from the internal output buffer of the sequence processing operation using a hook function ([`M_USER_HOOK`](../../Reference/seq/MseqDefine.md)). A buffer list is essentially a list of image buffers, whose individual buffer identifiers are passed using a user array. AVI, MP4, and elementary H.264 are supported formats for file sources and destinations.

Depending on the operation, certain types might not be supported as a source of an input or a destination of an output; for example, a buffer list cannot be used as the source of an input for a decompression operation or as the destination of an output for a compression operation. By default, when you allocate your sequence context, Aurora Imaging Library defines an [`M_USER_FEED`](../../Reference/seq/MseqDefine.md)source for each input and an [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md)destination for your output(s).

If the source of an input is set to a file or buffer list, the operation will be performed as soon as the sequence processing operation is started using [`MseqProcess`](../../Reference/seq/MseqProcess.md). However, if the source is an image feed, you must perform additional steps after starting the operation.

If the source of an input is an image feed, you must feed each image individually. Feeding your images individually is useful when your images are available gradually. For example, you might want to perform the sequence processing operation on an image immediately after it has been grabbed, preprocessed, and/or analyzed. To feed an image, call [`MseqFeed`](../../Reference/seq/MseqFeed.md) with the identifier of the buffer in which the image is stored. As soon as an image is fed, it is copied to the internal queue of the sequence processing operation where it waits to be operated on. Images can only be fed if [`M_ASYNCHRONOUS`](../../Reference/seq/MseqProcess.md)is specified when starting the operation.

If using [`MdigProcess`](../../Reference/dig/MdigProcess.md)to grab, process, and/or analyze each image on which you want to perform a sequence processing operation (for example, sequence compression), you can make the call to [`MseqFeed`](../../Reference/seq/MseqFeed.md)from the hook function of [`MdigProcess`](../../Reference/dig/MdigProcess.md). If your images are fed faster than they are operated on by the sequence operation, the queue will fill and [`MseqFeed`](../../Reference/seq/MseqFeed.md)will wait for the queue to free up before returning control to the calling thread. This will cause your processing function to stall, and frames might be dropped.

Once images are operated on by the sequence processing operation, they are written to an internal output buffer before being copied to the specified destination. If you set [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md) as the destination and this is the only destination, the output of the operation will be lost unless you retrieve it with a hook function. You can use [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md) with [`M_FRAME_END`](../../Reference/seq/MseqHookFunction.md)to hook a user-defined function that will be called every time the internal output buffer is modified. You can call [`MseqGetHookInfo`](../../Reference/seq/MseqGetHookInfo.md) with [`M_MODIFIED_BUFFER`](../../Reference/seq/MseqGetHookInfo.md)+[`M_BUFFER_ID`](../../Reference/seq/MseqGetHookInfo.md)from within your user-defined function to inquire the Aurora Imaging Library identifier of the internal output buffer.

> **Note:** You can use hook functions to retrieve data about the internal output buffer, even if a file and/or buffer list destination is specified.
