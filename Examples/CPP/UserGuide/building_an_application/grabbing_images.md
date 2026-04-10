---
title: "grabbing_images"
description: "Allocation of an explicit DCF, grab, and free statements"
ms.snippet: "userguide.building_an_application.grabbing_images"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# grabbing_images

> Allocation of an explicit DCF, grab, and free statements

**Language:** C++ | **Version:** 7.5+

```cpp
/* Allocate a system for a Zebra Rapixo CXP board. Use a different */
/* value or "M_DEFAULT" for other types of systems.              */
MappAlloc(M_NULL, M_DEFAULT, &AilApplication);
MsysAlloc(M_DEFAULT, M_SYSTEM_RAPIXOCXP, M_DEFAULT, M_DEFAULT, &AilSystem);
MdispAlloc(AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_WINDOWED, &AilDisplay);

/* Allocate a digitizer */
MdigAlloc(AilSystem, M_DEV0, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &AilDigitizer);

/* Allocate an image buffer with the default dimensions. */
MbufAlloc2d(AilSystem, 512, 512, 8 + M_UNSIGNED, M_IMAGE + M_DISP + M_GRAB, &AilImage);

/* Display the image buffer. */
MdispSelect(AilDisplay, AilImage);

/*Grab an image into image buffer*/
MdigGrab(AilDigitizer, AilImage);


/* Free all allocations */
MbufFree(AilImage);
MdigFree(AilDigitizer);
MdispFree(AilDisplay);
MsysFree(AilSystem);
MappFree(AilApplication);
```
