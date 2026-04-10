---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Color_spaces_and_converting_between_them
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Color spaces and converting between them"
---

# Color spaces and converting between them

A color space is a mathematical model that typically has three bands with which to describe color data, such as RGB (red, green, blue). Note that we use the term color bands, but they are also known as color components. Regardless of whether you are using the Aurora Imaging Library Color Analysis module to perform color transformation using relative color calibration ([`McolTransform`](../../Reference/col/McolTransform.md)), matching ([`McolMatch`](../../Reference/col/McolMatch.md)), projection ([`McolProject`](../../Reference/col/McolProject.md)), or distance ([`McolDistance`](../../Reference/col/McolDistance.md)), color data is always interpreted according to a specific color space.

## Color spaces

The major color spaces used today are RGB, HSL, HSV, and CIELAB, each of which has unique qualities with which to represent color.

*[Image: Color_Spaces.png]*

The color space that you use throughout an application must be consistent otherwise results can be skewed. For example, when calculating the distance between two color images or when performing a color match, all your color data should be in the same color space. For [`McolTransform`](../../Reference/col/McolTransform.md) and [`McolMatch`](../../Reference/col/McolMatch.md), you must also consider the context's source color space. For more information, see [Source color space](S03_Color_spaces_and_converting_between_them.md).

### RGB

RGB is based on red, green, and blue color band values. Typically, these bands are directly used for acquiring and displaying color. For example, when displaying a color image buffer, the first band is routed to the monitor's first output channel (usually red), the second band to the second channel (usually green), and the third band to the third channel (usually blue).

The RGB color space can be seen as a cube with a red, green, and blue axis. Colors located at the origin, [0, 0, 0], are considered to be black, while colors located at [255, 255, 255] (for an 8-bit buffer) are considered to be white. All other colors can be represented as a combination of red, green, and blue values within this range.

Acquisition and display devices can render RGB data differently. Since RGB maps to such devices, it is a device-dependent color space.

*[Image: Color_MonitorRGBDifference.png]*

Theoretically, there are as many RGB color spaces as there are color devices. Although there will always be some variance, color devices typically adhere to certain internationally accepted standards. To interpret color space data, Aurora Imaging Library uses standard RGB specifications (sRGB), as defined by the International Electrotechnical Commission (IEC) Project Team 61966-2-1.

### HSL

HSL is established from an RGB color model, but is based on hue, saturation, and luminance color band values. Since such bands are generally more intuitive, HSL can be seen as a color space designed to mimic the human way of describing colors. Like RGB, HSL is device-dependent.

In RGB, every color is a mixture of red, green, and blue, which can make it difficult to ascertain the exact band values of a particular color. However, in HSL, the color's hue is stored as a separate band (H), which is represented as an angular position on a circular color disk. The other bands control only the color's purity (S) and lightness (or luminance, L), which can be used to alter the color's quality, but not the color's basic hue. In the diagram below, the lightness (or luminance) band extends from one point of a bicone to the other, from black (0% lightness) to white (100% lightness). The colors at the center disk are situated at 50% lightness.

*[Image: Color_hsl.png]*

With the HSL model, band independence makes color manipulation much simpler. Most applications that allow for an interactive manipulation of colors represent the color with HSL. You can, for example, perform color matching with the hue (color) band only, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_BAND_MODE`](../../Reference/col/McolControl.md) set to [`M_COLOR_BAND_0`](../../Reference/col/McolControl.md). This can solve certain problems, such as matching dark orange and bright orange, which can be difficult in RGB. Also, matching the hue independently of the luminance can be useful if your image has non-uniform lighting, shadows, or highlights.

### HSV

HSV resembles HSL, except the third band is called value, and represents brightness as if the white point of the HSL model has been compressed to the central disk. In the diagram below, the model is shown as a simple cone, with hue (H) positioned around a circular disk, and saturation (S) increasing from the center out. The value (V) band ranges from black (the far point of the cone below the disk) to white (the center of the disk).

*[Image: Color_hsv.png]*

In this model, 100% brightness (or value) corresponds to maximum color richness, offering an intuitive advantage over HSL, which represents the richest colors at only 50% lightness.

The HSV color space reflects how paint colors change when creating tints (adding white) or shades (adding black), which is intuitively easy to understand. As with HSL, the separate bands allow for interactive manipulation.

### CIELAB

CIELAB (or LAB) is based on the color's luminance (L), its position between red and green (A), and its position between yellow and blue (B). Unlike RGB and HSL, LAB is intended to be device-independent, and was developed as a distinct color model intended to represent a completely human interpretation of color using statistical data taken from visual experiments. Color differences in LAB vary proportionally with human perception. For example, if two colors are at a distance of 5, they will appear roughly 5 times as different as two colors at a distance of 1. LAB was designed to be perceptually uniform, making it a good space to measure color difference.

Since LAB is based on color perception, its mathematical model better represents how humans distinguish color, and color differences are more meaningful. This can be seen in the following example, where you have to choose which color, A or B, is closer to color X.

*[Image: Color_RGBCIELABCompare.png]*

Mathematically, color A is the closest color, in an RGB color space. However, color B is intuitively closer, which is also what the distance in the LAB color space mathematically represents.

*[Image: Color_RGBCIELABCompare2.png]*

It can be preferable to use CIELAB over RGB since, like HSL, you can use CIELAB to discard the luminance (band L), and perform color matching with only band A and band B. Also, CIELAB can be more robust with colors that are visually alike, especially for minor color differences. For example, when matching a red target among color-samples with similar shades of red, CIELAB can outperform RGB and HSL.

With CIELAB, distances have been standardized by the International Commission on Illumination (CIE). A color distance of 1 with CIELAB corresponds to the smallest possible color difference a human can perceive.

## Source color space

When allocating a color matching or a relative color calibration context using [`McolAlloc`](../../Reference/col/McolAlloc.md), you must set the context's source color space to one of the following color spaces: [`M_RGB`](../../Reference/col/McolAlloc.md), [`M_HSL`](../../Reference/col/McolAlloc.md), or [`M_CIELAB`](../../Reference/col/McolAlloc.md). Relative color calibration contexts must be [`M_RGB`](../../Reference/col/McolAlloc.md).

> **Note:** Note that you should not confuse "color space", which is the mathematical model used to describe color, and "source color space," which is the specific color space in which the color matching context is defined. Advanced users should also be aware of the "reference color space," which refers to how Aurora Imaging Library interprets the color data of the source color space when performing an internal conversion before the color operation. For more information, see [Converting between color spaces](S03_Color_spaces_and_converting_between_them.md).

Aurora Imaging Library considers all color data required for color matching or relative color calibration to be in the source color space specified for the context. For example, if you set an [`M_HSL`](../../Reference/col/McolAlloc.md) source color space, Aurora Imaging Library assumes that the target image ([`McolMatch`](../../Reference/col/McolMatch.md)), as well as the color-sample elements (whether they be defined by an image or by triplet values using [`McolDefine`](../../Reference/col/McolDefine.md)), are also HSL. If they are not, Aurora Imaging Library still interprets and matches the color data as if it was HSL and does not produce an error. Therefore, unless all the color data is consistent with the source color space, you can receive misleading results.

Since color data is interpreted according to the context's source color space, be careful when restoring color images into an automatically allocated data buffer using [`MbufRestore`](../../Reference/buf/MbufRestore.md), when the color's type is not explicitly known. For example, if you are working in an [`M_RGB`](../../Reference/col/McolAlloc.md) color space, and you use an image restored from a file that contained YUV data, the Y, U, and V bands will be incorrectly read as R, G, and B bands, respectively. Instead, you should use [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) to allocate a color data buffer with an explicit data type (for example, [`M_RGB24`](../../Reference/buf/MbufAllocColor.md)), and then use [`MbufLoad`](../../Reference/buf/MbufLoad.md) to load the data from the color image into the specified color data buffer. In this case, [`MbufLoad`](../../Reference/buf/MbufLoad.md) will convert the image's color data to the proper type, as defined by the buffer (for example, [`M_RGB24`](../../Reference/buf/MbufAllocColor.md)).

When matching, you must set the encoding of the source color space according to the actual dynamic range of your color data, using [`McolControl`](../../Reference/col/McolControl.md) with [`M_ENCODING`](../../Reference/col/McolControl.md). By default, an 8-bit source color space encoding is assumed, regardless of the depth of your buffers. Since Aurora Imaging Library processes color data as is (even if it is inconsistent), you must remember to modify the encoding control according to your data, otherwise you can get incorrect results. For example, you will not get an error if you match a 16-bit target area with an 8-bit color-sample. Therefore if your color data was acquired with a 16-bit camera, you should set [`M_ENCODING`](../../Reference/col/McolControl.md) to [`M_16BIT`](../../Reference/col/McolControl.md). For more information, see [Color space encoding](S08_Advanced_color_matching_settings.md).

## Converting between color spaces

You can convert color data between HSL and RGB color spaces using [`MimConvert`](../../Reference/im/MimConvert.md) with [`M_HSL_TO_RGB`](../../Reference/im/MimConvert.md) or [`M_RGB_TO_HSL`](../../Reference/im/MimConvert.md). For efficiency when converting from a 3-band (RGB) buffer, you can calculate just the hue band of the HSL color space and save it in a 1-band buffer ([`M_RGB_TO_H`](../../Reference/im/MimConvert.md)). For more information on the algorithm used to convert between HSL and RGB color spaces, see [Extracting the luminance](S05_Converting_to_grayscale.md).

When using an RGB source color space ([`McolAlloc`](../../Reference/col/McolAlloc.md)), you can use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the [`ConversionModeOrComputeOption`](../../Reference/col/McolSetMethod.md) parameter to convert your RGB data to CIELAB or HSL before performing [`McolMatch`](../../Reference/col/McolMatch.md). This type of conversion can be useful if, for example, you have allocated a color matching context in an RGB color space, but you want to calculate with CIELAB or HSL colors. Note that when performing multiple match operations on RGB data in CIELAB or HSL, you might want to consider providing images that have already been converted to CIELAB or HSL, which might be faster.

When converting with [`McolSetMethod`](../../Reference/col/McolSetMethod.md), the converted color is essentially the same as the source color, except that it is represented in a different color space, and therefore has different (converted) color band values. For example, pure red has an RGB value of (255, 0, 0), while in HSL the same red has a value of (0, 255, 128). As shown in the following illustration, if you are using an RGB source color space, all color data (such as the color-sample elements) is assumed to be in RGB. However, if you use [`McolSetMethod`](../../Reference/col/McolSetMethod.md) to convert from RGB to HSL, the match operation is performed with the HSL version of the color. Note that in the following example, the target image is already in HSL.

*[Image: color_conversion_mode_rgb.png]*

You can also use [`MimConvert`](../../Reference/im/MimConvert.md) to convert sRGB to LAB ([`M_SRGB_TO_LAB`](../../Reference/im/MimConvert.md)), and to convert LAB to sRGB ([`M_LAB_TO_SRGB`](../../Reference/im/MimConvert.md)). In these cases, your RGB data must adhere to the IEC standards for sRGB (previously discussed); otherwise, unexpected results can occur. To obtain sRGB color, you can grab images using an sRGB camera and illuminant in a favorable environment. You can also perform a relative color calibration on an RGB image to transform it to sRGB, using [`McolTransform`](../../Reference/col/McolTransform.md) and an sRGB ColorChecker. For more information, see [Relative color calibration](S04_Relative_color_calibration.md).

Instead of LAB, you can use [`MimConvert`](../../Reference/im/MimConvert.md) to convert sRGB to and from LCH ([`M_SRGB_TO_LCH`](../../Reference/im/MimConvert.md) or [`M_LCH_TO_SRGB`](../../Reference/im/MimConvert.md)). The LCH color space is a reinterpretation of LAB that emphasizes polar coordinates (absolute black and absolute white), which can make LAB colors easier to deal with (similar to HSL being easier to deal with than RGB). LCH is based on luminance (L), chroma (C), and hue (H).

If you are converting to and from linear sRGB data (not gamma corrected), you must use the linear version of the sRGB conversion values ([`M_SRGB_LINEAR_TO_LAB`](../../Reference/im/MimConvert.md), [`M_SRGB_LINEAR_TO_LCH`](../../Reference/im/MimConvert.md), [`M_LAB_TO_SRGB_LINEAR`](../../Reference/im/MimConvert.md), or [`M_LCH_TO_SRGB_LINEAR`](../../Reference/im/MimConvert.md)). For more information, see [Gamma correction](S03_Color_spaces_and_converting_between_them.md).

> **Note:** [`MimConvert`](../../Reference/im/MimConvert.md) is available with Aurora Imaging Library Lite, while [`McolSetMethod`](../../Reference/col/McolSetMethod.md) requires the full version of Aurora Imaging Library.

### Normalizing the RGB color space

Normalizing a color space can help remove unwanted variations in shades from an image. For example, using blob analysis on images with shadows or sharp variations of the same color might find false borders, and return inaccurate results. You can fix the unwanted variations in color shades by normalizing the colorspace.

Using [`MimConvert`](../../Reference/im/MimConvert.md) with [`M_RGB_NORMALIZE`](../../Reference/im/MimConvert.md) on an RGB image will normalize the color of each pixel to smooth variations in shade. The bands of each individual pixel are divided by the sum of that pixel's band values. This division results in a number between 0 and 1 (inclusive) for every band, and has the effect of reducing variations over similar colors. If the destination buffer is an integer buffer, the normalized bands are then multiplied by the depth of the buffer; this scales the values, but retains the relative difference between the other colors.

After converting the RGB color data to normalized RGB data, you can convert it to any other color space with [`MimConvert`](../../Reference/im/MimConvert.md), and keep the normalized separations between colors in your image.

### Reference color space

To interpret the RGB source color space data when using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, the color matching context uses standard RGB specifications (sRGB) as its reference color space, which you can specify using [`McolAlloc`](../../Reference/col/McolAlloc.md) with the [`ColorSpaceProfileId`](../../Reference/col/McolAlloc.md) parameter. sRGB specifications are defined in the International Electrotechnical Commission (IEC) 61966-2-1. Note that when converting RGB using the HSL conversion mode, the reference color space is not required since HSL is based on the RGB color model.

### Gamma correction

Depending on the display device, the colors of grabbed images can appear over-saturated or too dark. This is typically caused by a non-linear relationship between a pixel's value and its displayed intensity. To compensate for this discrepancy, the acquisition process (for example, the camera doing the grab) can apply a gamma correction, thereby leaving your color data in a corrected (non-linear) state.

When converting RGB source colors before the match using [`McolSetMethod`](../../Reference/col/McolSetMethod.md) with the CIELAB conversion mode, Aurora Imaging Library requires uncorrected (linear) color data. Therefore, if gamma correction has been applied on the RGB source color data, you must remove it using [`McolControl`](../../Reference/col/McolControl.md) with [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) set to [`M_ENABLE`](../../Reference/col/McolControl.md).

By default, Aurora Imaging Library assumes the RGB source color data is in a corrected (non-linear) state, and removes the correction. Aurora Imaging Library also assumes that the data has been corrected according to the standards of the context's reference color space, as specified using [`McolAlloc`](../../Reference/col/McolAlloc.md) with the [`ColorSpaceProfileId`](../../Reference/col/McolAlloc.md) parameter. Aurora Imaging Library uses these standards (for example, sRGB) to remove the gamma correction. If your RGB source color data is in an uncorrected (linear) state, you must set [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) to [`M_DISABLE`](../../Reference/col/McolControl.md).

Regardless of whether your RGB source color data is in a corrected or uncorrected state, Aurora Imaging Library removes or keeps the correction according to [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) and, in the case of removing the correction, applies the standards of the reference color space (sRGB). Therefore, you will not receive an error if, for example, your color data is not corrected (linear) and you set [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) to [`M_ENABLE`](../../Reference/col/McolControl.md) (to remove the correction). Such a scenario would, in all likelihood, render inaccurate results.

Unless you are using the CIELAB conversion mode in an RGB source color space, [`M_CONVERSION_GAMMA`](../../Reference/col/McolControl.md) is ignored.
