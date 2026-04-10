---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Grading_using_the_semi_t10_standard
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Grading using the semi t10 standard"
---

# Grading using the SEMI T10 standard

To grade a Data Matrix code direct part marked (DPM) on a semiconductor part, you can use the SEMI T10 grading standard. For information on grading DPM codes under other circumstances, see [](S07_Grading_using_the_ISO_IEC_29158_standard.md).

Before you can grade your target codes, you need to configure (calibrate) your setup. The SEMI T10 standard only provides general guidelines. The following procedure is designed to assist in modifying your setup until ideal conditions are obtained.

1. Set the grading standard using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) set to [`M_SEMI_T10_GRADING`](../../Reference/code/McodeControl.md).
2. Set [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md), [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md), and [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) to [`M_DEFAULT`](../../Reference/code/McodeControl.md).
3. In the environment in which you will eventually be grading your codes, grab an image of a DataMatrix code perfectly marked on a sample part, using [`MdigGrab`](../../Reference/dig/MdigGrab.md).
4. Call [`McodeGrade`](../../Reference/code/McodeGrade.md). Then, retrieve measures, such as the horizontal/vertical mark growth and misplacement, and the symbol contrast, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_..._MARK_GROWTH`](../../Reference/code/McodeGetResult.md), [`M_..._MARK_MISPLACEMENT`](../../Reference/code/McodeGetResult.md), and [`M_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md).
5. If the measures are not ideal (for example, [`M_..._MARK_GROWTH`](../../Reference/code/McodeGetResult.md) is far from 50%, [`M_..._MARK_MISPLACEMENT`](../../Reference/code/McodeGetResult.md) is far from 0%, or [`M_SYMBOL_CONTRAST`](../../Reference/code/McodeGetResult.md) is far from 80%), then as necessary, adjust the image setup to:
   - Maximize the contrast and minimize noise in the image. To do so, you can adjust your camera's settings (for example, the optical focus, image exposure, gain, and white balance).
   - Avoid pixel saturation.
   - Have a cell size of at least 3. A smaller cell size results in a less robust result. To do so, you can adjust the distance between the camera and the code.
   - Have a uniform foreground and background grayscale level (that is, there should be no glare or dark spots present in the image). To do so, adjust the illumination in your setup.
6. Repeat steps 3 through 5 until you retrieve close to perfect measures.
7. Grade your target codes using [`McodeGrade`](../../Reference/code/McodeGrade.md). It is important not to modify the setup for the remainder of the grading process.
