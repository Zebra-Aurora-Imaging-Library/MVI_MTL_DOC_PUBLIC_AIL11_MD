---
title: "Cascaded_and_parallel_processing2"
description: "Example of an FPGA Register"
ms.snippet: "userguide.Using_AIL_with_a_Processing_FPGA.Cascaded_and_parallel_processing2"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Cascaded_and_parallel_processing2

> Example of an FPGA Register

**Language:** C++ | **Version:** 8.0+

```cpp
/* Output0 of the OffsetGain is connected to Input0 of the LutMap.           */
MfpgaSetLink(OffsetGainContext, M_OUTPUT0, LutMapContext, M_INPUT0, M_DEFAULT);

/* Queue the processing operation, we use the M_WAIT flag because            */
/* other MfpgaCommandQueue() follows.                                        */
/* This tells the software to wait for other MfpgaCommandQueue() operations. */
MfpgaCommandQueue(LutMapContext, M_DEFAULT, M_WAIT);

/* Queue the processing operation, using the M_DEFAULT flag.                 */
/* This tells the software to send the processing operation to the FPGA.     */
MfpgaCommandQueue(OffsetGainContext, M_DEFAULT, M_DEFAULT);
```
