---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case302"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case302"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case302

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// ...
InitializeCriticalSection(&FifoLock); //Initalizes mutex

// Setup triggered grab. 
MdigControl(AilDigitizer, M_GRAB_TRIGGER_SOURCE, M_AUX_IO0); //camera/digitizer aux I/O (input)
MdigControl(AilDigitizer, M_GRAB_TRIGGER_ACTIVATION, M_EDGE_RISING);
MdigControl(AilDigitizer, M_GRAB_TRIGGER_STATE, M_ENABLE);

// Setup rotary decoder; for simplicity, we assume the conveyor cannot move backward. 
MsysControl(SysId, M_ROTARY_ENCODER_BIT0_SOURCE + M_ROTARY_ENCODER1, M_AUX_IO14);
MsysControl(SysId, M_ROTARY_ENCODER_BIT1_SOURCE + M_ROTARY_ENCODER1, M_AUX_IO15);
MsysControl(SysId, M_ROTARY_ENCODER_OUTPUT_MODE + M_ROTARY_ENCODER1, M_STEP_FORWARD);
MsysControl(SysId, M_ROTARY_ENCODER_STATE + M_ROTARY_ENCODER1, M_ENABLE);

// Setup Timer 1 for Ejector A
MsysControl(SysId, M_IO_SOURCE + M_AUX_IO0, M_TIMER1);
MsysControl(SysId, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT0);
MsysControl(SysId, M_TIMER_DELAY + M_TIMER1, 0);
MsysControl(SysId, M_TIMER_DURATION + M_TIMER1, PULSE_WIDTH);

// Setup Timer 2 for Ejector B
MsysControl(SysId, M_IO_SOURCE + M_AUX_IO1, M_TIMER2);
MsysControl(SysId, M_TIMER_TRIGGER_SOURCE + M_TIMER2, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT1);
MsysControl(SysId, M_TIMER_DELAY + M_TIMER2, 0);
MsysControl(SysId, M_TIMER_DURATION + M_TIMER2, PULSE_WIDTH);

// Setup Timer 3 for Ejector C
MsysControl(SysId, M_IO_SOURCE + M_AUX_IO2, M_TIMER3);
MsysControl(SysId, M_TIMER_TRIGGER_SOURCE + M_TIMER3, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT2);
MsysControl(SysId, M_TIMER_DELAY + M_TIMER3, 0);
MsysControl(SysId, M_TIMER_DURATION + M_TIMER3, PULSE_WIDTH);

// Start all three timers at the same time 
MsysControl(SysId, M_TIMER_STATE + M_TIMER1, M_ENABLE);
MsysControl(SysId, M_TIMER_STATE + M_TIMER2, M_ENABLE);
MsysControl(SysId, M_TIMER_STATE + M_TIMER3, M_ENABLE);

// Allocate a command list where commands are scheduled based on rotary decoder 1's output signal.
MsysIoAlloc(SysId, M_IO_COMMAND_LIST1, M_IO_COMMAND_LIST, M_ROTARY_ENCODER1, &CmdListId);
if (CmdListId != M_NULL)
{
   // Latch counter upon detection of part
   MsysIoControl(CmdListId, M_REFERENCE_LATCH_TRIGGER_SOURCE + M_LATCH1, M_AUX_IO8);
   MsysIoControl(CmdListId, M_REFERENCE_LATCH_ACTIVATION + M_LATCH1, M_EDGE_RISING);
   MsysIoControl(CmdListId, M_REFERENCE_LATCH_STATE + M_LATCH1, M_ENABLE);

   // Hook a function to sensor's signal (M_AUX_IO8); hook-handler function reads and queues 
   // latched value. See previous example for sample code.
   MsysControl(SysId, M_IO_INTERRUPT_ACTIVATION + M_AUX_IO8, M_EDGE_RISING);
   MsysHookFunction(SysId, M_IO_CHANGE, &IoHookFunction, M_NULL);
   MsysControl(SysId, M_IO_INTERRUPT_STATE + M_AUX_IO8, M_ENABLE);

   // Start the grabbing and processing. 
   // The processing function is called each time a frame is grabbed.  
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
