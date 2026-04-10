---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Color
section: Basic_concepts
module_tag: col
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / color / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Color Analysis module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Color Analysis module are:

- **Area identifier image**. An image, provided with the color matching operation, that specifies the target areas.
- **Background pixels**. Pixels outside the target areas.
- **Best-matched color-sample**. The color-sample that most matches a target area's color, with respect to all color matching constraints.
- **Color distance**. The numerical difference between two colors.
- **Color-sample element**. The individual color data entities that define a color-sample.
- **Color identification**. Matching the color of each target area with the best color-sample.
- **Color matching**. Calling [`McolMatch`](../../Reference/col/McolMatch.md) to perform color identification or supervised color segmentation.
- **Color matching context**. An Aurora Imaging Library object that stores all color-samples and color matching settings.
- **Color-sample**. The information, in the context, that defines the color to be processed using relative color calibration or color matching. For relative color calibration, color-samples have an associated color-mapping between it and the reference color-sample.
- **Color space**. A mathematical model, that typically has 3 components (for example, RGB, HSL, LAB), with which to describe colors.
- **Color space encoding**. Color transformation from the range represented in an image buffer (for example, 0 to 255) to its native (theoretical) range (for example, all real numbers between 0 and 1).
- **First principal component**. The strongest principal component vector computed from a PCA. If no principal component is the strongest, one is arbitrarily considered the first principal component.
- **IEC**. Refers to the International Electrotechnical Commission, a globally-recognized standards organization in the field of electrotechnology.
- **Match score**. A measure of similarity between the color of the target area and the color of the color-sample, when performing color matching.
- **Outliers**. Pixels, within a target area, that do not relate to any color-sample.
- **Principal component analysis (PCA)**. A mathematical analysis which, when applied to a color image's data, results in vectors pointing towards the direction of maximum color variance. Each vector is considered a principal component. There are as many principal components as there are color bands.
- **Reference color-sample**. The information, in the relative color calibration context, with which to establish the color mapping used in the relative color calibration.
- **Reference color space**. The standard used to interpret the color space data (for example, sRGB).
- **Relative color calibration**. Calling [`McolTransform`](../../Reference/col/McolTransform.md) to adjust an image's color data according to the mapping associated with a color-sample in a relative color calibration context.
- **Relative color calibration context**. An Aurora Imaging Library object that stores the reference color-sample, the color-samples, and the color mapping with which to perform relative color calibration.
- **Relevance score**. A measure of confidence associated with the color match score.
- **Source color space**. The color space with which the Aurora Imaging Library Color Analysis module interprets color.
- **sRGB**. Standard RGB specifications, as defined by the IEC Project Team 61966-2-1.
- **Supervised color segmentation**. Matching the color of each target area pixel with the best color-sample. Used to determine the proportion of color in a target area, based on the color-samples.
- **Target**. The image with which to perform the color matching.
- **Target area**. A section of the target with which to match a color-sample.
- **Triplet**. A color-sample defined with three explicit color component values.
