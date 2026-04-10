---
doctype: Reference
module: 3dmap
function: index
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / 3dmap"
---

# 3dmap

> The functions prefixed with M3dmap make up the Aurora Imaging Library 3D Reconstruction module. The Aurora Imaging Library 3D Reconstruction module can be used to obtain corrected information from a laser line profiling setup. Such a setup consists of a device projecting a sheet of light, usually a laser diode, coupled with a camera. When an object moves under the laser plane, the camera takes images of the intersection of that plane with the object. This results in a set of images containing a laser line, each line representing a "slice" of the object being scanned. This module can also perform stereo vision triangulation if you use two or more cameras. Triangulation calculates the X-, Y-, and Z-world coordinate of a point from the point's X and Y coordinates on the image plane of each camera.

## M3dmap functions

- [M3dmapAddScan](M3dmapAddScan.md)
- [M3dmapAlignScan](M3dmapAlignScan.md)
- [M3dmapAlloc](M3dmapAlloc.md)
- [M3dmapAllocResult](M3dmapAllocResult.md)
- [M3dmapCalibrate](M3dmapCalibrate.md)
- [M3dmapCalibrateMultiple](M3dmapCalibrateMultiple.md) *(preliminary)*
- [M3dmapClear](M3dmapClear.md) *(preliminary)*
- [M3dmapControl](M3dmapControl.md)
- [M3dmapCopy](M3dmapCopy.md) *(preliminary)*
- [M3dmapCopyResult](M3dmapCopyResult.md) *(preliminary)*
- [M3dmapDraw](M3dmapDraw.md)
- [M3dmapDraw3d](M3dmapDraw3d.md)
- [M3dmapFree](M3dmapFree.md)
- [M3dmapGetResult](M3dmapGetResult.md)
- [M3dmapInquire](M3dmapInquire.md)
- [M3dmapRestore](M3dmapRestore.md)
- [M3dmapSave](M3dmapSave.md)
- [M3dmapStream](M3dmapStream.md) *(preliminary)*
- [M3dmapTriangulate](M3dmapTriangulate.md)

