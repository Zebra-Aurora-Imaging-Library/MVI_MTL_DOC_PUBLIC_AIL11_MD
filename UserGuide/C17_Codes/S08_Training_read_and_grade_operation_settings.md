---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Training_read_and_grade_operation_settings
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Training read and grade operation settings"
---

# Training read and grade operation settings

Once you have allocated a code context and added code models of the appropriate type to it, you can try reading or grading the code occurrences in your images using the default control type settings. For a more robust [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, you might need to customize the control type settings of the context and the code models so that they are optimized for your target images. The fastest way to set the most commonly adjusted control types is to train them using [`McodeTrain`](../../Reference/code/McodeTrain.md) with a sample set of your images. [`McodeTrain`](../../Reference/code/McodeTrain.md) establishes the optimal settings based on the code occurrences found in all the training images. If further adjustment is still needed, see [Customizing read and grade operation settings](S09_Customizing_read_operation_settings.md).

The following steps provide a basic methodology to train the control type settings of your context and its code models:

1. Allocate a training result buffer to hold the recommended settings, using [`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md).
2. Activate the control types to train. You can choose to activate all or some of the trainable control types, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md) or using [`McodeControl`](../../Reference/code/McodeControl.md) with the control type to activate combined with [`M_TRAIN`](../../Reference/code/McodeControl.md), respectively. Note that if [`M_TIMEOUT`](../../Reference/code/McodeControl.md)is activated, it might affect other trained control types.
3. Call[`McodeTrain`](../../Reference/code/McodeTrain.md) with the code context to train, the training result buffer, and a sample set of your images.
4. Ensure that the train operation was successful using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md). If successful, it should return [`M_STATUS_TRAIN_OK`](../../Reference/code/McodeGetResult.md).
5. Reconfigure a code context with the training results, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md).
6. Use the configured code context when calling [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md).

## Activating the control types to train

Typically, you activate all the trainable control types using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md) set to [`M_DEFAULT`](../../Reference/code/McodeControl.md). If required, you can then disable the activation of a few control types that you don't want trained. To do so, call [`McodeControl`](../../Reference/code/McodeControl.md) with the control type to deactivate combined with [`M_TRAIN`](../../Reference/code/McodeControl.md) and set the control value to [`M_DISABLE`](../../Reference/code/McodeControl.md). For example, if you know that your code occurrences will never appear rotated, you can call [`McodeControl`](../../Reference/code/McodeControl.md) with[`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) + [`M_TRAIN`](../../Reference/code/McodeControl.md) and set the control value to [`M_DISABLE`](../../Reference/code/McodeControl.md).

## Setting a code context from trained results

[`McodeTrain`](../../Reference/code/McodeTrain.md) saves its results in the specified code train result buffer. Typically, you reconfigure a code context with these training results, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_RESET_FROM_TRAINED_RESULTS`](../../Reference/code/McodeControl.md). This control type discards any existing code models from the context, and then adds the code models used for training to the context. If a control type was not trained, it is set to its value used during training; if a control type was trained, it is set to its trained value.

If required, you can retrieve the recommended setting for a specific control type using [`McodeGetResult`](../../Reference/code/McodeGetResult.md). This is especially useful if you want to reconfigure the code context that was used during training. In this case, you can use the retrieved values to selectively change the control types of the context to their trained value. Note that some control types are inter-dependent; changing the setting of one control type to its trained value might not be optimal if the settings of other control types are not also changed. This is especially true for control types that are automatically activated for training if another control type is activated (for example, [`M_CELL_NUMBER_...`](../../Reference/code/McodeControl.md)).

## Training images and verifying the training results

The recommended control type settings for a code context and its code models are based on the code occurrences found in all the training images. Each training image must have at least one occurrence of one of the code models. You can retrieve the buffer identifiers of the training images in which a code occurrence could not be read, using [`M_FAILED_IMAGES_ID`](../../Reference/code/McodeGetResult.md). For the most robust training results, provide images of all the different situations in which you will need to read or grade code occurrences of the code models during the critical loop.

You can retrieve or draw the results of the read operation that [`McodeTrain`](../../Reference/code/McodeTrain.md) internally performed on each training image. You can use this extra information to complement the status result. First, call [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_CODE_RESULT_ID`](../../Reference/code/McodeGetResult.md). This returns the identifiers of the internal code read result buffers; there is one per training image. You can then pass one of these result buffer identifiers to [`McodeGetResult`](../../Reference/code/McodeGetResult.md), and retrieve any type of result available for an [`McodeRead`](../../Reference/code/McodeRead.md) operation. You can also pass one of the result buffer identifiers to [`McodeDraw`](../../Reference/code/McodeDraw.md) to visualize the results for a specific training image.

You can retrieve detailed information about the code models that were enabled for training, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with the [`M_TRAIN_...`](../../Reference/code/McodeGetResult.md) result types. For example, [`M_TRAIN_ENABLED_CONTROL_TYPES`](../../Reference/code/McodeGetResult.md) retrieves a list of control types that were enabled for each context or model.

If [`McodeTrain`](../../Reference/code/McodeTrain.md) doesn't find at least one occurrence of one of the code models among the sample images, [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with[`M_STATUS`](../../Reference/code/McodeGetResult.md) returns [`M_STATUS_TRAIN_FAILED`](../../Reference/code/McodeGetResult.md).

## An example

The example _CodeTrain.cpp_ shows how to train control type settings using [`McodeTrain`](../../Reference/code/McodeTrain.md) and how retrieve some of the results obtained from the internal read operations for display purposes. To run this example, use Aurora Imaging Example Launcher.
