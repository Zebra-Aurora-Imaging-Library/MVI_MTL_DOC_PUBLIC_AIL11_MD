---
title: "cs"
description: "userguide.mil_in_net.errors"
ms.snippet: "userguide.ail_in_net.errors.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "7.5"
---

# cs

> userguide.mil_in_net.errors

**Language:** C# | **Version:** 7.5+

```csharp
using System;
using System.Text;

using Zebra.AuroraImagingLibrary;

namespace AILErrors
{
    class MyApplication
    {
        private AIL_ID _ailApplication = AIL.M_NULL;
        public MyApplication()
        {
            AIL.MappAlloc(AIL.M_NULL, AIL.M_DEFAULT, ref _ailApplication);

            // Tell Aurora Imaging Library to throw exceptions when an error occurs.
            // The type of Exception thrown by Aurora Imaging Library functions is AILException.
            AIL.MappControl(_ailApplication, AIL.M_ERROR, AIL.M_THROW_EXCEPTION);
        }
    }

    class MyImage
    {
        private AIL_ID _bufId = AIL.M_NULL;
        public MyImage(AIL_ID ownerSystem, AIL_INT numberOfBands, AIL_INT sizeX, AIL_INT sizeY)
        {
            try
            {
                AIL.MbufAllocColor(ownerSystem,
                                   numberOfBands,
                                   sizeX,
                                   sizeY,
                                   8 + AIL.M_UNSIGNED,
                                   AIL.M_IMAGE + AIL.M_DISP + AIL.M_PROC + AIL.M_GRAB,
                                   ref _bufId);
            }
            catch (AILException exception)
            {
                Console.WriteLine("Image Allocation Error : {0}", exception.Message);
            }
        }
    }
}
```
