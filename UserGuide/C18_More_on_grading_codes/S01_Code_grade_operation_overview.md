---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Code_grade_operation_overview
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Code grade operation overview"
---

# Code grade operation overview

Some applications require that you evaluate the quality of a printed or marked code. Due to the possibility for codes on a product to degrade through the production process, it is often important that codes meet certain standards before leaving the marking stations. This ensures that the code is readable by most types of scanners or imagers that support your code type. For this purpose, the industry has created a set of standard print and DPM quality specifications (grading standards). These standards can be used in two distinct phases: one to prepare the physical environment in which to best validate your code so that you test the printed code and not the setup in which it is read (such as lighting, code angle, quality of the code reader), and the other to validate the code that is being read. You can use [`McodeGrade`](../../Reference/code/McodeGrade.md) for both phases.

[`McodeGrade`](../../Reference/code/McodeGrade.md) can grade codes according to the ISO/IEC 15416, ISO/IEC 15415, ISO/IEC 29158, and SEMI T10 grading standards. There are many code attributes that can be graded, all of which contribute to a code's readability. Depending on the code type and the attribute being tested, most tests are graded from 0.0 to 4.0 (F to A), or given a pass or fail mark.

> **Note:** Note that [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support grading 4-state, Pharmacode, Postnet, and Planet 1D code types.

You can grade a single code occurrence or multiple code occurrences with each call to [`McodeGrade`](../../Reference/code/McodeGrade.md), depending upon your requirements. In addition, by default, [`McodeGrade`](../../Reference/code/McodeGrade.md) internally performs a read operation before performing the grading operation. However, you can perform the read operation using [`McodeRead`](../../Reference/code/McodeRead.md) and pass the code read result buffer to [`McodeGrade`](../../Reference/code/McodeGrade.md). This is done if you do not want to grade the result of every image that you need to read.

This chapter will guide you through the steps in using the Aurora Imaging Library Code module to prepare your code setup for verification, as well as provide additional details for understanding the grading results from the Aurora Imaging Library Code module.

For details on detecting the code type, training the code grade context, or setting up a code grade context to read your code, see [Codes](../C17_Codes/ChapterInformation.md).
