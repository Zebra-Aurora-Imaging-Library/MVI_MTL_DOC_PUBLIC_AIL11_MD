---
title: "graphic_list04"
description: "MgraDraw"
ms.snippet: "userguide.generating_graphics.graphic_list04"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# graphic_list04

> MgraDraw

**Language:** C++ | **Version:** 7.5+

```cpp
/* Add a line in the graphics list and draw this graphics list in an image. */
MgraLine(GraphicsContextId, GraphicsListId, 100, 100, 200, 200);
MgraDraw(GraphicsListId, ImageId, M_DEFAULT);
/* This has the same effect as doing
   MgraLine(GraphicsContextId, ImageId, 100, 100, 200, 200);
*/
```
