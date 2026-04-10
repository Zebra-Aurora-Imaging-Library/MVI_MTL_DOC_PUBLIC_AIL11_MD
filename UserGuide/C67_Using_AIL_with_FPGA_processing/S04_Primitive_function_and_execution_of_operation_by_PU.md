---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Primitive_function_and_execution_of_operation_by_PU
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Primitive function and execution of operation by PU"
---

# Primitive function and execution of operation by PU

The FPGA configuration that you load for FPGA processing contains one or more PUs that your primitive function can use to carry out specific image processing operations. With each PU that you want to use, you must associate an FPGA command context, using [`MfpgaCommandAlloc`](../../Reference/fpga/MfpgaCommandAlloc.md). An FPGA command context is an Aurora Imaging Library object that stores the information required to select and configure a PU, within a loaded FPGA configuration, to perform an image processing operation. The command context stores information regarding the target PU, the source and destination buffers, operation information, register settings, register read requests, link information, and the execution mode. In essence, the command context specifies the command required to perform a processing operation. Once your command is completed and the FPGA command context is no longer needed, it should be freed using [`MfpgaCommandFree`](../../Reference/fpga/MfpgaCommandFree.md).

When allocating the command context, you must specify the target PU. To do so, you must specify the PU's function identifier. Typically, you know the name of the PU, but not the function identifier. In this case, you will need to convert the name to the function identifier using [`MfpgaInquire`](../../Reference/fpga/MfpgaInquire.md). For more information, refer to the FDK documentation and examples.

There are certain PUs that require multiple passes to setup and use. For example, the PU that performs a LUT mapping needs two passes. The first pass sets up the lookup table and the second enables the PU to map the input image through the LUT. You must ensure that you set up this type of PU correctly.

For information about the PUs available in the Zebra PU library, refer to the FDK documentation.
