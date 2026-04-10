---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Steps_to_reading_or_verifying_a_string_in_an_image
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Steps to reading or verifying a string in an image"
---

# Steps to reading or verifying a string in an image

The following steps provide a basic methodology for using the Aurora Imaging Library Optical Character Recognition module:

1. Create a custom OCR font context using [`MocrAllocFont`](../../Reference/ocr/MocrAllocFont.md) and then use either [`MocrCopyFont`](../../Reference/ocr/MocrCopyFont.md) or [`MocrImportFont`](../../Reference/ocr/MocrImportFont.md) to add character representations to the OCR font context.
2. If necessary, specify the type of characters (alphabetic, numeric, or other) that should appear at specific positions in the string, using [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md).
   > **Note:** With custom fonts, you can hook a custom validation check function to the read/verify operations using [`MocrHookFunction`](../../Reference/ocr/MocrHookFunction.md). After checking character constraints on a found string, the validity function will be executed. If using an OCR font context allocated for a semi font, checksum calculations are performed automatically.
3. If necessary, adjust general processing controls to fit your application, using successive calls to [`MocrControl`](../../Reference/ocr/MocrControl.md).
4. Calibrate the OCR font context automatically to match the target image's character size and spacing, using [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md). Alternatively, you can set these values manually using [`MocrControl`](../../Reference/ocr/MocrControl.md).
   > **Note:** To physically modify the polarity and sizing of the font of an OCR font context, use [`MocrModifyFont`](../../Reference/ocr/MocrModifyFont.md).
5. Preprocess the OCR font context with [`MocrPreprocess`](../../Reference/ocr/MocrPreprocess.md) once all the constraints and processing controls are set.
6. Allocate a result buffer using [`MocrAllocResult`](../../Reference/ocr/MocrAllocResult.md). This buffer will be used to store subsequent read or verify result values.
7. Acquire or load a target image. Optionally, limit the area to be read or verified using either a child buffer, allocated using [`MbufChild...`](../../Reference/buf/MbufChild1d.md), or by defining a rectangular [`M_VECTOR`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md) ROI, set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). In addition, you can improve the quality of the image using the functions of some other Aurora Imaging Library module (such as the Aurora Imaging Library image processing function [`MimMorphic`](../../Reference/im/MimMorphic.md) with [`M_TOP_HAT`](../../Reference/im/MimMorphic.md) or [`M_BOTTOM_HAT`](../../Reference/im/MimMorphic.md)). Note that OCR can only process 8-bit unsigned buffers.
   > **Note:** The cleaner the background of a target image, the better the results of a read/verify operation.
8. Read or verify the string in the target image, using [`MocrReadString`](../../Reference/ocr/MocrReadString.md) or [`MocrVerifyString`](../../Reference/ocr/MocrVerifyString.md), respectively. These functions are performed according to the defined character constraints and processing controls.
9. Obtain the OCR results using [`MocrGetResult`](../../Reference/ocr/MocrGetResult.md).
10. Free all your allocated OCR objects using [`MocrFree`](../../Reference/ocr/MocrFree.md), unless [`M_UNIQUE_ID`](../../Reference/ocr/MocrAllocResult.md) was specified during allocation.

In general, the first five steps are performed once, while steps 6 to 10 are repeated as required. Since you can save the font calibration results, character constraints, and processing controls with the character representations (with [`MocrSaveFont`](../../Reference/ocr/MocrSaveFont.md) or [`MocrStream`](../../Reference/ocr/MocrStream.md)), steps 1 to 5 can be replaced by a single step which loads an OCR font context from disk using [`MocrRestoreFont`](../../Reference/ocr/MocrRestoreFont.md).

Before performing a read/verify operation, if any font-specific control or target constraint changes, the OCR font context should be preprocessed. Use [`MocrInquire`](../../Reference/ocr/MocrInquire.md) to establish if the OCR font context requires preprocessing.
