---
title: "Setting_PU_registers"
description: "Example of an FPGA Registers fields"
ms.snippet: "userguide.Using_AIL_with_a_Processing_FPGA.Setting_PU_registers"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Setting_PU_registers

> Example of an FPGA Registers fields

**Language:** C++ | **Version:** 8.0+

```cpp
/*************************************************************
* Section name  : USER
* USEROFF       : 0x60
**************************************************************/
typedef struct
{
  FPGA_GAINOFFSET_REG_CTRL    ctrl;    /* Address offset : [0x60] */
  FPGA_GAINOFFSET_REG_CLIPVAL clipval; /* Address offset : [0x68] */

} FPGA_GAINOFFSET_USER_ST;
```
