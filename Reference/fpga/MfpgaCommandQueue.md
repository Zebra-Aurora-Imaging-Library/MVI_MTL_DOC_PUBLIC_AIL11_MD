---
doctype: Reference
module: fpga
function: MfpgaCommandQueue
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga / MfpgaCommandQueue"
---

# MfpgaCommandQueue

| Board | Supported |
| --- | --- |
| Host System | No |
| V4L2 | No |
| Clarity UHD | No |
| Concord PoE | No |
| GenTL | No |
| GevIQ | No |
| GigE Vision | No |
| Indio | No |
| Iris GTX | No |
| Radient eV-CL | No |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | No |

> Put an FPGA command on the system command queue of the current thread.

## Syntax

```c
void MfpgaCommandQueue(
    AIL_FPGA_CONTEXT FpgaCommandContext,  //in
    AIL_INT64        CompletionMode,      //in
    AIL_INT64        QueueType            //in
)
```

## Description

This function sends the command, defined by the specified FPGA command context, to the system command queue of the current thread. The command will be executed when the target Processing FPGA hardware resources are available.

> **Note:** Note that the FPGA module is not supported with Distributed Aurora Imaging Library.

> **Note:** Note that the FPGA module is only supported on boards that support FPGA processing (**Pro** boards).

If you link multiple command contexts, their commands should be gathered in a complex command, using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_WAIT`](../../Reference/fpga/MfpgaCommandQueue.md). Add the last command to the complex command using [`M_DISPATCH`](../../Reference/fpga/MfpgaCommandQueue.md). If multiple commands are being dispatched at the same time (as in cascaded or parallel scenarios), then Aurora Imaging Library uses the synchronous/asynchronous setting and the completion mode of the last command (that is, the one with the call to [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_DISPATCH`](../../Reference/fpga/MfpgaCommandQueue.md)). Set the completion mode for all other commands in the complex command to [`M_DEFAULT`](../../Reference/fpga/MfpgaCommandQueue.md).

> **Note:** Note that two commands can typically run at the same time if they do not reference the same buffer and use different FPGA components that can access their buffers using different paths. When a command is sent asynchronously, you can use [`MthrWait`](../../Reference/thr/MthrWait.md) with [`M_THREAD_WAIT`](../../Reference/thr/MthrWait.md) to force the current thread to wait for the completion of all commands in the thread's system command queue. Alternatively, you can use [`MbufHookFunction`](../../Reference/buf/MbufHookFunction.md) with [`M_MODIFIED_BUFFER`](../../Reference/buf/MbufHookFunction.md) to notify your Aurora Imaging Library application when the operation has finished processing the image.

## Parameters

### `FpgaCommandContext` *(in, AIL_FPGA_CONTEXT)*

Specifies the handle of the FPGA command context associated with the PU, which defines the command. The command context must have been previously allocated on the system using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md).

### `CompletionMode` *(in, AIL_INT64)*

Specifies when the processing operation will be tagged as completed. This parameter can be set to one of the following values.

*For specifying when the command is completed*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. If any of the processing operations have destination buffers, the default value will be [`M_DESTINATION_WRITTEN`](../../Reference/fpga/MfpgaCommandQueue.md). If none of the processing operations have a destination buffer but at least support interrupts, then the default value will be [`M_PROCESSING_COMPLETED`](../../Reference/fpga/MfpgaCommandQueue.md). If none of the processing operations have a destination buffer and none of the processing operations support interrupts, then the default value will be [`M_SOURCE_READ`](../../Reference/fpga/MfpgaCommandQueue.md). |
| `M_DESTINATION_WRITTEN` | Specifies that the command is complete when all destination buffers are written. You should select this completion mode rather than [`M_PROCESSING_COMPLETED`](../../Reference/fpga/MfpgaCommandQueue.md) if you want to ensure that all Processing FPGA operations have completed and that the results are available to the Host. |
| `M_PROCESSING_COMPLETED` | Specifies that the command is complete when the PU generates its end-of-processing interrupt (interrupt 0). The end-of-processing interrupt only indicates that the PU's processing operations are complete but supplies no information on the transfer of resulting data. The PU must support interrupts to use this mode, otherwise an error will be generated. |
| `M_SOURCE_READ` | Specifies that the command is complete when all source buffers have been read. |

### `QueueType` *(in, AIL_INT64)*

Specifies whether [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) should load the current command (or complex command) into hardware or store it into a complex command. This parameter can be set to one of the following:

*For specifying how to handle the current command*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISPATCH` *(default)* | Informs the Aurora Imaging Library driver that subsequent commands are not related to the current one. [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) will send the current command to the system command queue. If multiple related commands are gathered in a complex command, then [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) will add the current command to the complex command and then send the complex command to the system command queue. Processing will start immediately if the system command queue is empty and the required Processing FPGA resources are available. |
| `M_WAIT` | Informs the Aurora Imaging Library driver to wait for other commands, using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md), before being queued in the system command queue. The current command is stored in a complex command. The driver expects to gather multiple commands into a complex command until a command is sent using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_DISPATCH`](../../Reference/fpga/MfpgaCommandQueue.md). |
