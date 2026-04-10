---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm02"
description: "4-Sight GPm I/O command list"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm02

> 4-Sight GPm I/O command list

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a command list based on the rotary decoder's output signal
MsysIoAlloc(SysId, M_IO_COMMAND_LIST1, M_IO_COMMAND_LIST, M_ROTARY_ENCODER1, &CmdListId);

// Setup timer
MsysControl(SysId, M_TIMER_DELAY + M_TIMER1, 0);
MsysControl(SysId, M_TIMER_DURATION + M_TIMER1, 10000000); //time in nsec
MsysControl(SysId, M_TIMER_TRIGGER_SOURCE + M_TIMER1, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT1);
MsysControl(SysId, M_TIMER_STATE + M_TIMER1, M_ENABLE);

// Trigger the timer with an M_IMPULSE command 50 rotary decoder pulses after a specific moment;
// this will cause the timer to output a pulse of 10 msecs
MsysIoCommandRegister(CmdListId, M_IMPULSE, ReferenceStamp, 50, M_DEFAULT, M_IO_COMMAND_BIT1, M_NULL);
```
