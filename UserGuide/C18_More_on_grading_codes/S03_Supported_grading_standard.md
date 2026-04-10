---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Supported_grading_standard
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Supported grading standard"
---

# Supported grading standard

[`McodeGrade`](../../Reference/code/McodeGrade.md) supports grading using the grading standards listed below. The selected grading standard affects the implementation of some control types, and can cause different results when grading the same code.

- **ISO/IEC 15415**. Used for printed 2D codes (for example, QR, MicroQR, DotCode, and Atzec). For details, see [](S06_Grading_using_the_ISO_IEC_15416_and_15415_standard.md).
- **ISO/IEC 15416**. Used for printed 1D codes (for example, BC-412, Codabar, and Code 39). For details, see [](S06_Grading_using_the_ISO_IEC_15416_and_15415_standard.md).
- **ISO/IEC 29158**. Used for DPM (direct part marking) 2D codes (for example, Aztec, Data Matrix and QR). For details, see [](S07_Grading_using_the_ISO_IEC_29158_standard.md).
- **SEMI T10**. Used for DPM (direct part marking) Data Matrix codes on semiconductor parts. DPM is the process of etching or printing codes directly into or on to parts. For details, see [](S08_Grading_using_the_semi_t10_standard.md).

To use these grading standards, first, set the grading standard using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md). By default, the latest supported edition of these standards is used. To specify a specific edition, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md).
