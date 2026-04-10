---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Pattern_matching
section: Preprocess_the_search_model
module_tag: pat
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / pattern / Preprocess the search model"
---

# Preprocessing the Pattern Matching context

When you are ready to search for the allocated model (either manually or automatically), you must preprocess the Pattern Matching context. The preprocessing stage uses the known model(s) to decide on the optimal search strategy for subsequent search operations. Preprocessing should be performed after all search constraints have been set. Use the [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) function to preprocess the Pattern Matching context.

[`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) has a parameter that allows you to specify a typical target image. Providing a typical image is optional; you can set this parameter to [`M_NULL`](../../Reference/pat/MpatPreprocess.md). If you provide this image, it helps [`MpatPreprocess`](../../Reference/pat/MpatPreprocess.md) improve the search's robustness and optimize the strategy for subsequent search operations. You should only specify a typical image if the model occurrence will always appear on the same type of background.

Note that when you load or restore a Pattern Matching context (using[`MpatRestore`](../../Reference/pat/MpatRestore.md)), you must preprocess it, even if it was preprocessed before saving.
