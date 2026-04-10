---
title: "ail_unique_ids"
description: "Using Aurora Imaging Library unique identifiers"
ms.snippet: "userguide.building_an_application.ail_unique_ids"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# ail_unique_ids

> Using Aurora Imaging Library unique identifiers

**Language:** C++ | **Version:** 7.5+

```cpp
// Allocate Aurora Imaging Library objects required to grab and save an image
// using standard Aurora Imaging Library identifiers and manual freeing.
void GrabAndSave(AIL_ID AilSystem, AIL_STRING filePath)
   {
   AIL_ID AilDigitizer = MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_NULL);
   AIL_ID AilBuffer = MbufAlloc2d(AilSystem, 512, 512, 8, M_IMAGE + M_GRAB + M_PROC, M_NULL);

   MdigGrab(AilDigitizer, AilBuffer);
   MbufSave(filePath, AilBuffer);

   MbufFree(AilBuffer);
   MdigFree(AilDigitizer);
   }

// Allocate Aurora Imaging Library objects required to grab and save an image
// using Aurora Imaging Library smart identifiers and automatic freeing.
void GrabAndSaveUnique(AIL_ID AilSystem, AIL_STRING filePath)
   {
   AIL_UNIQUE_DIG_ID AilDigitizer = MdigAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, M_UNIQUE_ID);
   AIL_UNIQUE_BUF_ID AilBuffer = MbufAlloc2d(AilSystem, 512, 512, 8, M_IMAGE + M_GRAB + M_PROC, M_UNIQUE_ID);

   MdigGrab(AilDigitizer, AilBuffer);
   MbufSave(filePath, AilBuffer);
   // All objects allocated within the function are freed automatically.
   }
```
