---
title: "Actually_Setting_Values"
description: "Example of how to initialize a PU register"
ms.snippet: "userguide.Using_AIL_with_a_Processing_FPGA.Setting_PU_registers.Actually_Setting_Values"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Actually_Setting_Values

> Example of how to initialize a PU register

**Language:** C++ | **Version:** 8.0+

```cpp
/* Sets the gain offset registers */

/* Specifies the stream input port 0 data type. */
switch(Src1Type)
   {
   case 8+M_UNSIGNED:
      pGOShadow->ctrl.f.srctype = 0x1; break;
   case 16+M_UNSIGNED:
      pGOShadow->ctrl.f.srctype = 0x3; break;
   default:
      MosPrintf(AIL_TEXT("Gain offset error: Invalid input stream port 0 data type.\n"));
      break;
   }


/* Specifies the stream input port 1 data type. */
switch(Src2Type)
   {
   case 8+M_UNSIGNED:
      pGOShadow->ctrl.f.offtype = 0x1; break;
   case 16+M_UNSIGNED:
      pGOShadow->ctrl.f.offtype = 0x3; break;
   default:
      MosPrintf(AIL_TEXT("Gain offset  error: Invalid input stream port 1 data type.\n"));
      break;
   }

/* Specifies the stream input port 2 data type. */
switch(Src3Type)
   {
   case 8+M_UNSIGNED:
      pGOShadow->ctrl.f.gaintype = 0x1;   break;
   case 16+M_UNSIGNED:
      pGOShadow->ctrl.f.gaintype = 0x3;   break;
   default:
      MosPrintf(AIL_TEXT("Gain offset error: Invalid input stream port 3 data type.\n"));
      break;
   }

/* Specifies the stream output port 0 data type. */
switch(DstType)
   {
   case 8+M_UNSIGNED:
      pGOShadow->ctrl.f.dsttype = 0x1;   break;
   case 16+M_UNSIGNED:
      pGOShadow->ctrl.f.dsttype = 0x3;   break;
   default:
      MosPrintf(AIL_TEXT("Gain offset  error: Invalid output stream port 0 data type.\n"));
      break;
   }

/* Specifies the clipping value. */
pGOShadow->clipval.f.clipval  = lMaxValue;

/* Computes the number of bits to right-shift the output image. */
j=1;
for(i=0; i<32; i++)
   {
   if(Src4 & j)
      {
      pGOShadow->ctrl.f.shift = i;
      break;
      }
   j <<= 1;
   }
```
