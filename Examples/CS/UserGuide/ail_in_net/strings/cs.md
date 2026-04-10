---
title: "cs"
description: "userguide.mil_in_net.strings"
ms.snippet: "userguide.ail_in_net.strings.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "7.5"
---

# cs

> userguide.mil_in_net.strings

**Language:** C# | **Version:** 7.5+

```csharp
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace StringsWithAIL.SingleString
{
    class MyDigitizer
    {
        private AIL_ID _digId = AIL.M_NULL;

        public MyDigitizer(AIL_ID ownerSystem)
        {
            // Allocate the default digitizer on the specified system.
            AIL.MdigAlloc(ownerSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref _digId);
        }

        public string Format
        {
            get
            {
                // Initialize a new StringBuilder object.
                // Aurora Imaging Library will resize it to the appropriate size.
                StringBuilder format = new StringBuilder();
                AIL.MdigInquire(_digId, AIL.M_FORMAT, format);

                // Use the ToString method to return a string from the StringBuilder object.
                return format.ToString();
            }
        }
    }
}
```
