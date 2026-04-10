---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: OCR_module
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / OCR module"
---

# Aurora Imaging Library OCR module

Many types of industries require the analysis of character strings in images. For example, the semiconductor industry requires serial numbers, printed on wafers, to be read for tracking purposes. The pharmaceutical industry requires analysis of medicine bottle labels to ensure, for example, that expiry dates are properly printed.

The Aurora Imaging Library Optical Character Recognition (OCR) module is template-based and provides a powerful and easy to use function set for reading and verifying mechanically generated character strings in 8-bit grayscale images, providing results such as quality (match) scores and validity flags. The module is especially designed to operate on character strings in degraded images, with up to ± 180° of rotation in the target string. The OCR module can be used in conjunction with other Aurora Imaging Library functions to develop hardware independent OCR programs for machine vision applications.

The module offers the choice of loading a set of grayscale character representations from an Aurora Imaging Library font file, or gathering the needed grayscale character representations from image buffers and/or other files to create an Aurora Imaging Library font. Each character in the string to be read (target string) is compared to each character representation in this font. The representation with the closest match is chosen and its ASCII value is returned. You can adjust the value at which this match is considered a success by setting the acceptance level. A read operation yields a string of ASCII characters, a confidence score for each character, and its position in the target string, as well as a confidence score for the whole string. A verify operation also provides a validity flag for each character.

For applications requiring custom font types, the OCR module supports the creation of custom Aurora Imaging Library fonts that can be saved to disk and restored as needed. Two predefined Aurora Imaging Library font files are provided to read and verify semiconductor wafer serial numbers of standard SEMI font character types.

The module also supports user-specified character constraints, the automatic resizing of a font to better match the target characters (automatic font calibration), and the reading of texts that span multiple lines. To ensure recognition accuracy, checksum calculations are performed when analyzing standard SEMI font character strings. For strings of custom font types, user-defined functions can validate the read character string automatically (for example, using custom checksum functions).

> **Note:** Note that if the strings you must read are made up of dot-matrix characters, use the [Aurora Imaging Library SureDotOCR module](../C15_SureDotOCR/S01_SureDotOCR_module.md).
