---
doctype: Reference
module: buf
function: index
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / buf"
---

# buf

> The functions prefixed with Mbuf make up the Buffer module. The Buffer module allows you to allocate and control data buffers (storage areas) and containers (Aurora Imaging Library objects that hold buffers and other containers) that are typically operated on by functions of several Aurora Imaging Library modules. Examples of buffers include image buffers and lookup table (LUT) buffers. The module allows you to isolate regions of a buffer using child buffers or regions of interest, copy regions or bit-planes of a buffer to another buffer, and convert images grabbed from a camera with a Bayer filter into 3-band color images. The module can archive and retrieve buffer data in common storage formats (such as TIFF, JPEG, and AVI). The module also allows you to compress and decompress images and sequences, lossily or losslessly.

## Mbuf functions

- [MbufAlloc1d](MbufAlloc1d.md) *(preliminary)*
- [MbufAlloc2d](MbufAlloc2d.md) *(preliminary)*
- [MbufAllocColor](MbufAllocColor.md) *(preliminary)*
- [MbufAllocComponent](MbufAllocComponent.md) *(preliminary)*
- [MbufAllocContainer](MbufAllocContainer.md) *(preliminary)*
- [MbufAllocDefault](MbufAllocDefault.md) *(preliminary)*
- [MbufBayer](MbufBayer.md) *(preliminary)*
- [MbufChild1d](MbufChild1d.md)
- [MbufChild2d](MbufChild2d.md)
- [MbufChildColor2d](MbufChildColor2d.md)
- [MbufChildColor2dClip](MbufChildColor2dClip.md) *(preliminary)*
- [MbufChildColor](MbufChildColor.md)
- [MbufChildContainer](MbufChildContainer.md) *(preliminary)*
- [MbufChildMove](MbufChildMove.md)
- [MbufClear](MbufClear.md) *(preliminary)*
- [MbufClearCond](MbufClearCond.md) *(preliminary)*
- [MbufClone](MbufClone.md)
- [MbufControl](MbufControl.md) *(preliminary)*
- [MbufControlArea](MbufControlArea.md)
- [MbufControlContainer](MbufControlContainer.md) *(preliminary)*
- [MbufControlFeature](MbufControlFeature.md) *(preliminary)*
- [MbufConvert3d](MbufConvert3d.md) *(preliminary)*
- [MbufCopy](MbufCopy.md) *(preliminary)*
- [MbufCopyClip](MbufCopyClip.md)
- [MbufCopyColor2d](MbufCopyColor2d.md)
- [MbufCopyColor](MbufCopyColor.md)
- [MbufCopyComponent](MbufCopyComponent.md) *(preliminary)*
- [MbufCopyCond](MbufCopyCond.md)
- [MbufCopyMask](MbufCopyMask.md) *(preliminary)*
- [MbufCreate2d](MbufCreate2d.md)
- [MbufCreateColor](MbufCreateColor.md)
- [MbufCreateComponent](MbufCreateComponent.md)
- [MbufDiskInquire](MbufDiskInquire.md) *(preliminary)*
- [MbufExport](MbufExport.md)
- [MbufExportSequence](MbufExportSequence.md)
- [MbufFree](MbufFree.md)
- [MbufFreeComponent](MbufFreeComponent.md) *(preliminary)*
- [MbufGet1d](MbufGet1d.md)
- [MbufGet2d](MbufGet2d.md)
- [MbufGet](MbufGet.md)
- [MbufGetArc](MbufGetArc.md)
- [MbufGetColor2d](MbufGetColor2d.md)
- [MbufGetColor](MbufGetColor.md)
- [MbufGetHookInfo](MbufGetHookInfo.md)
- [MbufGetLine](MbufGetLine.md)
- [MbufGetList](MbufGetList.md)
- [MbufHookFunction](MbufHookFunction.md)
- [MbufImport](MbufImport.md)
- [MbufImportSequence](MbufImportSequence.md)
- [MbufInquire](MbufInquire.md) *(preliminary)*
- [MbufInquireContainer](MbufInquireContainer.md) *(preliminary)*
- [MbufInquireFeature](MbufInquireFeature.md) *(preliminary)*
- [MbufLink](MbufLink.md) *(preliminary)*
- [MbufLoad](MbufLoad.md)
- [MbufPut1d](MbufPut1d.md)
- [MbufPut2d](MbufPut2d.md)
- [MbufPut](MbufPut.md)
- [MbufPutColor2d](MbufPutColor2d.md)
- [MbufPutColor](MbufPutColor.md)
- [MbufPutLine](MbufPutLine.md)
- [MbufPutList](MbufPutList.md)
- [MbufRestore](MbufRestore.md)
- [MbufSave](MbufSave.md)
- [MbufStream](MbufStream.md) *(preliminary)*
- [MbufSetRegion](MbufSetRegion.md)
- [MbufTransfer](MbufTransfer.md) *(preliminary)*

