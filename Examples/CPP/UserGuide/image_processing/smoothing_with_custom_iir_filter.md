---
title: "smoothing_with_custom_iir_filter"
ms.snippet: "userguide.image_processing.smoothing_with_custom_iir_filter"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# smoothing_with_custom_iir_filter

**Language:** C++ | **Version:** 7.5+

```cpp
//<DESCRIPTION NAME="userguide.image_processing.smoothing_with_custom_iir_filter"/>

AIL_ID   AilSystem   = 0,           /* System identifier.            */
         AilSrcImage = 0,           /* Source image identifier       */
         AilDstImage = 0,           /* Destination image identifier  */
         AilFilter   = 0;           /* Filter identifier.            */

 /* Smoothing the source image. */

 /* Allocate the kernel. */
 MimAlloc(AilSystem, M_LINEAR_FILTER_IIR_CONTEXT, M_DEFAULT, &AilFilter);

 /* Set the kernel properties. */
 MimControl(AilFilter, M_FILTER_TYPE, M_DERICHE);
 MimControl(AilFilter, M_FILTER_SMOOTHNESS, 50.0);
 MimControl(AilFilter, M_FILTER_OPERATION, M_SMOOTH);

 /**/
 /* Smooth the image. */
 MimConvolve(AilSrcImage, AilDstImage, AilFilter);
 /**/

 /* Free the kernel. */
 MimFree(AilFilter);
```
