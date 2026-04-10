---
title: "different_error_handling_code_used_generated_allocation_or_grabbing"
description: "Different error handling code is generated depending if error was allocation or grabbing"
ms.snippet: "userguide.different_error_handling_code_used_generated_allocation_or_grabbing"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# different_error_handling_code_used_generated_allocation_or_grabbing

> Different error handling code is generated depending if error was allocation or grabbing

**Language:** C++ | **Version:** 7.5+

```cpp
MappGetError(M_DEFAULT, M_GLOBAL, M_NULL);
AIL_ID AilDigitizer = MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
MdigGrab(AilDigitizer, AilBuffer);
AIL_INT GlobalErrorOpcode = MappGetError(M_DEFAULT, M_GLOBAL_OPCODE, M_NULL);
if (GlobalErrorOpcode == M_DIG_ALLOC){
	/* Error handling code for a failed allocation */
}
else if (GlobalErrorOpcode == M_DIG_GRAB){
	/* Error handling code for a failed grab */
}
```
