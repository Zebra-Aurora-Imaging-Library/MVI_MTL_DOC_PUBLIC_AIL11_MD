---
title: "defining_a_font02"
description: "Define a font from an image using MocrImportFont."
ms.snippet: "userguide.optical_character_recognition.defining_a_font02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# defining_a_font02

> Define a font from an image using MocrImportFont.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Define a font directly from an image file using MocrImportFont */

AIL_ID TheFont;
/* Allocate an OCR font context */
MocrAllocFont(AilSystem, M_DEFAULT, 6, 21, 33, 3, 3, 15, 27, 3, 6, M_FOREGROUND_BLACK, &TheFont);
/* Import character representations from the font definition image */
MocrImportFont(AIL_TEXT("CharImageForFontDefinition.mim"), M_AIL_TIFF, M_LOAD_CHARACTER, AIL_TEXT("ABC123"), TheFont);
/* Save the OCR font context to disk */
MocrSaveFont(AIL_TEXT("TheFontMocrImportFont.mfo"), M_SAVE, TheFont);

MocrFree(TheFont);
```
