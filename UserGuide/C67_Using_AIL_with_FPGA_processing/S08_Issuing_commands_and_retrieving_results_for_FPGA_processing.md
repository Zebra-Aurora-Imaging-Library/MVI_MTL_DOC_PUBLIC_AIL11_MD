---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Issuing_commands_and_retrieving_results_for_FPGA_processing
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Issuing commands and retrieving results for FPGA processing"
---

# Issuing commands and retrieving results for FPGA processing

Once you have specified register settings, source and destination image buffers, register result retrieval requests, and optionally linked information for each command context, you must queue the command(s), defined by these contexts, on the thread's system command queue using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md). Once on the queue, the commands for FPGA processing will be executed in first-in first-out order as resources become available. Note that two commands can typically run at the same time if they don't reference the same buffers and they use different FPGA components that can access these buffers using different paths.

For linked command contexts, you will have to combine the commands, defined by these contexts, into a complex command once you are ready to queue them. You combine commands into a complex command by queuing all linked commands, except the last, using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_WAIT`](../../Reference/fpga/MfpgaCommandQueue.md). Then you should add the last command to the complex command using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_DISPATCH`](../../Reference/fpga/MfpgaCommandQueue.md); this closes the complex command and queues it on the system command queue.

*[Image: queueContext.png]*

The following example shows how to link two command contexts and then combine these commands into a complex command.

> **Code example:** [userguide.Using_AIL_with_a_Processing_FPGA.Cascaded_and_parallel_processing2](userguide.Using_AIL_with_a_Processing_FPGA.Cascaded_and_parallel_processing2)

The completion mode of an [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) call specifies when the command should be tagged as complete. When tagged as complete, you can read back results from your destination buffer(s) or the variables passed to the register result retrieval requests. For complex commands, the completion mode of the last command added to the complex command determines the completion mode for the entire complex command. All other commands in the complex command should have their completion mode set to [`M_DEFAULT`](../../Reference/fpga/MfpgaCommandQueue.md).

When you allocate an FPGA command context, you must specify how the command will be dispatched to the Zebra imaging board. You can specify that a command should run synchronously or asynchronously. A synchronous command ([`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md) with [`M_SYNCHRONOUS`](../../Reference/fpga/MfpgaCommandAlloc.md)) causes the thread from which the command is dispatched to wait for the command to complete before executing subsequent statements. An asynchronous command ( [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md) with [`M_ASYNCHRONOUS`](../../Reference/fpga/MfpgaCommandAlloc.md)) causes the thread to continue executing subsequent statements without waiting for the command to complete. For complex commands, Aurora Imaging Library uses the synchronous/asynchronous setting of the last command in the complex command (that is, the call to [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md) with [`M_DISPATCH`](../../Reference/fpga/MfpgaCommandQueue.md)).

The Host processor supports both [`M_SYNCHRONOUS`](../../Reference/fpga/MfpgaCommandAlloc.md) and [`M_ASYNCHRONOUS`](../../Reference/fpga/MfpgaCommandAlloc.md) settings; it can continue to work if the command is asynchronous.
