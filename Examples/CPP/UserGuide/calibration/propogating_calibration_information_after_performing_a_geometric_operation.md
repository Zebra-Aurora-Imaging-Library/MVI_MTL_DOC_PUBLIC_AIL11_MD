---
title: "propogating_calibration_information_after_performing_a_geometric_operation"
description: "Rotating a calibrated image 90 degrees"
ms.snippet: "userguide.calibration.propogating_calibration_information_after_performing_a_geometric_operation"
ms.language: "C++"
ms.env: "vc"
ms.version: "8.0"
---

# propogating_calibration_information_after_performing_a_geometric_operation

> Rotating a calibrated image 90 degrees

**Language:** C++ | **Version:** 8.0+

```cpp
// Generate the warping matrix to rotate the source image +90 degrees around its center.
// This is the composition of a translation to the source's center, a rotation, and then a
// translation from the destination's center.
AIL_DOUBLE SrcCenterX = 0.5*(SrcSizeX-1);
AIL_DOUBLE SrcCenterY = 0.5*(SrcSizeY-1);
AIL_DOUBLE DstCenterX = 0.5*(DstSizeX-1);
AIL_DOUBLE DstCenterY = 0.5*(DstSizeY-1);
AIL_ID WarpMatrixArrayBufId = MbufAlloc2d(SysId, 3, 3, 32+M_FLOAT, M_ARRAY, M_NULL);
MgenWarpParameter(M_NULL, WarpMatrixArrayBufId, M_NULL, M_WARP_POLYNOMIAL,
   M_TRANSLATE, -SrcCenterX, -SrcCenterY);
MgenWarpParameter(WarpMatrixArrayBufId, WarpMatrixArrayBufId, M_NULL, M_WARP_POLYNOMIAL,
   M_ROTATE, 90.0, M_NULL);
MgenWarpParameter(WarpMatrixArrayBufId, WarpMatrixArrayBufId, M_NULL, M_WARP_POLYNOMIAL,
   M_TRANSLATE, DstCenterX, DstCenterY);

// Warp the image.
MimWarp(SrcImageBufId, DstImageBufId, WarpMatrixArrayBufId, M_NULL, M_WARP_POLYNOMIAL,
   M_BILINEAR+M_OVERSCAN_CLEAR);
// Same as:
// MimRotate(SrcImageBufId, DstImageBufId, 90.0, M_DEFAULT, M_DEFAULT, M_DEFAULT,
//    M_DEFAULT, M_BILINEAR+M_OVERSCAN_CLEAR);

// Warp the pixel-to-world mapping of the calibration context 
// and associate the context to the warped image.
AIL_ID DstContextCalId = McalAlloc(SysId, M_LINEAR_INTERPOLATION, M_DEFAULT, M_NULL);
McalWarp(SrcImageBufId, DstContextCalId, WarpMatrixArrayBufId, M_NULL, 0.0, 0.0,
   (AIL_DOUBLE)DstSizeX, (AIL_DOUBLE)DstSizeY, M_DEFAULT, M_DEFAULT, M_WARP_POLYNOMIAL,
   M_DEFAULT);

if (McalInquire(DstContextCalId, M_CALIBRATION_STATUS, M_NULL) == M_CALIBRATED)
   {
   McalAssociate(DstContextCalId, DstImageBufId, M_DEFAULT);
   }
else
   {
   MosPrintf(AIL_TEXT("Calibration warping has failed.\n"));
   }
```
