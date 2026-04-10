---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Source_and_destination_image_buffers
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Source and destination image buffers"
---

# Source and destination image buffers

Before using a PU, you must specify the appropriate source and destination image buffers for the command. Some PUs do not produce image data and, therefore, they do not require destination image buffers. To specify the source and destination buffers, use [`MfpgaSetSource`](../../Reference/fpga/MfpgaSetSource.md) and [`MfpgaSetDestination`](../../Reference/fpga/MfpgaSetDestination.md), respectively. These functions require that you pass the handle of the buffer and not its identifier. To get the handle of the buffer, you must call [`MfuncInquire`](../../Reference/func/MfuncInquire.md) with [`M_BUFFER_INFO`](../../Reference/func/MfuncInquire.md).

Depending on your FPGA configuration, there can be limitations on the types of buffers from which PUs can receive data using their stream input port(s). Similarly, there can also be limitations on the buffers to which they can transmit data from their stream output port(s). You must keep these limitations in mind when allocating your buffer(s). [`MfpgaSetDestination`](../../Reference/fpga/MfpgaSetDestination.md) and [`MfpgaSetSource`](../../Reference/fpga/MfpgaSetSource.md) will not automatically convert the buffers so that they are appropriate for the operation.

> **Note:** Note that FPGA processing can typically be performed if the source buffer is allocated on-board, the destination buffer is allocated in non-paged Host memory or on-board, and the FPGA configuration includes a path from the PU(s) to the memory banks in which the specified source and destination buffers are located.

When allocating your image buffers for FPGA processing ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md)), you can have Aurora Imaging Library automatically select the optimal memory location for the image buffers. To do so, add [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAlloc1d.md) to the other required attributes (for example, [`M_PROC`](../../Reference/buf/MbufAlloc1d.md)+[`M_GRAB`](../../Reference/buf/MbufAlloc1d.md)). Note that although the buffers will be allocated in memory accessible for FPGA processing, you must ensure that a path between the PU and the buffers is available.

To explicitly allocate your buffer in a specific memory location for FPGA processing, you can add one of the following attributes to [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAlloc1d.md). Note that by explicitly allocating your buffer in a specific memory location, you can make your code less portable to other boards with a different number of memory banks or Host memory restrictions. However, you might obtain some performance benefits.

- [`M_ON_BOARD`](../../Reference/buf/MbufAlloc1d.md). Explicitly allocates the buffer in on-board memory.
- [`M_HOST_MEMORY`](../../Reference/buf/MbufAlloc1d.md). Explicitly allocates the buffer in Host memory.
- [`M_MEMORY_BANK_n`](../../Reference/buf/MbufAlloc1d.md). Explicitly specifies the on-board memory bank in which to allocate your image buffer. The variable _n_ specifies the rank of the on-board memory bank in which your image must be allocated.

> **Note:** Note that [`M_MEMORY_BANK_n`](../../Reference/buf/MbufAlloc1d.md) is not required on Zebra imaging boards with a single bank of on-board memory.

If you want to route the stream output of one PU to the stream input port of another PU, you will have to cascade the PUs using the appropriate functions. For more information on these functions, see [Cascaded and parallel processing](S07_Cascaded_and_parallel_asynchronous_processing_functions.md).
