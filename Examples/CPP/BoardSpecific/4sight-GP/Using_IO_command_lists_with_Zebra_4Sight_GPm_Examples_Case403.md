---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case403

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// Defines the processing function, called by MdigProcess()
AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
   {
   PHOOK_PARAM pHookParam = (PHOOK_PARAM)HookDataPtr;
   AIL_ID ModifiedBufferId;

   /* Retrieve the AIL_ID of the grabbed buffer. */
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER+M_BUFFER_ID, &ModifiedBufferId);

   /* Execute the processing  */
   // ... PUT YOUR PROCESSING FUNCTION HERE

   return 0;
   }
```
