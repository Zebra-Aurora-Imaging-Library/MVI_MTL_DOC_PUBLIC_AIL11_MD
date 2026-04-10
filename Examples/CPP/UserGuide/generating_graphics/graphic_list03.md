---
title: "graphic_list03"
description: "Annotating display"
ms.snippet: "userguide.generating_graphics.graphic_list03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# graphic_list03

> Annotating display

**Language:** C++ | **Version:** 7.5+

```cpp
/* Add a line in the graphics list. */
MgraLine(GraphicsContextId, GraphicsListId, 100, 100, 200, 200);

/* Annotate a display using the graphics list. */
MdispControl(DisplayId, M_ASSOCIATED_GRAPHIC_LIST_ID, GraphicsListId);

/* Zoom the display. The line's thickness does not change; it remains at one pixel. */
MdispZoom(DisplayId, 2.0, 2.0);

/* Change the color of the line in the graphics list. This will automatically update
   the display. */
MgraControlList(GraphicsListId, M_GRAPHIC_INDEX(0), M_DEFAULT, M_COLOR, M_COLOR_GREEN);
```
