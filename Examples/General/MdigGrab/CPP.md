---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "C++"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** C++

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```cpp
///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MDigGrab.cpp 
//
// Description: This program demonstrates how to grab from a camera in
//              continuous and monoshot mode.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

#include <AIL.h> 

int MosMain(void)
{ 
   AIL_ID AilApplication,  // Application identifier. 
          AilSystem,       // System identifier.      
          AilDisplay,      // Display identifier.     
          AilDigitizer,    // Digitizer identifier.    
          AilImage;        // Image buffer identifier.

   // Allocate defaults. 
   MappAllocDefault(M_DEFAULT, &AilApplication, &AilSystem,
                             &AilDisplay, &AilDigitizer, &AilImage);

   // Grab continuously. 
   MdigGrabContinuous(AilDigitizer, AilImage);

   // When a key is pressed, halt. 
   MosPrintf(AIL_TEXT("\nDIGITIZER ACQUISITION:\n"));
   MosPrintf(AIL_TEXT("----------------------\n\n"));
   MosPrintf(AIL_TEXT("Continuous image grab in progress.\n"));
   MosPrintf(AIL_TEXT("Press any key to stop.\n\n"));
   MosGetch();

   // Stop continuous grab. 
   MdigHalt(AilDigitizer);

   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Continuous grab stopped.\n\n"));
   MosPrintf(AIL_TEXT("Press any key to do a single image grab.\n\n"));
   MosGetch();

   // Monoshot grab. 
   MdigGrab(AilDigitizer, AilImage);

   // Pause to show the result. 
   MosPrintf(AIL_TEXT("Displaying the grabbed image.\n"));
   MosPrintf(AIL_TEXT("Press any key to end.\n\n"));
   MosGetch();

   // Free defaults. 
   MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilImage);

   return 0;
}


```
