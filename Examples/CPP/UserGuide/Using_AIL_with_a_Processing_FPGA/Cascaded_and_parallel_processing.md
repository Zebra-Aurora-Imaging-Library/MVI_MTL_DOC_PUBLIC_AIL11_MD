---
title: "Cascaded_and_parallel_processing"
description: "Example of an FPGA Register"
ms.snippet: "userguide.Using_AIL_with_a_Processing_FPGA.Cascaded_and_parallel_processing"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# Cascaded_and_parallel_processing

> Example of an FPGA Register

**Language:** C++ | **Version:** 8.0+

```cpp
/*************************************************************
* Register name : ctrl
* Address       : 0x60
**************************************************************/
typedef union
{
   AIL_UINT64 u64;
   AIL_UINT32 u32;
   AIL_UINT16 u16;
   AIL_UINT8  u8;

   struct
   {
      AIL_UINT64 dsttype              : 2;  /* Bits<1:0>,      */
             /* The data type and depth of the output image. */
      AIL_UINT64 gaintype             : 2;  /* Bits<3:2>,      */
             /* The data type and depth of the gain image.   */
      AIL_UINT64 offtype              : 2;  /* Bits<5:4>,      */
             /* The data type and depth of the offset image. */
      AIL_UINT64 srctype              : 2;  /* Bits<7:6>,      */
             /* The data depth and type of the input image on
             /* which to operate.                            */
      AIL_UINT64 shift                : 5;  /* Bits<12:8>,     */
             /* Number of bits to right-shift.               */
      AIL_UINT64 rsvd0                : 19; /* Bits<31:13>,    */
             /* Reserved Field                               */
   } f;

} FPGA_GAINOFFSET_REG_CTRL;
```
