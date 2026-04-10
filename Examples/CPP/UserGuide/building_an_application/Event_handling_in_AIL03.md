---
title: "Event_handling_in_AIL03"
description: "User-defined hook-handler function."
ms.snippet: "userguide.building_an_application.Event_handling_in_AIL03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Event_handling_in_AIL03

> User-defined hook-handler function.

**Language:** C++ | **Version:** 7.5+

```cpp
/*User's processing (hook-handler) function*/
AIL_INT MFTYPE UserHookFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
{
    /*Void pointer to structure being cast to the structures type*/
    HookDataStruct *UserHookDataPtr = (HookDataStruct *)HookDataPtr;
    AIL_ID ModifiedBufferId;

    /* Retrieve the AIL_ID of the modified buffer. */
    MbufGetHookInfo(HookId, M_MODIFIED_BUFFER + M_BUFFER_ID, &ModifiedBufferId);

    /* Increment the frame counter. */
    UserHookDataPtr->ProcessedImageCount++;

    /* Perform an operation on the modified buffer. */
    MimArith(ModifiedBufferId, M_NULL, UserHookDataPtr->AilImageDisp, M_NOT);

    return 0;
}
```
