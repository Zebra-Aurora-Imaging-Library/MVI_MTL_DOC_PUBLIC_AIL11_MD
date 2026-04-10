---
doctype: UserGuide
part: "2D related information"
chapter: Sequence
section: Steps_to_performing_a_sequence_operation
module_tag: seq
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / Sequence / Steps to performing a sequence operation"
---

# Steps to performing a sequence operation

The following steps provide a basic methodology for using the Aurora Imaging Library Sequence module:

1. Allocate a sequence context, using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md). The sequence context is required to indicate the sequence operation that the Sequence module should perform and to hold the sequence operation settings. When allocating the context, choose the required sequence operation, and select the underlying hardware or software that the operation should use.
2. Define the source of each input and the destination(s) of each output of your sequence operation, using [`MseqDefine`](../../Reference/seq/MseqDefine.md); each output can have multiple destinations. By default, the source of an input is set to [`M_USER_FEED`](../../Reference/seq/MseqDefine.md); in which case, Aurora Imaging Library expects the images to be passed individually (manual image feed). By default, the destination of an output is set to [`M_USER_HOOK`](../../Reference/seq/MseqDefine.md); in which case, Aurora Imaging Library assumes that you will retrieve the images individually from the processing operation's internal output buffer, using a hook function.
3. If the destination of an output is a hook-based retrieval, hook a function to the event that occurs when a frame has finished being processed, using [`MseqHookFunction`](../../Reference/seq/MseqHookFunction.md).
4. Specify the processing operation settings and any additional settings for your sources and destinations, using [`MseqControl`](../../Reference/seq/MseqControl.md).
5. Start the sequence processing session, using [`MseqProcess`](../../Reference/seq/MseqProcess.md)with [`M_START`](../../Reference/seq/MseqProcess.md). You must also specify whether the session will run synchronously or asynchronously. If you specify [`M_SYNCHRONOUS`](../../Reference/seq/MseqProcess.md), your application will wait until the sequence operation has finished before executing the next function; this option is not available if the source of an input is a manual image feed. If you specify [`M_ASYNCHRONOUS`](../../Reference/seq/MseqProcess.md), other functions can execute while the sequence operation is performed in a background thread.
6. If the source of an input is a manual image feed, feed each image to the sequence processing operation, using [`MseqFeed`](../../Reference/seq/MseqFeed.md).
7. Stop the sequence processing session, using [`MseqProcess`](../../Reference/seq/MseqProcess.md) with [`M_STOP`](../../Reference/seq/MseqProcess.md) specifying [`M_WAIT`](../../Reference/seq/MseqProcess.md). The sequence processing session will finish performing the specified operation before stopping.
8. Free your sequence context, using [`MseqFree`](../../Reference/seq/MseqFree.md), unless [`M_UNIQUE_ID`](../../Reference/seq/MseqAlloc.md) was specified during allocation.
