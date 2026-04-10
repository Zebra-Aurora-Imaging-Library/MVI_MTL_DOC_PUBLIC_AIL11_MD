---
title: "gev_capability"
description: "gige gev capability"
ms.snippet: "boardspecific.gige.gev_capability"
ms.language: "C++"
ms.env: "vc"
ms.version: "10"
---

# gev_capability

> gige gev capability

**Language:** C++ | **Version:** 10+

```cpp
/* Inquire whether your GigE Vision device supports action commands. */
MdigInquire(AilDigitizer, M_GC_CONTROL_PROTOCOL_CAPABILITY, &DeviceCapability);
if ((DeviceCapability & M_GC_ACTION_SUPPORT) == M_GC_ACTION_SUPPORT)
{
// Your GigE Vision device supporst action commands.
}
```
