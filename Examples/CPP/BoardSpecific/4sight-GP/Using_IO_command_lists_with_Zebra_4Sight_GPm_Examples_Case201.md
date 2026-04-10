---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case201"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case201"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case201

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// I/O assignment information:
// M_AUX_IO0 (output): Sent to ejector from timer 1
// M_AUX_IO8 (input): Sensor A
// M_AUX_IO9 (input): Sensor B
// M_TIMER1: generate ejector pulse in time duration
#define PULSE_WIDTH         10000000   // 10 ms

static CRITICAL_SECTION FifoLock;
static std::queue<AIL_INT64> SensorAQueue;
static std::queue<AIL_INT64> SensorBQueue;
static AIL_ID CmdListIdA, CmdListIdB;
static AIL_ID SysId;
```
