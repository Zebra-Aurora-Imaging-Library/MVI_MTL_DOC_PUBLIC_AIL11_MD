---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Geometric_Model_Finder
section: Calibration
module_tag: mod
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / model-finder / Calibration"
---

# Camera calibration

The Model Finder module supports camera calibration. If the target is calibrated, matching is performed and results are calculated in the world coordinate system, compensating for any image distortion. Camera calibration is performed transparently, without the need for any setting adjustments.

## Requirements

To search in a calibrated target, all models in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) type of Model Finder context must also be calibrated. To calibrate image-type, result-type, and merge-type models, use calibrated model sources or associate a camera calibration context with the models, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md). Note that defining models from calibrated model sources and adjusting individual model settings is done in pixels. Synthetic models in these types of contexts can be used with a calibrated target if defined in real-world units and associated with the camera calibration context of the target.

The models in an[`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context can be associated with different camera calibration contexts; however, you should ensure that all camera calibration contexts use the same absolute coordinate system (otherwise, results will be skewed). Targets must also be calibrated using the same absolute coordinate system as the models in an [`M_GEOMETRIC`](../../Reference/mod/MmodAlloc.md) or [`M_GEOMETRIC_CONTROLLED`](../../Reference/mod/MmodAlloc.md) context.

For a synthetic model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context, the model is considered to have the same units as the target. If the target is calibrated, the model will be interpreted in world units. Conversely, if the target does not have a camera calibration context associated with it, the model will be interpreted in pixel units. The model will be defined in the same coordinate system as the target. You cannot associate a calibration context with the model.

Targets can be either physically corrected or not, without affecting the robustness of the search.

Targets can be grabbed from any number of cameras, each with its own camera calibration context, as long as the same absolute coordinate system has been used. For example, if a model has been extracted from a model source image taken using camera A, the model can be used to locate occurrences in a target image grabbed by camera B or camera C (whether or not the image has been physically transformed) since the same absolute coordinate system has been used to calibrate all images. Aurora Imaging Library will transparently convert between camera calibration contexts.

*[Image: Calibration.png]*

If the target is calibrated, results can be retrieved in real-world or pixel units. Note, however, in the presence of distortion some results are meaningless when converted from real-world to pixel units (for example, angle, scale, and transformation coefficient results). For example, if a model appears warped in the target, but the camera calibration context of the target compensates for this during the model search, the resulting angle is meaningful in the real-world coordinate system, and meaningless in the pixel coordinate system.

When using [`MmodDraw`](../../Reference/mod/MmodDraw.md) to draw features from a calibrated model or results from a calibrated target, the destination drawing buffer must also be calibrated, using the same absolute coordinate system, otherwise annotations will be skewed. Aurora Imaging Library transparently takes into account the camera calibration when drawing these annotations.

## Models and their camera calibration context

Camera calibration contexts associated with the models ([`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md)) can be saved with the Model Finder context using [`MmodSave`](../../Reference/mod/MmodSave.md) or [`MmodStream`](../../Reference/mod/MmodStream.md) with [`M_WITH_CALIBRATION`](../../Reference/mod/MmodSave.md). In this case, you must also use [`M_WITH_CALIBRATION`](../../Reference/mod/MmodRestore.md) when restoring the context ([`MmodRestore`](../../Reference/mod/MmodRestore.md) or [`MmodStream`](../../Reference/mod/MmodStream.md)). If you do not save/restore the camera calibration contexts, you must re-associate them to each model after restoring the context. Note that the camera calibration contexts associated with synthetic models are not saved or restored by [`MmodSave`](../../Reference/mod/MmodSave.md), [`MmodRestore`](../../Reference/mod/MmodRestore.md), or [`MmodStream`](../../Reference/mod/MmodStream.md).

When you use [`M_WITH_CALIBRATION`](../../Reference/mod/MmodSave.md), the camera calibration is saved or restored in/from the same file as the Model Finder context and cannot be managed independently from the context. When the context is freed, the camera calibrations are automatically freed as well.

If necessary, you can disassociate a camera calibration context from a model, using [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASSOCIATED_CALIBRATION`](../../Reference/mod/MmodControl.md) set to [`M_NULL`](../../Reference/mod/MmodControl.md). This can be useful if you want to use a calibrated model with an uncalibrated target.

## Setting the aspect ratio control

When dealing with aspect ratio distortion, you can use [`MmodControl`](../../Reference/mod/MmodControl.md) with [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) to compensate for the distortion, if the target is not calibrated. This will allow you to avoid using a camera calibration context during the search. This will not compensate for aspect ratio distortion in the model, so this method is mostly useful when searching for synthetic models in a target that has aspect ratio distortion. This is not supported for a model in an [`M_SHAPE_...`](../../Reference/mod/MmodAlloc.md) type of Model Finder context.

When performing the search, results will be returned for the corrected target coordinate system. The results will be converted to the original target coordinate system if you draw them using [`MmodDraw`](../../Reference/mod/MmodDraw.md).

## Using transformation coefficients with calibrated images

You can use forward and reverse transformation coefficients if you want to map points of interest other than the reference axis origin (see [Forward and reverse transformation coefficients](S11_Customizing_search_settings.md)). When using a calibrated model and a calibrated target, forward and reverse transformation coefficients (see [`MmodGetResult`](../../Reference/mod/MmodGetResult.md)) are given for the real world. This means that they can be used to convert any coordinates in the model world coordinate system to the corresponding coordinates in the target world coordinate system for an occurrence or vice versa. To get the forward or reverse transformation coefficients, use [`MmodGetResult`](../../Reference/mod/MmodGetResult.md) with [`M_..._FORWARD`](../../Reference/mod/MmodGetResult.md) or with [`M_..._REVERSE`](../../Reference/mod/MmodGetResult.md), respectively. Apply the transformation coefficients to the coordinates using the following equations:

`_x_ <sub>d</sub> = _A_ _x_ <sub>s</sub> + _B_ _y_ <sub>s</sub> + _C_`

`_y_ <sub>d</sub> = _E_ _x_ <sub>s</sub> + _F_ _y_ <sub>s</sub> + _D_`

For models that do not have an [`M_ASPECT_RATIO`](../../Reference/mod/MmodControl.md) result type, note the following:

`_E_ = -_B_`

`_F_ = _A_`

where `A, B, C, D, E, and F` are the transformation coefficients (forward or reverse). `x <sub>s</sub> and y<sub> s</sub>` specify the source coordinates (with respect to the origin of the model coordinate system for a forward transformation or target coordinate system for a reverse transformation); `x <sub>d</sub> and y<sub> d</sub>` are the destination coordinates (with respect to the origin of the target coordinate system for a forward transformation or model coordinate system for a reverse transformation).

Once the transformation coefficients have been applied to the coordinates, you can convert the real-world coordinates back to pixel coordinates using [`McalTransformCoordinate`](../../Reference/cal/McalTransformCoordinate.md). These coordinates can be used to determine equivalent pixel positions in the model image and target image. After the transformation, you can draw the point at the equivalent positions in the model image and the target image using one of the [`Mgra...`](../../Reference/gra/MgraText.md) functions. Note that the [`Mgra...`](../../Reference/gra/MgraAlloc.md) functions can also take the coordinates of the point in world units if you set [`MgraControl`](../../Reference/gra/MgraControl.md) with [`M_INPUT_UNITS`](../../Reference/gra/MgraControl.md) to [`M_WORLD`](../../Reference/gra/MgraControl.md).

See the image below for an example of a forward transformation.

*[Image: TransCoeff.png]*
