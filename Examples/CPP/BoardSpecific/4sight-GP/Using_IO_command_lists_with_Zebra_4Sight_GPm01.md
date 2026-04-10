---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm01"
description: "4-Sight GPm I/O command list"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm01

> 4-Sight GPm I/O command list

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate a command list based on the internal clock
MsysIoAlloc(SysId, M_IO_COMMAND_LIST1, M_IO_COMMAND_LIST, M_CLOCK, &CmdListId);

// Route the state of the I/O command list's register bits to auxiliary output signals
MsysControl(SysId, M_IO_SOURCE+M_AUX_IO0, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT0);
MsysControl(SysId, M_IO_SOURCE+M_AUX_IO1, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT1);
MsysControl(SysId, M_IO_SOURCE+M_AUX_IO2, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT2);
MsysControl(SysId, M_IO_SOURCE+M_AUX_IO3, M_IO_COMMAND_LIST1 + M_IO_COMMAND_BIT3);

// Add a command to the I/O command list to immediately change the state of M_IO_COMMAND_BIT0
// such that a low to high signal transition occurs on M_AUX_IO0
MsysIoCommandRegister(CmdListId, M_EDGE_RISING, M_REFERENCE_VALUE_CURRENT, 
                                               0, M_DEFAULT, M_IO_COMMAND_BIT0, M_NULL);

// Add a command to the I/O command list to change the state of M_IO_COMMAND_BIT0 10 ms 
// after the function is called such that a high to low signal transition occurs on M_AUX_IO0
MsysIoCommandRegister(CmdListId, M_EDGE_FALLING, M_REFERENCE_VALUE_CURRENT, 
                                           0.010, M_DEFAULT, M_IO_COMMAND_BIT0, M_NULL);

// Inquire the current timestamp so that it can be used as a reference point
MsysIoInquire(CmdListId, M_REFERENCE_VALUE, &ReferenceStamp);

// Add a command to the I/O command list to change the state of M_IO_COMMAND_BIT3 15 ms 
// after the above reference moment, such that a positive 3 ms pulse occurs on M_AUX_IO3
MsysIoCommandRegister(CmdListId, M_PULSE_HIGH, ReferenceStamp, 0.015, 0.003, 
                                                M_IO_COMMAND_BIT3, M_NULL);

// ...

// Free the I/O command list
MsysIoFree(CmdListId);
```
