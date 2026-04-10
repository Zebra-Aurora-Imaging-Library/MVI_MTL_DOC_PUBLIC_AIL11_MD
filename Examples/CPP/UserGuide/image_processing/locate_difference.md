---
title: "locate_difference"
description: "MimLocateDifference()"
ms.snippet: "userguide.image_processing.locate_difference"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# locate_difference

> MimLocateDifference()

**Language:** C++ | **Version:** 7.5+

```cpp
AIL_ID  LocateDifferenceResult = MimAllocResult(M_DEFAULT_HOST, M_DEFAULT, M_LOCATE_DIFFERENCE_RESULT, M_NULL);  /*Allocate the result buffer for LocateDifference results */
MimLocateDifference(M_LOCATE_DIFFERENCE_CONTEXT_TOLERANCE_MAX, Src1, Src2, LocateDifferenceResult, 1, 0, M_DEFAULT);  /*Perform the Locate difference operation*/ 
AIL_INT NbDiff; 
MimGetResult(LocateDifferenceResult, M_NUMBER, &NbDiff); /*Retrieve the number of differences*/
std::vector< AIL_INT >    PosX(NbDiff); /*Initialize arrarys in X and Y with size of NbDiff*/
std::vector< AIL_INT >    PosY(NbDiff);
MimGetResult(LocateDifferenceResult, M_POSITION_X, PosX); /*write the positions of the differences in X and Y to the corresponding array*/
MimGetResult(LocateDifferenceResult, M_POSITION_Y, PosY);
```
