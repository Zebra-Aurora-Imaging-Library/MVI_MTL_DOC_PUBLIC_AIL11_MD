---
title: "combination_values02"
description: "Passing AIL_TEXT in both a variable and directly."
ms.snippet: "userguide.building_an_application.combination_values02"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# combination_values02

> Passing AIL_TEXT in both a variable and directly.

**Language:** C++ | **Version:** 7.5+

```cpp
//Using the AIL_TEXT macro on its own to specify a file path

MbufSave(AIL_TEXT("C:/myfolder/myfile.mim"), AILImageBuf);


//Passing a string to the AIL_TEXT macro, which is then enclosed in a variable
AIL_CONST_TEXT_PTR AILFilePath = AIL_TEXT("C:/myfolder/myfile.mim");

//Passing the variable as the argument for the file name parameter. Beware to not enclose the variable in AIL_TEXT()
MbufSave(AILFilePath, AILImageBuf);
```
