---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case203"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case203"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case203

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// IO interrupt hook function to read and queue latched values of I/O command list A and B so that 
// the two values can be used to schedule a command for the newly grabbed part if necessary.
static AIL_INT MFTYPE IoHookFunction(AIL_INT HookType, AIL_ID EventId, void *UserDataPtr)
{
   AIL_INT PinNb = 0;
   AIL_INT64 NumberOfSensorAObjects;
   AIL_INT64 NumberOfSensorBObjects;

   // If the signal that called the hooked function is M_AUX_IO8. 
   MsysGetHookInfo(SysId, EventId, M_IO_INTERRUPT_SOURCE, &PinNb);
   if (PinNb == M_AUX_IO8)
   {

      // Get and queue latched value of I/O command list A.
      // This is the total number of parts that have passed sensor A.
      MsysGetHookInfo(SysId, EventId,
         M_REFERENCE_LATCH_VALUE + M_IO_COMMAND_LIST1 + M_LATCH1, &NumberOfSensorAObjects);
      // Get and queue latched value of I/O command list B.
      // This is the number of parts that have passed sensor B when a new part passes sensor A
      MsysGetHookInfo(SysId, EventId,
         M_REFERENCE_LATCH_VALUE + M_IO_COMMAND_LIST2 + M_LATCH1, &NumberOfSensorBObjects);
      EnterCriticalSection(&FifoLock);
      SensorAQueue.push(NumberOfSensorAObjects);
      SensorBQueue.push(NumberOfSensorBObjects);
      LeaveCriticalSection(&FifoLock);
   }

   return M_NULL;
}

//Each time a new frame is grabbed in a buffer, this function is called.
static AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr)
{
   BOOL ShouldRejectTheObject = FALSE;
   AIL_INT64 NumberOfSensorAObjects;
   AIL_INT64 NumberOfSensorBObjects;
   AIL_INT64 NumberOfObjectsBetweenSensors;
   AIL_ID ModifiedBufferId;

   /* Retrieve the AIL_ID of the grabbed buffer. */
   MdigGetHookInfo(HookId, M_MODIFIED_BUFFER + M_BUFFER_ID, &ModifiedBufferId);

   /* Execute the processing  */
   // ... PUT YOUR PROCESSING FUNCTION HERE AND SET ShouldRejectTheObject ACCORDINGLY

   // Reference latch FIFO should not be empty, but just in case.
   while (SensorAQueue.empty() || SensorBQueue.empty())
      MosSleep(1);

   // Remove a latched value from the queue even if not ejecting. 
   EnterCriticalSection(&FifoLock);
   NumberOfSensorAObjects = SensorAQueue.front();
   SensorAQueue.pop();
   NumberOfSensorBObjects = SensorBQueue.front();
   SensorBQueue.pop();
   LeaveCriticalSection(&FifoLock);

   // If required, insert an ejection pulse after the proper number of parts have passed sensor B.
   if (ShouldRejectTheObject)
   {
      // Establish number of parts between sensors A and B at the moment when the part was first grabbed.
      NumberOfObjectsBetweenSensors = NumberOfSensorAObjects - NumberOfSensorBObjects;
      // Insert ejection pulse after number of parts between the sensors have passed sensor B.
      MsysIoCommandRegister(CmdListIdB, M_IMPULSE, NumberOfSensorBObjects,
         (AIL_DOUBLE)NumberOfObjectsBetweenSensors, M_DEFAULT, M_IO_COMMAND_BIT0, M_NULL);
   }


   return 0;
}
```
