---
title: "Steps_to_develop_a_function_to_program_the_Processing_FPGA"
description: "Setting up a primitive function"
ms.snippet: "userguide.Using_AIL_with_a_Processing_FPGA.Steps_to_develop_a_function_to_program_the_Processing_FPGA"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Steps_to_develop_a_function_to_program_the_Processing_FPGA

> Setting up a primitive function

**Language:** C++ | **Version:** 8.0+

```cpp
/* Inquire the buffer info object for the source and destination buffers. */
MfuncInquire(AilSourceImage, M_BUFFER_INFO, &Src);
MfuncInquire(AilDestinationImage, M_BUFFER_INFO, &Dest);

/* Allocate an asynchronous command context on the AddConstant processing unit.
*/
if(MfpgaCommandAlloc(MfuncBufOwnerSystemId(Src), M_DEV0, FPGA_ADDCONST_FID,
                     M_DEFAULT,M_DEV0, M_ASYNCHRONOUS, M_DEFAULT,
                     &AddConstantContext))
  {
  /* Initialize the shadow register structure. */
  InitializeShadowRegisters(&ShadowRegisters, Src, ConstantValue, ControlFlag);

  /* Input0 of AddConstant processing unit is connected to the Src buffer. */
  MfpgaSetSource(AddConstantContext, Src, M_INPUT0, M_DEFAULT);

  /* Output0 of AddConstant processing unit is connected to the Dest buffer. */
  MfpgaSetDestination(AddConstantContext, Dest, M_OUTPUT0, M_DEFAULT);

  /* Pass the initialized shadow register structure, it will be programmed   */
  /* when the processing operation is dispatched (before processing starts). */
  MfpgaSetRegister(AddConstantContext, M_USER, 0, sizeof(ShadowRegisters),
                    (void*)(&ShadowRegisters), M_WHEN_DISPATCHED);

  /* Queue the processing operation, because this context is asynchronous, */
  /* MfpgaComamndQueue will return before the operation is completed. */
  MfpgaCommandQueue(AddConstantContext, M_DEFAULT, M_DEFAULT);

  /* Free the allocated context. */
  MfpgaCommandFree(AddConstantContext, M_DEFAULT);
  }
```
