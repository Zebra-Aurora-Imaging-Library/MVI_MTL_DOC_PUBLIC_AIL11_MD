---
title: "defining_a_font03"
description: "Define a font from an ASCII file using MocrImportFont."
ms.snippet: "userguide.optical_character_recognition.defining_a_font03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# defining_a_font03

> Define a font from an ASCII file using MocrImportFont.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Define a font from an ASCII file using MocrImportFont */

AIL_ID TheFont;
/* Allocate an OCR font context */
MocrAllocFont(AilSystem, M_DEFAULT, 6, 21, 33, 3, 3, 15, 27, 3, 6, M_FOREGROUND_BLACK, &TheFont);
/* Import character representations from the font definition ASCII file */
MocrImportFont(AIL_TEXT("AsciiFileForFontDefinition.txt"), M_FONT_ASCII, M_LOAD_CHARACTER, M_NULL, TheFont);
/* Save the OCR font context to disk */
MocrSaveFont(AIL_TEXT("TheFontMocrImportFromASCIIFile.mfo"), M_SAVE, TheFont);

MocrFree(TheFont);
```
