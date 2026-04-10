---
doctype: Reference
module: code
function: index
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code"
---

# code

> The functions prefixed with Mcode make up the Code module. The Code module allows you to read, write, and grade symbols (codes) from most popular one-dimensional (1D) and two-dimensional (2D) symbologies (code types). The supported 1D code types include 4-state, BC412, Codeabar, Code 39, Code 93, Code 128, EAN 8, EAN 13, EAN 14, GS1-128, GS1 Databar, Industrial 2 of 5 (ITF-14), Interleaved 2 of 5 (ITF-14), Pharmacode, Planet, Postnet, UPC-A, and UPC-E. The supported 2D code types are Aztec, Data Matrix, Maxicode, QR code, Micro QR code, PDF417, MicroPDF417, and Truncated PDF417. Composite codes, which are combinations of 1D and 2D code types, are also supported. The module easily handles rotated, scaled, and degraded code occurrences, even in complex scenes. To help setup for reading or grading, the Code module can detect the type of most 1D code occurrences in an image. In addition, the module can optimize the read or grading operation settings by training them using a set of sample images.

## Mcode functions

- [McodeAlloc](McodeAlloc.md)
- [McodeAllocResult](McodeAllocResult.md)
- [McodeControl](McodeControl.md) *(preliminary)*
- [McodeDetect](McodeDetect.md)
- [McodeDraw](McodeDraw.md)
- [McodeFree](McodeFree.md)
- [McodeGetResult](McodeGetResult.md) *(preliminary)*
- [McodeGrade](McodeGrade.md) *(preliminary)*
- [McodeInquire](McodeInquire.md)
- [McodeModel](McodeModel.md)
- [McodeRead](McodeRead.md)
- [McodeRestore](McodeRestore.md)
- [McodeSave](McodeSave.md)
- [McodeStream](McodeStream.md) *(preliminary)*
- [McodeTrain](McodeTrain.md)
- [McodeWrite](McodeWrite.md)

