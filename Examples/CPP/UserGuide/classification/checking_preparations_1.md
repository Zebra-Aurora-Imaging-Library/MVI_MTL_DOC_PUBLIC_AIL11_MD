---
title: "checking_preparations_1"
description: "Defining the hook function"
ms.snippet: "userguide.classification.checking_preparations_1"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# checking_preparations_1

> Defining the hook function

**Language:** C++ | **Version:** 7.5+

```cpp
// Create a hook function that will print the index of any source entry that wasn't able to be prepared
AIL_INT MFTYPE HookFuncCheckPrepareEntryStatus(AIL_INT HookType, AIL_ID EventId, void* pUserData)
{
   AIL_INT SourceEntryIndex{ 0 };
   MclassGetHookInfo(EventId, M_SRC_ENTRY_INDEX + M_TYPE_AIL_INT, &SourceEntryIndex);

   AIL_INT Status{ -1 };
   MclassGetHookInfo(EventId, M_STATUS + M_TYPE_AIL_INT, &Status);

   if (Status != M_COMPLETE)
   {
      MosPrintf(AIL_TEXT("Entry %d was not successfully prepared!"), SourceEntryIndex);
   }

   return M_NULL;
}
```
