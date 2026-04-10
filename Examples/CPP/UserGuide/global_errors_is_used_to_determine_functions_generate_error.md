---
title: "global_errors_is_used_to_determine_functions_generate_error"
description: "global errors used to help functions generate error"
ms.snippet: "userguide.global_errors_is_used_to_determine_functions_generate_error"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# global_errors_is_used_to_determine_functions_generate_error

> global errors used to help functions generate error

**Language:** C++ | **Version:** 7.5+

```cpp
MappGetError(M_DEFAULT, M_GLOBAL, M_NULL);
AIL_ID AilDigitizer = MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
MdigGrab(AilDigitizer, AilBuffer);
AIL_INT GlobalErrorCode = MappGetError(M_DEFAULT, M_GLOBAL, M_NULL);
if (GlobalErrorCode != M_NULL_ERROR){
	/* Error handling code */
}
```
