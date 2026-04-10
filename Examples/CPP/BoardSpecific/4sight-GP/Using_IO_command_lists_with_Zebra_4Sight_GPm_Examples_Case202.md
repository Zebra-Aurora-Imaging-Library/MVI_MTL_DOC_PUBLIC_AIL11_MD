---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case202"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case202"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case202

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// ...
InitializeCriticalSection(&FifoLock); //Initalizes mutex

// Setup triggered grab. 
MdigControl(AilDigitizer, M_GRAB_TRIGGER_SOURCE, M_AUX_IO0); // camera/digitizer aux I/O (input)
MdigControl(AilDigitizer, M_GRAB_TRIGGER_ACTIVATION, M_EDGE_RISING);
MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_ENABLE);

//Setup ejector pulse. M_IO_COMMAND_LIST2 is I/O command list B.
MsysControl(SysId, M_IO_SOURCE + M_AUX_IO0, M_TIMER1);
MsysControl(SysId, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_IO_COMMAND_LIST2 + M_IO_COMMAND_BIT0);
MsysControl(SysId, M_TIMER_TRIGGER_ACTIVATION + M_TIMER1, M_EDGE_RISING); 
MsysControl(SysId, M_TIMER_DELAY + M_TIMER1, 0);
MsysControl(SysId, M_TIMER_DURATION + M_TIMER1, PULSE_WIDTH); // 10 msec
MsysControl(SysId, M_TIMER_STATE + M_TIMER1, M_ENABLE);

// Allocate the first command list (I/O command list A) based on sensor A (M_AUX_IO8)
MsysIoAlloc(SysId, M_IO_COMMAND_LIST1, M_IO_COMMAND_LIST, M_AUX_IO8, &CmdListIdA);
// Allocate the second command list (I/O command list B) based on sensor B (M_AUX_IO9)
MsysIoAlloc(SysId, M_IO_COMMAND_LIST2, M_IO_COMMAND_LIST, M_AUX_IO9, &CmdListIdB);
if (CmdListIdB != M_NULL)
{
   // Latch the counter value of I/O command list A and B upon detection of a part at sensor A. 
   // This corresponds to the total number of parts that have passed sensor A.
   MsysIoControl(CmdListIdA, M_REFERENCE_LATCH_TRIGGER_SOURCE + M_LATCH1, M_AUX_IO8);
   MsysIoControl(CmdListIdA, M_REFERENCE_LATCH_ACTIVATION + M_LATCH1, M_EDGE_RISING);
   MsysIoControl(CmdListIdA, M_REFERENCE_LATCH_STATE + M_LATCH1, M_ENABLE);

   // This corresponds to the number of parts that have passed 
   // sensor B the moment sensor A detects a new part.
   MsysIoControl(CmdListIdB, M_REFERENCE_LATCH_TRIGGER_SOURCE + M_LATCH1, M_AUX_IO8);
   MsysIoControl(CmdListIdB, M_REFERENCE_LATCH_ACTIVATION + M_LATCH1, M_EDGE_RISING);
   MsysIoControl(CmdListIdB, M_REFERENCE_LATCH_STATE + M_LATCH1, M_ENABLE);

   // Hook a function to sensor A's signal (M_AUX_IO8);
   // hook-handler function reads and queues latched value of each I/O command list.
   MsysControl(SysId, M_IO_INTERRUPT_ACTIVATION + M_AUX_IO8, M_EDGE_RISING);
   MsysHookFunction(SysId, M_IO_CHANGE, IoHookFunction, M_NULL);
   MsysControl(SysId, M_IO_INTERRUPT_STATE + M_AUX_IO8, M_ENABLE);

   // Start the grabbing and processing. 
   // The processing function is called with each time a frame is grabbed. 
   MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
      M_START, M_DEFAULT, ProcessingFunction, M_NULL);

   // Print a message and wait for a key press before stopping the processing. 
   MosPrintf(AIL_TEXT("Press <Enter> to stop. \n\n"));
   MosGetch();
   MdigProcess(AilDigitizer, AilGrabBufferList, AilGrabBufferListSize,
      M_STOP, M_DEFAULT, ProcessingFunction, M_NULL);
}
// ...
```
