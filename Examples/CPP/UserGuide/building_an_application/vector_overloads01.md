---
title: "vector_overloads01"
description: "Vector overloads examples"
ms.snippet: "userguide.building_an_application.vector_overloads01"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# vector_overloads01

> Vector overloads examples

**Language:** C++ | **Version:** 7.5+

```cpp
// Get pixel values from the image. Vector is resized automatically.
// Data type AIL_UINT8 is checked against the buffer data type in debug mode.
std::vector<AIL_UINT8> Pixels;
MbufGet(ImageId, Pixels);

// Do some operations on the pixel values...

// Put values back into the image. Size and data type are validated in debug mode.
MbufPut(ImageId, Pixels);
```
