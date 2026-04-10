---
title: "Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401"
description: "4-Sight GPm I/O command list examples"
ms.snippet: "boardspecific.4sight-GP.Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# Using_IO_command_lists_with_Zebra_4Sight_GPm_Examples_Case401

> 4-Sight GPm I/O command list examples

**Language:** C++ | **Version:** 7.5+

```cpp
// M_AUX_IO14: Receives rotary encoder bit 0
// M_AUX_IO15: Receives rotary encoder bit 1
// M_AUX_IO8 (input): Object detector
// M_AUX_IO0 (output): trigger to the camera, 100 positions after object detector
#define CAMERA_TRIGGER_OFFSET 100   // in rotary encoder steps

AIL_INT MFTYPE ProcessingFunction(AIL_INT HookType, AIL_ID HookId, void* HookDataPtr);

typedef struct _HOOK_PARAM
{
   AIL_ID AilSystem;
   AIL_ID CmdListId;
} HOOK_PARAM, *PHOOK_PARAM;
```
