---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Search_targets
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Search targets"
---

# Search targets

You can search for instances of a model in an image, or you can search for instances of it in an Edge Finder result buffer. If you are searching for instances of an [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md) model that is in a shape-specific context, you can also search for it in an[`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) Model Finder result buffer. In all cases, use [`MmodFind`](../../Reference/mod/MmodFind.md) to find the model. This function will write the results of the search in a specified Model Finder result buffer.

## Finding models in an image

To search for instances of models in an image, the target image must be a 1-band, 8-bit unsigned image. The minimum and maximum target image sizes are 16x16 and 32768x32768 pixels, respectively. It is important to note that the minimum and maximum target image sizes are Aurora Imaging Library limits. To make sure that you have enough memory, you can call [`MmodFind`](../../Reference/mod/MmodFind.md); if you don't have enough memory, you will get an Aurora Imaging Library error.

For an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the target image must be in the same calibrated state as the models. If the models are calibrated, the target must be; if the models are not calibrated, the target must not be.

For a synthetic model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model cannot be calibrated and will inherit the camera calibration of the target image. If the target is calibrated, the model will be interpreted in world units, and conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units.

You can search for a synthetic model in a calibrated or non-calibrated target image. If the target is calibrated, the model units should be the same as the calibrated units. In addition, you must associate the camera calibration context of the target image with the model using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Aurora Imaging Library needs the camera calibration context for internal purposes at preprocessing time. Note that Aurora Imaging Library will not use the camera calibration context to compensate for incongruencies between the model and the target, nor will Aurora Imaging Library map the model's coordinate system to that of the target.

Note that the edge extraction process, applied to the target image, uses the same denoising operation and detail level as the one for all image-type models in the Model Finder context. For more information, see [Extracting edges](S05_Defining_and_adding_models_to_your_model_finder_context.md).

## Finding models in an Edge Finder result buffer

When using an Edge Finder result buffer as the target, Model Finder searches for active edges of the models only in the included edges of the result buffer. Therefore, using the Edge Finder result buffer as a target, instead of an image, allows you to be more specific about the edges in which to search. For example, you could select out unwanted edges in the target, which will speed up the search and further calculations, or create a target that is composed of user-defined edges.

Some things have to be done before you can search for instances of a model in an Edge Finder result buffer:

- The Edge Finder result buffer must have been previously allocated using [`MedgeAllocResult`](../../Reference/edge/MedgeAllocResult.md).
- The Edge Finder result buffer must be compatible with the Model Finder context. Enable this using [`MedgeControl`](../../Reference/edge/MedgeControl.md) with [`M_MODEL_FINDER_COMPATIBLE`](../../Reference/edge/MedgeControl.md).
- [`MedgeCalculate`](../../Reference/edge/MedgeCalculate.md) must have already been called using this result buffer.

If the models in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md)type of Model Finder context are calibrated, the results in the Edge Finder result buffer must also be calibrated. If the models are not calibrated, the results must not be calibrated either.

In the case of synthetic models, the same rules apply as when using a calibrated image as a target.

## Finding models in an M_SHAPE_SEGMENT Model Finder result buffer

If you are searching for instances of an [`M_RECTANGLE`](../../Reference/mod/MmodDefine.md) model that is in a shape-specific context, you can also search for it in an[`M_SHAPE_SEGMENT`](../../Reference/mod/MmodAlloc.md) Model Finder result buffer. This can be useful if you want to restrict the search to segment-type edges.
