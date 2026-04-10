---
title: "image_buffer_allocation_example"
description: "Allocate and display an image buffer."
ms.snippet: "userguide.image_buffer_allocation_example"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# image_buffer_allocation_example

> Allocate and display an image buffer.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Allocate a system for a Zebra Rapixo CXP board. Use a different */
/* value or "M_DEFAULT" for other types of systems.              */
MappAlloc (M_NULL, M_DEFAULT, &AilApplication);
MsysAlloc (M_DEFAULT, M_SYSTEM_RAPIXOCXP, M_DEFAULT, M_DEFAULT, &AilSystem);
MdispAlloc (AilSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_WINDOWED, &AilDisplay);

/* Allocate an image buffer with the default dimensions to do graphics. */
MbufAlloc2d (AilSystem, 512, 512, 8 + M_UNSIGNED, M_IMAGE + M_DISP + M_GRAB, &AilImage);

/* Draw and display a circle */
MbufClear (AilImage, 0L);
MgraArcFill (M_DEFAULT, AilImage, 256L, 240L, 100L, 100L, 0.0, 360.0);
MgraText (M_DEFAULT, AilImage, 238L, 234L, AIL_TEXT(" Aurora Imaging Library "));

/* Display the image buffer. */
MdispSelect (AilDisplay, AilImage);

/* Print a message. */
MosPrintf (AIL_TEXT("A circle was drawn in the displayed image buffer.\n"));

/* Free all allocations */
MbufFree (AilImage);
MdispFree (AilDisplay);
MsysFree (AilSystem);
MappFree (AilApplication);
```
