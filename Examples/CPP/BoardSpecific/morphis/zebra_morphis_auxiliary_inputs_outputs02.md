---
title: "zebra_morphis_auxiliary_inputs_outputs02"
description: "Morphis auxiliary inputs"
ms.snippet: "boardspecific.morphis.zebra_morphis_auxiliary_inputs_outputs02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# zebra_morphis_auxiliary_inputs_outputs02

> Morphis auxiliary inputs

**Language:** C++ | **Version:** 7.5+

```cpp
 /* Set the interrupt mode to be generated on a rising edge for bit 1. */
MsysControl(AilSystem,M_IO_INTERRUPT_ACTIVATION+M_AUX_IO1, M_EDGE_RISING);

/* Enable the interrupt state of bit 1. */
MsysControl(AilSystem,M_IO_INTERRUPT_STATE+M_AUX_IO1, M_ENABLE);

/* Hook your function (YourHookFunction) and any related hook data
 * (YourHookData)to a change of functional state of the user bit.
 */

MsysHookFunction(AilSystem,M_IO_CHANGE,YourHookFunction, (void*)&YourHookData);

/* Perform processing operations...*/

/* Unhook the function */
MsysHookFunction(AilSystem,M_IO_CHANGE+M_UNHOOK,
                             YourHookFunction, (void*)&YourHookData);
```
