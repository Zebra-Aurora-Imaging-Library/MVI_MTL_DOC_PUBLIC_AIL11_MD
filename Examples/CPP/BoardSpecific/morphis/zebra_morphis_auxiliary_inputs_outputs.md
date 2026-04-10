---
title: "zebra_morphis_auxiliary_inputs_outputs"
description: "morphis auxiliary outputs"
ms.snippet: "boardspecific.morphis.zebra_morphis_auxiliary_inputs_outputs"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# zebra_morphis_auxiliary_inputs_outputs

> morphis auxiliary outputs

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set bidirectinal signal M_AUX_IO1 to output mode */
MsysControl(AilSystem, M_IO_MODE + M_AUX_IO1, M_OUTPUT);
/* Set the source of the output signal to user-bit 1 */
MsysControl(AilSystem, M_IO_SOURCE + M_AUX_IO1, M_USER_BIT1);
/* Set the state of bit 1 of the main static-user-output register to on.*/
MsysControl(AilSystem,M_USER_BIT_STATE + M_USER_BIT1, M_ON);

/* Set user-bits 2 and 3 to output mode. */
MsysControl(AilSystem, M_IO_MODE + M_AUX_IO2, M_OUTPUT);
MsysControl(AilSystem, M_IO_MODE + M_AUX_IO3, M_OUTPUT);
/* Set user-bit 2 to on, and all other bits off. */
MsysInquire(AilSystem,M_USER_BIT_STATE_ALL, &CurrentValue);
NewValue = (CurrentValue&0xFFFFFFFD)|0x4;
MsysControl(AilSystem, M_USER_BIT_STATE_ALL, NewValue);
```
