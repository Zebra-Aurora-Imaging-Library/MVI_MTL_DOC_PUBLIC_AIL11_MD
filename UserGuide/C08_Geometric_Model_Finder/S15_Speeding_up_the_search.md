---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Speeding_up_the_search
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Speeding up the search"
---

# Speeding up the search

There are some things you should do to help ensure that your search runs as quickly as possible:

- Adjust the search speed of the algorithm.
- Disable calculations specific to range search strategies, if possible.
- Define several models with different expected angles.
- Limit the position range.
- Limit the search scale.
- Specify the exact number of expected occurrences.
- Clean up your model.
- Change the search levels (for controlled geometric Model Finder contexts only).
- Use an appropriate context type for your search.

## Adjust the search speed of the algorithm

You can control the search speed of the algorithm used by your Model Finder context, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SPEED`](../../Reference/mod/MmodControl.md). When you preprocess the context, Aurora Imaging Library analyzes the patterns of the models and determines which shortcuts are appropriate for the search speed setting. At higher search speed settings, the search can take all reasonable shortcuts; therefore, when possible, the search is performed faster than at lower speeds. You can typically use higher speed settings when high accuracy is not an issue; this is because increasing the speed might affect the robustness and accuracy.

## Disable calculations specific to range search strategies

If you expect that the occurrences sought are close to the specified nominal position, angle, or scale (for example, in a registration application), you can try disabling the calculations specific to the search strategies for the corresponding range ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_POSITION_RANGE`](../../Reference/mod/MmodControl.md), [`M_SEARCH_ANGLE_RANGE`](../../Reference/mod/MmodControl.md), and/or [`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md)) to see if Model Finder can still find the required occurrences. Disabling one or more of these calculations might speed up the search depending on the model and the target.

## Define several models with different expected angles

The actual angle of the occurrence does not affect search speed. If you need to search for a model at discrete angles only (for example, at intervals of 90 degrees), it is typically more efficient to define several models with different expected angles, than to search through the full angular range.

## Limit the position range

Limiting the position range of the model to the minimum required generally decreases the search time. Set the position range of each model to the minimum needed, using [`MmodControl`](../../Reference/mod/MmodControl.md) with the [`M_POSITION_DELTA_NEG_X`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_NEG_Y`](../../Reference/mod/MmodControl.md), [`M_POSITION_DELTA_POS_X`](../../Reference/mod/MmodControl.md), and [`M_POSITION_DELTA_POS_Y`](../../Reference/mod/MmodControl.md) control types. If the occurrence must be at a specific position, set these control types to zero.

## Limit the search scale

It is best to set a fixed scale at which to search for occurrences of a model, instead of searching in a range. Sometimes, you must search for model occurrences that can vary in size; in these cases, enabling [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_SEARCH_SCALE_RANGE`](../../Reference/mod/MmodControl.md) might be necessary. In these cases, keep the scale range as small as possible to avoid slowing down your search. You should not use the scale range to cover different expected scales at different positions. In this case, it is typically more efficient to define several models with different expected scales. This is because a large scale range could potentially slow down your operation; as well, you could find unwanted occurrences.

## Specify the exact number of expected occurrences

If you know the exact number of occurrences for which to search, specify it using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_NUMBER`](../../Reference/mod/MmodControl.md). This will typically reduce search time. If you search for more occurrences than required, the search might take longer than necessary.

## Clean up your model and your target

Typically, reducing the number of unwanted edges in your models and your target will accelerate your search. There are several different ways to clean up your model and your target:

- **Reduce the noise.** Noise in your models can cause inaccurate results. In addition, noise in your models and/or your target can also increase the length of time for a search. The most efficient way to reduce noise is to use the [`MmodControl`](../../Reference/mod/MmodControl.md)   [`M_SMOOTHNESS`](../../Reference/mod/MmodControl.md) and [`M_DETAIL_LEVEL`](../../Reference/mod/MmodControl.md) control types. These affect all image-type models in your context, as well as the target when it is an image.
- **Use masking.** You can use [`MmodMask`](../../Reference/mod/MmodMask.md) to mask regions of the model. Using [`MmodMask`](../../Reference/mod/MmodMask.md), you can effectively remove areas of the model from consideration. This might help speed up your search.
- **Use the Edge Finder module.** You can use the Edge Finder module to extract only the required edges from the image that you want to use as your model source or target, and then use the Edge Finder results as the model source or target instead.
- **Limit the number of active edgels Aurora Imaging Library processes in the target.** By calling [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ACTIVE_EDGELS`](../../Reference/mod/MmodControl.md), you can adjust the degree to which Aurora Imaging Library processes edgels in the target ([`MmodFind`](../../Reference/mod/MmodFind.md)) for a geometric ([`MmodAlloc`](../../Reference/mod/MmodAlloc.md) with [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md)) type of Model Finder context. The target edgels that Aurora Imaging Library processes are considered active. By limiting the active edgels in the target, you can speed up the search. To draw the target's active edgels, use [`MmodDraw`](../../Reference/mod/MmodDraw.md) with [`M_DRAW_ACTIVE_EDGELS`](../../Reference/mod/MmodDraw.md).

## Change the search levels

It is possible to speed up the search by adjusting the resolution levels for the search. This can be done using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_FIRST_LEVEL`](../../Reference/mod/MmodControl.md) and [`M_LAST_LEVEL`](../../Reference/mod/MmodControl.md). This should be done cautiously; their default setting ([`M_AUTO`](../../Reference/mod/MmodControl.md)) determines the resolution levels automatically and usually provides the best results for the search operation.

## Use an appropriate context type for your search

As has been discussed throughout this chapter, certain types of contexts have advantages and disadvantages with regards to the type of model being sought in the target. For instance, if the occurrences in a target vary in scale and have various distinct angles, an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) type of Model Finder context would be appropriate. Whereas, if the occurrences in the targets have a nominal difference in scale and angle, but have a high geometric complexity, the [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context could be the most appropriate. When searching for general elliptical shapes, the [`M_SHAPE_ELLIPSE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context will search for a range of aspect ratios ([`M_MODEL_ASPECT_RATIO`](../../Reference/mod/MmodControl.md)) relative to the nominal aspect ratio, and will typically provide better and/or faster results than the two previously mentioned context types. If the ellipses are in fact circles, the [`M_SHAPE_CIRCLE`](../../Reference/mod/MmodAlloc.md) type of Model Finder context will typically provide better and/or faster results then the other types of contexts.
