---
title: "grabbing_with_your_digitizer_simulateddigitizer"
description: "Avoiding errors with simulated digitizers"
ms.snippet: "userguide.grabbing_with_your_digitizer_simulateddigitizer"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# grabbing_with_your_digitizer_simulateddigitizer

> Avoiding errors with simulated digitizers

**Language:** C++ | **Version:** 7.5+

```cpp
 bool UseClarityUHDSystem = MsysInquire(AilSystem, M_SYSTEM_TYPE, M_NULL) == M_SYSTEM_CLARITY_UHD_TYPE;

 if (UseClarityUHDSystem)
{
    MdigControl(AilSystem, M_INPUT_FILTER, M_LOW_PASS_0);
}
```
