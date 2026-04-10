---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case303"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case303"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case303

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
//Each time a new frame is grabbed in a buffer, this function is called.
static AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
{
   BOOL ObjectTypeA, ObjectTypeB, ObjectTypeC = FALSE;
   AIL_INT64 RefStamp;
   AIL_ID ModifiedBufferId;

   /* Retrieve the AIL_ID of the grabbed buffer. */
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER + M_BUFFER_ID, &ModifiedBufferId);

   /* Execute the processing  */
   // ... PUT YOUR PROCESSING FUNCTION HERE AND SET ObjectTypeA/B/C ACCORDINGLY

   // Reference latch FIFO should not be empty, but just in case.
   while (ReferenceStamp.empty())
      MosSleep(1);

   // Remove a latched value from the queue. 
   EnterCriticalSection(&FifoLock);
   RefStamp = ReferenceStamp.front();
   ReferenceStamp.pop();
   LeaveCriticalSection(&FifoLock);

   // ObjectTypeA/B/C are boolean values that are set by the processing section of the code
   if (ObjectTypeA)
      MsysIoCommandRegister(CmdListId, M_IMPULSE, RefStamp,
         EJECTOR_OFFSET_A, M_DEFAULT, M_IO_COMMAND_BIT0, M_NULL);
   else if (ObjectTypeB)
      MsysIoCommandRegister(CmdListId, M_IMPULSE, RefStamp, 
         EJECTOR_OFFSET_B, M_DEFAULT, M_IO_COMMAND_BIT1, M_NULL);
   else if (ObjectTypeC)
      MsysIoCommandRegister(CmdListId, M_IMPULSE, RefStamp, 
         EJECTOR_OFFSET_C, M_DEFAULT, M_IO_COMMAND_BIT2, M_NULL);

   return 0;
}
```
