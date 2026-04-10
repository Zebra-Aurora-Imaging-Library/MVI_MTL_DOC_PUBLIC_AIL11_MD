---
title: "roi_opacity_overlay01"
description: "Add red overlay for pixel outside of ROI."
ms.snippet: "userguide.displaying_an_image.roi_opacity_overlay01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# roi_opacity_overlay01

> Add red overlay for pixel outside of ROI.

**Language:** C++ | **Version:** 7.5+

```cpp
MdispControl(DispId, M_REGION_OUTSIDE_COLOR, M_COLOR_RED);
MdispControl(DispId, M_REGION_OUTSIDE_SHOW, M_OPAQUE);
```
