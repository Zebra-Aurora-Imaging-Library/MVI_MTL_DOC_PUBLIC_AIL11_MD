---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Advanced_Geometric_Matcher_module
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Advanced Geometric Matcher module"
---

# Advanced Geometric Matcher module

The Advanced Geometric Matcher module (AGM) allows you to find models based on edges. Specifically, the module finds model occurrences in the target by determining edge-related scores for each candidate, such as coverage, detection, and fit. This can result in the AGM module offering a more stable execution time than the Model Finder module when dealing with multiple targets of the same size. Note that with AGM, successively searching in targets of different sizes will slow the execution time. AGM can prove particularly useful when there are variations in the occurrences and/or you need to search for model occurrences on a cluttered background.

*[Image: AGM_single_model_cluttered_background.png]*

AGM allows you to search for model occurrences using either a single-definition model or a composite-definition model. A single-definition model is defined from a single appearance of the model to find, using an entire image, a region of an image, an Edge Finder result buffer, or a CAD DXF file, while a composite-definition model is trained, using labeled images, to construct the best possible model. Note that the target can be an image or an Edge Finder result buffer.

In the case of a cluttered background, searching using a model defined from a single appearance might result in false occurrences being found due to background distraction. Unlike Model Finder, the AGM module allows you to train a model using multiple training images within which regions are identified as positive (true model occurrences) or negative (clearly non-model regions or potential false positives). This can result in a trained model that can better discriminate true occurrences from false occurrences in cluttered targets.

When your target contains a lot of occurrences, it is typically faster to search using a single-definition model, compared to using a composite-definition model (or the Model Finder module).

Note that although there can be variations in the occurrences, they must be at approximately the same scale as the model. Note that you can search for a model at a specific angle or within a range of angles.

The module does not provide support for camera calibration. If the training or target images are associated with a camera calibration context, or if the Edge Finder result buffer is associated with a camera calibration context and [`M_RESULT_OUTPUT_UNITS`](../../Reference/edge/MedgeControl.md) is not set to [`M_PIXEL`](../../Reference/edge/MedgeControl.md), an Aurora Imaging Library error will be generated.

The module also allows you to restore an AGM context from a file or memory stream, or save an AGM context to a file or memory stream.
