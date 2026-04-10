---
doctype: Reference
module: fpga
function: index
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / fpga"
---

# fpga

> The functions prefixed with Mfpga make up the FPGA module. The FPGA module allows you to perform custom processing operations using a board that supports FPGA processing. Performing processing operations on-board frees up the Host for other tasks. You can allocate one or more command contexts to carry out a required operation using a specific processing unit (PU) on a target Processing FPGA. You can configure, link, and queue multiple commands, as well as store necessary information in PU user-specific registers. This module assumes that the appropriate FPGA configuration with the required PUs has been loaded into the Processing FPGA. For information on configuring a Processing FPGA and using this module, see [Using Aurora Imaging Library for FPGA processing](../../UserGuide/C67_Using_AIL_with_FPGA_processing/ChapterInformation.md) and refer to the FDK documentation.

## Mfpga functions

- [MfpgaCommandAlloc](MfpgaCommandAlloc.md)
- [MfpgaCommandControl](MfpgaCommandControl.md)
- [MfpgaCommandFree](MfpgaCommandFree.md)
- [MfpgaCommandInquire](MfpgaCommandInquire.md)
- [MfpgaCommandQueue](MfpgaCommandQueue.md)
- [MfpgaControl](MfpgaControl.md)
- [MfpgaGetHookInfo](MfpgaGetHookInfo.md)
- [MfpgaGetRegister](MfpgaGetRegister.md)
- [MfpgaHookFunction](MfpgaHookFunction.md)
- [MfpgaInquire](MfpgaInquire.md)
- [MfpgaSetDestination](MfpgaSetDestination.md)
- [MfpgaSetLink](MfpgaSetLink.md)
- [MfpgaSetRegister](MfpgaSetRegister.md)
- [MfpgaSetSource](MfpgaSetSource.md)

