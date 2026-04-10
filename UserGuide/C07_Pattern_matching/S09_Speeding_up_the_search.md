---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Speeding_up_the_search
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Speeding up the search"
---

# Speeding up the search

To ensure the fastest possible search, you should:

- Choose an appropriate model.
- Set the search speed to the highest possible setting for your application.
- Set the search region to the minimum required. Search time is roughly proportional to the region searched, so don't search the whole image if you don't need to.
- Search the smallest range of angles required.
- Select the lowest positional accuracy that you need.
- Set the certainty level to the lowest reasonable value (so that the search can stop as soon as a good match is found).
- Search for multiple models at the same time, if possible.

If you can't attain the required speed using these techniques, you can use some more advanced techniques discussed in [Pattern matching algorithm (for advanced users)](S10_Pattern_matching_algorithm_for_advanced_users.md).

## Choose the appropriate model

The size of a model affects the search speed. In general, small models take longer to find than larger ones, although very large models can also be time-consuming. In general, the optimal size is approximately 128x128 pixels if you are searching a large region (for example, most of the image). Small models are found quickly when the search region is not too large.

## Adjust the search speed setting

For any model, it is possible to set the speed of the search. Increasing the search speed reduces the search time, however, as you increase the speed, the robustness of the search operation (the likelihood of finding a model) can decrease. When you call [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md), Aurora Imaging Library analyzes the pattern in the model, and determines appropriate shortcuts; only shortcuts that are considered safe for a particular model are taken. This also means that higher search speeds might not be any faster for certain models, depending on the particular pattern. Higher search speeds can reduce the positional accuracy very slightly.

You adjust the search speed setting, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SPEED`](../../Reference/pat/MpatControl.md). This function has five settings:

- [`M_VERY_HIGH`](../../Reference/pat/MpatControl.md).
- [`M_HIGH`](../../Reference/pat/MpatControl.md).
- [`M_MEDIUM`](../../Reference/pat/MpatControl.md).
- [`M_LOW`](../../Reference/pat/MpatControl.md).
- [`M_VERY_LOW`](../../Reference/pat/MpatControl.md).

As expected, the [`M_VERY_HIGH`](../../Reference/pat/MpatControl.md) and [`M_HIGH`](../../Reference/pat/MpatControl.md) speed settings allow the search to take all possible shortcuts, performing the search as fast as possible. Higher speed settings are recommended when searching on a good quality image or when using a simple model. Note, the search might have a lower tolerance for rotation when using this setting.

The [`M_MEDIUM`](../../Reference/pat/MpatControl.md) speed setting is the default setting and is recommended for medium quality images or more complex models. A search with this setting is capable of withstanding up to approximately 5° of rotation for typical models.

Use the [`M_LOW`](../../Reference/pat/MpatControl.md) or the [`M_VERY_LOW`](../../Reference/pat/MpatControl.md) speed settings only if the image quality is particularly poor and you have encountered problems at higher speeds. The speed settings are discussed further in the algorithm description at the end of this chapter.

## Effectively choose the search region and search angle

You can improve performance by not searching the whole image unnecessarily. Search time is roughly proportional to the region searched; set the search region to the minimum required, using [`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_OFFSET...`](../../Reference/pat/MpatControl.md) and [`M_SEARCH_SIZE...`](../../Reference/pat/MpatControl.md), or using a rectangular region of interest (ROI) set using [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md). The latter method is more flexible but has its own limitations, as outlined in [](S07_Search_constraints.md).

You can also improve performance by selecting the lowest positional accuracy. In addition, for an angular search, select lowest angular accuracy ([`MpatControl`](../../Reference/pat/MpatControl.md) with [`M_SEARCH_ANGLE_ACCURACY`](../../Reference/pat/MpatControl.md)) and range required, in combination with the highest tolerance possible.
