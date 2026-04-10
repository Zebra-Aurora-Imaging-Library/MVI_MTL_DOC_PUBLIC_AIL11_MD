---
title: "zebra_4sight_auxiliary_inputs_outputs02"
description: "4-Sight auxiliary inputs"
ms.snippet: "boardspecific.4sight.zebra_4sight_auxiliary_inputs_outputs02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# zebra_4sight_auxiliary_inputs_outputs02

> 4-Sight auxiliary inputs

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set user bit 1 to input mode. */
MsysControl(M_DEFAULT_HOST,M_IO_MODE+M_AUX_IO1, M_INPUT);

/* Set the interrupt mode to be generated on a rising edge for bit 1. */
MsysControl(M_DEFAULT_HOST,M_IO_INTERRUPT_ACTIVATION+M_AUX_IO1, M_EDGE_RISING);

/* Enable the interrupt state of bit 1. */
MsysControl(M_DEFAULT_HOST,M_IO_INTERRUPT_STATE+M_AUX_IO1, M_ENABLE);

/* Hook your function (YourHookFunction) and any related hook data
 * (YourHookData)to a change of functional state of the user bit.
 */

MsysHookFunction(M_DEFAULT_HOST,M_IO_CHANGE,YourHookFunction, (void*)&YourHookData);

/* Perform processing operations...*/

/* Unhook the function */
MsysHookFunction(M_DEFAULT_HOST,M_IO_CHANGE+M_UNHOOK,YourHookFunction, (void*)&YourHookData);
```
