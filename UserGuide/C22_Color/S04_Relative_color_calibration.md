---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Relative_color_calibration
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Relative color calibration"
---

# Relative color calibration

Various conditions, such as different cameras and illuminants, can cause colors to seem dissimilar.

*[Image: ColorRelativeCalibration_ColorComparison.png]*

With [`McolTransform`](../../Reference/col/McolTransform.md), you can perform a relative color calibration. This transforms an image's color data to better resemble the color data of a specified reference.

*[Image: ColorRelativeCalibration_ColorComparisonAndTransformationBasic.png]*

Transforming colors to be consistent can increase the accuracy of subsequent color operations, such as distance and matching. For example, you have a particular camera in an assembly line that interprets color images of food as you expect. You must then compare these colors, using a color distance operation, with images of food taken in other assembly lines with other cameras that interpret the same colors differently. Flagging problems due to color inconsistencies in the food across the multiple lines can prove difficult since colors are already inconsistent based on the cameras taking the images.

To manage this, you must establish one or more color-samples, each of which represents a particular camera's interpretation of color. You must also establish the reference color-sample. This is a unique type of color-sample that represents the color you expect. For example, the color data taken by camera 1. For each color-sample, Aurora Imaging Library calculates a mapping ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)) to transform its color data to resemble the color data of the reference color-sample (such as having a similar color temperature and distribution). The context holds the color-samples, the reference color-sample, and the operation settings (strategy) with which to establish the color mapping. A preprocessed context holds the actual color mapping.

*[Image: ColorRelativeCalibration_ColorComparisonAndTransformationBasicWithSamples.png]*

By specifying the color-sample that correlates to the camera capturing the image to transform ([`McolTransform`](../../Reference/col/McolTransform.md)), Aurora Imaging Library applies the associated mapping to correct the image's color to better resemble the reference color-sample. All colors from all cameras can now be coherent (relative to the reference color), and subsequent color operations can be more accurate.

*[Image: ColorRelativeCalibration_ColorComparisonAndTransformationBasicWithSamplesAndCamera.png]*

Aurora Imaging Library assumes all color data that a relative color calibration context uses is 8-bit RGB (3-band). If color data is inconsistent (for example, between the source image and the color-sample), Aurora Imaging Library still processes it all as 8-bit RGB.

## Steps to performing relative color calibration

The following steps provide a basic methodology for using relative color calibration in Aurora Imaging Library:

1. Allocate a relative color calibration context, using [`McolAlloc`](../../Reference/col/McolAlloc.md) with [`M_COLOR_CALIBRATION_RELATIVE`](../../Reference/col/McolAlloc.md). The context holds the color-samples with their associated mapping and the reference color-sample.
2. Specify the processing strategy, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md). The strategy sets how to calculate the color-sample mappings.
3. Define the color-samples using [`McolDefine`](../../Reference/col/McolDefine.md). You must define one or more color-samples. You must define one, and only one, reference color-sample.
4. Preprocess the context, using [`McolPreprocess`](../../Reference/col/McolPreprocess.md). Preprocessing calculates and associates a mapping with each color-sample in the context. There is no mapping for the reference color-sample.
5. Optionally, perform drawing operations, using [`McolDraw`](../../Reference/col/McolDraw.md).
6. Transform color data with relative color calibration, using [`McolTransform`](../../Reference/col/McolTransform.md).
   You do not specify a result buffer in which transformation results are written. Aurora Imaging Library produces the transformed image directly in the destination image buffer specified with [`McolTransform`](../../Reference/col/McolTransform.md). Typically, you use the transformed image in subsequent color based operations, such as color matching or distance.
7. Free all your allocated objects, using [`McolFree`](../../Reference/col/McolFree.md), unless [`M_UNIQUE_ID`](../../Reference/col/McolAlloc.md) was specified during allocation.

Context modifications are part of the preprocessing (or training) phase. For such changes to take effect, you must typically call [`McolPreprocess`](../../Reference/col/McolPreprocess.md) before [`McolTransform`](../../Reference/col/McolTransform.md). To inquire if this is necessary, use [`McolInquire`](../../Reference/col/McolInquire.md) with [`M_PREPROCESSED`](../../Reference/col/McolInquire.md). You can also use [`McolInquire`](../../Reference/col/McolInquire.md) to retrieve other information about the context in general, as well as its color-samples and the reference color-sample.

[`McolTransform`](../../Reference/col/McolTransform.md) corrects an image's color data according to the preestablished mapping of the specified color-sample. No consideration is made to other specifics of the application, such as the particular camera or illuminant that you are using.

## Color-samples and color elements

Color-samples are unified sets of color data that you define using [`McolDefine`](../../Reference/col/McolDefine.md). As discussed, operations such as [`McolPreprocess`](../../Reference/col/McolPreprocess.md) and [`McolTransform`](../../Reference/col/McolTransform.md) require color-samples. Aurora Imaging Library stores them with the context.

Color-samples are made up of one or more color elements. When you define a color-sample, you automatically add the first color element to the color-sample. You can define a color-sample (and consequently the first color-sample element) from:

1. A region of an image ([`M_IMAGE`](../../Reference/col/McolDefine.md)).
2. Three explicit color band values ([`M_TRIPLET`](../../Reference/col/McolDefine.md)).

By default, when defining image type color-samples for relative color calibration, Aurora Imaging Library uses the color data of each individual pixel in the color-sample to perform its calculations. For certain operational strategies, you can specify to use color statistics instead (such as the mean color of the color-sample). For more information, see [Selecting strategy](S04_Relative_color_calibration.md). Note that when using color-samples for color matching, Aurora Imaging Library always uses an estimation of the color (such as the mean).

To add color elements to color-samples that already exist in the context, specify [`M_ADD_COLOR_TO_SAMPLE`](../../Reference/col/McolDefine.md). Aurora Imaging Library now considers this the color of the color-sample. Subsequent operations use this color data as if it was originally part of the color-sample.

If the type of the existing color-sample is [`M_IMAGE`](../../Reference/col/McolDefine.md), the type of any additional color element must be [`M_IMAGE`](../../Reference/col/McolDefine.md). Also for [`M_IMAGE`](../../Reference/col/McolDefine.md), you must ensure that the source image from which to define the additional color-sample elements has the same format as the original source image used to define the initial color-sample element. If the color-sample is [`M_TRIPLET`](../../Reference/col/McolDefine.md), additional color elements must be [`M_TRIPLET`](../../Reference/col/McolDefine.md).

When adding a color-sample or color element with [`M_IMAGE`](../../Reference/col/McolDefine.md), you must specify a source image, and optionally specify a rectangular region within it. For good results, use the best source image possible. You can apply a mask to the color-sample or color element, using [`McolMask`](../../Reference/col/McolMask.md), to ignore unwanted data, or specify a non-rectangular region.

If the source image buffer has a region of interest (ROI), which can be set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md), Aurora Imaging Library ignores all pixels outside the ROI when defining the color-sample. Aurora Imaging Library ignores these pixels as if you had explicitly masked them with [`McolMask`](../../Reference/col/McolMask.md).

The color-sample index starts at 0. Each subsequent color-sample you add to the context is given a sequential index number. If you delete a color-sample, Aurora Imaging Library shifts all entries with higher indices down by one. This also applies when adding or deleting color elements from a color-sample.

To consistently access a color-sample using the same value, you can assign a label to a color-sample. If you don't, Aurora Imaging Library assigns one automatically. To change it afterward, use [`McolControl`](../../Reference/col/McolControl.md) with [`M_SAMPLE_LABEL_VALUE`](../../Reference/col/McolControl.md).

You can delete color elements from color-samples, or color-samples from the context, using [`McolDefine`](../../Reference/col/McolDefine.md). You can delete all color-samples at once (you must add color-samples one at a time). When you add, modify, or delete a color-sample, you must preprocess the context ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)) before any subsequent call to [`McolTransform`](../../Reference/col/McolTransform.md).

### Reference color-sample

As previously discussed, the reference color-sample is a unique type of color-sample. It defines the reference color of the relative color calibration context. After preprocessing the context, all its color-samples have an associated mapping to transform them according to the reference color-sample. Each relative color calibration context must have one reference color-sample.

Managing the reference color-sample is nearly identical to managing any other color-sample. Use [`M_REFERENCE_SAMPLE`](../../Reference/col/McolDefine.md) to add the reference color-sample to the context, delete the reference color-sample from the context, or add a color element to the reference color-sample. To delete a color element from the reference color-sample, use [`M_REFERENCE_SAMPLE_ITEM()`](../../Reference/col/McolDefine.md).

## Operation mode

With [`McolSetMethod`](../../Reference/col/McolSetMethod.md), you can set the operation with which to establish the color mappings that Aurora Imaging Library associates to the color-samples. This can affect the precision, speed, and flexibility of your relative color calibration application.

The following operations can be set:[`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) (default), [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md), or[`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md). [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) and [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md)are global type operations. Aurora Imaging Library establishes the color mapping using images as a whole. [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) is a local type operation. Aurora Imaging Library establishes the color mapping point-to-point. Global operations are more flexible. In certain cases, [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) can also be faster. Point-to-point operations are more precise, though they have more requirements, such as data alignment or pairing (transform the color at this exact point to the color at that exact point).

You can further affect how Aurora Imaging Library calculates the color mapping by using the color calibration intent settings in [`McolSetMethod`](../../Reference/col/McolSetMethod.md). These settings enhance or curtail the inherent precision of the operation. Aurora Imaging Library considers the operation and quality settings to be the strategy with which to establish the color mappings. For more information, see [Color calibration intent](S04_Relative_color_calibration.md).

Keep in mind that strategies calling for strict adherence to color data can cause overfitting. This refers to misleading transformations resulting from over emphasizing minor color fluctuations or outlier data. To avoid overfitting, select the proper strategy given the precision requirements of your application and provide realistic color data when establishing the mappings between color-samples and the reference color-sample. For more information, see [Selecting strategy](S04_Relative_color_calibration.md).

### Histogram-based

Histogram-based transformations are well known in the field of color image processing. Such transformations create a color-sample's mapping based on transforming its color distribution to resemble the color distribution of the reference color-sample.

Histogram-based transformations are typically effective when dealing with similar scenes (similar images and color data). Previous examples showing the color correction of foodstuff use [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md). These plates of food look generally the same; their colors do not differ radically and outlier color data is minimal. The animation below shows how histogram-based color calibration is performed.

*[Image: ColorCalibration]*

Unlike [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md), [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) does not require a point-to-point correspondence between the color-sample and the reference color-sample. Color fluctuations, misalignment, and outliers have less effect on histogram-based transformations and overfitting issues are less likely. For example, the foodstuff images are not aligned, and the histogram-based relative color calibration works well.

To reduce processing time, consider resizing very large images. This also reduces memory allocation.

### Color-to-color

For [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md), Aurora Imaging Library creates a color-sample's mapping based on transforming its color to the color of the reference color-sample on a point-to-point basis. This makes [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) the most precise operation. It is most effective when your application requires an exact reproduction of color.

Since the color-to-color operation depends on a strict point-to-point correspondence to establish a color-sample's mapping, consider using a ColorChecker image to define and preprocess your color-samples.

*[Image: ColorRelativeCalibration_ColorChecker.png]*

ColorCheckers are well known in the color management field. They hold the color information you intend to process in a square grid. Although not mandatory (you can use any image), ColorCheckers can prove convenient. You can grab them with the required camera and illuminant to create image type color-samples ([`M_IMAGE`](../../Reference/col/McolDefine.md)). Since [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) deals with the exact reproduction of color, using ColorCheckers with explicit color values ([`M_TRIPLET`](../../Reference/col/McolDefine.md)) can also prove effective.

The following is an example of the type of color issues that the color-to-color operation can correct.

*[Image: ColorRelativeCalibration_StrategyOperationColorToColorPoster.png]*

These images show the same 2 posters with different colors caused by 2 different cameras and illuminants. The first poster has the color you want. The second poster has the color to correct. Using the same 2 cameras and illuminants, you can grab a ColorChecker image.

*[Image: ColorRelativeCalibration_StrategyOperationColorToColorChecker.png]*

You can now use [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) to create a mapping to transform the color data of the second ColorChecker (color-sample) to the color data of the first ColorChecker (reference color-sample). You can then use the mapping to correct the color of the poster.

*[Image: ColorRelativeCalibration_StrategyOperationColorToColorPosterWithTransformation.png]*

You can use any image with a color-to-color operation, as long as you can pair the colors in the color-sample with the colors in the reference color-sample. ColorCheckers simplify this. When using[`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md), it is recommended to have a true pixel-wise correspondence between images. For example, the ColorChecker image for the color-sample should be exactly the same as the ColorChecker image for the reference color-sample (the images should be perfectly aligned, have the same dimensions, the same angle of rotation, and be at the same scale). Of course, the actual colors in the ColorCheckers should be different. In such cases, it is also useful to make a correspondence between the colors in a ColorChecker image (the color-sample) and [`M_TRIPLET`](../../Reference/col/McolDefine.md) color values (the reference color-sample).

### Global mean variance

For [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md), Aurora Imaging Library creates a color-sample's mapping based on transforming its mean and variance (standard deviation) to resemble the mean and variance of the reference color-sample. Such transformations can correct shifts in white-balance. They can also correct general color distortion that cameras, illuminants, and contrast dynamics in scenes can cause. The following is an example of the type of color issues with which you can use [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md).

*[Image: ColorRelativeCalibration_StrategyOperationGlobalMeanVarianceBoard.png]*

These images show an electronic board with 3 different color casting effects caused by 3 different illuminants and cameras. By using any one of the images as the reference color, the [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) operation can correct the other two colors accordingly.

*[Image: ColorRelativeCalibration_StrategyOperationGlobalMeanVarianceBoardWithTransformation.png]*

When using [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md), you need not have color data or content that is aligned or similar. The following is an example of how [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) can use the global color distribution of the mapping, established with a board image, to transform a color image that is completely different.

*[Image: ColorRelativeCalibration_StrategyOperationGlobalMeanVarianceBoardWithTransformationNature.png]*

You should typically use [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md)when you require a global impression of color, rather than the dynamics of color variation.

## Color calibration intent

You can specify the following color calibration intent (quality) settings, using [`McolSetMethod`](../../Reference/col/McolSetMethod.md):[`M_BALANCE`](../../Reference/col/McolSetMethod.md), [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md), or [`M_PRECISION`](../../Reference/col/McolSetMethod.md). These settings can increase or decrease the accuracy inherent in the operation by limiting or expanding the amount of color information the operation internally processes. For [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) and [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md), the default is [`M_BALANCE`](../../Reference/col/McolSetMethod.md). For [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md), the default is [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md)(no other color calibration intent is available).

With [`M_BALANCE`](../../Reference/col/McolSetMethod.md), the operation considers a moderate amount of color information to perform its calculations. This is a standard setting that represents a compromise between accuracy and robustness. There is a reduced risk of overfitting the color data, compared with [`M_PRECISION`](../../Reference/col/McolSetMethod.md).

With [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md), the operation considers a minimal amount of color information to perform its calculations. This setting represents the lowest possible accuracy, and the lowest risk of overfitting the color data. It also represents the fastest setting, which can be useful for very large images.

With [`M_PRECISION`](../../Reference/col/McolSetMethod.md), the operation considers a high amount of color information to perform its calculations. This setting represents the highest possible accuracy, and the highest risk of overfitting the color data. For example, if you perform the relative color calibration on grabbed images containing outlier colors that you did not consider during preprocessing, incorrect color effects can result.

## Computation option

By default, Aurora Imaging Library uses all pixel values between the reference color-sample and the color-samples to generate the color information that the operation requires. For an [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) operation, Aurora Imaging Library can use color statistics to generate the color information.

To specify how to generate the color information, call [`McolSetMethod`](../../Reference/col/McolSetMethod.md)and set the computation option to [`M_COMPUTE_ITEM_PIXELS`](../../Reference/col/McolSetMethod.md)(the default) or [`M_COMPUTE_ITEM_STAT`](../../Reference/col/McolSetMethod.md) (for color-to-color only).

Based on the particulars of your application, you must evaluate if color statistics are appropriate. Insufficient color information can lead to misleading transformations (such as overfitting).

## Selecting strategy

There are numerous options with which to base your strategy (operation and quality settings). The following is an overview of what to consider:

- Precision.
  This refers to the accuracy requirements of your application. [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) (with [`M_PRECISION`](../../Reference/col/McolSetMethod.md)) is the most precise, while [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) is the least. [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) attempts to reconcile both. The precision that [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) obtains is often comparable to [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md).
- Speed.
  This refers to the time requirements of your application.[`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) (with [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md)) offers the fastest processing, while [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) takes the longest to process.
  Strategy typically affects the speed of preprocessing ([`McolPreprocess`](../../Reference/col/McolPreprocess.md)) more than the relative color calibration itself ([`McolTransform`](../../Reference/col/McolTransform.md)). For example, while preprocessing with[`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md) is longer than [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md), the speed of the transformation is the same for both.
- Flexibility.
  This refers to the adaptability requirements of your application. [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) (with [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md)) is the most flexible. It usually proves useful when other operations fail, since its generality makes it less affected by the misalignment of color data or a strong dissimilarity between color data. [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) can also prove less prone to error if you must transform unknown images that contain many outlier colors.
  [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) is the least flexible. Since this is a point-to-point operation, minor misalignments can heavily influence transformations. [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md)offers a good compromise in flexibility. Although color-data should be basically similar, you need not have a point-to-point correspondence.
- Set up.
  This refers to the preprocessing requirements of your application. [`M_GLOBAL_MEAN_VARIANCE`](../../Reference/col/McolSetMethod.md) (with [`M_GENERALIZATION`](../../Reference/col/McolSetMethod.md)) is the simplest to set up. Since this is a very general type of operation, the color-samples you provide need not be especially accurate nor complete.
  [`M_COLOR_TO_COLOR`](../../Reference/col/McolSetMethod.md) is the most difficult to set up. Since this is a point-to-point operation, it can fail if you are using misaligned images. You can also provide a ColorChecker image, but you must externally define it. ColorCheckers are industry standards, and not native to Aurora Imaging Library.
  As in most cases, [`M_HISTOGRAM_BASED`](../../Reference/col/McolSetMethod.md)offers a middle ground, allowing you to set up using real-world images.
