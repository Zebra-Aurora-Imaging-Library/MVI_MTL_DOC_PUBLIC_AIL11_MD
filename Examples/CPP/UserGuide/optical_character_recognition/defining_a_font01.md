---
title: "defining_a_font01"
description: "Define a font from an image using MocrCopyFont."
ms.snippet: "userguide.optical_character_recognition.defining_a_font01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# defining_a_font01

> Define a font from an image using MocrCopyFont.

**Language:** C++ | **Version:** 7.5+

```cpp
/* Define a font from an image using MocrCopyFont */

AIL_ID TheFontDefinitionImage;
MbufRestore(AIL_TEXT("CharImageForFontDefinition.mim"), AilSystem, &TheFontDefinitionImage);

AIL_ID TheFont;
/* Allocate an OCR font context */
MocrAllocFont(AilSystem, M_DEFAULT, 6, 21, 33, 3, 3, 15, 27, 3, 6, M_FOREGROUND_BLACK, &TheFont);
/* Copy font characters from the image to the font context */
MocrCopyFont(TheFontDefinitionImage, TheFont, M_COPY_TO_FONT, AIL_TEXT("ABC123"));
/* Save the OCR font context to disk */
MocrSaveFont(AIL_TEXT("TheFontMocrCopyFont.mfo"), M_SAVE, TheFont);

MocrFree(TheFont);
MbufFree(TheFontDefinitionImage);
```
