---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Cascaded_and_parallel_asynchronous_processing_functions
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Cascaded and parallel asynchronous processing functions"
---

# Cascaded and parallel processing

When designing a primitive function for your application, you might encounter situations where the entire operation must be carried out by two or more PUs. For example, your application might require a Bayer filter as well as a LUT mapping.

Depending on the application, you might want specific PU operations to be cascaded (the result of one PU transferred directly to another PU) or carried out in parallel, or a combination of both to reduce the number of memory accesses. If the PUs are appropriately interconnected in the FPGA configuration, Aurora Imaging Library allows you to do so. Refer to your FPGA documentation for more information.

## Cascaded processing operation

You can cascade the processing operations of the PUs associated with two command contexts using [`MfpgaSetLink`](../../Reference/fpga/MfpgaSetLink.md).

*[Image: SplitterPU.png]*

When you link two command contexts, the stream output of one PU is used as the stream input for the other PU. To transfer image data between two PUs, the stream output of the source PU and the stream input of the destination PU must be configured to the same image data format.

To cascade several PUs, you must create a command context for each PU and then link the command contexts two at a time. Note that calling [`MfpgaSetLink`](../../Reference/fpga/MfpgaSetLink.md) to route the output of one PU to the stream input of another PU requires that this path is available between the PUs in the FPGA configuration.

To ensure that the cascaded processing operations start at the right moment in time, there are special considerations when queuing their commands. For more information, see [Issuing commands and retrieving results for FPGA processing](S08_Issuing_commands_and_retrieving_results_for_FPGA_processing.md).

## Parallel processing operation

You can also send data simultaneously from memory or from a PU to two or more different PUs. To do so, the FPGA configuration must include a splitter. The splitter receives image data at its stream input port and copies this data to all its stream output ports. If you cascade the stream output ports of the splitter with the stream input ports of the required PUs, the required PUs will simultaneously process the same data. For example, you can simultaneously send the result of a PU that performs a Convolve operation to a PU that performs a gain and offset operation and a PU that performs a min/max operation.

*[Image: splitter.png]*

> **Note:** Note that there must be a path between the stream output ports of the splitter and the stream input ports of the required PUs. The number of output ports of the splitter should match the number of connected PUs. In addition, the stream input and outputs of the splitter must have the same width and be connected to compatible ports on the PUs or TUs.

Similar to when cascading processing operations, to ensure that the processing operations are performed in parallel, there are special considerations when queuing their commands. For more information, see [Issuing commands and retrieving results for FPGA processing](S08_Issuing_commands_and_retrieving_results_for_FPGA_processing.md).

> **Note:** Note that splitters do not need to be allocated and programmed like PUs. They do not have registers and automatically forward any data received on their input port to all output ports.
