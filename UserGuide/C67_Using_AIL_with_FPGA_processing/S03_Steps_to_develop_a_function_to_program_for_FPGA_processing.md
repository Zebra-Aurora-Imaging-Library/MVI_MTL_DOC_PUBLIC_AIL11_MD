---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Steps_to_develop_a_function_to_program_for_FPGA_processing
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Steps to develop a function to program for FPGA processing"
---

# Steps to develop a function that performs FPGA processing

To perform an operation for FPGA processing when there is no equivalent Aurora Imaging Library function, you must at least create a primitive function that uses the functions of the Aurora Imaging Library Mfpga module to set up and dispatch all appropriate commands. Also, you must include the Aurora Imaging Library FPGA header file _ailfpga.h_ to make a call to any function of the Mfpga module.

To create the primitive function:

1. Inquire the handle of your source and destination buffers using [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md). Aurora Imaging Library FPGA functions do not take buffer identifiers; instead, they take the handle of the buffers.
2. To enable basic parameter checking when using Mfpga functions, use [`MfpgaControl`](../../Reference/fpga/MfpgaControl.md) with [`M_ERROR`](../../Reference/fpga/MfpgaControl.md) and [`M_PRINT_ENABLE`](../../Reference/fpga/MfpgaControl.md). Note that this will also report errors when attempting to link the target PU with a PU not in the FPGA configuration, and when attempting to use an invalid interrupt.
3. Allocate an FPGA command context for the target PU using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md). The FPGA command context is used to contain the necessary command information to perform the required operation using the specified PU.
4. Specify the source image buffer(s) using [`MfpgaSetSource`](../../Reference/fpga/MfpgaSetSource.md).
5. Specify the destination image buffer(s) using [`MfpgaSetDestination`](../../Reference/fpga/MfpgaSetDestination.md).
6. Set other processing information in the PU's user-specific registers using [`MfpgaSetRegister`](../../Reference/fpga/MfpgaSetRegister.md).
7. To route the stream output of one PU to the stream input port of another PU (that is, to cascade the PUs) repeat steps 2 through 5, creating and setting up a FPGA command context for each PU that you need to cascade. Then, link the command contexts, using [`MfpgaSetLink`](../../Reference/fpga/MfpgaSetLink.md).
8. Queue the command(s) defined by the command context(s), using [`MfpgaCommandQueue`](../../Reference/fpga/MfpgaCommandQueue.md).
9. Retrieve non-image results from the PU's registers using [`MfpgaGetRegister`](../../Reference/fpga/MfpgaGetRegister.md).
10. Free the command context using [`MfpgaCommandFree`](../../Reference/fpga/MfpgaCommandFree.md).

For examples of the type of code that should be included within your primitive function to set up and dispatch a command for FPGA processing, refer to the FDK documentation.

> **Note:** Note that you can call your primitive function from the callee function of a user-defined function to obtain basic error reporting or other built-in Aurora Imaging Library features. Doing so might incur some small overhead, but provides the benefit of parameter checking. For more information on creating a user-defined Aurora Imaging Library function, see [The Aurora Imaging Library function development module](../C68_The_AIL_function_development_module/ChapterInformation.md).

Whenever you want to associate an FPGA command context with a PU, you must make sure the PU is actually present in the FPGA configuration loaded in the FPGA. In addition, when implementing cascaded or parallel processing operations, it is necessary to make sure that the PUs are interconnected properly in the FPGA configuration.
