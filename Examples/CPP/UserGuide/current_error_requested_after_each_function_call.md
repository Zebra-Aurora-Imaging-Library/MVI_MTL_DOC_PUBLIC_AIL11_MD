---
title: "current_error_requested_after_each_function_call"
description: "error handling after each function call requested"
ms.snippet: "userguide.current_error_requested_after_each_function_call"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# current_error_requested_after_each_function_call

> error handling after each function call requested

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_ID AilDigitizer = MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
AIL_INT ErrorCode = MappGetError(M_DEFAULT, M_CURRENT, M_NULL);
if (ErrorCode != M_NULL_ERROR){
	/* Error handling code for a failed allocation */
}
MdigGrab(AilDigitizer, AilBuffer);
ErrorCode = MappGetError(M_DEFAULT, M_CURRENT, M_NULL);
if (ErrorCode != M_NULL_ERROR){
	/* Error handling code for a failed grab */
}
```
