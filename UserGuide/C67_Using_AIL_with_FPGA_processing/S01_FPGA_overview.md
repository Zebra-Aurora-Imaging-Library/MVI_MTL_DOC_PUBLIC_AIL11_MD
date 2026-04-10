---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: FPGA_overview
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / FPGA overview"
---

# Using Aurora Imaging Library for FPGA processing - overview

Some Zebra imaging boards can be used for FPGA processing. This can be used to perform some processing operations on-board to free up the Host for other tasks.

*[Image: FPGAtop.png]*

Before using FPGA processing, you must configure it with an FPGA configuration that contains the appropriate processing units (PUs) to carry out the required set of tasks.

*[Image: FPGAoverview.png]*

You can contact Zebra to inquire whether any FPGA configurations meet your processing requirements. However, depending on your application, you might need to develop a custom FPGA configuration using Zebra PUs and, if required, custom PUs. If this is the case, you must purchase and use the Zebra FPGA Developer's Toolkit (FDK).

To use PUs, either individually or routing an output of one PU to an input of another PU (cascaded or parallel processing operations), you must use the Aurora Imaging Library FPGA module to create, set up, and dispatch the required command(s) to the board. The Aurora Imaging Library FPGA module allows you to create a command and set its target PU, specify the image buffers to associate with the stream input and output ports of the PU, and set the PU's user-specific registers (with required processing information). Besides associating image buffers with the ports, the Aurora Imaging Library FPGA module also allows you to route the output of one command's PU to an input port of another command's PU, if the PUs are interconnected in the loaded FPGA configuration.

You can create a simple function (primitive function) that calls all the required functions of the Aurora Imaging Library FPGA module. However, if incorrect parameters are passed to your primitive function, you are not notified and you cannot use Aurora Imaging Library's tracing mechanism to determine the cause of the unexpected results. To enable basic error checking and parameter validation, you must call your primitive function from a user-defined Aurora Imaging Library function; to do so, see [The Aurora Imaging Library function development module](../C68_The_AIL_function_development_module/ChapterInformation.md). Note that this error checking adds a small overhead to the processing function.

When your Zebra imaging board also has an on-board processor, you must use a user-defined Aurora Imaging Library function because the Aurora Imaging Library FPGA functions must be executed by the on-board processor.

Although an FPGA configuration also has components (referred to as transfer units and memory controllers) required to transfer data to/from the required PUs and to/from memory, the Aurora Imaging Library FPGA module handles these transparently.
