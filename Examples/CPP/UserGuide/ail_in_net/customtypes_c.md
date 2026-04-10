---
title: "customtypes_c"
description: "this tag is added for tests."
ms.snippet: "userguide.ail_in_net.customtypes_c"
ms.language: "C++"
ms.env: "vc"
ms.version: "9.0"
---

# customtypes_c

> this tag is added for tests.

**Language:** C++ | **Version:** 9.0+

```cpp
class MyDisplay
{
private:
    AIL_ID _dispId;

public:
    MyDisplay(AIL_ID ownerSystem)
    {
        // Allocate the default display on the specified system.
        MdispAlloc(ownerSystem, M_DEFAULT, AIL_TEXT("M_DEFAULT"), M_DEFAULT, &_dispId);

        // Set a custom title for the display window.
        MdispControl(_dispId, M_TITLE, AIL_TEXT("My Display"));
    }

    AIL_DOUBLE GetZoomFactorX()
    {
        AIL_DOUBLE xFactor = 0.0;
        MdispInquire(_dispId, M_ZOOM_FACTOR_X, &xFactor);
        return xFactor;
    }

    bool GetOverlayEnabled()
    {
        AIL_INT overlay = MdispInquire(_dispId, M_OVERLAY, M_NULL);
        return (overlay == M_ENABLE) ? true : false;
    }
};
```
