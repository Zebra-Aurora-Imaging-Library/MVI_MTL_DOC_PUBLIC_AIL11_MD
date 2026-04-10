---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Approximating_calibration_settings
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Approximating calibration settings"
---

# Approximating calibration settings (if necessary)

Ideally, the setup and the configuration for grading should be done following the calibration procedures described in the related sections earlier in this chapter, using a standard test card. If you are unable to configure your setup according to the calibration specifications mentioned in the related section earlier in this chapter, you can use approximate values. These should give you good results, but will not be as accurate as a full calibration.

1. Adjust the image setup to:
   - Maximize the contrast and minimize noise in the image.
   - Avoid pixel saturation.
   - Have a cell size of at least 3. A smaller cell size results in a less robust result.
   - Have a uniform foreground and background grayscale level (that is, there should be no glare or dark spots present in the image).
2. Set [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) and [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) to the expected minimum and maximum possible grayscale values in the target image. For example, if the minimum and maximum possible values are 10 and 230, respectively, given your lighting conditions, specify these values.
