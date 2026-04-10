---
title: "retrieving_results_and_annotation"
description: "Retrieve all char names from all strings"
ms.snippet: "userguide.suredotocr.retrieving_results_and_annotation"
ms.language: "C++"
ms.env: "vc"
ms.version: "10.20"
---

# retrieving_results_and_annotation

> Retrieve all char names from all strings

**Language:** C++ | **Version:** 10.20+

```cpp
//Number of strings.
MdmrGetResult(AilDmrResult, M_GENERAL, M_DEFAULT, M_STRING_NUMBER + M_TYPE_AIL_INT, &NumberOfStringRead);

//Number of characters for each string.
AIL_INT* NbChar = new AIL_INT[NumberOfStringRead];
MdmrGetResult(AilDmrResult, M_ALL, M_GENERAL, M_FORMATTED_STRING_CHAR_NUMBER + M_TYPE_AIL_INT, NbChar);

//Total number of characters (summation).
AIL_INT TotalNbChar = 0;
for(AIL_INT k = 0; k < NumberOfStringRead; k++)
   {
   TotalNbChar += NbChar[k];
   }

//Size of each character name.
AIL_INT* CharNameSize = new AIL_INT[TotalNbChar];
MdmrGetResult(AilDmrResult, M_ALL, M_INDEX_IN_FORMATTED_STRING(M_ALL), M_CHAR_NAME + M_STRING_SIZE + M_TYPE_AIL_INT, CharNameSize);

//Size of all character names (summation).
AIL_INT SizoForAllName = 0;
for(AIL_INT k = 0; k < TotalNbChar; k++)
   SizoForAllName += CharNameSize[k];

//Total required array size for all character names for all strings.
AIL_TEXT_CHAR* AllName = new AIL_TEXT_CHAR[SizoForAllName];
MdmrGetResult(AilDmrResult, M_ALL, M_INDEX_IN_FORMATTED_STRING(M_ALL), M_CHAR_NAME, AllName);
```
