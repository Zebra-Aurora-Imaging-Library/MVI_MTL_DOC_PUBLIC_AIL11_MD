---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Search_constraints
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Search constraints"
---

# Search constraints

When a model is defined (whether manually or automatically), it is assigned a set of default search constraints. You can change the following constraints:

- The search mode.
- The number of occurrences to find.
- The threshold for acceptance and certainty.
- The model's reference position.
- The region to search in the target image.
- The positional accuracy.
- The search speed.

## Specifying the search mode

When searching for multiple models, you can specify whether to use the search settings from each model, or from the first model in the context, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md). When searching for multiple occurrences of multiple models in your target image, selecting one search mode over the other could provide better results in your target application. Significant testing is required for identifying when one mode will improve the results of your find operation over the other.

When [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the search is performed using each model's current search settings. Note that, all models must use the same search region in the target image. The [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md) mode is best used when searching at an angle or when searching for an unknown number of occurrences in a region. For example, you must process an image of coins dumped on a tray. While the coins are spread out (so there is very little overlap), you cannot guarantee the change is not rotated nor can you know in advance how many coins are present.

When [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md), the search is performed using the first model's current search settings. The settings of all other models are ignored. In addition, you cannot use [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md) to perform an angular search ([`M_SEARCH_ANGLE...`](../../Reference/pat/MpatControl.md)). The [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md) mode is best used when searching for a maximum known number of occurrences in a region without knowing how many instances of each model are in the target image. For example, you know there will be 8 coins, but you don't know the exact amount of money (that is, you don't know how many of each coin exists in your target image).

## Specifying the number of matches

You can specify how many matches to try to find, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_NUMBER`](../../Reference/pat/MpatControl.md). If all you need is one good match, set the required number of occurrences to one (the default value) and avoid unnecessary searches for further matches. If a correlation has a match score greater than or equal to the certainty level, it is automatically considered an occurrence (default 80%), the remaining occurrences will be the best of those greater than or equal to the acceptance level.

When you ask for a specific number of matches (using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_NUMBER`](../../Reference/pat/MpatControl.md)), the [`MpatFind`](../../Reference/pat/MpatFind.md) function might not find that number; you should always call [`MpatGetResult`](../../Reference/pat/MpatGetResult.md) with [`M_NUMBER`](../../Reference/pat/MpatGetResult.md) to see how many occurrences were actually found. When multiple results are found (in [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md)), they are returned in decreasing order of match score (that is, best match first). When using [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), the occurrences of each model are grouped together in decreasing order of match scores from the first model to the last. To get the model index associated with a specific model occurrence, use [`MpatGetResult`](../../Reference/pat/MpatGetResult.md) with [`M_INDEX`](../../Reference/pat/MpatGetResult.md).

## Setting the acceptance level

The level at which the correlation (match score) between the model and the pattern in the image is considered a match is called the acceptance level.

You can set the acceptance level for the specified model, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_ACCEPTANCE`](../../Reference/pat/MpatControl.md). If the correlation between the target image and the model is less than this level, they are not considered a match. A perfect match is 100%, a typical match is 80% or higher (depending on the image), and no correlation is 0%. If your images have considerable noise and/or distortion, you might have to set the level below the default value of 70%. However, keep in mind that such poor-quality images increase the chance of false matches and will probably increase the search time.

Note, perfect matches are generally unobtainable because of noise introduced when grabbing images.

## Setting the certainty level

The certainty level is the match score above (or equal to) which the algorithm can assume that it has found a very good match and can stop searching the rest of the image for a better one. The certainty level is very important because it can greatly affect the speed of the search. To understand why, you need to know a little about how the search algorithm works.

Since a brute force correlation of the entire model, at every point of the image, would take several minutes, it is not practical. Therefore, the algorithm has to be intelligent. It first performs a rough but quick search to find likely match candidates, and then checks out these candidates in more detail to see which are acceptable.

A significant amount of time can be saved if several candidate matches never have to be examined in detail. This can be done by setting a certainty level that is reasonable for your needs, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_CERTAINTY`](../../Reference/pat/MpatControl.md). A good level is slightly lower than the expected score. If you absolutely must have the best match in the image, set the level to 100%. This would be necessary if, for example, you expect the target image to contain other patterns that look similar to your model. Unwanted patterns might have a high score, but this will force the search algorithm to ignore them. Symmetrical models fall into this category. At certain angles symmetrical models might seem like an occurrence in the target image, but if the search was completed, a match with a higher score would be found.

If you know that the model is unique in the image, then anything that reaches the acceptance level must be the match you want; therefore, you can set the certainty and acceptance levels to the same value. Otherwise, the certainty should be set to a value above the acceptance level.

Another common case is a pattern that usually produces very good scores (say above 80%), but occasionally a degraded image produces a much lower score (say 50%). Obviously, you must set the acceptance level to 50%; otherwise you will never get a match in the degraded image. However, you cannot set the certainty level to 50% because you take the risk that it will find a false match (above 50%) in a good image before it finds the real match that scores 90%. A better value is about 80%, meaning that most of the time the search will stop as soon as it sees the real match, but in a degraded image (where nothing reaches the certainty level), it will take the extra time to look for the best match that reaches the acceptance level.

## Redefining the model's reference position

The coordinates returned by [`MpatGetResult`](../../Reference/pat/MpatGetResult.md), after a call to [`MpatFind`](../../Reference/pat/MpatFind.md), are the coordinates of the models' reference position transformed (in pixel or real-world units, depending on whether the camera setup is calibrated). By default, this reference position is defined to be at the geometric center of the model. When using pixel units, results are returned relative to the top-left corner of the target image. When using calibration units, results are returned with respect to the relative coordinate system.

If there is a particular spot from which you would like results returned, you can change the model's reference position, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE_X`](../../Reference/pat/MpatControl.md) and [`M_REFERENCE_Y`](../../Reference/pat/MpatControl.md). For example, if your model has a hole and you want to find results with respect to this hole, change the reference position of the model accordingly. Note that you can define the reference position to be outside of the model's boundary.

*[Image: pattern_matching_center_point.png]*

## Selecting the search region

Instead of searching the entire target image, you can limit the search region using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_OFFSET...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE...`](../../Reference/pat/MpatControl.md). The search region specifies the region in which to find the model's reference position. Therefore, the search region can even be smaller than the model. If you have redefined the model's reference position (with [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_REFERENCE...`](../../Reference/pat/MpatControl.md)), make sure that the search region covers this new reference position and takes into account the angular search range of the model.

*[Image: pattern_matching_search_region.png]*

Search time is roughly proportional to the region searched; always set the search region to the minimum required when speed is a consideration.

Alternatively, you can limit the operation to a region of the image buffer using a rectangular region of interest (ROI), set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). Doing so is more flexible than setting a search region using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_OFFSET...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE...`](../../Reference/pat/MpatControl.md) because it allows the search region to be at an angle. In addition, using an ROI allows you to define your search region in world units. However, for some regions, it might increase processing time compared to using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_OFFSET...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE...`](../../Reference/pat/MpatControl.md).

In general, you should not use a child buffer to delimit the search region to a portion of an image; this might cause the search operation to have border or edge effects (caused by incomplete neighbourhood operations and/or overscan pixels) and be less accurate as this operation does not assume that there is valid data outside of the buffer.

When limiting the search region of a model defined with[`M_CIRCULAR_OVERSCAN`](../../Reference/pat/MpatDefine.md), it is recommended that the search region always include the center of the model, even if the reference position has moved from its default position.

*[Image: pattern_matching_reference_pos.png]*

## Positional accuracy

You can set the positional accuracy for your search. To do so, use [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_ACCURACY`](../../Reference/pat/MpatControl.md). It can be set to:

- [`M_LOW`](../../Reference/pat/MpatControl.md) (typically ± 0.20 pixel).
- [`M_MEDIUM`](../../Reference/pat/MpatControl.md) (typically ± 0.10 pixel).
- [`M_HIGH`](../../Reference/pat/MpatControl.md) (typically ± 0.05 pixel).

Note, the actual precision achieved is dependent on the quality of the model and of the image (the tolerances listed above are typical for high-quality, low-noise images).

A less precise positional accuracy will speed up the search. Positional accuracy is also slightly affected by the search speed setting ([`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SPEED`](../../Reference/pat/MpatControl.md)).

## Selecting the speed setting

You can specify the algorithm's search speed, using [`MpatControl`](../../Reference/pat/MpatControl.md)with [`M_SPEED`](../../Reference/pat/MpatControl.md). When the search speed is set to [`M_VERY_HIGH`](../../Reference/pat/MpatControl.md) or [`M_HIGH`](../../Reference/pat/MpatControl.md), the search algorithm takes more shortcuts, and the search is performed faster. However, as you increase the speed, the robustness of the search operation (the likelihood of finding a model) can decrease. For more information on search speed, see [Speeding up the search](S09_Speeding_up_the_search.md).
