---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Advanced_Geometric_Matcher
section: Training_a_model
module_tag: agm
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / advanced-geometric-matcher / Training a model"
---

# Training a model

A composite-definition model must be successfully trained, using labeled training images, before you can use it to search for occurrences. To train a model, use [`MagmTrain`](../../Reference/agm/MagmTrain.md). This function will store the trained composite-definition model in a train AGM result buffer ([`M_GLOBAL_EDGE_BASED_TRAIN_RESULT`](../../Reference/agm/MagmAllocResult.md)).

## Labeling training images

Unlike the source image used to define a single-definition model, training images contain appearances of the model and background. Appearances teach the composite-definition model what is a favorable feature of an occurrence, while background samples identify which features are not. The model appearances and background samples must be labeled using an ROI containing blue and red rectangles, respectively; the model is trained accordingly. Use the AGM labeling tool, described later in this section, to interactively draw the rectangles and automatically define the ROI.

Blue rectangles define positive model locations, while red rectangles define negative model locations. You must draw blue rectangles around instances of the required model. [`MagmTrain`](../../Reference/agm/MagmTrain.md) can only train one model; therefore, each blue rectangle must identify an occurrence of the same model. You should attempt to label your training images such that all model occurrences appear in the exact same position within each blue rectangle. If the blue rectangles are not precise, you can use [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_REALIGN_STRENGTH`](../../Reference/agm/MagmControl.md) to allow the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation to displace the blue rectangles. The higher the realignment strength, the further [`MagmTrain`](../../Reference/agm/MagmTrain.md) can displace the blue rectangles so that the model occurrences appear in the same position within each blue rectangle. You can use [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) to set whether to automatically label the negative model locations in the training images or to use manually defined negative (red) rectangles.

If [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_AUTO`](../../Reference/agm/MagmControl.md), you don't draw red rectangles around the background samples; [`MagmTrain`](../../Reference/agm/MagmTrain.md) will automatically identify and label the background areas of your training images. In this case, a minimum of one training image must have an ROI with a blue rectangle. The ROI can be composed of several non-contiguous and/or overlapping areas, but it must contain at least one blue rectangle and cannot contain any red rectangles. Note that there can be training images with no ROI (no rectangles). In [`M_AUTO`](../../Reference/agm/MagmControl.md) mode, these images are included when identifying background areas to label.

If [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_DEFINED`](../../Reference/agm/MagmControl.md), you must draw red rectangles around the background samples. It is important to also include background samples that resemble the model, since these are most likely to be misidentified as occurrences. In this case, the set of training images must contain red and blue rectangles, associated to the images using regions of interest (ROIs). The ROI can be composed of several non-contiguous and/or overlapping areas, but it must contain at least one red rectangle and/or one blue rectangle. Considering all the rectangles across all training images, there must be a minimum of one red rectangle and one blue rectangle. Note that there can be training images with only red rectangles, only blue rectangles, or no rectangles. The training images with no rectangles are not used during training.

If after training, the [`MagmFind`](../../Reference/agm/MagmFind.md) operation returns false occurrences, you should return to this step and draw red rectangles around each of the incorrectly identified occurrences. Note that the more rectangles you add, the longer the training duration.

Training images must be 1-band, 8-bit unsigned images. The minimum size of each training image, red rectangle, and blue rectangle is 6 x 6 pixels, while the maximum size is 32768 x 32768 pixels. Note that all rectangles must be the same size. The center of each rectangle must be inside the training image. If [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) is set to [`M_DISABLE`](../../Reference/agm/MagmControl.md), all rectangles must have an angle of 0.0°.

*[Image: AGM_chess_training_image.png]*

### Labeling your training images using the AGM labeling tool

You can label your training images interactively using the _AgmLabelingTool_ example, accessible from Aurora Imaging Example Launcher.

_AgmLabelingTool_ provides an interface to help you interactively label your training images. After launching the tool and specifying the folder that contains your images, you must select an appearance of your model. This model appearance will be used as a reference for the labeling process. Note that all rectangles will be the same size as the reference model. Label all model appearances and, if required, background samples by adding and validating positive (blue) and negative (red) rectangles. Upon saving, you will be provided with a container of labeled training images for use with [`MagmTrain`](../../Reference/agm/MagmTrain.md). You will also be provided with an image of the reference model that you can use, instead of the trained composite-definition model, for the fit and coverage calculations.

### Labeling your training images using the Graphics and Buffer modules

You can also use the Graphics and Buffer modules to label your training images. To do so, use the Aurora Imaging Library 2D graphics functions, such as [`MgraAllocList`](../../Reference/gra/MgraAllocList.md), [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_COLOR`](../../Reference/gra/MgraControl.md), and [`MgraRect`](../../Reference/gra/MgraRect.md), to add positive (blue) and, if required, negative (red) rectangles to a graphics list, and then use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) to associate the graphics list with the training image buffer. Repeat the process for each training image. You can add the labeled image buffers to a container using [`MbufCopyComponent`](../../Reference/buf/MbufCopyComponent.md) or create an array of image buffers, which you can then pass to [`MagmTrain`](../../Reference/agm/MagmTrain.md).

## Training settings

With successive calls to [`MagmControl`](../../Reference/agm/MagmControl.md), you can customize the training settings of the model in the train AGM context. You can inquire about these settings using [`MagmInquire`](../../Reference/agm/MagmInquire.md).

Fundamental training settings include the maximum label overlap, realignment strength, smoothness factor, threshold mode, the mode with which to handle substandard labeled image regions, and whether to use rotated labeled image regions.

### Maximum label overlap

If [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_AUTO`](../../Reference/agm/MagmControl.md), [`MagmTrain`](../../Reference/agm/MagmTrain.md) automatically labels background areas with red rectangles. You can set the maximum allowable overlap between two automatically generated red rectangles, and the maximum allowable overlap between an automatically generated red rectangle and a blue rectangle, using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_NEGATIVE_LABELS_MAX_OVERLAP`](../../Reference/agm/MagmControl.md) (the default setting is 20.0%) and [`M_NEGATIVE_LABELS_POSITIVE_MAX_OVERLAP`](../../Reference/agm/MagmControl.md) (the default setting is 50.0%), respectively.

### Realignment strength

All model occurrences should appear in the exact same position within each blue rectangle. If the blue rectangles are not precise, you can allow the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation to displace them; the realignment strength determines the strength with which [`MagmTrain`](../../Reference/agm/MagmTrain.md) can displace the blue rectangles to better align them. Set the realignment strength using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_REALIGN_STRENGTH`](../../Reference/agm/MagmControl.md). The higher the realignment strength, the further [`MagmTrain`](../../Reference/agm/MagmTrain.md) can displace the blue rectangles so that the model occurrences appear in the same position within each blue rectangle. When the realignment strength is set to 0.0 (default), no realignment occurs.

### Smoothness

The smoothness factor determines the degree of noise reduction applied to the training images. Set the smoothness factor value using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_SMOOTHNESS`](../../Reference/agm/MagmControl.md) (the default setting is 50.0). Increasing the smoothness factor reduces noise, resulting in the extraction of fewer unwanted edges.

### Threshold

The threshold mode determines the edge extraction sensitivity. Set the threshold using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md) (the default setting is [`M_HIGH`](../../Reference/agm/MagmControl.md)). A higher threshold decreases the sensitivity of the edgel detection, resulting in the extraction of fewer edgels. You should use the highest possible threshold that still detects all pertinent edgels.

Note that when [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) is set to [`M_ENABLE`](../../Reference/agm/MagmControl.md), thresholding is not applied to the training images or target images, and the magnitude of the gradient is used instead. Note also that the threshold is copied to the find AGM context and used to extract the edges of the model when [`M_MODEL_SOURCE`](../../Reference/agm/MagmControl.md) is set to [`M_USER_IMAGE`](../../Reference/agm/MagmControl.md).

### Label noise

Some labeled regions in the training images might not be ideal for training the model. Training the model using labels that are hard to correctly classify can cause the model to overfit on the training data, resulting in a distorted trained composite-definition model. For example:

- Blue (positive) rectangles that have missing or extra features in comparison to other blue rectangles.
- Red (negative) rectangles that contain similar features to the model.
- Incorrectly labeled regions (model appearances labeled with red (negative) rectangles or background samples labeled with blue (positive) rectangles).
- Labeled regions with a lot of noise.

Set the mode with which to handle substandard labeled image regions using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_TRAIN_LABELS_NOISE_MODE`](../../Reference/agm/MagmControl.md) (the default setting is [`M_DISABLE`](../../Reference/agm/MagmControl.md)). If [`M_TRAIN_LABELS_NOISE_MODE`](../../Reference/agm/MagmControl.md) is set to [`M_LABELS_CONFIDENCE`](../../Reference/agm/MagmControl.md), labels that are hard to correctly classify, such as those mentioned above, will have less of an affect on the trained composite-definition model, preventing overfitting.

### Rotated labels

Training your composite-definition model using rotated labeled image regions increases the likelihood of finding occurrences at any angle. A composite-definition model trained without rotation is more likely to miss rotated occurrences. If you want your composite-definition model to be trained using rotated labeled image regions, you must enable this using [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md). When [`M_TRAIN_DETECTION_ANGLE`](../../Reference/agm/MagmControl.md) is disabled, calling [`MagmTrain`](../../Reference/agm/MagmTrain.md) with training images that contain rotated regions will generate an error.

## Training status

Once you have used [`MagmTrain`](../../Reference/agm/MagmTrain.md) to train the composite-definition model, you should retrieve the model's training status using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_STATUS`](../../Reference/agm/MagmGetResult.md).

If Aurora Imaging Library returns [`M_STATUS_TRAIN_OK`](../../Reference/agm/MagmGetResult.md), the composite-definition model was successfully trained; you can then copy it to a find AGM context, using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md), and use it to search for occurrences. If Aurora Imaging Library returns [`M_STATUS_TRAIN_FAILED`](../../Reference/agm/MagmGetResult.md) or [`M_STATUS_TRAIN_AUTO_LABELING_FAILED`](../../Reference/agm/MagmGetResult.md), the model was not successfully trained, and you cannot copy the model to a find AGM context.

### When a composite-definition model is not successfully trained

You must successfully train your composite-definition model before you can use it to search for occurrences. By default, AGM uses edgels to construct the trained composite-definition model. This means that the training will fail if no edgels are found within the blue (positive) rectangles of your training images. If this is the case, you should re-call [`MagmTrain`](../../Reference/agm/MagmTrain.md) after making the necessary modifications. For example, you can try:

- Modifying the positive labels on your training images to ensure that discernible model appearances are located within the blue rectangles. Aurora Imaging Library extracts edges by analyzing intensity transitions. If the regions within your blue rectangles do not contain intensity transitions, no edgels will be found. If your model is uniform in intensity, you should ensure that your blue rectangles encompass the entire model. This will ensure that the model's boundary is present for the edge detection.
- Modifying the smoothness value ([`M_SMOOTHNESS`](../../Reference/agm/MagmControl.md)). Smoothing the image reduces intensity transitions, which can obscure edges. If the smoothness level is too high, edges might disappear. Lowering the smoothness value can allow Aurora Imaging Library to find previously undetected edgels. Note that you should still specify a smoothness level high enough to remove unwanted edgels.
- Modifying the threshold mode ([`M_THRESHOLD_MODE`](../../Reference/agm/MagmControl.md)) to ensure that relevant edges are extracted. Aurora Imaging Library determines which edges are extracted from the training images based on a thresholding of the magnitude of the gradient at each edgel position. If there are no edges above the set threshold within the blue rectangles, no edgels will be found. Note that you should still use the highest possible threshold that detects all pertinent edgels. If there are contrast variations within or between images, such that there is no single threshold capable of detecting all pertinent edgels without introducing noisy background edges, you should avoid extracting edges and use the magnitude of the gradient instead. To do so, set [`MagmControl`](../../Reference/agm/MagmControl.md) with [`M_USE_MAGNITUDE_TARGET`](../../Reference/agm/MagmControl.md) to [`M_ENABLE`](../../Reference/agm/MagmControl.md).

## Using a trained composite-definition model

Once you have successfully trained a composite-definition model, you can copy the model to a find AGM context using [`MagmCopyResult`](../../Reference/agm/MagmCopyResult.md) with [`M_TRAINED_MODEL`](../../Reference/agm/MagmCopyResult.md). Before doing so, you can first evaluate the trained composite-definition model using [`MagmGetResult`](../../Reference/agm/MagmGetResult.md) with [`M_TRAINING_ERROR`](../../Reference/agm/MagmGetResult.md) and [`M_TRAINING_CONFIDENCE`](../../Reference/agm/MagmGetResult.md). The training error tells you the percentage of training image labels that were misclassified by the [`MagmTrain`](../../Reference/agm/MagmTrain.md) operation. A perfect training error of 0.0% means that the trained composite-definition model is able to detect all positively labeled positions without incorrectly detecting any negatively labeled positions. The training confidence tells you the difference between the detection scores at the positive positions and the detection scores at the negative positions. A good training confidence is 20.0% or higher. The higher the training confidence, the better the trained composite-definition model will be at discriminating true occurrences from false occurrences. If the training error is too high, or the training confidence is too low, you should adjust your training settings and re-train the model until you achieve good results. Note that the training confidence is only meaningful when the training error is 0.0%.

To further evaluate the trained model after you copy it to a find AGM context, you can:

- Draw the trained composite-definition model using [`MagmDraw`](../../Reference/agm/MagmDraw.md) with [`M_DRAW_TRAINED_MODEL`](../../Reference/agm/MagmDraw.md). If the drawing does not resemble your required model, you should modify the training images and/or modify the training settings and re-call [`MagmTrain`](../../Reference/agm/MagmTrain.md).
- Perform a search by following the steps outlined in [Finding steps](S02_Steps_to_train_a_model_and_find_occurrences.md). A false occurrence might be found if a background sample in your target and your model share too many features. If the search returns false occurrences, you can identify them in your training images using negative (red) labels and re-call [`MagmTrain`](../../Reference/agm/MagmTrain.md). This will remove the unfavorable features from the composite-definition model. You can do this repeatedly until no false occurrences are found.

When your composite-definition model is adequately trained, you can use it to search for occurrences in your target. For more information on searching for occurrences, see [Finding model occurrences](S05_Finding_model_occurrences.md).
