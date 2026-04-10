---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Using_an_FPGA_configuration
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Using an FPGA configuration"
---

# Processing an image using FPGA processing

To process images using FPGA processing, perform the following steps:

1. Use the Firmware Updater utility in Aurora Imaging Control Center to load the FPGA configuration. For more information on how to load a specific configuration, refer to the FDK documentation.
   The Boards Information utility displays details of the PUs (processing units) and TUs (transfer units) implemented in the firmware for Zebra Rapixo CXP Pro (using the Zebra FDK). You can find this utility in Aurora Imaging Configurator, under **Boards Rapixo CXP**. For a description of PUs and their interconnections, refer to the FDK documentation.
   > **Note:** Note that a valid Zebra FPGA configuration file has a (_.mbf_) or ( _.firmware_) extension and can be found in the installed Drivers directory (for example, _C:\Program Files\Aurora Imaging Library\11\Drivers\Board_Name\Firmware_, where Board_Name should be RapixoCXP in the case of a Zebra RapixoCXP board).
2. Include the Aurora Imaging Library FPGA header file, _ailfpga.h_.
   > **Note:** Note that the FPGA module is not supported in Distributed Aurora Imaging Library applications.
3. Grab the image(s) into one or more on-board FPGA-accessible buffer(s) ([`MbufAlloc...`](../../Reference/buf/MbufAlloc1d.md) with [`M_GRAB`](../../Reference/buf/MbufAlloc2d.md) + [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAlloc2d.md)).
4. Set up and call the primitive function (or the user-defined Aurora Imaging Library function) that dispatches the required commands for FPGA processing. To set up the primitive function (or the user-defined Aurora Imaging Library function), see [Steps to develop a function that performs FPGA processing](S03_Steps_to_develop_a_function_to_program_for_FPGA_processing.md).
   Note that FPGA processing can typically be performed if the source buffer is allocated on-board, the destination buffer is allocated in non-paged Host memory or on-board, and the FPGA configuration includes a path from the PU(s) to the memory banks in which the specified source and destination buffers are located. Using the [`M_FPGA_ACCESSIBLE`](../../Reference/buf/MbufAlloc2d.md) attribute when allocating a buffer ensures that the buffer is in memory accessible for FPGA processing, although there might not be an available path between the PU and the memory bank, depending on the selected FPGA configuration.
   If calling a primitive function and the target PU is not in the loaded FPGA configuration, an error is generated.
