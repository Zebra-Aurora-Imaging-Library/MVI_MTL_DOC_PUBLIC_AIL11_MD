---
title: "combination_values01"
description: "Unpacking the RGB macro"
ms.snippet: "userguide.building_an_application.combination_values01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# combination_values01

> Unpacking the RGB macro

**Language:** C++ | **Version:** 7.5+

```cpp
//Inquire the encoded RGB value
MdispInquire(DisplayId, M_TRANSPARENT_COLOR, &RGBValue);

//Unpack encoded RGB value into its constituent parts.
RGB_R_Value = M_RGB888_R(RGBValue);
RGB_G_Value = M_RGB888_G(RGBValue);
RGB_B_Value = M_RGB888_B(RGBValue);
```
