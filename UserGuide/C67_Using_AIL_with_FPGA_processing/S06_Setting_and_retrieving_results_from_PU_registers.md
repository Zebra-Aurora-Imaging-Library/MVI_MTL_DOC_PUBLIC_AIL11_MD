---
doctype: UserGuide
part: "Miscellaneous"
chapter: Using_AIL_with_FPGA_processing
section: Setting_and_retrieving_results_from_PU_registers
module_tag: fpga
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / fpga / Setting and retrieving results from PU registers"
---

# Setting and retrieving results from PU registers

Each PU has a number of user-defined memory-mapped registers that allow you to set the PU's operation settings. Most PUs that return non-image results store these results in one or more registers.

You can set a PU's registers using up to 16 calls to [`MfpgaSetRegister`](../../Reference/fpga/MfpgaSetRegister.md). Similarly, you can set up a request to retrieve the values of a PU's registers using up to 16 calls to [`MfpgaGetRegister`](../../Reference/fpga/MfpgaGetRegister.md). 16 calls is usually more than sufficient because you can write to or read from multiple registers at a single time.

For examples of header files and code snippets for setting the PU's registers, refer to the FDK documentation.
