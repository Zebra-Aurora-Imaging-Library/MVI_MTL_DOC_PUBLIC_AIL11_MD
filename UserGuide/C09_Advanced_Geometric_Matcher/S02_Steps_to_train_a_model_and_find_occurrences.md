---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Steps_to_train_a_model_and_find_occurrences
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Steps to train a model and find occurrences"
---

# Steps to finding model occurrences and training a model

To use the AGM module, you can start by following the [finding steps](S02_Steps_to_train_a_model_and_find_occurrences.md) to search for a single-definition model. If you are not satisfied with the results, you can follow the [training steps](S02_Steps_to_train_a_model_and_find_occurrences.md) to train a composite-definition model, and then return to the finding steps to search for the trained model. Note that you can repeat the training phase until you obtain satisfactory results.

## Finding steps

The following steps provide a basic methodology for using the Aurora Imaging Library Advanced Geometric Matcher module to find model occurrences:

1. Allocate a find AGM context, using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_FIND`](../../Reference/agm/MagmAlloc.md).
2. Allocate a find AGM result buffer to hold the results of the find operation, using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_FIND_RESULT`](../../Reference/agm/MagmAllocResult.md).
3. Define and add a single-definition model to the find AGM context, using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_SINGLE`](../../Reference/agm/MagmDefine.md).
   Alternatively, you can copy a trained composite-definition model to the find AGM context, using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md), if you have performed the [training steps](S02_Steps_to_train_a_model_and_find_occurrences.md) in the subsection below.
4. Specify your required search settings, using [`MagmControl`](../../Reference/agm/MagmControl.md).
5. Preprocess your find AGM context, using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md).
6. Search the target for model occurrences, using [`MagmFind`](../../Reference/agm/MagmFind.md). Note that calling [`MagmFind`](../../Reference/agm/MagmFind.md) successively with targets of different sizes will slow the execution time.
7. If necessary, use [`MagmDraw`](../../Reference/agm/MagmDraw.md) to draw extracted features from the context or result buffer. For example, you can draw edges of the model or of a found occurrence.
8. Retrieve the required results from the find AGM result buffer, using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md).
   If your results have false positives, you can try re-calling [`MagmFind`](../../Reference/agm/MagmFind.md) after modifying search settings, such as [`M_ACCEPTANCE_DETECTION`](../../Reference/agm/MagmControl.md), [`M_ACCEPTANCE_FIT`](../../Reference/agm/MagmControl.md), and [`M_ACCEPTANCE_COVERAGE`](../../Reference/agm/MagmControl.md). If false occurrences are still found, you can repeat these steps with a trained composite-definition model, obtained from following the [training steps](S02_Steps_to_train_a_model_and_find_occurrences.md) in the subsection below.
9. If necessary, save your AGM contexts, using [`MagmSave`](../../Reference/agm/MagmSave.md) or [`MagmStream`](../../Reference/agm/MagmStream.md).
   Once you save an AGM context, you can later restore it, using [`MagmRestore`](../../Reference/agm/MagmRestore.md) or [`MagmStream`](../../Reference/agm/MagmStream.md). Restoring an AGM context instead of creating it again can be more efficient, especially if the restored context requires no further modifications. You can save or restore an AGM context to/from a file or memory stream.
10. Free all your allocated objects, using [`MagmFree`](../../Reference/agm/MagmFree.md), unless[`M_UNIQUE_ID`](../../Reference/agm/MagmAlloc.md) was specified during allocation.

If results of the [`MagmFind`](../../Reference/agm/MagmFind.md) operation are unsatisfactory (for example, Aurora Imaging Library finds false positives), you can improve results using the faulty occurrences to train a composite-definition model. Once you have trained a model, you must copy it into a find AGM context and again call [`MagmFind`](../../Reference/agm/MagmFind.md).

## Training steps

The following steps provide a basic methodology for using the Aurora Imaging Library Advanced Geometric Matcher module to train a model:

1. Prepare the images to use to train the model. Associate the images with ROIs that identify model appearances and background samples. Blue rectangles in the ROI define positive model locations, while red rectangles in the ROI define negative model locations. Each blue rectangle must identify an occurrence of the same model. All rectangles must be the same size. Use the AGM labeling tool (accessible by running _AgmLabelingTool.cpp_ in Aurora Imaging Example Launcher) to interactively draw the rectangles and automatically define the ROI.
   If you want the negative model locations to be automatically labeled, you must not add any red rectangles to your training images, and you must set [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) to [`M_AUTO`](../../Reference/agm/MagmControl.md).
2. Allocate a train AGM context, using [`MagmAlloc`](../../Reference/agm/MagmAlloc.md) with [`M_GLOBAL_EDGE_BASED_TRAIN`](../../Reference/agm/MagmAlloc.md).
3. Allocate a train AGM result buffer to hold the trained composite-definition model, using [`MagmAllocResult`](../../Reference/agm/MagmAllocResult.md) with [`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md).
4. Define and add a composite-definition model to the train AGM context, using [`MagmDefine`](../../Reference/agm/MagmDefine.md) with [`M_COMPOSITE`](../../Reference/agm/MagmDefine.md).
5. Specify training settings, such as [`M_EMPTY_REGIONS_MODE`](../../Reference/agm/MagmControl.md) and [`M_SMOOTHNESS`](../../Reference/agm/MagmControl.md), using [`MagmControl`](../../Reference/agm/MagmControl.md).
6. Preprocess your train AGM context, using [`MagmPreprocess`](../../Reference/agm/MagmPreprocess.md).
7. Train the composite-definition model, using [`MagmTrain`](../../Reference/agm/MagmTrain.md) with the training images set up in the first step.
8. Verify the status of the training operation and trained composite-definition model, using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_STATUS`](../../Reference/agm/MagmGetResult.md).
   If the model is not completely trained, you can use [`MagmControl`](../../Reference/agm/MagmControl.md) to adjust the model settings and/or you can modify the blue and red rectangles in your training images; then, re-call [`MagmTrain`](../../Reference/agm/MagmTrain.md).
9. Find model occurrences, using the [finding steps](S02_Steps_to_train_a_model_and_find_occurrences.md) in the subsection above. Note that after allocating a find AGM context, you must copy the trained composite-definition model to it, using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md).
   If the search results include false positives, you can mark them as background samples in the training images, using red rectangles, and then retrain your composite-definition model.
