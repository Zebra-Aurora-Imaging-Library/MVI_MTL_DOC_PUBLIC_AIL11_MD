---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Developing_a_custom_function_to_run_the_primitive_function
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Developing a custom function to run the primitive function"
---

# Developing a user-defined Aurora Imaging Library function to run the primitive function

Although creating a primitive function that calls the appropriate functions of the Aurora Imaging Library FPGA module is a critical step to issuing commands for FPGA processing, sometimes additional steps are required. If your Zebra imaging board has an on-board processor, you must call your primitive function from the callee function of a user-defined Aurora Imaging Library function; for more information on how to define a user-defined Aurora Imaging Library function, see [The Aurora Imaging Library function development module](../C68_The_AIL_function_development_module/ChapterInformation.md).

If your Zebra imaging board does not have an on-board processor, you can also call your primitive function from the callee function of a user-defined function to obtain basic error reporting or other built-in Aurora Imaging Library features. Doing so might incur some small overhead, but provides the benefit of parameter checking.

To develop a user-defined Aurora Imaging Library function to call the primitive function, perform the following:

1. Create a caller function to register and check the parameters passed to the caller function before sending the information to the callee function.
2. Create a callee function to recuperate the values of the parameters registered in the caller function and call the primitive function. To log any of your user-defined error messages with the Aurora Imaging Library error handling mechanism, use [`MfuncErrorReport`](../../Reference/func/MfuncErrorReport.md).
   > **Note:** Note that when your Zebra imaging board has an on-board processor, the callee and primitive functions must be precompiled and preloaded into the shell of the on-board processor.

## Caller function and callee function and execution of operation specified by command context

When you allocate an Aurora Imaging Library function context in the caller function of your user-defined Aurora Imaging Library function using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md), you must specify the identifier to associate with the Aurora Imaging Library function. This identifier need not match the PU's function identifier that is specified when allocating the command context using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md).

## Operation synchronization

When you allocate your Aurora Imaging Library function context using [`MfuncAlloc`](../../Reference/func/MfuncAlloc.md), you must specify whether your caller function should wait for the callee function to finish executing before executing the next statement in the caller function. This should not be confused with the synchronization specified by [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md), which specifies whether the current thread should issue the FPGA processing command synchronously or asynchronously.

The following illustrates the primitive function call from the callee function of a user-defined function.

*[Image: FuncSynchronicity.png]*

Choosing whether to set the user-defined function as synchronous or asynchronous depends on the application. A synchronous function can be easier to code, but using an asynchronous function allows you to perform Host and FPGA processing operations simultaneously. Note that for Zebra imaging boards with on-board processors, the thread of the on-board processor always waits until each FPGA processing command has completed its synchronous operation.
