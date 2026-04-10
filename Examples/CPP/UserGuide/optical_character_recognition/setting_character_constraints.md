---
title: "setting_character_constraints"
description: "Example of setting character constraints"
ms.snippet: "userguide.optical_character_recognition.setting_character_constraints"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# setting_character_constraints

> Example of setting character constraints

**Language:** C++ | **Version:** 7.5+

```cpp
/* Set character constraints for each position of the string to read. */
MocrSetConstraint(OcrFont, 0, M_LETTER, AIL_TEXT("K")); /* Must be K. */
MocrSetConstraint(OcrFont, 1, M_LETTER, M_NULL); /* Any letter. */
MocrSetConstraint(OcrFont, 2, M_DIGIT, M_NULL); /* Any digit. */
MocrSetConstraint(OcrFont, 3, M_DIGIT, AIL_TEXT("12")); /* Must be 1 or 2. */
MocrSetConstraint(OcrFont, 4, M_DIGIT, M_NULL); /* Any digit. */
MocrSetConstraint(OcrFont, 5, M_DEFAULT,M_NULL); /* Any character. */
```
