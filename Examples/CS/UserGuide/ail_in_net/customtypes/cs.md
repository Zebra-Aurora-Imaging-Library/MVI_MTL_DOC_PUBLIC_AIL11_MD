---
title: "cs"
description: "this tag is added for tests."
ms.snippet: "userguide.ail_in_net.customtypes.cs"
ms.language: "C#"
ms.env: "cs"
ms.version: "9.0"
---

# cs

> this tag is added for tests.

**Language:** C# | **Version:** 9.0+

```csharp
using System.Text;
using Zebra.AuroraImagingLibrary;

namespace DataTypes
{
    class MyDisplay
    {
        private AIL_ID _dispId = AIL.M_NULL;
        public MyDisplay(AIL_ID ownerSystem)
        {
            // The AIL_TEXT macro in C++ is replaced by a string in C#.
            AIL.MdispAlloc(ownerSystem, AIL.M_DEFAULT, "M_DEFAULT", AIL.M_DEFAULT, ref _dispId);

            // Set a custom title for the display window.
            AIL.MdispControl(_dispId, AIL.M_TITLE, "My Display");
        }

        public double ZoomFactorX
        {
            get
            {
                // The double type has the same representation in 32-bit and 64-bit
                // No need to use a custom data type to write portable code.
                double xFactor = 0.0;
                AIL.MdispInquire(_dispId, AIL.M_ZOOM_FACTOR_X, ref xFactor);
                return xFactor;
            }
        }

        public bool OverlayEnabled
        {
            get
            {
                // Using the AIL_INT type ensures that the code will be portable
                // It allows using the same code to compile a 32-bit and 64-bit
                // version of the application.
                AIL_INT overlay = AIL.MdispInquire(_dispId, AIL.M_OVERLAY, AIL.M_NULL);
                return (overlay == AIL.M_ENABLE) ? true : false;
            }
        }
    }
}
```
