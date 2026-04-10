---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Grading_using_the_ISO_IEC_15416_and_15415_standard
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Grading using the ISO IEC 15416 and 15415 standard"
---

# Grading using the ISO/IEC 15415 and ISO/IEC 15416 standards

To grade printed codes, Aurora Imaging Library supports ISO/IEC 15415 and ISO/IEC 15416. For 1D codes, use ISO/IEC 15416. For 2D codes, use ISO/IEC 15415. For 2D cross-row and composite code types, use ISO/IEC 15416 and ISO/IEC 15415.

> **Note:** Note that [`McodeGrade`](../../Reference/code/McodeGrade.md) is not supported for 4-state, Pharmacode, Postnet, and Planet 1D code types.

Before you can validate the print quality of a code, you must configure (calibrate) your setup so that you are analyzing the printed code and not how well the code can be read in the current environment. When calibrating for the ISO/IEC 15415 and ISO/IEC 15416 standards, you adjust the environment and establish exact values for the following [`McodeGrade`](../../Reference/code/McodeGrade.md) grading settings:

- [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md): This is the minimum calibrated reflectance (Rmin). It specifies the expected minimum possible grayscale value in the target image.
- [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md): This is the maximum calibrated reflectance (Rmax). It specifies the expected maximum possible grayscale value in the target image.

The general ISO standards ISO/IEC 15416 and ISO/IEC 15415 provide only general guidelines for configuring your setup for code verification. The following procedure is designed to assist in modifying your setup until ideal conditions are obtained. A grabbed image of a near-perfect code is used since you know the grade of a perfect code should be 4.0 (sometimes A), as long as the setup is not influencing the grade.

Note that if rigorous calibration is not possible (for example, because you don't have a standard test card or you have workflow constraints), you can approximate the grading setting. See [](S09_Approximating_calibration_settings.md).

1. Set the grading standard to ISO/IEC 15416 and ISO/IEC 15415 using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md).
2. In the environment in which you will eventually be grading your codes, grab an image of a perfectly printed sample code using [`MdigGrab`](../../Reference/dig/MdigGrab.md) (for example, an image of a code conformance test card for your code type, such as the GS1 Data Matrix calibrated conformance standard test card or the NIST-traceable EAN/UPC calibrated conformance test card).
   > **Note:** If grading a 2D code, you can calibrate using 1D or 2D test cards. For a 1D code type, a 1D code test card is required.
3. Set the aperture of the code model to the recommended aperture for the code. Typically, you set [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) to [`M_RELATIVE`](../../Reference/code/McodeControl.md) and [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) to [`M_AUTO`](../../Reference/code/McodeControl.md); see [](S04_Settings_for_a_code_grade_operation.md) for more information on setting the aperture.
4. Call [`McodeGrade`](../../Reference/code/McodeGrade.md). Then, retrieve the overall grade using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md).
5. If the resulting grade is not a 4.0 (perfect grade), then as necessary, adjust the image setup to:
   - Maximize the contrast and minimize noise in the image.
   - Avoid pixel saturation.
   - Have a cell size of at least 3. A smaller cell size results in a less robust result.
   - Have a uniform foreground and background grayscale level (that is, there should be no glare or dark spots present in the image).
6. Repeat steps 1-4 until the resulting grade is an "A" (or the best possible grade given the surface on which the code is printed).
7. For 1D code types, get the recommended aperture size, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_RECOMMENDED_APERTURE_SIZE`](../../Reference/code/McodeGetResult.md).
   Smooth the image using multiple calls to[`MimConvolve`](../../Reference/im/MimConvolve.md) with [`M_SMOOTH`](../../Reference/im/MimConvolve.md). [`MimConvolve`](../../Reference/im/MimConvolve.md) should be called at least once and at most a number of times equal to the following: `(_Recommended Aperture size_ + 0.5)/2`
   Find the minimum and maximum pixel values in the smoothed image using [`MimFindExtreme`](../../Reference/im/MimFindExtreme.md) with [`M_MIN_VALUE`](../../Reference/im/MimFindExtreme.md) + [`M_MAX_VALUE`](../../Reference/im/MimFindExtreme.md). These values will be used as the minimum calibrated reflectance and the maximum calibrated reflectance, respectively.
8. For 2D code types, get the minimum and maximum reflectance (grayscale) values, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_R_MIN`](../../Reference/code/McodeGetResult.md)and [`M_R_MAX`](../../Reference/code/McodeGetResult.md), respectively. These values will be used as the minimum calibrated reflectance and the maximum calibrated reflectance, respectively.
9. Set the minimum and maximum calibrated reflectance to the values previously determined in step 6 or 7, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) and [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md), respectively.
10. If the sample code and target code are of different code types, remove your sample code model from your code context and add a new code model for your target code type.
11. Grade your target codes. It is important not to modify the setup for the remainder of the grading process.

> **Note:** Note that for better accuracy, after adjusting your setup using a perfect grade code, you can also grab and test images of non-perfect codes with known grades, if present on your code conformance test card. For these codes, instead of retrieving the overall grade in step 3, retrieve the grade obtained for the code attribute that is not perfect using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_..._GRADE`](../../Reference/code/McodeGetResult.md) to see if the returned grade matches the known grade.
