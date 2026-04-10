---
title: "derivatives_with_custom_iir_filter"
ms.snippet: "userguide.image_processing.derivatives_with_custom_iir_filter"
ms.language: "C++"
ms.env: "vc"
ms.version: "7.5"
---

# derivatives_with_custom_iir_filter

**Language:** C++ | **Version:** 7.5+

```cpp
//<DESCRIPTION NAME="userguide.image_processing.derivatives_with_custom_iir_filter"/>

AIL_ID   AilSystem      = 0,           /* System identifier.                        */
         AilSrcImage    = 0,           /* Source image identifier                   */
         AilDstDxImage  = 0,           /* X derivative destination image identifier */
         AilDstDyImage  = 0,           /* Y derivative destination image identifier */
         AilFilter      = 0;           /* Filter identifier.                        */

 /* Calculate derivatives. */

 /* Allocate the kernel. */
 MimAlloc(AilSystem, M_LINEAR_FILTER_IIR_CONTEXT, M_DEFAULT, &AilFilter);

 /* Set the kernel properties. */
 MimControl(AilFilter, M_FILTER_TYPE, M_DERICHE);
 MimControl(AilFilter, M_FILTER_SMOOTHNESS, 50.0);
 MimControl(AilFilter, M_FILTER_RESPONSE_TYPE, M_SLOPE);

 /**/
 /* Extract relevant derivatives. */
 MimControl(AilFilter, M_FILTER_OPERATION, M_FIRST_DERIVATIVE_X);
 MimConvolve(AilSrcImage, AilDstDxImage, AilFilter);

 MimControl(AilFilter, M_FILTER_OPERATION, M_FIRST_DERIVATIVE_Y);
 MimConvolve(AilSrcImage, AilDstDyImage, AilFilter);
 /**/

 /* Free the kernel. */
 MimFree(AilFilter);
```
