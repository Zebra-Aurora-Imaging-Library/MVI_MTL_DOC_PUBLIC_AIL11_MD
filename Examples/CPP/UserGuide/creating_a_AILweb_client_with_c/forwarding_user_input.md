---
title: "forwarding_user_input"
description: "Forward user mouse input to a display using Aurora Imaging Web Library."
ms.snippet: "userguide.creating_a_AILweb_client_with_c.forwarding_user_input"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# forwarding_user_input

> Forward user mouse input to a display using Aurora Imaging Web Library.

**Language:** C++ | **Version:** 7.5+

```cpp
	/* Forward user mouse input to a display using AILweb */
   LRESULT CALLBACK WndProc(HWND hWnd, UINT message, WPARAM wParam, LPARAM lParam)
      {
      switch(message)
         {
         case WM_MOUSEMOVE:
            AIWL::MdispMessage(displayIdentifier, M_MOUSE_MOVE, LOWORD(lParam), HIWORD(lParam), M_NULL, M_DEFAULT);
            break;
         }
      return 0;
      }
```
