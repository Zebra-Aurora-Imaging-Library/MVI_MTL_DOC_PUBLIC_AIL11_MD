---
title: "MdigGrab"
description: "This program demonstrates how to grab from a camera in continuous and monoshot mode."
ms.language: "C#"
---

# MdigGrab

> This program demonstrates how to grab from a camera in continuous and monoshot mode.

**Language:** C#

**Functions used:** `MdispAlloc`, `MappAlloc`, `MappFree`, `MbufAllocColor`, `MbufClear`, `MbufFree`, `MdigAlloc`, `MdigFree`, `MdigGrab`, `MdigGrabContinuous`, `MdigHalt`, `MdigInquire`, `MdispFree`, `MdispSelect`, `MsysAlloc`, `MsysFree`

**Categories:** Overview, General, Industries, Applications, Modules, Buffer, Display, Digitizer, What's New, Older

```csharp
﻿///////////////////////////////////////////////////////////////////////////////
// Aurora Imaging Library
// Filename:    MdigGrab.cs
//
// Description: This program demonstrates how to grab from a camera in
//              continuous and monoshot mode.
//
// (C) 1992-2026 Zebra Technologies Corp. and/or its affiliates
// All Rights Reserved
///////////////////////////////////////////////////////////////////////////////

using System;
using System.Collections.Generic;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace MDigGrab
{
    class Program
    {
        static void Main(string[] args)
        {
            AIL_ID AilApplication = AIL.M_NULL;         // Application identifier.
            AIL_ID AilSystem = AIL.M_NULL;              // System identifier.
            AIL_ID AilDisplay = AIL.M_NULL;             // Display identifier.
            AIL_ID AilDigitizer = AIL.M_NULL;           // Digitizer identifier.
            AIL_ID AilImage = AIL.M_NULL;               // Image buffer identifier.

            // Allocate defaults.
            AIL.MappAllocDefault(AIL.M_DEFAULT, ref AilApplication, ref AilSystem, ref AilDisplay, ref AilDigitizer, ref AilImage);

            // Grab continuously.
            AIL.MdigGrabContinuous(AilDigitizer, AilImage);

            // When a key is pressed, halt.
            Console.Write("\nDIGITIZER ACQUISITION:\n");
            Console.Write("----------------------\n\n");
            Console.Write("Continuous image grab in progress.\n");
            Console.Write("Press any key to stop.\n\n");
            Console.ReadKey(true);

            // Stop continuous grab.
            AIL.MdigHalt(AilDigitizer);

            // Pause to show the result.
            Console.Write("Continuous grab stopped.\n\n");
            Console.Write("Press any key to do a single image grab.\n\n");
            Console.ReadKey(true);

            // Monoshot grab.
            AIL.MdigGrab(AilDigitizer, AilImage);

            // Pause to show the result.
            Console.Write("Displaying the grabbed image.\n");
            Console.Write("Press any key to end.\n\n");
            Console.ReadKey(true);

            // Free defaults.
            AIL.MappFreeDefault(AilApplication, AilSystem, AilDisplay, AilDigitizer, AilImage);
        }
    }
}

```
