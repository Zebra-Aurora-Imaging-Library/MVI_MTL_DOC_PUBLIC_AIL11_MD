---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case301"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case301"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case301

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// M_AUX_IO0 (output): Ejector A
// M_AUX_IO1 (output): Ejector B
// M_AUX_IO2 (output): Ejector C
// M_AUX_IO8 (input): sensor
// M_TIMER1: generate ejector pulse in time duration for ejector A
// M_TIMER2: generate ejector pulse in time duration for ejector B
// M_TIMER3: generate ejector pulse in time duration for ejector C
// M_AUX_IO14: Rotary encoder pin 1
// M_AUX_IO15: Rotary encoder pin 2
#define PULSE_WIDTH         10000000   // 10 ms
#define EJECTOR_OFFSET_A    800        // rotary decoder output counts to ejector A
#define EJECTOR_OFFSET_B    1000       // rotary decoder output counts to ejector B
#define EJECTOR_OFFSET_C    1200       // rotary decoder output counts to ejector C

static std::queue<AIL_INT64> ReferenceStamp;
static CRITICAL_SECTION FifoLock;
static AIL_ID CmdListId;
static AIL_ID SysId;
```
