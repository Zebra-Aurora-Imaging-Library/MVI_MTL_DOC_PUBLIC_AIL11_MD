---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Steps_to_performing_a_pattern_search
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Steps to performing a pattern search"
---

# Steps to performing a pattern search

The following steps provide a basic methodology for using the Aurora Imaging Library Pattern Matching module:

1. Allocate a Pattern Matching context, using [`MpatAlloc`](../../Reference/pat/MpatAlloc.md).
2. Load or grab an image from which to define a model. This image is known as the model's source image. This must be an 8-bit unsigned grayscale image that should be (or contain) a good example of a typical model.
3. Define and add your model to this Pattern Matching context, using [`MpatDefine`](../../Reference/pat/MpatDefine.md) with the model's source image.
4. Optionally, repeat the two previous steps as many times as necessary to grab, define, and add your models to this Pattern Matching context. All models in the same context must be the same size and have the same resolution, but models can come from different model's source images.
5. Optionally, mask any irrelevant areas of the model using [`MpatMask`](../../Reference/pat/MpatMask.md).
6. Optionally, specify your required search settings for both the context and the individual model(s), using successive calls to [`MpatControl`](../../Reference/pat/MpatControl.md).
   When searching for several models, the search settings can either be set for the first model (when [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md)) or in each model (when [`M_SEARCH_MODE`](../../Reference/pat/MpatControl.md) is set to [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md)). Note that, with [`M_FIND_BEST_MODELS`](../../Reference/pat/MpatControl.md), all models must be of the same size, and with [`M_FIND_ALL_MODELS`](../../Reference/pat/MpatControl.md), all models must use the same search region.
7. Preprocess the Pattern Matching context, using [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md).
8. Allocate a result buffer to store the results of your search, using [`MpatAllocResult`](../../Reference/pat/MpatAllocResult.md).
9. Grab a target image. Optionally, process it to improve its quality.
10. Search the target image for occurrences of the models in your Pattern Matching context, using [`MpatFind`](../../Reference/pat/MpatFind.md).
11. Retrieve the required results from the result buffer, using [`MpatGetResult`](../../Reference/pat/MpatGetResult.md). You can use [`MpatDraw`](../../Reference/pat/MpatDraw.md) to draw the occurrences' bounding box and/or a cross at the occurrences' reference position.
   The coordinates resulting from a search return the reference position of the model, relative to the center of the top-left pixel of the target image. To determine what were the equivalent coordinates of the model's reference position in the model's source image, use [`MpatInquire`](../../Reference/pat/MpatInquire.md) with [`M_ORIGINAL_X`](../../Reference/pat/MpatInquire.md) and [`M_ORIGINAL_Y`](../../Reference/pat/MpatInquire.md).
12. Optionally, save your Pattern Matching context, using [`MpatSave`](../../Reference/pat/MpatSave.md) or [`MpatStream`](../../Reference/pat/MpatStream.md).
13. Free all your allocated Pattern Matching objects, using [`MpatFree`](../../Reference/pat/MpatFree.md), unless [`M_UNIQUE_ID`](../../Reference/pat/MpatAlloc.md) was specified during allocation.

In general, the first seven steps are performed once, while steps 8 through 11 are repeated as required. Note, in practice, models are usually saved on disk, using [`MpatSave`](../../Reference/pat/MpatSave.md); therefore steps 1 through 6 are often replaced by a single step that restores a saved model from disk, using [`MpatRestore`](../../Reference/pat/MpatRestore.md).
