---
title: "vector_overloads03"
description: "Vector overloads examples"
ms.snippet: "userguide.building_an_application.vector_overloads03"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# vector_overloads03

> Vector overloads examples

**Language:** C++ | **Version:** 7.5+

```cpp
MimHistogram(ImageId, HistResultId);

// Get histogram values. Vector is resized automatically. Data is cast to the
// vector type (AIL_INT32) without having to specify a combination flag.
std::vector<AIL_INT32> Histogram;
MimGetResult(HistResultId, M_VALUE, Histogram);
```
