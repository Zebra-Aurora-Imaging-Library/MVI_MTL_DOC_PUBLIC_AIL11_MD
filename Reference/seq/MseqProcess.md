---
doctype: Reference
module: seq
function: MseqProcess
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / seq / MseqProcess"
---

# MseqProcess

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

> Starts or stops a sequence processing session.

## Syntax

```c
void MseqProcess(
    AIL_ID    ContextSeqId,  //in
    AIL_INT64 Command,       //in
    AIL_INT64 CommandFlag    //in
)
```

## Description

This function starts or stops a sequence processing session. The sequence processing session performs the operation specified in the sequence context.

If you define a file ([`M_FILE`](../../Reference/seq/MseqDefine.md)) or buffer list ([`M_BUFFER_LIST`](../../Reference/seq/MseqDefine.md)) as the source of the input for the operation, the sequence processing session performs the operation as soon as the session is started ([`M_START`](../../Reference/seq/MseqProcess.md)). You can specify a synchronous ([`M_SYNCHRONOUS`](../../Reference/seq/MseqProcess.md)) or asynchronous ([`M_ASYNCHRONOUS`](../../Reference/seq/MseqProcess.md)) operation. For synchronous operations, the application will wait until the operation has finished before executing the next function; whereas, for asynchronous operations, other functions can execute while the operation is performed in a background thread.

If you define an image feed as the source ([`M_USER_FEED`](../../Reference/seq/MseqDefine.md)) and call [`MseqProcess`](../../Reference/seq/MseqProcess.md) with [`M_START`](../../Reference/seq/MseqProcess.md), the sequence processing session will wait for an image to be queued using [`MseqFeed`](../../Reference/seq/MseqFeed.md). As soon as an image is queued, the sequence processing session operates on it. If no images are queued, the sequence processing session idles and waits for images from [`MseqFeed`](../../Reference/seq/MseqFeed.md). You can only call [`MseqFeed`](../../Reference/seq/MseqFeed.md) if the session has been started and set to [`M_ASYNCHRONOUS`](../../Reference/seq/MseqProcess.md).

While you cannot start multiple sequence processing sessions simultaneously for a sequence context, you can allocate multiple sequence contexts and start a separate sequence processing session for each context.

To stop the sequence processing session immediately, without finishing the operation, use [`M_STOP`](../../Reference/seq/MseqProcess.md) with [`M_NULL`](../../Reference/seq/MseqProcess.md). To wait for the sequence processing session to complete the operation completely, use [`M_STOP`](../../Reference/seq/MseqProcess.md) with [`M_WAIT`](../../Reference/seq/MseqProcess.md).

> **Note:** H.264 video encoding is optimized for Intel CPUs and can be subject to performance and stability issues when used with other CPUs.

## Parameters

### `ContextSeqId` *(in, AIL_ID)*

Specifies the identifier of the sequence context. The sequence context must have been previously allocated using [`MseqAlloc`](../../Reference/seq/MseqAlloc.md).

### `Command` *(in, AIL_INT64)*

Specifies the command to give the sequence processing session.

### `CommandFlag` *(in, AIL_INT64)*

Specifies the synchronization state of the sequence processing session.

## Parameter Associations

### For starting or stopping the sequence processing session

---

### `M_START`

Specifies to start the sequence processing session.

| Value | Description |
| --- | --- |
| `M_ASYNCHRONOUS` | Specifies for the sequence processing session to run asynchronously. Other functions in your application can execute while the operation is performed in a background thread. |
| `M_SYNCHRONOUS` | Specifies for the sequence processing session to run synchronously. Your application will wait until the operation has finished before executing the next function. |

---

### `M_STOP`

Specifies to stop the sequence processing session.

| Value | Description |
| --- | --- |
| `M_NULL` | Specifies to stop the sequence processing session immediately, cancelling any further sequence processing from occuring. |
| `M_WAIT` | Specifies to stop the sequence processing session only after it has finished performing the specified operation. |
