---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case103"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case103"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case103

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// IO interrupt hook function to read and queue latched value so it can be used as
// the reference value to schedule a command for the newly grabbed part if necessary.
static AIL_INT MFTYPE IoHookFunction(AIL_INT HookType, AIL_ID EventId, void *UserDataPtr)
{
   AIL_INT PinNb = 0;
   AIL_INT64  RefStamp;

   // If the signal that called the hooked function is M_AUX_IO8.  
   MsysGetHookInfo(SysId, EventId, M_IO_INTERRUPT_SOURCE, &PinNb);
   if (PinNb == M_AUX_IO8)
   {
      // Get and queue latched value. 
      MsysGetHookInfo(SysId, EventId, M_REFERENCE_LATCH_VALUE + M_IO_COMMAND_LIST1 + M_LATCH1,
         &RefStamp);
      EnterCriticalSection(&FifoLock);
      ReferenceStamp.push(RefStamp);
      LeaveCriticalSection(&FifoLock);
   }
   return M_NULL;
}

//Each time a new frame is grabbed in a buffer, this function is called.
static AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
{
   BOOL ShouldRejectTheObject = FALSE;
   AIL_INT64 RefStamp;
   AIL_ID ModifiedBufferId;

   /* Retrieve the AIL_ID of the grabbed buffer. */
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER + M_BUFFER_ID, &ModifiedBufferId);

   /* Execute the processing  */
   // ... PUT YOUR PROCESSING FUNCTION HERE AND SET ShouldRejectTheObject ACCORDINGLY

   // Reference latch FIFO should not be empty, but just in case.
   while (ReferenceStamp.empty())
      MosSleep(1);

   // Remove a latched value from the queue even if not ejecting. 
   EnterCriticalSection(&FifoLock);
   RefStamp = ReferenceStamp.front();
   ReferenceStamp.pop();
   LeaveCriticalSection(&FifoLock);

   // If required, insert an ejection pulse after the proper distance.
   if (ShouldRejectTheObject)
      MsysIoCommandRegister(CmdListId, M_IMPULSE, RefStamp,
      EJECTOR_OFFSET, M_DEFAULT, M_IO_COMMAND_BIT1, M_NULL);
   return M_NULL;
}
```
