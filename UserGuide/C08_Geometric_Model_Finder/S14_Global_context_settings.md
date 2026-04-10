---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Global_context_settings
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Global context settings"
---

# Global context settings

The global search settings of a Model Finder context determine the strategy used to search for all of the context's models in the target. These settings directly affect the speed and robustness of the search. The context settings can be adjusted to fit your individual application's needs, using [`MmodControl`](../../Reference/mod/MmodControl.md) with the index parameter set to [`M_CONTEXT`](../../Reference/mod/MmodControl.md). We recommend experimenting with different settings to find the particular settings that provide your application with the necessary robustness.

This section discusses some of the Model Finder context settings that were not previously discussed in this chapter.

## Setting the search speed

You can specify the algorithm's search speed for an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SPEED`](../../Reference/mod/MmodControl.md). The speed can be set to [`M_MEDIUM`](../../Reference/mod/MmodControl.md) (default), [`M_HIGH`](../../Reference/mod/MmodControl.md), or [`M_VERY_HIGH`](../../Reference/mod/MmodControl.md). When you preprocess the context, Aurora Imaging Library analyzes the patterns of the models and determines which shortcuts are appropriate for the search speed setting. At higher search speed settings, the search can take all reasonable shortcuts; therefore, when possible, the search is performed faster than at lower speeds. You can typically use higher speed settings when high accuracy is not an issue; this is because increasing the speed might affect the robustness, as well as the accuracy, of the search.

## Accuracy

You can set the positional accuracy for your search, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ACCURACY`](../../Reference/mod/MmodControl.md). For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, it can be set to:

- [`M_LOW`](../../Reference/mod/MmodControl.md).
- [`M_MEDIUM`](../../Reference/mod/MmodControl.md).
- [`M_HIGH`](../../Reference/mod/MmodControl.md).

Note that the accuracy varies depending on the image quality (for example, saturation, noise level, and dynamic range) and with respect to the quality of the contours (for example, smooth, wavy, and broken). With that being said, with good target image conditions, you can expect an accuracy of 0.1 sub-pixel accuracy. With excellent target image conditions (high dynamic range, low noise, no distortion, high resolution), you can greatly improve the sub-pixel accuracy by up to 1/50<sup>th</sup> of a pixel.

The default accuracy setting for an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context is [`M_MEDIUM`](../../Reference/mod/MmodControl.md).

This control type is not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context. You can consider an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context to have an equivalent accuracy setting of [`M_MEDIUM`](../../Reference/mod/MmodControl.md) as well.

> **Note:** Note that increasing the accuracy can slightly reduce the search speed.

## Timing out your search

In time critical applications, you can set a time limit in milliseconds for Model Finder to find occurrences of the specified models, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_TIMEOUT`](../../Reference/mod/MmodControl.md). After the time limit, the search will stop even if the required number of occurrences is not found. Results are still returned for those occurrences found up to the timeout. However, it is not possible to predict which occurrences will be found before the time limit is reached. The default value is 2000 msecs.

You can use [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_TIMEOUT_END`](../../Reference/mod/MmodGetResult.md) to check whether the timeout limit has been reached.

## First and last levels

Model searches use a hierarchical method to reduce the time needed to complete the search. Using this method, Model Finder produces a series of smaller, lower-resolution versions of both the target and the models, and the search begins on a much-reduced scale. This series of subsampled images is sometimes called a resolution pyramid because of the shape formed when the different resolution levels are stacked on top of each other.

*[Image: Hierarchy.png]*

The resolution level for the initial and final stages of the search are typically set automatically. These default levels usually provide the best results for search operations, and you should leave them in most circumstances. If required for an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, you can set the resolution level for the initial and final stages of the search, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) or [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md). You can set the first and last levels to any integer value from 0 to 7. Level 0 is the original target size and each higher level is half the size (and resolution) of the previous one. The higher the level, the faster the initial search; however, the search will be less reliable at higher levels.

Very small models and search regions cannot be subsampled as many times as large ones. If you specify too high a level, the highest practical level will be used.

These control types are not supported for an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.
