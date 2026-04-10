---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case101"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case101"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case101

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// M_AUX_IO14: Receives rotary encoder bit 0  
// M_AUX_IO15: Receives rotary encoder bit 1
// M_AUX_IO8 (input): Receives signal from sensor. 
//                    sensor sends same signal to M_AUX_IO0 of camera/digitizer.
// M_AUX_IO1 (output): Sent to ejector from timer
// M_TIMER1: Generates ejector pulse. Timer used so can specify pulse duration 
//           in time and moment to execute command in rotary decoder output counts.
#define PULSE_WIDTH         10000000   // 10 ms
#define EJECTOR_OFFSET      500        // rotary decoder output counts

static std::queue<AIL_INT64> ReferenceStamp;
static CRITICAL_SECTION FifoLock;
static AIL_ID CmdListId;
static AIL_ID SysId;
```
