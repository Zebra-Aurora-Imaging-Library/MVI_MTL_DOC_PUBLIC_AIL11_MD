---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Grading_using_the_ISO_IEC_29158_standard
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Grading using the ISO IEC 29158 standard"
---

# Grading using the ISO/IEC 29158 standard (for DPM)

Direct part marking (DPM) codes are codes that are etched directly on to the parts instead of labels. To grade 2D DPM (direct part marking) codes, use ISO/IEC 29158. The ISO/IEC 29158 standard is an extension of the ISO/IEC 15415 standard and is intended for codes that are marked directly on a surface. The light and dark elements of these codes are determined from the physically altered surface conditions (for example, etched and not etched).

When you calibrate for the ISO/IEC 29158 standard, you adjust the environment and establish exact values for the following [`McodeControl`](../../Reference/code/McodeControl.md) grading settings:

- [`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeControl.md). This is the expected reflectance value (Rcal). This corresponds to the maximum possible intensity of the centers of the white elements.
- [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeControl.md). This is the expected mean light value (Mcal). This corresponds to the expected mean intensity of the centers of the white elements of the code occurrence.

Ideally, all factors affecting the intensity of grabbed images (that is, lights, exposure and gain settings, and surface characteristics) during calibration and production are identical. This is strongly advised. Otherwise, you can establish and set [`M_SYSTEM_RESPONSE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_SYSTEM_RESPONSE_TARGET`](../../Reference/code/McodeControl.md); their ratio tells the code grade algorithm how much brighter or darker a perfect marked code would appear within the target setup, when compared to the calibration setup. If the difference in these settings can be expressed as a constant that is proportional to the change in the intensity (for example, exposure), then providing the calibration and target values of the constant might provide more accurate grades.

The ISO/IEC 29158 standard is implemented in two phases: the reflectance calibration phase and the target grading phase. Each phase establishes the settings of the environmental factors that result in an acceptable image of a perfect reference code (printed) and target code (direct part marked), respectively. The second phase is only necessary if you need to set [`M_SYSTEM_RESPONSE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_SYSTEM_RESPONSE_TARGET`](../../Reference/code/McodeControl.md); otherwise these control types can be left to their default value of 1. The values of each phase are used to produce the grades associated with the target code.

Note that if rigorous calibration is not possible (for example, because you don't have a standard test card or you have workflow constraints), you can approximate the grading setting. See [](S09_Approximating_calibration_settings.md).

1. To perform the reflectance calibration phase:
   1. Place an A-grade code, printed on a code conformance test card, in the field of view (for example, the GS1 Data Matrix calibrated conformance standard test card or the NIST-traceable EAN/UPC calibrated conformance test card); the code is used as the reference code. The reference code can be of any code type (even 1D code types).
   2. Adjust the lighting according to the ISO/IEC 29158 standard.
   3. Allocate a code context (using [`McodeAlloc`](../../Reference/code/McodeAlloc.md)) and add a code model that has the same type as the reference code. For this phase, set [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md).
   4. Set the aperture of the code model to the recommended aperture for the reference code. Typically, you set [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) to [`M_RELATIVE`](../../Reference/code/McodeControl.md) and [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) to [`M_AUTO`](../../Reference/code/McodeControl.md); see[](S04_Settings_for_a_code_grade_operation.md).
   5. Grab an image of the reference code, using [`MdigGrab`](../../Reference/dig/MdigGrab.md).
   6. Call [`McodeGrade`](../../Reference/code/McodeGrade.md).
   7. Calculate the mean light ratio: (([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeGetResult.md)) / [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md)).
      By default, [`MbufControl`](../../Reference/buf/MbufControl.md) with [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) is set to the maximum possible grayscale value of the buffer. If the image does not use the full range of possible grayscale values, set [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) to the expected maximum value, prior to calling [`McodeGrade`](../../Reference/code/McodeGrade.md).
   8. If the mean light ratio is not within 70% and 86%, adjust your environment factors (the location, camera, and/or lighting setup) and repeat steps e to g until the mean light ratio is within this range.
   9. Take note of [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeGetResult.md) and [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeGetResult.md).
2. To perform the target grading phase:
   1. Calculate the System Response value (SRcal) for the reflectance calibration environment. To do so, use a user-defined aggregate of the environment factors that you used to create the conditions for an acceptable reference code during the reflectance calibration phase. For example, you could set the System Response value to be (Exposure time * Gain factor).
   2. If the reference code (printed) and target code (DPM) are of different code types, allocate another code context and add a code model that has the same type as the target code.
   3. Set the grading standard to ISO/IEC 29158 using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) set to [`M_ISO_DPM_GRADING`](../../Reference/code/McodeControl.md).
   4. Load the reflectance and mean light values from the reflectance calibration phase into the code context of the target code. To do so with one call, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DPM_CALIBRATION_RESULTS`](../../Reference/code/McodeControl.md) and the result buffer from the reflectance calibration phase. Alternatively, retrieve the reflectance and mean light values separately from this result buffer, and set the values into the code context of the target code using [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeControl.md), respectively.
   5. Set the System Response value derived from the reflectance calibration phase (SRcal) using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SYSTEM_RESPONSE_CALIBRATION`](../../Reference/code/McodeControl.md).
   6. Acquire an image of the target (DPM) code.
      > **Note:** All environmental factors must be the same as those used in the reflectance calibration phase, except those factors used to calculate the System Response value (SRcal).
   7. Set the System Response value for the target grading environment (the current environment) using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SYSTEM_RESPONSE_TARGET`](../../Reference/code/McodeControl.md). Use the same formula as the one used to calculate it for the reflectance calibration. For example, if you set the System Response value for the reflectance calibration phase to (Exposure time * Gain factor), set [`M_SYSTEM_RESPONSE_TARGET`](../../Reference/code/McodeControl.md) to (Exposure time * Gain factor) for the current environment.
   8. Call [`McodeGrade`](../../Reference/code/McodeGrade.md).
   9. Calculate the mean light ratio: (([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_MEAN_LIGHT_TARGET`](../../Reference/code/McodeGetResult.md)) / [`McodeInquire`](../../Reference/code/McodeInquire.md) with [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeInquire.md)).
   10. If the mean light ratio is not within 70% and 86%, adjust your environment factors (the location, camera, and/or lighting setup) and repeat steps f to i until the mean light ratio is within this range.
3. Grade your target codes. If you skipped step 2, make sure to change the code model in your context using [`McodeModel`](../../Reference/code/McodeModel.md); also, using the reflectance and mean light values retrieved from the result buffer during the reflectance calibration phase, set [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeControl.md), respectively. It is important not to modify the setup for the remainder of the grading process.

Note that if the image of the target code has its mean light ratio within the above mentioned range under the same environmental conditions as the reference code, you can leave [`M_SYSTEM_RESPONSE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_SYSTEM_RESPONSE_TARGET`](../../Reference/code/McodeControl.md) to their default values of 1.

For more information, especially regarding the guidelines for lighting environments and reflectance calibration configurations, refer to the ISO/IEC 29158 specification.

For an example, see [](S10_Code_grading_examples.md).
