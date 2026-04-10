---
title: "hook_handler_function_prints_main_error_message_to_console"
description: "hook handler function prints main error message to console"
ms.snippet: "userguide.hook_handler_function_prints_main_error_message_to_console"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# hook_handler_function_prints_main_error_message_to_console

> hook handler function prints main error message to console

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_INT MFTYPE ErrorHandlingTypeFunction(AIL_INT HookType, AIL_ID EventId, void *UserDataPtr, AIL_STRING ErrorMessage)
{
	MappGetError(M_DEFAULT, M_CURRENT + M_MESSAGE, ErrorMessage);
	MosPrintf(AIL_TEXT("Aurora Imaging Library Error: "), ErrorMessage.c_str(), AIL_TEXT("\n"));

	return 0;
}
```
