---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Registration
section: High_dynamic_range
module_tag: reg
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / registration / High dynamic range"
---

# High dynamic range registration

The Aurora Imaging Library Registration module offers high dynamic range (HDR) registration to improve detail in images.

HDR registration merges two or more grayscale images of the same subject that were taken with different exposure times, resulting in different levels of saturation. Longer exposure times produce brighter images in which details in dark areas appear, but brighter areas become saturated and lose detail. Similarly, shorter exposure times produce darker images in which details in bright areas appear, but darker areas lose detail.

When the images are fused together, the resulting HDR image will show details in both dark areas and bright areas.

*[Image: HDR_Example.png]*

## Steps to performing a high dynamic range registration operation

The following steps provide a basic methodology for using the Registration module to perform an HDR registration operation.

1. Allocate a high dynamic range registration context, using [`MregAlloc`](../../Reference/reg/MregAlloc.md) with [`M_HIGH_DYNAMIC_RANGE`](../../Reference/reg/MregAlloc.md).
2. Optionally, allocate a registration result buffer to hold the results of the registration operation, using [`MregAllocResult`](../../Reference/reg/MregAllocResult.md) with [`M_HIGH_DYNAMIC_RANGE_RESULT`](../../Reference/reg/MregAllocResult.md). If you are only interested in obtaining an HDR image from one set of input images contained in a single input image array, you do not need a result buffer.
3. If necessary, allocate an image buffer to hold the resulting HDR image (alternatively, the image can be stored in the result buffer). The image buffer should be 1-band and of the same size and type as the input images; the recommended bit depth is twice that of the input images.
4. Set up an array with the identifiers of image buffers that hold the input images. All the images should be of the same scene, arranged from longest exposure (first index) to shortest exposure (last index), in 1-band buffers of the same size and type.
5. Verify that the number of registration elements is sufficient for your application (by default, it is 16). A registration element contains the information required to register an image; therefore, the context must contain at least the same number of registration elements as images that need to be registered (per call to [`MregCalculate`](../../Reference/reg/MregCalculate.md)). You can inquire the number using [`MregInquire`](../../Reference/reg/MregInquire.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregInquire.md). To change the number of registration elements, use [`MregControl`](../../Reference/reg/MregControl.md) with [`M_NUMBER_OF_REGISTRATION_ELEMENTS`](../../Reference/reg/MregControl.md). The minimum is 2.
6. Specify the control settings for your HDR registration operation using [`MregControl`](../../Reference/reg/MregControl.md) (for example, the[fusion](S06_High_dynamic_range.md) settings control the criteria by which images and pixels are included in the final HDR image calculation).
7. Perform the registration using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with the HDR registration context, the array of input images, a destination image or result buffer, the number of images in the image array, and the processing mode ([`M_COMPUTE`](../../Reference/reg/MregCalculate.md), [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md), or [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md)).
8. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), retrieve the required results from the result buffer, using [`MregGetResult`](../../Reference/reg/MregGetResult.md).
9. If you passed a result buffer to [`MregCalculate`](../../Reference/reg/MregCalculate.md), you can draw the HDR image from the result buffer into an image buffer using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_HDR_IMAGE`](../../Reference/reg/MregDraw.md). You can use [`MregGetResult`](../../Reference/reg/MregGetResult.md) to establish the size and type of the image buffer required to draw the HDR image.
10. If necessary, save your registration context using [`MregSave`](../../Reference/reg/MregSave.md) or [`MregStream`](../../Reference/reg/MregStream.md).
11. You can clear the result buffer to perform additional operations, using [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_RESET`](../../Reference/reg/MregCalculate.md).
12. Free all your allocated objects, using [`MregFree`](../../Reference/reg/MregFree.md), unless [`M_UNIQUE_ID`](../../Reference/reg/MregAlloc.md) was specified during allocation.

## Input images

Input images must be of the same scene, type, and resolution. Additionally, they must be 1-band 8- or 16-bit unsigned images.

The minimum number of input images is 2. The order of input images must go from the image with the longest exposure (the brightest image with details in dark areas) to the image with the shortest exposure (the darkest image with details in bright areas).

If you want to calculate the HDR image in one call to[`MregCalculate`](../../Reference/reg/MregCalculate.md) using provided input images, set [`M_COMPUTE`](../../Reference/reg/MregCalculate.md) as the processing mode. In this case, you can have [`MregCalculate`](../../Reference/reg/MregCalculate.md) write the resulting HDR image directly in the specified image buffer. However, if it would be too time consuming to wait for all required input images, you can call [`MregCalculate`](../../Reference/reg/MregCalculate.md) multiple times with [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md) and the available set of input images. Then, for the last set of images to merge into the same working HDR image, call [`MregCalculate`](../../Reference/reg/MregCalculate.md) with [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md). The last call will compute the resulting HDR image from all previously accumulated images. For both [`M_ACCUMULATE`](../../Reference/reg/MregCalculate.md) and [`M_ACCUMULATE_AND_COMPUTE`](../../Reference/reg/MregCalculate.md), you must use a result buffer instead of an image buffer.

If you are using multiple calls, the Nth input image used to generate the current HDR image will use the settings of the registration element at index N-1. For example, if you are merging your eighth image, the settings of the registration element at index 7 will be used.

Note that if the difference between the exposures of two consecutive input images is too large, subsequent input images will fail until the end of the series is reached. This is because the common merge area between the second image and the working HDR image will be too small, and since all subsequent images will have increasingly shorter exposures, the difference of exposure will only become greater, preventing a sufficiently large merge area from ever being established. In such cases, the resulting HDR image will consist of the fusion of only the images preceding the exposure gap.

## Customizing your HDR operation

You can customize your high dynamic range registration operation using [`MregControl`](../../Reference/reg/MregControl.md).

The animation below illustrates the HDR registration process. The first input image becomes the working HDR image; no control settings are applied to this first image. Subsequent images, including the working HDR image, are evaluated using the limits set with [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). For each input image, [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) is used to determine whether the input image has enough pixels in the common merge area to be included in the HDR calculation. If the image meets this requirement, the image merges with the working HDR image and this new image becomes the working HDR image. The process will then repeat until the last image is reached, after which point tone mapping is applied.

*[Image: HDR_Intermediate_Examples]*

### Fusion

To set the criteria that determines which pixels will be included in the HDR image based on their saturation levels, use [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). These thresholds are set as a percentage of the total number of pixels in the working HDR image; Aurora Imaging Library will internally convert them to a pixel value threshold. In the case of [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md), which eliminates underexposed pixels in the input images, this pixel threshold is the highest pixel value that excludes at most the percentage of pixels set by[`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md). All pixels with values below this threshold will be excluded. Conversely, in the case of [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md), which eliminates overexposed (saturated) pixels, this pixel threshold is the lowest pixel value that includes at least the percentage of pixels set by [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). All pixels with values above this threshold will be excluded. For example, if [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) is set to 95%, then Aurora Imaging Library will internally select the lowest pixel value that allows for at least 95% of pixels in the image to be included (that is, at most 5% of the highest pixels in the image to be excluded).

[`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) are not applied to the first input image; it becomes the working HDR image. The thresholds will only be applied to subsequent input images and working HDR images. Once the underexposed and overexposed pixels have been excluded from both the working HDR image and the input image, the pixels that remain and are common to both images will form the common merge area. Only pixels in the common merge area will be merged with the working HDR image.

The input image will be included in the calculation of the HDR image if there are enough pixels in the common merge area established by [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md). This requirement is met if the ratio between the pixels in the common merge area and the total number of pixels is at least the value of [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md). You can adjust this requirement to be more restrictive by setting a higher value for [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md). Note that this could result in less input images being included in the HDR calculation, since those which do not contribute enough will be excluded.

### Gain

Once the common merge area has been established and the input image is almost ready to be fused with the working HDR image, gain is applied to the input image. By default, the gain for each input image is calculated automatically. To retrieve the automatically calculated gain used for a specific input image when [`M_GAIN_MODE`](../../Reference/reg/MregControl.md) is set to [`M_AUTO`](../../Reference/reg/MregControl.md), use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_IMAGE_GAIN`](../../Reference/reg/MregControl.md).

To manually set the gain, use [`MregControl`](../../Reference/reg/MregControl.md)with [`M_GAIN_MODE`](../../Reference/reg/MregControl.md) set to [`M_USER_DEFINED`](../../Reference/reg/MregControl.md). Then, you can set the gain for a specific input image by setting its corresponding registration element in the registration context, using [`MregControl`](../../Reference/reg/MregControl.md) with [`M_IMAGE_GAIN`](../../Reference/reg/MregControl.md).

## Tone mapping

The resulting raw HDR image has twice the bit depth of the input images. By default, Aurora Imaging Library applies tone mapping to the resulting raw HDR image to reduce its bit depth to that of the original input images. You can disable tone mapping using [`M_TONE_MAPPING_MODE`](../../Reference/reg/MregControl.md), or you can draw the resulting HDR image without tone mapping using [`MregDraw`](../../Reference/reg/MregDraw.md) with [`M_DRAW_HDR_FULL_IMAGE`](../../Reference/reg/MregDraw.md).

Tone mapping distributes pixel intensities non-linearly within a specified range, typically to maintain the tonal details of the resulting raw HDR image when it is being mapped to a lower bit depth and/or for display on a monitor. To modify how tone mapping is applied, use[`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md), [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md), and [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md).

To prioritize maintaining details in dark or bright areas, adjust the value of [`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md). Increasing the value towards 1 will give bright areas a broader intensity range, which improves the detail in bright areas and lowers the overall brightness of the final image. Conversely, decreasing the value towards 0 will give dark areas a broader intensity range, which improves the detail in dark areas and increases the overall brightness of the final image.

To improve the intensity distribution of the image, adjust [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md), which will filter out the most saturated and the least saturated pixels, respectively. This allows more intensity distribution elsewhere in the image. The thresholds are set as a percentage of the resulting raw HDR image's pixel intensity levels. For example, if [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) is set to 90%, pixels in the brightest 10% of intensities will be excluded.

Note that [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) should only be used to remove unneeded pixel intensity levels and provide a better intensity resolution to the final HDR image.

## Example of HDR

The _SimpleHDR.cpp_ example demonstrates HDR registration in Aurora Imaging Library. To run this and other examples, use Aurora Imaging Example Launcher.

## Troubleshooting

The following subsections describe some common issues encountered when performing an HDR registration operation and how to solve them. Use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_HDR_STATUS`](../../Reference/reg/MregGetResult.md) to get the status of the entire HDR registration operation and use [`MregGetResult`](../../Reference/reg/MregGetResult.md) with [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) to get the status of the merge of a specific image. Note that [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) reports only one issue (for example, [`M_HDR_INSUFFICIENT_CONTRAST`](../../Reference/reg/MregGetResult.md)) per image, even if the criteria for multiple are met.

### M_HDR_STATUS value below 1.0

The value of [`M_HDR_STATUS`](../../Reference/reg/MregGetResult.md) represents the ratio between the number of input images that were successfully processed and the total number of input images. When this value is below 1.0, it means that some images were not successfully processed. This can happen for multiple reasons:

- The input images were not in the correct order. The input images must be added in the order of darkest details (brightest image) to brightest details (darkest image). Otherwise, the resulting HDR image will only consist of the first image. There are several ways to solve this:
  - Use decreasing exposure times, with the first image having the longest exposure time and the last image having the shortest exposure time. This approach is limited by the time available to take the necessary images.
  - Use a different aperture for each image, starting from the largest aperture (smallest number) and ending with the smallest (largest number). Note that this approach will cause variations in the focal length and might cause the resulting HDR image to be blurry.
  - Use a different ISO setting for each image, starting from the most sensitive (largest number) and ending with the least sensitive (smallest number). Note that images taken with high ISO settings might have more noise, which will result in an HDR image with more noise.
- The level of visible detail is not consistent from one input image to the next. For example, if you have a series of images of a checkerboard in black and white, the images will show a strong level of detail for blacks and whites, but will have little detail in the shades of gray in between. This can cause problems when merging the middle images. To solve this, add an object with varying tones of gray in the scene to be photographed. This will provide a reference and allow every input image to meet the minimum level of detail to contribute.
- The difference between the exposure of each image is too small, resulting in many similar-looking images. To solve this, increase the difference of exposure between each image. Note that in this case, a less than perfect [`M_HDR_STATUS`](../../Reference/reg/MregGetResult.md) value can sometimes be acceptable, as details will not be missing. It becomes a problem when multiple adjacent images have been rejected.
- The difference between the exposure of two consecutive input images is too large, resulting in a common merge area that is too small between the second image and the working HDR image. This causes the subsequent input images to fail until the end of the series is reached. Since the order of images is from darkest details to brightest details, the resulting HDR image will have fewer details in the brightest areas. To solve this, decrease the difference of exposure between consecutive images.
- [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) are too extreme and are causing a loss of detail. These values are meant to eliminate only the most saturated pixels (using[`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md)) and underexposed pixels (using[`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md)). To solve this, decrease the value of [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) or increase the value of [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md).
- [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) is too high. This means that the inclusion criteria is too restrictive and not enough images are being included in the resulting HDR image. To solve this, set [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md) to the lowest value that yields acceptable improvement in the detail level of the HDR image.
- The first input image is too saturated. When this happens, a common merge area between the first and second image cannot be established, and therefore the HDR fusion operation cannot proceed. To solve this, decrease the exposure time of the first image. Alternatively, if you have more than two input images, remove the first image.

### Image status of M_HDR_IMAGE_LIMITED_TO_SINGLE_COLOR

If the value of [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_IMAGE_LIMITED_TO_SINGLE_COLOR`](../../Reference/reg/MregGetResult.md), it means that the grayscale tones in the current HDR image have been reduced to a single tone within the merge area.

There are two ways to solve this:

- Reduce the difference between the exposures of each input image to increase the size of the merge area.
- Try less extreme values for [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md).

### Image status of M_HDR_INSUFFICIENT_CONTRAST

If the value of [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_INSUFFICIENT_CONTRAST`](../../Reference/reg/MregGetResult.md), it means that the number of tones available in the merge area is insufficient.

There are two ways to solve this:

- Improve the dynamic range of each input image by making sure that it uses the full range of tones available with the image type used.
- Try less extreme values for [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md).

### Image status of M_HDR_INSUFFICIENT_CONTRIBUTION

If the value of [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_INSUFFICIENT_CONTRIBUTION`](../../Reference/reg/MregGetResult.md), it means that the added image does not increase the dynamic range already present in the current working HDR image.

There are multiple ways to solve this:

- Verify the order of the input images to make sure that they are in the proper sequence (from the longest exposure time to the shortest).
- Increase the difference between the exposures of each input image to better differentiate them.
- When the image has few tones that have a high contrast between them, add an object with varying tones of gray in the scene to be photographed. This will provide a reference.

### Image status of M_HDR_MERGE_AREA_GAIN_INCORRECT

If the value of [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_MERGE_AREA_GAIN_INCORRECT`](../../Reference/reg/MregGetResult.md), it means that the gain to be applied to the input image is less than 1.

To solve this, verify the order of the input images to make sure that they are in the proper sequence (from the longest exposure time to the shortest).

### Image status of M_HDR_MERGE_AREA_SIZE_INSUFFICIENT

If the value of [`M_HDR_IMAGE_STATUS`](../../Reference/reg/MregGetResult.md) is [`M_HDR_MERGE_AREA_SIZE_INSUFFICIENT`](../../Reference/reg/MregGetResult.md), it means that the size of the merge area is below the threshold specified by [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md).

There are multiple ways to solve this:

- Reduce the difference between the exposures of each input image to increase the size of the merge area.
- Improve the dynamic range of each input image by making sure that it uses the full range of tones offered by the image.
- Try less stringent values for [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md), [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md), and [`M_FUSION_COVERAGE`](../../Reference/reg/MregControl.md).

### Blocks appearing in the resulting HDR image

Black or white blocks can sometimes be seen in the resulting HDR image. There are multiple ways to solve this.

- In the case of black blocks, the values of [`M_FUSION_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_LOW_THRESHOLD`](../../Reference/reg/MregControl.md) need to be validated. One of them could have a value that is too high, causing the elimination of key pixels in the end result.
- In the case of white blocks, the values of [`M_FUSION_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) and [`M_TONE_MAPPING_HIGH_THRESHOLD`](../../Reference/reg/MregControl.md) need to be validated. One of them could have a value that is too low, causing the elimination of key pixels in the end result.
- The value of [`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md) might need to be lower.
  If an object in the image uses a smaller range of shades of gray and gets mapped to a larger range of shades of gray, the tone transitions will look blocky due the increased step between each tone displayed. For example, an object with a range of 40 shades of gray out a possible 256 (8-bit) that gets mapped to a range of 160 shades of gray will look blocky. No post-processing is applied to minimize this effect. Additionally, other details in the image might see their intensity range reduced, resulting in a flat appearance.

### Darker areas appearing flat with few details

When the order of input images is incorrect, the darker areas of an image can have significantly fewer details than expected, as well as severe blocking and banding.

To solve this, adjust the order of the input images so that the first image has the darkest details (brightest image with the longest exposure) and the last image has the brightest details (darkest image with the shortest exposure).

### Resulting image is too dark or too bright

When the resulting image is too dark or too bright, modify the [`M_TONE_MAPPING_COEFFICIENT`](../../Reference/reg/MregControl.md), which controls the distribution of pixel intensities during the tone mapping of the raw HDR image. A higher value will lower the overall brightness by giving a broader intensity range to the brighter details. A lower value will increase the overall brightness by giving a broader intensity range to the darker details.
