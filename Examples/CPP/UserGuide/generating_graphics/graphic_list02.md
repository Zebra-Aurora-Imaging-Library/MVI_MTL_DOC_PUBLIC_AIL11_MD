---
title: "graphic_list02"
description: "Changing settings"
ms.snippet: "userguide.generating_graphics.graphic_list02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# graphic_list02

> Changing settings

**Language:** C++ | **Version:** 7.5+

```cpp
/* Add a first dot in a graphics list, with color blue. */
MgraControl(GraphicsContextId, M_COLOR, M_COLOR_BLUE);
MgraDot(GraphicsContextId, GraphicsListId, 100, 120);
MgraInquireList(GraphicsListId, M_LIST, M_DEFAULT, M_LAST_LABEL, &BlueDotLabel);

/* Add a second dot in a graphics list, with color red. */
MgraControl(GraphicsContextId, M_COLOR, M_COLOR_RED);
MgraDot(GraphicsContextId, GraphicsListId, 100, 130);
MgraInquireList(GraphicsListId, M_LIST, M_DEFAULT, M_LAST_LABEL, &RedDotLabel);
/* (Note that the first dot remains blue). */

/* ... */

/* Change the position of the blue dot in the list. */
MgraInquireList(GraphicsListId, M_GRAPHIC_LABEL(BlueDotLabel), M_DEFAULT,
                M_POSITION_X, &PositionX);
PositionX *= 2.0;
MgraControlList(GraphicsListId, M_GRAPHIC_LABEL(BlueDotLabel), M_DEFAULT,
                M_POSITION_X, PositionX);
```
