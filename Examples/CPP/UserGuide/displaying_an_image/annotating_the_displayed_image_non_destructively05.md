---
title: "annotating_the_displayed_image_non_destructively05"
description: "Drawing in overlay"
ms.snippet: "userguide.displaying_an_image.annotating_the_displayed_image_non_destructively05"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# annotating_the_displayed_image_non_destructively05

> Drawing in overlay

**Language:** C++ | **Version:** 7.5+

```cpp
HDC hCustomDC;
HGDIOBJ hpen, hpenOld;
AIL_TEXT_CHAR chText[80];
SIZE  TxtSz;
RECT  Txt;
int   Count;

/* Create a device context for drawing.  */
MbufControl(AilOverlayImage, M_DC_ALLOC, M_DEFAULT);
/* Inquire the handle of the device context. */
hCustomDC = ((HDC)MbufInquire(AilOverlayImage, M_DC_HANDLE, M_NULL));
if (hCustomDC){
   /* Create a blue pen. */
   hpen=CreatePen(PS_SOLID, 1, RGB(0, 0, 255));
   hpenOld = SelectObject(hCustomDC,hpen);

   /* Draw a cross in the overlay buffer.  */
   MoveToEx(hCustomDC,0,ImageHeight/2,NULL);
   LineTo(hCustomDC,ImageWidth,ImageHeight/2);
   MoveToEx(hCustomDC,ImageWidth/2,0,NULL);
   LineTo(hCustomDC,ImageWidth/2,ImageHeight);

   /* Write text in the overlay buffer.  */
   MosStrcpy(chText, 80, AIL_TEXT("GDI Overlay Text "));
   Count = (int) MosStrlen(chText);
   GetTextExtentPoint(hCustomDC, chText, Count, &TxtSz);

   Txt.left = (AIL_INT32)(ImageWidth*3/18);
   Txt.top  = (AIL_INT32)(ImageHeight*4/6);
   Txt.right  = (AIL_INT32)(Txt.left + TxtSz.cx);
   Txt.bottom = (AIL_INT32)(Txt.top  + TxtSz.cy);
   SetTextColor(hCustomDC,RGB(0, 0, 255));
   DrawText(hCustomDC, chText, Count, &Txt, DT_RIGHT);

   Txt.left = (AIL_INT32)(ImageWidth*12/18);
   Txt.top  = (AIL_INT32)(ImageHeight*4/6);
   Txt.right  = (AIL_INT32)(Txt.left + TxtSz.cx);
   Txt.bottom = (AIL_INT32)(Txt.top  + TxtSz.cy);
   SetTextColor(hCustomDC,RGB(255, 0, 0));
   DrawText(hCustomDC, chText, Count, &Txt, DT_RIGHT);

   /* Deselect and destroy the blue pen.  */
   SelectObject(hCustomDC,hpenOld);
   DeleteObject(hpen);
}

/* Free the created device context.  */
MbufControl(AilOverlayImage, M_DC_FREE, M_DEFAULT);

/* Signal Aurora Imaging Library that the overlay buffer was modified.  */
MbufControl(AilOverlayImage, M_MODIFIED, M_DEFAULT);
```
