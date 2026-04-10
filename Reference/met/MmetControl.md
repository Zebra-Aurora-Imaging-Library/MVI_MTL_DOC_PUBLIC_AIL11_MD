---
doctype: Reference
module: met
function: MmetControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / met / MmetControl"
---

# MmetControl

| Board | Supported |
| --- | --- |
| Host System | Yes |
| V4L2 | Yes |
| Clarity UHD | Yes |
| Concord PoE | No |
| GenTL | Yes |
| GevIQ | Yes |
| GigE Vision | Yes |
| Indio | No |
| Iris GTX | Yes |
| Radient eV-CL | Yes |
| Rapixo CL | Yes |
| Rapixo CoF | Yes |
| Rapixo CXP | Yes |
| USB3 Vision | Yes |

> Control a setting for a metrology context, a derived metrology region object, features, tolerances, or a result buffer.

## Syntax

```c
void MmetControl(
    AIL_ID     MetId,         //out
    AIL_INT    LabelOrIndex,  //in
    AIL_INT64  ControlType,   //in
    AIL_DOUBLE ControlValue   //in
)
```

## Description

This function sets the specified control for a metrology context, a derived metrology region object, features, tolerances, or a result buffer. These settings control the execution of [`MmetCalculate`](../../Reference/met/MmetCalculate.md), and affect how results are retrieved by [`MmetGetResult`](../../Reference/met/MmetGetResult.md). You can also use this function to specify whether features can be drawn with [`MmetDraw`](../../Reference/met/MmetDraw.md). Most of the control type settings in [`MmetControl`](../../Reference/met/MmetControl.md) can be inquired using [`MmetInquire`](../../Reference/met/MmetInquire.md).

If a camera calibration context is associated with the target image, set values in real-world units (for example, tolerance values, warning values, and positional values). Otherwise, use pixel units.

## Parameters

### `MetId` *(out, AIL_ID)*

Specifies the identifier of the metrology context, the derived metrology region object, or the result buffer whose settings you want to control. The metrology context or the derived metrology region object must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md). The metrology result buffer must have been previously allocated on the required system using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies to control the metrology context, the derived metrology region object, features, tolerances, or the result buffer. Set this parameter to one of the following values.

*For specifying what to control*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the index value of an existing individual feature. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing individual feature. |
| `M_TOLERANCE_INDEX` | Specifies the index value of an existing individual tolerance. |
| `M_TOLERANCE_LABEL` | Specifies the label value of an existing individual tolerance. |
| `M_ALL_FEATURES` | Applies the specified control setting to all features. This value should only be used with [`M_DELETE`](../../Reference/met/MmetControl.md). |
| `M_ALL_TOLERANCES` | Applies the specified control setting to all tolerances. |
| `M_CONTEXT` *(default)* | Controls a setting of the metrology context, which has been set using the [`MetId`](../../Reference/met/MmetControl.md) parameter. |
| `M_DERIVED_GEOMETRY_REGION` | Controls a setting of the derived metrology region object, which has been set using the [`MetId`](../../Reference/met/MmetControl.md) parameter. |
| `M_GENERAL` | Controls a setting of a metrology result buffer. The setting is applied to all feature and tolerance results in the metrology result buffer. When using [`M_GENERAL`](../../Reference/met/MmetControl.md), [`MetId`](../../Reference/met/MmetControl.md) must specify a metrology result buffer. |
| `M_GLOBAL_FRAME` | Applies the specified control setting to the global frame of the context. In this case, [`MetId`](../../Reference/met/MmetControl.md) must specify a metrology context. |
| `M_MEASURED_FEATURES` | Applies the specified control setting to all measured features. In this case, [`MetId`](../../Reference/met/MmetControl.md) must specify a metrology context. |

### `ControlType` *(in, AIL_INT64)*

Specifies the type of control to set.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the required value for the control.

## Parameter Associations

### For controlling general context settings

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control general metrology context settings. For controlling general context settings, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter must be set to [`M_CONTEXT`](../../Reference/met/MmetControl.md) and the [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_ASSOCIATED_CALIBRATION`

Associates the specified camera calibration context with the template reference of the metrology context. To specify the template reference, use [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_TEMPLATE_REFERENCE_ID`](../../Reference/met/MmetControl.md).  When saving the metrology context, if you do not save the camera calibration context associated with the template reference ([`MmetSave`](../../Reference/met/MmetSave.md) or [`MmetStream`](../../Reference/met/MmetStream.md) with [`M_WITH_CALIBRATION`](../../Reference/met/MmetSave.md)), you must re-associate the template reference with its camera calibration context after restoring the context ([`MmetRestore`](../../Reference/met/MmetRestore.md) or [`MmetStream`](../../Reference/met/MmetStream.md)). Note that you can also associate a camera calibration context to the template reference using other Aurora Imaging Library mechanisms ( [`Mcal...`](../../Reference/cal/McalAssociate.md)); in this case, [`M_ASSOCIATED_CALIBRATION`](../../Reference/met/MmetControl.md) is ignored.  > **Note:** You cannot associate the template reference with a calibrated context whose Y-axis has been inverted using [`McalGrid`](../../Reference/cal/McalGrid.md) with [`M_Y_AXIS_COUNTER_CLOCKWISE`](../../Reference/cal/McalGrid.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Removes the association to the camera calibration. |
| `Calibration context identifier` | Specifies the identifier of the camera calibration context to associate with the template reference of the metrology context. |

---

### `M_TEMPLATE_REFERENCE_ID`

Associates a template reference to the context. The template reference is a companion buffer (typically an image) useful for training the metrology context and drawing the results on a typical target image.  Note this control type creates an internal copy of the buffer associated with the metrology context. Freeing the buffer passed to [`MmetControl`](../../Reference/met/MmetControl.md) (using [`MbufFree`](../../Reference/buf/MbufFree.md)) will not effect this internal copy.

| Value | Description |
| --- | --- |
| `M_NULL` | Releases the template reference buffer. |
| `Image identifier` | Specifies the identifier of the buffer. |

---

### `M_TIMEOUT`

Sets the maximum measurement and validation time for [`MmetCalculate`](../../Reference/met/MmetCalculate.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of measurement and validation time. |
| `Value > 0.0` | Specifies the maximum measurement and validation time, in msec. |

### For controlling global processing settings

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control processing settings for a measured feature. For controlling processing settings, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to [`M_MEASURED_FEATURES`](../../Reference/met/MmetControl.md) or an existing measured feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_CHAIN_ALL_NEIGHBORS`

Sets whether edge chains are built with as much edgel information as possible. An edge chain refers to the set of connected edgels that form an edge. Edge chains are built using an 8-connected lattice.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that edge chains are built with the least amount of edgel information possible.  *[Image: chainneighborsdisable.png]* |
| `M_ENABLE` *(default)* | Specifies that edge chains are built with as much edgel information as possible.  *[Image: chainneighborsenable.png]*  Enabling [`M_CHAIN_ALL_NEIGHBORS`](../../Reference/met/MmetControl.md) can result in greater accuracy, since edge chains can contain more information, though calculations can take slightly longer. |

---

### `M_EXTRACTION_SCALE`

Sets the scale of the image at which to do the edge extraction. Once the extraction is complete, the results are scaled to the original scale of the image.  An extraction scale less than one can speed calculations but can be less reliable. The default extraction scale setting is typically sufficient.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the extraction scale. |

---

### `M_FILL_GAP_ANGLE`

Sets the aperture angle where the feature extraction searches for edgel extremity candidates when filling gaps in features. That is, [`M_FILL_GAP_ANGLE`](../../Reference/met/MmetControl.md), along with [`M_FILL_GAP_DISTANCE`](../../Reference/met/MmetControl.md), define a region where two chain extremities can be linked. Note that two chain extremities can only be linked if both are included in the search region of the other.  For more information on filling gaps, see[Filling the edge gaps](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the aperture angle, in degrees. |

---

### `M_FILL_GAP_DISTANCE`

Sets the maximum distance radius where the feature extraction searches for edgel extremity candidates when filling gaps in features. That is, [`M_FILL_GAP_DISTANCE`](../../Reference/met/MmetControl.md), along with [`M_FILL_GAP_ANGLE`](../../Reference/met/MmetControl.md), define a region where two chain extremities can be linked. Note that two chain extremities can only be linked if both are included in the search region of the other.  For more information on filling gaps, see[Filling the edge gaps](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. Typically, the default value is 0.0 pixels, however for an [`M_MEASURED`](../../Reference/met/MmetAddFeature.md) [`M_POINT`](../../Reference/met/MmetAddFeature.md) feature, the default value is 3.0 pixels. |
| `M_INFINITE` | Specifies an infinite maximum distance radius. |
| `Value >= 0.0` | Specifies the maximum distance radius, in pixels. Note that generally, you should not set the distance to a large value. Typically, you would set [`M_FILL_GAP_DISTANCE`](../../Reference/met/MmetControl.md) to a value in the range of 2 to 5 pixels. |

---

### `M_FILTER_SMOOTHNESS`

Sets the degree of smoothness (strength of denoising) applied by the edge extraction filter during the neighborhood operation.  The smoothing operation evens out rough edges and reduces noise; in effect, the smoothness factor affects which edges are extracted. For more information on smoothing edges, see [Smoothing for IIR filters](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value.  A value of 100.0 results in a strong noise reduction effect, while a value of 0.0 has almost no noise reduction effect. |

---

### `M_FILTER_TYPE`

Sets the type of filter used to extract edges. The type of filter determines the distribution of the neighborhood's influence. It establishes the contribution of each neighboring pixel to the edge calculation.  For more information on filtering, see [Filter type](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DERICHE` | Specifies a Deriche infinite support filter. |
| `M_FREI_CHEN` | Specifies a Frei Chen filter. |
| `M_PREWITT` | Specifies a Prewitt filter. |
| `M_SHEN` *(default)* | Specifies a Shen-Castan infinite support exponential filter. |
| `M_SOBEL` | Specifies a Sobel filter. |

---

### `M_FLOAT_MODE`

Sets whether to perform all edge extraction operations using floating-point precision calculations.  For more information on using floating-point precision when extracting edges, see [Calculating and retrieving results](../../UserGuide/C10_Edge_Finder/S07_Calculating_and_retrieving_results.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edge extractions are not forced to be performed using floating-point precision calculations. |
| `M_ENABLE` | Specifies that all edge extractions are forced to be performed using floating-point precision calculations. |

---

### `M_MAGNITUDE_TYPE`

Sets how to calculate the magnitude of the edge at each edgel position.  For more information on the magnitude, see [Magnitude type](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORM` | Specifies that the magnitude will be used. |
| `M_SQR_NORM` *(default)* | Specifies that the square of the magnitude will be used. |

---

### `M_REGION_ACCURACY_HIGH`

Sets whether to use high accuracy to define the metrology region associated with a feature, when dealing with a calibrated image. When using high accuracy, the metrology region closely follows the image distortion; when using low accuracy, the Metrology module approximates the distortion. For example, a rectangular metrology region remains a rectangle and a circular metrology region becomes the best approximated elliptic region.  [`M_REGION_ACCURACY_HIGH`](../../Reference/met/MmetControl.md) also applies when drawing the metrology region, using [`MmetDraw`](../../Reference/met/MmetDraw.md) with [`Operation`](../../Reference/met/MmetDraw.md) set to [`M_DRAW_REGION`](../../Reference/met/MmetDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that high accuracy is not used when defining a metrology region. This value is recommended when the image distortion of the calibrated image is minor. This typically results in a quicker processing time. |
| `M_ENABLE` *(default)* | Specifies that high accuracy is used when defining a metrology region. |

---

### `M_THRESHOLD_MODE`

Sets the threshold of the edge extraction.  For more information on thresholding edges, see [Thresholding](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies no threshold. All edges are extracted. |
| `M_HIGH` *(default)* | Specifies a high threshold. [`M_HIGH`](../../Reference/met/MmetControl.md) should be used for images with some contrast variations, noise, and non-uniform illumination. [`M_HIGH`](../../Reference/met/MmetControl.md) always results in a lower (or equal) number of edgels than [`M_MEDIUM`](../../Reference/met/MmetControl.md). |
| `M_LOW` | Specifies a low threshold. All edges over a minimum noise-based estimated threshold are extracted. [`M_LOW`](../../Reference/met/MmetControl.md) always results in a lower (or equal) number of edgels than [`M_DISABLE`](../../Reference/met/MmetControl.md). |
| `M_MEDIUM` | Specifies a medium threshold. [`M_MEDIUM`](../../Reference/met/MmetControl.md) should be used for multi-contrast images, or for images with a lot of noise or non-uniform illumination. [`M_MEDIUM`](../../Reference/met/MmetControl.md) always results in a lower (or equal) number of edgels than [`M_LOW`](../../Reference/met/MmetControl.md). |
| `M_USER_DEFINED` | Specifies that the threshold values will be user-defined. Set the threshold values with [`M_THRESHOLD_VALUE_LOW`](../../Reference/met/MmetControl.md) and [`M_THRESHOLD_VALUE_HIGH`](../../Reference/met/MmetControl.md). |
| `M_VERY_HIGH` | Specifies a very high threshold. Only the strongest edges in the image are extracted. [`M_VERY_HIGH`](../../Reference/met/MmetControl.md) always results in a lower (or equal) number of edgels than [`M_HIGH`](../../Reference/met/MmetControl.md). |

---

### `M_THRESHOLD_TYPE`

Sets the type of hysteresis threshold used when performing the edge extraction.  Edge chains are built such that the magnitude values of all connected edgels are stronger than a lower bound threshold value and such that at least one of the edgel's magnitude in each edge chain is stronger than a upper bound threshold value.  Note that [`M_THRESHOLD_TYPE`](../../Reference/met/MmetControl.md) sets how to use the lower and upper threshold bounds, while [`M_THRESHOLD_MODE`](../../Reference/met/MmetControl.md) defines what the bounds are.  For more information on thresholding, see [Thresholding](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FULL_HYSTERESIS` | Specifies that the lower bound threshold value is 0.0. |
| `M_HYSTERESIS` *(default)* | Specifies that both the lower bound threshold value and the upper bound threshold value will be used. |
| `M_NO_HYSTERESIS` | Specifies that the lower bound threshold value is equal to the upper bound threshold value. |

---

### `M_THRESHOLD_VALUE_HIGH`

Sets the upper bound of the hysteresis threshold value. [`M_THRESHOLD_VALUE_HIGH`](../../Reference/met/MmetControl.md) is ignored unless [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_THRESHOLD_MODE`](../../Reference/met/MmetControl.md) is set to [`M_USER_DEFINED`](../../Reference/met/MmetControl.md). Note that lower threshold values result in a more sensitive edgel detection; that is, a higher (or equal) number of edgels are detected.  For more information on thresholding, see [Thresholding](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the upper bound of the hysteresis threshold. |

---

### `M_THRESHOLD_VALUE_LOW`

Sets the lower bound of the hysteresis threshold value. [`M_THRESHOLD_VALUE_LOW`](../../Reference/met/MmetControl.md) is ignored unless [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_THRESHOLD_MODE`](../../Reference/met/MmetControl.md) is set to [`M_USER_DEFINED`](../../Reference/met/MmetControl.md). Note that lower threshold values result in a more sensitive edgel detection; that is, a higher (or equal) number of edgels are detected.  For more information on thresholding, see [Thresholding](../../UserGuide/C10_Edge_Finder/S05_Customizing_the_edge_extraction_settings.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the lower bound of the hysteresis threshold. |

### For controlling features

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control how the features are calculated. For controlling features, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to the following: [`M_GLOBAL_FRAME`](../../Reference/met/MmetControl.md) or an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_ACTIVE`

Sets whether to enable or disable the feature for calculation.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the feature. In this case, the feature is not calculated and the status of the feature ([`M_STATUS`](../../Reference/met/MmetGetResult.md)) will return [`M_FAIL`](../../Reference/met/MmetGetResult.md). This is also the case for the status of all the derived features and tolerances, if any. |
| `M_ENABLE` | Specifies to calculate the feature. This is the default value. |

---

### `M_ANGLE_END`

Sets the end angle of a constructed arc. The angle is relative to the reference frame's X-axis and follows the counter-clockwise convention.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ANGLE_START`

Sets the start angle of a constructed arc. The angle is relative to the reference frame's X-axis and follows the counter-clockwise convention.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_EDGEL_ANGLE_RANGE`

Sets the angular range within which an edgel's gradient angle must fall for Aurora Imaging Library to consider it an active edgel. Active edgels apply to physically measured features and constructed features based on physically measured edgels or based on external edgels. Note that this control type is ignored for points (edgels with no angle information) in an external edgel feature.  By default, Aurora Imaging Library measures the specified angular range, in the counter clockwise direction, relative to the metrology ROI's orientation. To change the relative angle from which to measure the angular range, use [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md).  In the following examples, the angular range is 60.0° and the relative angle is the same as the region's orientation. Note that for the ring-sector region, the orientation is radial.  *[Image: MetEdgelAngleRange.png]*  Note that constructed edgel features are not built from a region, but from a specified measured feature. Therefore in this case, the region's orientation refers to the orientation of the region from which its specified measured feature was built.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angular range, in degrees. |

---

### `M_EDGEL_DENOISING_MODE`

Sets how to denoise edgels. Denoising refers to establishing and eliminating noisy (unwanted) edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies no denoising. |
| `M_GAUSSIAN` | Specifies a Gaussian type of denoising. |
| `M_MEAN` | Specifies a mean type of denoising. |
| `M_MEDIAN` | Specifies a median type of denoising. |

---

### `M_EDGEL_DENOISING_RADIUS`

Sets the radius by which to denoise edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the radius, in pixels. |

---

### `M_EDGEL_DENOISING_USE_ORDER`

Sets whether Aurora Imaging Library should use and maintain the order of the edgels when denoising them.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library should not use and maintain the order of the edgels when denoising them. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library should use and maintain the order of the edgels when denoising them. This can improve the speed and accuracy of the denoising. This also ensures that the denoising itself does not reorder the edgels; keeping the original order can be useful for future operations. |

---

### `M_EDGEL_RELATIVE_ANGLE`

Sets the relative angle from which to measure the gradient angle range ([`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md)). You can use [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md), along with [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md), to select the active edgels of a physically measured feature or the active edgels of a constructed feature based on physically measured edgels or based on external edgels. Typically, the relative angle is the same as the region's orientation. For an example on how [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md) is applied, along with the gradient angle range, see [`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md). Note that this control type is ignored for points (edgels with no angle information) in an external edgel feature.  Note that constructed edgel features are not built from a region, but from a specified measured feature. Therefore in this case, the region's orientation refers to the orientation of the region from which its specified measured feature was built.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies that the gradient angle range is measured relative to the reverse angle (orientation) of the region of interest (+ 180°). |
| `M_SAME` *(default)* | Specifies that the gradient angle range is measured relative to the same angle (orientation) as the region of interest. |
| `M_SAME_OR_REVERSE` | Specifies that the gradient angle range is measured relative to either the same or the reverse angle (orientation) of the region of interest. |

---

### `M_EDGEL_SELECTION_RANK`

Sets the edgels to select relative to the orientation of the metrology region. The metrology region is scanned column by column, and the specified edgel encountered within a column is selected. Note that this control type takes effect after [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md) and[`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) are satisfied. [`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md) applies to [`M_RECTANGLE`](../../Reference/met/MmetControl.md) derived metrology regions.  In the following example, there are multiple edges in the metrology region. The solid gray line illustrates the edgels selected when[`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md) is set to 1; the dotted gray line illustrates the edgels selected when[`M_EDGEL_SELECTION_RANK`](../../Reference/met/MmetControl.md) is set to 2.  *[Image: EdgeSelectionRank.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edgels are selected. |
| `M_LAST` | Selects the last edgel in each column, relative to the metrology region. |
| `Value > 0` | Specifies which edgels to select, based on their rank, relative to the metrology region. |

---

### `M_EDGEL_TYPE`

Sets the type of edgels to use when building constructed edgel features from a measured base feature using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_COPY_FEATURE_EDGELS`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACTIVE_EDGELS` *(default)* | Specifies to use the active edgels of the measured feature's metrology region. Active edgels must satisfy all edgel constraints ([`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) and [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md)). |
| `M_ALL_EDGELS` | Specifies to use all the edgels of the measured feature's metrology region. |
| `M_FITTED_EDGELS` | Specifies to use the fitted edgels of the measured feature's metrology region. The fitted edgels are those active edgels used by the fit operation to define a measured feature. Fitted edgels must satisfy all edgel constraints ([`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetControl.md) and [`M_EDGEL_RELATIVE_ANGLE`](../../Reference/met/MmetControl.md)) and fit constraints ([`M_FIT_...`](../../Reference/met/MmetControl.md)). |

---

### `M_FIT_COVERAGE_MIN`

Sets the approximate portion of the feature that must be covered by fitted edgels. In the following example, about 50.0% of the segment feature covers the edgels.  *[Image: MetFitCoverageMin.png]*  The actual coverage that results from this setting depends on the scale of the image at which the feature edges were extracted, as specified with [`M_EXTRACTION_SCALE`](../../Reference/met/MmetControl.md); a scale of 1 provides the best estimation.  [`M_FIT_COVERAGE_MIN`](../../Reference/met/MmetControl.md) is valid for all fit operations for physically measured or constructed features. For constructed features using a fit operation performed on multiple physically measured edgel features, the accuracy of the minimum fit coverage is based on the average of each feature's extraction scale.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum coverage, as a percentage. |

---

### `M_FIT_DISTANCE_MAX`

Sets the greatest possible gap between an active edgel and an iterative fit for the edgel to be considered during the next iteration. The higher the value, the farther away an active edgel can be. [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md) is valid for all fit operations for physically measured or constructed features.  You can specify [`M_FIT_DISTANCE_MAX`](../../Reference/met/MmetControl.md) when [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) is set to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum distance. |
| `Value` | Specifies the maximum distance, in pixel units. |

---

### `M_FIT_DISTANCE_OUTLIERS`

Sets whether to automatically determine the distance that establishes outliers (edgels/points), or to use a user-defined distance. Outliers are excluded from the robust best fit. This setting only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically determines the distance. |
| `M_USER_DEFINED` | Specifies that the distance is set with [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md). |

---

### `M_FIT_DISTANCE_OUTLIERS_VALUE`

Sets an explicit distance value that Aurora Imaging Library uses to establish outliers (outlier edgels/points), which are excluded from the robust best fit. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md), and [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetControl.md) must be set to [`M_USER_DEFINED`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the distance; value must be greater than 0.0. |

---

### `M_FIT_INTERNAL_NUMBER_OF_POINTS`

Sets the minimum number of points from which Aurora Imaging Library performs the robust fit to build the feature. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically determines the number of points. For a segment, Aurora Imaging Library uses 2 points. For a circle or arc, Aurora Imaging Library uses 3. |
| `Value` | Specifies the number of points. Less points typically result in faster fit calculations. For a segment, there is a 2 point minimum, and for a circle or arc, there is a 3 point minimum. |

---

### `M_FIT_ITERATIONS_MAX`

Sets the maximum number of fit iterations used to compute the feature. The more iterations, the better the fit, but the slower the calculation. [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md) is valid for all fit operations for physically measured or constructed features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum number of fit iterations will be automatically determined. |
| `Value >= 1` | Specifies the maximum number of fit iterations. Only integer values are accepted. For an [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md), a setting of one will consider all edgels/points in the fit. Settings higher than one will progressively eliminate outlying edgels (outlier edgels/points) in the fit. |

---

### `M_FIT_TYPE`

Sets the type of best fit with which to build the feature. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROBUST_FIT` | Specifies a robust best fit. This can be effective when dealing with outliers (outlier edgels/points). When you use a robust best fit, you can further control how the feature is built, by specifying [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetControl.md), [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetControl.md), [`M_FIT_INTERNAL_NUMBER_OF_POINTS`](../../Reference/met/MmetControl.md), and [`M_FIT_ITERATIONS_MAX`](../../Reference/met/MmetControl.md).  This is not a valid control value for features of type [`M_POINT`](../../Reference/met/MmetAddFeature.md) or [`M_EDGEL`](../../Reference/met/MmetAddFeature.md). |
| `M_STANDARD_FIT` *(default)* | Specifies a standard best fit. In this case, more importance is typically given to the edgels whose gradient orientation follows the region's orientation. However, when adding a segment feature in an [`M_INFINITE`](../../Reference/met/MmetControl.md) metrology region with an [`M_RADIAL`](../../Reference/met/MmetControl.md) orientation, or in an [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) metrology region, more importance is given to the edgels whose gradient orientation is more perpendicular to the radial direction. |

---

### `M_FIT_VARIATION_MAX`

Sets the maximum allowable difference in the value of the feature's underlying coefficients, from one fit iteration to the next. If at least one coefficient of the feature varies more, between two consecutive fit iterations, than the specified value, the iteration process will continue. Note that:  - An arc feature has 5 coefficients: the X-coordinate of its center, the Y-coordinate of its center, its radius, its start angle, and its end angle. - A circle feature has 3 coefficients: the X-coordinate of its center, the Y-coordinate of its center and its radius. - A segment feature has 4 coefficients: the X-coordinate of its start point, the Y-coordinate of its start point, the X-coordinate of its end point, and the Y-coordinate of its end point.  These coefficients are the same as those required when building the corresponding features with a parametric operation.  [`M_FIT_VARIATION_MAX`](../../Reference/met/MmetControl.md) is valid for all fit operations for physically measured or constructed features.  You can specify [`M_FIT_VARIATION_MAX`](../../Reference/met/MmetControl.md) when [`M_FIT_TYPE`](../../Reference/met/MmetControl.md) is set to [`M_STANDARD_FIT`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum variation will be determined automatically. |
| `0.0 <= Value <= 100.0` | Specifies the maximum variation, as a percentage. |

---

### `M_LINE_A`

Sets the coefficient _A_ of the line equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient's value. |

---

### `M_LINE_B`

Sets the coefficient _B_ of the line equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient's value. |

---

### `M_LINE_BISECTOR_TYPE`

Sets how to construct a line feature when building it as a bisector. This only applies when using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) set to [`M_LINE`](../../Reference/met/MmetAddFeature.md) and [`M_BISECTOR`](../../Reference/met/MmetAddFeature.md), and specifying two lines as the base features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BIGGEST_ANGLE` | Specifies that the newly constructed line is built at the middle of the biggest angle formed by the two lines specified as base features. |
| `M_FEATURE_ORDER` *(default)* | Specifies that the newly constructed line is built according to the order in which you specified the two line base features; that is, at the middle of the angle formed between the first and second specified lines. |
| `M_SMALLEST_ANGLE` | Specifies that the newly constructed line is built at the middle of the smallest angle formed by the two lines specified as base features. |

---

### `M_LINE_C`

Sets the coefficient _C_ of the line equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-0.5 * AIL_INT32_MAX <= Value <= 0.5 * AIL_INT32_MAX` *(default)* | Specifies the coefficient's value.  > **Note:** If the value that you need to specify for C is out of range, you can multiply A, B, and C by `(0.5 *_AIL_INT32_MAX_) / _C_` to normalize the coefficients. |

---

### `M_NUMBER_MAX`

Sets the maximum number of subfeatures to calculate for a multiple feature. Note that [`M_NUMBER_MAX`](../../Reference/met/MmetControl.md) is valid for point features only.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum number of subfeatures. This value must be greater than 1 for measured multiple point features. |

---

### `M_NUMBER_MIN`

Sets the minimum number of subfeatures to calculate for a multiple feature. Note that [`M_NUMBER_MIN`](../../Reference/met/MmetControl.md) is valid for point features only.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the minimum number of subfeatures. This value must be greater than 1 for measured multiple point features. |

---

### `M_OCCURRENCE`

Sets the point to use if there is more than one candidate when adding a constructed point feature built at the intersection of two base features ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_POINT`](../../Reference/met/MmetAddFeature.md) and [`M_INTERSECTION`](../../Reference/met/MmetAddFeature.md) or [`M_EXTENDED_INTERSECTION`](../../Reference/met/MmetAddFeature.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the index value. |

---

### `M_POSITION`

Sets the position, along the contour of the specified base feature, at which to construct the point feature ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_POINT`](../../Reference/met/MmetAddFeature.md) and [`M_POSITION_ABSOLUTE`](../../Reference/met/MmetAddFeature.md) or [`M_POSITION_RELATIVE`](../../Reference/met/MmetAddFeature.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the position value. Set the position as a linear value when using [`M_POSITION_ABSOLUTE`](../../Reference/met/MmetAddFeature.md) and as a percentage when using [`M_POSITION_RELATIVE`](../../Reference/met/MmetAddFeature.md). |

---

### `M_POSITION_END_X`

Sets the X-coordinate of the end point of a segment constructed using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_END_Y`

Sets the Y-coordinate of the end point of a segment constructed using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_POSITION_START_X`

Sets the X-coordinate of the start point of a segment constructed using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_START_Y`

Sets the Y-coordinate of the start point of a segment constructed using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_POSITION_X`

Sets the X-coordinate of the center of a constructed circle or arc, or sets the X-coordinate of a constructed local frame or point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_Y`

Sets the Y-coordinate of the center of a constructed circle or arc, or sets the Y-coordinate of a constructed local frame or point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_RADIUS`

Sets the radius of a constructed circle or arc.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius, in pixel or world units. |

---

### `M_REFERENCE_FRAME`

Sets the feature's reference frame.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the reference frame is the global frame. |
| `Value > 0` | Specifies the label of the reference frame. |

---

### `M_SORT`

Sets the sorting order that Aurora Imaging Library applies to the index of subpoints, when retrieving results of multiple point features. A multiple point feature is a point feature that contains more than one point. By default, the subpoints (points within the point feature) are sorted in ascending order, from the subpoint with the lowest index to the subpoint with the highest index.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` | Specifies that results of multiple point features are sorted in descending order, according to the subpoint's index. |
| `M_SORT_UP` *(default)* | Specifies that results of multiple point features are sorted in ascending order, according to the subpoint's index. |

### For controlling constructed edgel features

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control constructed edgel features. In this case, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter must be set to the label or index of the feature, using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**, and the [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_EDGEL_PROVIDED_ORDER`

Sets whether the egdels/points used to construct the edgel feature are listed in a meaningful order.  This setting applies to edgel features that are constructed using externally established edgels/points ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md)), which can come from other Aurora Imaging Library modules or third party software. Such edgels/points are specified using one or more arrays in [`MmetPut`](../../Reference/met/MmetPut.md). The order in which the edgels/points are specified in the arrays is considered meaningfully when, for example, they are listed according to their orientation (such as when they come from a laser line profile) or according to a coherent path (such as when they follow each other in a circular shape). Specifying ordered edgels/points can be useful when, for example, denoising or resampling edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies unordered edgels/points. |
| `M_SEQUENTIAL` | Specifies ordered edgels/points. In this case, Aurora Imaging Library maintains the order in which the edgels/points were specified using [`MmetPut`](../../Reference/met/MmetPut.md). |

---

### `M_EDGEL_RESAMPLING_LINE_ANGLE`

Sets the angle of a line along which to resample edgels. For example, you can resample along a laser line.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable resampling along a line. In this case, the resampling that Aurora Imaging Library performs (and subsequent operations) can be slower, as more internal computations are required. |
| `Value >= 0.0` | Specifies the angle of the line along which to resample edgels. The angle is relative to the reference frame's X-axis and follows the counter-clockwise convention. If you specify an angle of 0.0, Aurora Imaging Library resamples the edgels along a line at that angle. If you want Aurora Imaging Library to resample without using a line, set [`M_EDGEL_RESAMPLING_LINE_ANGLE`](../../Reference/met/MmetControl.md) to [`M_DISABLE`](../../Reference/met/MmetControl.md). |

---

### `M_EDGEL_RESAMPLING_MODE`

Sets how to resample edgels. Resampling refers to using internal estimation techniques (mean or median) to modify and improve the edgels that Aurora Imaging Library uses.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies no resampling. |
| `M_MEAN` | Specifies a mean type of resampling. |
| `M_MEDIAN` | Specifies a median type of resampling. |

---

### `M_EDGEL_RESAMPLING_RADIUS`

Sets the radius by which to resample edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the radius. |

---

### `M_EDGEL_RESAMPLING_USE_ORDER`

Sets whether Aurora Imaging Library should use and maintain the order of the edgels when resampling them.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library should not use and maintain the order of the edgels then resampling them. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library should use and maintain the order of the edgels when resampling them. This can improve the speed and accuracy of the resampling. This also ensures that the resampling itself does not reorder the edgels; keeping the original order can be useful for future operations. |

---

### `M_GROUP_EDGEL_GAP_SIZE`

Sets the maximum allowable gap (distance) between edgels for Aurora Imaging Library to consider them part of the same group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library should automatically determine the gap size. |
| `Value >= 0.0` | Specifies the gap size, in pixels. |

---

### `M_GROUP_MIN_NUMBER_OF_EDGELS`

Sets the minimum number of edgels a group must have. Aurora Imaging Library removes all groups that do not meet this minimum number.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there is no minimum number of edgels that a group must have. |
| `Value > 0` | Specifies the minimum number of edgels that a group must have. |

---

### `M_GROUP_SELECTION_MODE`

Sets how to group edgels.  Aurora Imaging Library initially establishes the groups based on the gaps (distance) between the edgels. By default, Aurora Imaging Library automatically determines the gap size to group edgels. Edgels that are closer to each other than the gap size are considered part of the same group. To explicitly specify the gap size, use [`M_GROUP_EDGEL_GAP_SIZE`](../../Reference/met/MmetControl.md). Aurora Imaging Library then eliminates any potential group, if it does not have the minimum number of edgels, which you can set with [`M_GROUP_MIN_NUMBER_OF_EDGELS`](../../Reference/met/MmetControl.md) (by default, there is no minimum requirement). Once these basic groups are formed, you can either select one group or all groups using [`M_GROUP_SELECTION_MODE`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSEST_TO_POINT` | Specifies to select one group, based on its proximity to a point or the local frame's origin. By default, Aurora Imaging Library selects the group (from the initially established groups) that is closest to the global frame's origin. To select the group that is closest to a different point, use [`M_GROUP_SELECTION_POINT`](../../Reference/met/MmetControl.md). |
| `M_OCCURRENCE` *(default)* | Specifies to select one or all group(s), based on the index of the group of edgels. By default, Aurora Imaging Library selects all of the initially established groups. The group index is determined at runtime when creating the groups. To select only one group based on its index, use [`M_GROUP_SELECTION_OCCURRENCE`](../../Reference/met/MmetControl.md). |

---

### `M_GROUP_SELECTION_OCCURRENCE`

Sets whether to select one group of edgels or all groups of edgels.  This setting only has an effect if [`M_GROUP_SELECTION_MODE`](../../Reference/met/MmetControl.md) is set to [`M_OCCURRENCE`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use all groups of edgels. |
| `Value > 0` | Specifies the index of the group of edgels to select. The index number of the groups is determined at runtime. |

---

### `M_GROUP_SELECTION_POINT`

Sets the point or local frame feature from which to select the group of edgels.  This setting only has an effect if [`M_GROUP_SELECTION_MODE`](../../Reference/met/MmetControl.md) is set to [`M_CLOSEST_TO_POINT`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL_FRAME` *(default)* | Specifies a virtual point at the location (origin) of the global frame. |
| `Value > 0` | Specifies the label of an existing point feature or local frame feature. |

### For setting the geometry of the derived metrology region

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to set the geometry of the derived metrology region. In this case, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter must be set to [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetControl.md) and the [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a derived metrology region object. You must also call [`MmetSetRegion`](../../Reference/met/MmetSetRegion.md) with [`M_FROM_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetSetRegion.md) and specify the label or index of the measured feature that will be established from the area delimited by the derived metrology region.

---

### `M_REGION_GEOMETRY`

Sets the geometry of the derived metrology region. Each [`M_REGION_GEOMETRY`](../../Reference/met/MmetControl.md) setting states the required set of [`M_REGION_...`](../../Reference/met/MmetControl.md) controls that fully define the derived metrology region and its orientation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ARC` | Specifies that the feature will be established with an arc metrology region. You can use [`M_ARC`](../../Reference/met/MmetControl.md) to establish point features.  To define an arc metrology region, you must set its end angle ([`M_REGION_ANGLE_END`](../../Reference/met/MmetControl.md)), start angle ([`M_REGION_ANGLE_START`](../../Reference/met/MmetControl.md)), position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)), and radius ([`M_REGION_RADIUS`](../../Reference/met/MmetControl.md)).  *[Image: MetRoiArc.png]* |
| `M_INFINITE` *(default)* | Specifies that the feature will be established with an infinite metrology region. An infinite metrology region is an X- Y-plane, which makes it similar to a rectangle metrology region. However, a rectangle is bounded by its width and height, while an infinite metrology region has no such boundary. You can use [`M_INFINITE`](../../Reference/met/MmetControl.md) to establish any feature.  To define an infinite metrology region, you must set its angle ([`M_REGION_ANGLE`](../../Reference/met/MmetControl.md)) and position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)). The orientation of an infinite metrology region is established by its angle. |
| `M_RECTANGLE` | Specifies that the feature will be established with a rectangle metrology region. You can use [`M_RECTANGLE`](../../Reference/met/MmetControl.md) to establish segment or edgel features.  To define a rectangle metrology region, you must set its angle ([`M_REGION_ANGLE`](../../Reference/met/MmetControl.md)), height ([`M_REGION_HEIGHT`](../../Reference/met/MmetControl.md)), position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)), and width ([`M_REGION_WIDTH`](../../Reference/met/MmetControl.md)).  *[Image: MetRoiRectangle.png]* |
| `M_RING` | Specifies that the feature will be established with a ring metrology region. You can use [`M_RING`](../../Reference/met/MmetControl.md) to establish circle or edgel features.  To define a ring metrology region, you must set its position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)), end radius ([`M_REGION_RADIUS_END`](../../Reference/met/MmetControl.md)), and start radius ([`M_REGION_RADIUS_START`](../../Reference/met/MmetControl.md)).  *[Image: MetRoiRing.png]* |
| `M_RING_SECTOR` | Specifies that the feature will be established with a ring-sector metrology region. You can use [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) to establish radial segment, arc, or edgel features.  To define a ring-sector metrology region, you must set its end angle ([`M_REGION_ANGLE_END`](../../Reference/met/MmetControl.md)), start angle ([`M_REGION_ANGLE_START`](../../Reference/met/MmetControl.md)), position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)), end radius ([`M_REGION_RADIUS_END`](../../Reference/met/MmetControl.md)) and start radius ([`M_REGION_RADIUS_START`](../../Reference/met/MmetControl.md)).  *[Image: MetRoiRingSector.png]* |
| `M_SEGMENT` | Specifies that the feature will be established with a linear segment metrology region. You can use [`M_SEGMENT`](../../Reference/met/MmetControl.md) to establish point features.  To define a linear segment metrology region, you must set its end point ([`M_REGION_END`](../../Reference/met/MmetControl.md) or [`M_REGION_END_X`](../../Reference/met/MmetControl.md) and [`M_REGION_END_Y`](../../Reference/met/MmetControl.md)), and start point ([`M_REGION_START`](../../Reference/met/MmetControl.md) or [`M_REGION_START_X`](../../Reference/met/MmetControl.md) and [`M_REGION_START_Y`](../../Reference/met/MmetControl.md)).  *[Image: MetRoiSegment.png]* |

### For setting the geometry data of the derived metrology region

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to set the geometry data required to define a derived metrology region. In this case, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter must be set to [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetControl.md) and the [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a derived metrology region object.  The controls in the following table that you must set depend on the geometry specified with [`M_REGION_GEOMETRY`](../../Reference/met/MmetControl.md).

---

### `M_REGION_ANGLE`

Sets the angle of the derived metrology region, relative to the reference frame. [`M_REGION_ANGLE`](../../Reference/met/MmetControl.md) applies to [`M_INFINITE`](../../Reference/met/MmetControl.md) and [`M_RECTANGLE`](../../Reference/met/MmetControl.md) derived metrology regions, unless otherwise specified.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_RADIAL` | Specifies that the infinite metrology region has a radial orientation, from 0.0° to 360.0°, based on the specified position ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md)). [`M_RADIAL`](../../Reference/met/MmetControl.md) applies to [`M_INFINITE`](../../Reference/met/MmetControl.md) only.  *[Image: InfiniteRadialRegion1.png]*  In such a region, edgels can only be considered part of the physically measured feature if their gradient angle conforms to the direction of a radii pointing out from the region's position. [`M_RADIAL`](../../Reference/met/MmetControl.md) is usually used to define a metrology region for a physically measured circular feature whose radius is unknown. Typically the position of this metrology region is placed at the feature's center, resulting in valid edgels only being possible if they encircle the position. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. In this case, you must set [`M_REGION_ANGLE_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the angle of the derived metrology region. In this case, you must set [`M_REGION_ANGLE_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).  When specifying the label of a point, Aurora Imaging Library considers the angle to be between the X-axis along the metrology region's position (for example, [`M_REGION_POSITION`](../../Reference/met/MmetControl.md)), and a theoretical straight line that connects that position to the specified point feature. By default, the X-axis along the metrology region's position is aligned along the X-axis of the reference frame. The following example shows how a specified point establishes the angle of a rectangle metrology region.  *[Image: MetRegionAngleWithPoint.png]* |

---

### `M_REGION_ANGLE_END`

Sets the end angle of the derived metrology region, relative to the reference frame. [`M_REGION_ANGLE_END`](../../Reference/met/MmetControl.md) applies to [`M_ARC`](../../Reference/met/MmetControl.md) or [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. In this case, you must set [`M_REGION_ANGLE_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the end angle of the metrology region. In this case, you must set [`M_REGION_ANGLE_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).  When specifying the label of a point, Aurora Imaging Library considers the end angle to be between the X-axis along the metrology region's position (for example, [`M_REGION_POSITION`](../../Reference/met/MmetControl.md)), and a theoretical straight line that connects that position to the specified point feature. By default, the X-axis along the metrology region's position is aligned along the X-axis of the reference frame. The following example shows how a specified point establishes the end angle of an arc metrology region. The example also shows how a specified point establishes the start angle of an arc metrology region ([`M_REGION_ANGLE_START`](../../Reference/met/MmetControl.md)).  *[Image: MetRegionStartEndAngleWithPoints.png]* |

---

### `M_REGION_ANGLE_END_TYPE`

Sets whether you are using a feature or an explicit value to specify the end angle of the derived metrology region ([`M_REGION_ANGLE_END`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_ANGLE_START`

Sets the start angle of the derived metrology region, relative to the reference frame. [`M_REGION_ANGLE_START`](../../Reference/met/MmetControl.md) applies to [`M_ARC`](../../Reference/met/MmetControl.md) or [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. In this case, you must set [`M_REGION_ANGLE_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the start angle of the metrology region. In this case, you must set [`M_REGION_ANGLE_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).  When specifying the label of a point, Aurora Imaging Library considers the start angle to be between the X-axis along the metrology region's position (for example, [`M_REGION_POSITION`](../../Reference/met/MmetControl.md)), and a theoretical straight line that connects that position to the specified point feature. By default, the X-axis along the metrology region's position is aligned along the X-axis of the reference frame. The following example shows how a specified point establishes the start angle of an arc metrology region. The example also shows how a specified point establishes the end angle of an arc metrology region ([`M_REGION_ANGLE_END`](../../Reference/met/MmetControl.md)).  *[Image: MetRegionStartEndAngleWithPoints.png]* |

---

### `M_REGION_ANGLE_START_TYPE`

Sets whether you are using a feature or an explicit value to specify the start angle of the derived metrology region ([`M_REGION_ANGLE_START`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_ANGLE_TYPE`

Sets whether you are using a feature or an explicit value to specify the angle of the derived metrology region ([`M_REGION_ANGLE`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_END`

Sets the end point of the derived metrology region. [`M_REGION_END`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_END`](../../Reference/met/MmetControl.md), you must set [`M_REGION_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the end point of the metrology region. |

---

### `M_REGION_END_TYPE`

Sets whether you are using a feature or an explicit value to specify the end point of the derived metrology region ([`M_REGION_END`](../../Reference/met/MmetControl.md), or [`M_REGION_END_X`](../../Reference/met/MmetControl.md) and [`M_REGION_END_Y`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_END_X`

Sets the X-coordinate of the end point of the derived metrology region, relative to the reference frame. [`M_REGION_END_X`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_END_X`](../../Reference/met/MmetControl.md), you must set [`M_REGION_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end point's X-coordinate, in pixel or world units. |

---

### `M_REGION_END_Y`

Sets the Y-coordinate of the end point of the derived metrology region, relative to the reference frame. [`M_REGION_END_Y`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_END_Y`](../../Reference/met/MmetControl.md), you must set [`M_REGION_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the end point's Y-coordinate, in pixel or world units. |

---

### `M_REGION_HEIGHT`

Sets the height of the derived metrology region. [`M_REGION_HEIGHT`](../../Reference/met/MmetControl.md) applies to [`M_RECTANGLE`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the rectangle's height. In this case, you must set [`M_REGION_HEIGHT_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).  When specifying the label of a point, Aurora Imaging Library considers the height to be at the perpendicular intersection point between the Y-axis along the metrology region's position (for example, [`M_REGION_POSITION`](../../Reference/met/MmetControl.md)), and the X-axis along the specified point feature. By default, the X- and Y-axes along the metrology region's position and along the specified point are aligned with the reference frame. The example also shows how a specified point establishes the width of a rectangle metrology region ([`M_REGION_WIDTH`](../../Reference/met/MmetControl.md)).  *[Image: MetRegionHeightWidthWithPoint.png]* |
| `Value >= 0.0` *(default)* | Specifies the rectangle's height, in pixel or world units. In this case, you must set [`M_REGION_HEIGHT_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |

---

### `M_REGION_HEIGHT_TYPE`

Sets whether you are using a feature or an explicit value to specify the height of the derived metrology region ([`M_REGION_HEIGHT`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_POSITION`

Sets the position of the derived metrology region. [`M_REGION_POSITION`](../../Reference/met/MmetControl.md) applies to all derived metrology regions except [`M_SEGMENT`](../../Reference/met/MmetControl.md).  To use [`M_REGION_POSITION`](../../Reference/met/MmetControl.md), you must set [`M_REGION_POSITION_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the position of the metrology region. |

---

### `M_REGION_POSITION_TYPE`

Sets whether you are using a feature or an explicit value to specify the position of the derived metrology region ([`M_REGION_POSITION`](../../Reference/met/MmetControl.md), or [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_POSITION_X`

Sets the X-coordinate of the position of the derived metrology region, relative to the reference frame. [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md) applies to all derived metrology regions except [`M_SEGMENT`](../../Reference/met/MmetControl.md).  To use [`M_REGION_POSITION_X`](../../Reference/met/MmetControl.md), you must set [`M_REGION_POSITION_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the position's X-coordinate, in pixel or world units. |

---

### `M_REGION_POSITION_Y`

Sets the Y-coordinate of the position of the derived metrology region, relative to the reference frame. [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md) applies to all derived metrology regions except [`M_SEGMENT`](../../Reference/met/MmetControl.md).  To use [`M_REGION_POSITION_Y`](../../Reference/met/MmetControl.md), you must set [`M_REGION_POSITION_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the position's Y-coordinate, in pixel or world units. |

---

### `M_REGION_RADIUS`

Sets the radius of the derived metrology region. [`M_REGION_RADIUS`](../../Reference/met/MmetControl.md) applies to [`M_ARC`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the arc's radius. In this case, you must set [`M_REGION_RADIUS_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md). |
| `Value >= 0.0` *(default)* | Specifies the radius, in pixel or world units. In this case, you must set [`M_REGION_RADIUS_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |

---

### `M_REGION_RADIUS_END`

Sets the end radius of the derived metrology region. [`M_REGION_RADIUS_END`](../../Reference/met/MmetControl.md) applies to [`M_RING`](../../Reference/met/MmetControl.md) or [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the end radius. In this case, you must set [`M_REGION_RADIUS_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md). |
| `Value >= 0.0` *(default)* | Specifies the end radius, in pixel or world units. In this case, you must set [`M_REGION_RADIUS_END_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |

---

### `M_REGION_RADIUS_END_TYPE`

Sets whether you are using a feature or an explicit value to specify the end radius of the derived metrology region ([`M_REGION_RADIUS_END`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_RADIUS_START`

Sets the start radius of the derived metrology region. [`M_REGION_RADIUS_START`](../../Reference/met/MmetControl.md) applies to [`M_RING`](../../Reference/met/MmetControl.md) or [`M_RING_SECTOR`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the start radius. In this case, you must set [`M_REGION_RADIUS_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md). |
| `Value >= 0.0` *(default)* | Specifies the start radius, in pixel or world units. In this case, you must set [`M_REGION_RADIUS_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |

---

### `M_REGION_RADIUS_START_TYPE`

Sets whether you are using a feature or an explicit value to specify the start radius of the derived metrology region ([`M_REGION_RADIUS_START`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_RADIUS_TYPE`

Sets whether you are using a feature or an explicit value to specify the radius of the derived metrology region ([`M_REGION_RADIUS`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_START`

Sets the start point of the derived metrology region. [`M_REGION_START`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_START`](../../Reference/met/MmetControl.md), you must set [`M_REGION_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the start point of the metrology region. |

---

### `M_REGION_START_TYPE`

Sets whether you are using a feature or an explicit value to specify the start point of the derived metrology region ([`M_REGION_START`](../../Reference/met/MmetControl.md) or [`M_REGION_START_X`](../../Reference/met/MmetControl.md) and [`M_REGION_START_Y`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

---

### `M_REGION_START_X`

Sets the X-coordinate of the start point of the derived metrology region, relative to the reference frame. [`M_REGION_START_X`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_START_X`](../../Reference/met/MmetControl.md), you must set [`M_REGION_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start point's X-coordinate, in pixel or world units. |

---

### `M_REGION_START_Y`

Sets the Y-coordinate of the start point of the derived metrology region, relative to the reference frame. [`M_REGION_START_Y`](../../Reference/met/MmetControl.md) applies to [`M_SEGMENT`](../../Reference/met/MmetControl.md) derived metrology regions.  To use [`M_REGION_START_Y`](../../Reference/met/MmetControl.md), you must set [`M_REGION_START_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the start point's Y-coordinate, in pixel or world units. |

---

### `M_REGION_WIDTH`

Sets the width of the derived metrology region. [`M_REGION_WIDTH`](../../Reference/met/MmetControl.md) applies to [`M_RECTANGLE`](../../Reference/met/MmetControl.md) derived metrology regions.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the rectangle's width. In this case, you must set [`M_REGION_WIDTH_TYPE`](../../Reference/met/MmetControl.md) to [`M_FEATURE_LABEL_VALUE`](../../Reference/met/MmetControl.md).  When specifying the label of a point, Aurora Imaging Library considers the width to be at the perpendicular intersection point between the X-axis along the metrology region's position (for example, [`M_REGION_POSITION`](../../Reference/met/MmetControl.md)), and the Y-axis along the specified point feature. By default, the X- and Y-axes along the metrology region's position and along the specified point are aligned with the reference frame. The example also shows how a specified point establishes the height of a rectangle metrology region ([`M_REGION_HEIGHT`](../../Reference/met/MmetControl.md)).  *[Image: MetRegionHeightWidthWithPoint.png]* |
| `Value >= 0.0` *(default)* | Specifies the rectangle's width, in pixel or world units. In this case, you must set [`M_REGION_WIDTH_TYPE`](../../Reference/met/MmetControl.md) to [`M_PARAMETRIC`](../../Reference/met/MmetControl.md). |

---

### `M_REGION_WIDTH_TYPE`

Sets whether you are using a feature or an explicit value to specify the width of the derived metrology region ([`M_REGION_WIDTH`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies to use a feature. |
| `M_PARAMETRIC` *(default)* | Specifies to use an explicit value. |

### For controlling geometric tolerances

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control the geometric tolerances calculated for a metrology context. For controlling geometric tolerances, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to [`M_ALL_TOLERANCES`](../../Reference/met/MmetGetResult.md) or an existing tolerance label or index using **M_TOLERANCE_LABEL()** or **M_TOLERANCE_INDEX()**. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context. Unless otherwise specified, the following settings apply to any geometric tolerance ([`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md)).

---

### `M_AREA_BETWEEN_CURVES_DEFINE_SIDE`

Sets the reference frame that determines the side of the curve on which to validate the area, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance, and enable the [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) setting.  [`M_AREA_BETWEEN_CURVES_DEFINE_SIDE`](../../Reference/met/MmetControl.md) is typically used if the two specified curves (edgel features) that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance validates cross over each other, causing areas on both sides of the curves. In this case, when enabling [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md), Aurora Imaging Library must establish the side on which the area is considered positive, and must add the areas on that side only. Areas on the other side (which is considered the negative side) are ignored.  The positive and negative sides are determined by whether the Y-axis of the reference frame (by default, the global frame) initially intersects with either the first or second curve that is specified with the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. If the Y-axis initially intersects with the first specified curve, all the areas on the side of the first curve facing the direction of the Y-axis are considered positive and Aurora Imaging Library adds them; all the areas on the other side of the first curve are considered negative and Aurora Imaging Library ignores them. If the Y-axis initially intersects with the second specified curve, all the areas on the side of the second curve facing the direction of the Y-axis are considered negative and Aurora Imaging Library ignores them; all the areas on the other side of the second curve are considered positive and Aurora Imaging Library adds them.  *[Image: Metro_AreaBetwixtCurves_Side_Matters.png]*  If neither curve intersects the Y-axis, or if only one curve intersects the Y-axis, the sign of the areas between the curves is computed based on the order in which the two curves are defined. If the points delimiting an area on the first specified curve appear in a clockwise order, then this area will be negative. If they appear in a counter-clockwise order, then this area will be positive.  *[Image: metro_areabetwixtcurves_side_matters_no_intersect.png]*  Note that both the reference frame, and the order in which you specify the two curves with [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), determine the side on which to validate the tolerance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the label value of a local frame feature. |
| `M_FEATURE_LABEL` | Specifies the label value of a local frame feature. |
| `M_GLOBAL_FRAME` *(default)* | Specifies the global frame. |

---

### `M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`

Sets whether to validate the area on only one side of the curve, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) is typically used if the two specified curves (edgel features) that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance validates cross over each other, causing areas on both sides of the curves.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not validate the area on only one side of the curve. Areas on both sides are either all added, or added and subtracted, depending on whether [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetControl.md) is enabled or disabled. |
| `M_ENABLE` | Specifies that Aurora Imaging Library validates the area on only one side of the curve. To enable [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md), you must disable [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetControl.md) (it is disabled by default).  When enabling [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md), Aurora Imaging Library must establish the side on which the area is considered positive, and must add the areas on that side only. Areas on the other side (which is considered the negative side) are ignored.  By default, the positive and negative sides are determined by whether the global reference frame's Y-axis initially intersects with either the first or second curve that is specified with the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. To determine the positive and negative sides by using a reference frame other than the global frame, use [`M_AREA_BETWEEN_CURVES_DEFINE_SIDE`](../../Reference/met/MmetControl.md). For more information about this behavior, see [`M_AREA_BETWEEN_CURVES_DEFINE_SIDE`](../../Reference/met/MmetControl.md). |

---

### `M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`

Sets whether Aurora Imaging Library should subtract opposing areas, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetControl.md) only has an effect if the two specified curves (edgel features) that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance validates cross over each other, causing areas on both sides of the curves.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not subtract opposing areas. All areas between the curves are added. By default, Aurora Imaging Library adds the areas on both sides of the curves. To add the areas on only one side, enable [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md). |
| `M_ENABLE` | Specifies that Aurora Imaging Library subtracts the areas on one side of the curve (opposing areas) from the areas on the other side of the curve, so that the result is positive. The side with the smaller total area is always subtracted from the side with the greater total area. To enable [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetControl.md), you must disable [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetControl.md) (it is disabled by default). |

---

### `M_AREA_UNDER_CURVE_ALLOW_NEGATIVE`

Sets whether Aurora Imaging Library allows negative (opposing) areas when establishing the area tolerance for [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md) or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md). Negative areas occur when the specified curve (edgel) feature that the tolerance is validating crosses over the specified line feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not allow negative areas when establishing the area tolerance. If there are negative areas, the tolerance fails. |
| `M_ENABLE` | Specifies that Aurora Imaging Library allows negative areas when establishing the area tolerance. In this case, Aurora Imaging Library subtracts the negative areas from the larger, positive areas when establishing the area tolerance.  *[Image: Metro_AreaUnderCurve_Allow_Negative.png]*  The negative areas are those located on the side of the line where the sum of the areas is the smallest. |

---

### `M_CURVE_EDGEL_GAP_SIZE`

Sets the maximum gap between edgels for Aurora Imaging Library to consider them part of the same curve of edgels, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md), or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md) tolerance. If the gap between edgels is greater than the maximum gap, those edgels will not be part of the same curve.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL_SIZE_DIAGONAL` *(default)* | Specifies twice the diagonal size of the pixels from which the edgels were taken. |
| `Value > 0.0` | Specifies the size, in pixels. |

---

### `M_CURVE_INFO`

Sets the information that Aurora Imaging Library uses to establish the curves of edgels, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the index of a reference frame feature (local frame). The reference frame indicates the direction of the curves, and is used to sort the edgels accordingly. That is, [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) assumes the direction of the curves' edgels is relative to the reference frame's X-axis. Specifying a direction can make the tolerance faster to calculate, and less sensitive to gaps between edgels.  When you set [`M_CURVE_INFO`](../../Reference/met/MmetControl.md) to a reference frame, [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) assumes that the curves are unbounded (open), as edgel direction does not apply to bounded (closed) curves. |
| `M_FEATURE_LABEL` | Specifies the label of a reference frame feature (local frame). The reference frame indicates the direction of the curves, and is used to sort the edgels accordingly. That is, [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) assumes the direction of the curves' edgels is relative to the reference frame's X-axis. Specifying a direction can make the tolerance faster to calculate, and less sensitive to gaps between edgels.  When you set [`M_CURVE_INFO`](../../Reference/met/MmetControl.md) to a reference frame, [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) assumes that the curves are unbounded (open), as edgel direction does not apply to bounded (closed) curves. |
| `M_AUTO_CURVE_CLOSED` | Specifies that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance is validating bounded (closed) curves (edgel features). This setting is typically used when you want to validate closed curves that pass through all the edgels. Such curves should not cross over themselves and must form only one closed shape (for example, the curve cannot form a figure 8, which can be seen as two circular shapes).  With this setting, the tolerance does not assume that the curves follow a specific direction (since the shape is closed, direction does not apply). Such curves should have edgels with very minimal gaps between them. If the curves (edgel features) that you specified were externally established with sequential edgels ([`M_EDGEL_PROVIDED_ORDER`](../../Reference/met/MmetControl.md) set to [`M_SEQUENTIAL`](../../Reference/met/MmetControl.md)), Aurora Imaging Library maintains the order of the edgels, which can make calculations faster and more accurate (provided that the specified edgel order forms a properly closed curve). |
| `M_AUTO_CURVE_OPENED` *(default)* | Specifies that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance is validating unbounded (opened) curves (edgel features). This setting is typically used when you want to validate opened curves that pass through all the edgels. Such curves should not cross over themselves and must form one fully opened shape (for example, the curve cannot form the number 6, which can be seen as touching and partially closed).  With this setting, the tolerance does not assume that the curves follow a specific direction. Such curves should have edgels with very minimal gaps between them. To make the tolerance less sensitive to gaps (and potentially faster to calculate), specify a direction for the curve, by setting [`M_CURVE_INFO`](../../Reference/met/MmetControl.md) to a reference frame.  If the curves (edgel features) that you specified were externally established with sequential edgels ([`M_EDGEL_PROVIDED_ORDER`](../../Reference/met/MmetControl.md) set to [`M_SEQUENTIAL`](../../Reference/met/MmetControl.md)), Aurora Imaging Library maintains the order of the edgels, which can make calculations faster and more accurate (provided that the specified edgel order forms a properly opened curve). |

---

### `M_DISTANCE_MODE`

Sets how to apply the angle at which to establish the distance between features when adding a distance tolerance. This affects [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md) with [`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md) or [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies that Aurora Imaging Library establishes the angle by taking the distance at which the features are either the farthest (for [`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md)) or the closest (for [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md)). |
| `M_FERET_AT_ANGLE` | Specifies that Aurora Imaging Library calculates the distance tolerance using the feret at the angle that is set ([`M_ANGLE`](../../Reference/met/MmetControl.md)). This only applies to [`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md).  Aurora Imaging Library measures the angle ([`M_ANGLE`](../../Reference/met/MmetControl.md)) relative to the coordinate system of the first base feature with which to validate the maximum distance tolerance. |
| `M_GAP_AT_ANGLE` | Specifies that Aurora Imaging Library calculates the distance tolerance using the gap at the angle that is set ([`M_ANGLE`](../../Reference/met/MmetControl.md)). This only applies to [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md).  Aurora Imaging Library measures the angle ([`M_ANGLE`](../../Reference/met/MmetControl.md)) relative to the first base feature with which to validate the minimum distance tolerance. |

---

### `M_GAIN`

Sets the gain Aurora Imaging Library uses to correct systematic bias between multiple imaging vision set-ups. Systematic bias can, for example, exist between two different Coordinate Measuring Machines (CMMs).  Note that tolerance values, which Aurora Imaging Library uses to determine the status of features (and the status of tolerances themselves), are a composite of measured value, gain ([`M_GAIN`](../../Reference/met/MmetControl.md)), and offset ([`M_OFFSET`](../../Reference/met/MmetControl.md)).  *[Image: FormulaForToleranceValue.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the gain. |

---

### `M_OFFSET`

Sets the offset Aurora Imaging Library uses to correct systematic bias between multiple imaging vision set-ups. Systematic bias can, for example, exist between two different Coordinate Measuring Machines (CMMs).  Note that tolerance values, which Aurora Imaging Library uses to determine the status of features (and the status of tolerances themselves), are a composite of measured value, offset ([`M_OFFSET`](../../Reference/met/MmetControl.md)), and gain ([`M_GAIN`](../../Reference/met/MmetControl.md)).  *[Image: FormulaForToleranceValue.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the offset. |

---

### `M_VALUE_MAX`

Sets the maximum acceptable value for the geometric tolerance Aurora Imaging Library uses to validate the feature or the relationship between multiple features. For example, if you use a length tolerance to validate the linear dimension of a segment feature, and you set [`M_VALUE_MAX`](../../Reference/met/MmetControl.md) to 100 pixels, the feature (and the tolerance itself) can only be valid if the segment's length is less than 100 pixels. Similarly, if you are using a perpendicularity tolerance to validate the relationship between two segment features, and you set [`M_VALUE_MAX`](../../Reference/met/MmetControl.md) to 0.05°, the features (and the tolerance itself) can only be valid if the angle between the segments is between 90.0° and 90.05°.  To get the status ([`M_PASS`](../../Reference/met/MmetGetResult.md), [`M_WARNING`](../../Reference/met/MmetGetResult.md), or [`M_FAIL`](../../Reference/met/MmetGetResult.md)) of the feature or the geometric tolerance itself, use [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_STATUS`](../../Reference/met/MmetGetResult.md).  Features can only have a passing status if their geometric tolerance has a passing status. Aurora Imaging Library determines the status of geometric tolerances according to the [`M_VALUE_MAX`](../../Reference/met/MmetControl.md), [`M_VALUE_MIN`](../../Reference/met/MmetControl.md), [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md), and [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) settings. The following is an example of (non-zero) minimum and maximum tolerance values and warning values:  *[Image: metro_tolerance_pass_warning_fail.png]*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum value. |

---

### `M_VALUE_MIN`

Sets the minimum value for a geometric tolerance to have a valid status. To get the status of the geometric tolerance ([`M_PASS`](../../Reference/met/MmetGetResult.md), [`M_WARNING`](../../Reference/met/MmetGetResult.md), or [`M_FAIL`](../../Reference/met/MmetGetResult.md)), use [`MmetGetResult`](../../Reference/met/MmetGetResult.md)with [`M_STATUS`](../../Reference/met/MmetGetResult.md).  The geometric tolerance's status is determined according to the [`M_VALUE_MAX`](../../Reference/met/MmetControl.md), [`M_VALUE_MIN`](../../Reference/met/MmetControl.md), [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md), and [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) settings. For an example of (non-zero) minimum and maximum tolerance values and warning values, see [`M_VALUE_MAX`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum value. |

---

### `M_VALUE_WARNING_MAX`

Sets the maximum warning value for a geometric tolerance to have a valid status. To get the status of the geometric tolerance ([`M_PASS`](../../Reference/met/MmetGetResult.md), [`M_WARNING`](../../Reference/met/MmetGetResult.md), or [`M_FAIL`](../../Reference/met/MmetGetResult.md)), use [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_STATUS`](../../Reference/met/MmetGetResult.md).  The geometric tolerance's status is determined according to the [`M_VALUE_MAX`](../../Reference/met/MmetControl.md), [`M_VALUE_MIN`](../../Reference/met/MmetControl.md), [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md), and [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) settings. For an example of (non-zero) minimum and maximum tolerance values and warning values, see [`M_VALUE_MAX`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EQUAL_PASS_MAX` *(default)* | Specifies that the maximum warning value is equal to the maximum pass value ([`M_VALUE_MAX`](../../Reference/met/MmetControl.md)). |
| `Value` | Specifies the maximum warning value. |

---

### `M_VALUE_WARNING_MIN`

Sets the minimum warning value for a geometric tolerance to have a valid status. To get the status of the geometric tolerance ([`M_PASS`](../../Reference/met/MmetGetResult.md), [`M_WARNING`](../../Reference/met/MmetGetResult.md), or [`M_FAIL`](../../Reference/met/MmetGetResult.md)), use [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_STATUS`](../../Reference/met/MmetGetResult.md).  The geometric tolerance's status is determined according to the [`M_VALUE_MAX`](../../Reference/met/MmetControl.md), [`M_VALUE_MIN`](../../Reference/met/MmetControl.md), [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md), and [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) settings. For an example of (non-zero) minimum and maximum tolerance values and warning values, see [`M_VALUE_MAX`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EQUAL_PASS_MIN` *(default)* | Specifies that the minimum warning value is equal to the minimum pass value ([`M_VALUE_MIN`](../../Reference/met/MmetControl.md)). |
| `Value` | Specifies the minimum warning value. |

### For controlling features or geometric tolerances

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control features and geometric tolerances of a metrology context. For controlling features, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to [`M_GLOBAL_FRAME`](../../Reference/met/MmetControl.md) or one or all features. For controlling geometric tolerances, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to one or all tolerances. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_ANGLE`

Sets the angle used to construct a feature or define a tolerance, when applicable. For more information, refer to the feature or tolerance with which you are using the angle setting (for example, [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_ANGLE`](../../Reference/met/MmetAddFeature.md)/[`M_ANGLE_ABSOLUTE`](../../Reference/met/MmetAddFeature.md)/[`M_ANGLE_RELATIVE`](../../Reference/met/MmetAddFeature.md)/[`M_SECTOR_RELATIVE`](../../Reference/met/MmetAddFeature.md)/ [`M_PARAMETRIC`](../../Reference/met/MmetAddFeature.md), or [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md) with [`M_ANGULARITY`](../../Reference/met/MmetAddTolerance.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the angle. Depending on the build operation, you must set the angle in either degrees, or as a percentage. |

---

### `M_DRAWABLE`

Sets whether the feature or tolerance will be drawn using [`MmetDraw`](../../Reference/met/MmetDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the feature or tolerance will not be drawn. In this case, you cannot affect them in an interactive display. |
| `M_ENABLE` *(default)* | Specifies that the feature or tolerance will be drawn. |

### For controlling clone transformations

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to transform clone-type features ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_CLONE_FEATURE`](../../Reference/met/MmetAddFeature.md)). For example, [`M_CLONE_SCALE`](../../Reference/met/MmetControl.md) can specify that the cloned feature will be twice the size of its source. To specify clone transformations, set the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter to the label or index of an existing clone-type feature, using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**. This is the cloned feature to which the transformation ([`M_CLONE_...`](../../Reference/met/MmetControl.md)) will apply. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_CLONE_ANGLE`

Sets the rotation angle, in the counterclockwise direction, that is applied to the cloned feature ([`LabelOrIndex`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_CLONE_OFFSET_X`

Sets the translation value, in the X-direction, that is applied to the cloned feature ([`LabelOrIndex`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-offset for the cloned feature, in pixel or world units. |

---

### `M_CLONE_OFFSET_Y`

Sets the translation value, in the Y-direction, that is applied to the cloned feature ([`LabelOrIndex`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-offset for the cloned feature, in pixel or world units. |

---

### `M_CLONE_SCALE`

Sets the scale factor that is applied to the cloned feature ([`LabelOrIndex`](../../Reference/met/MmetControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the scale factor. |

### For controlling results

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to control the results calculated and stored in a metrology result buffer. For controlling results, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter must be set to [`M_GENERAL`](../../Reference/met/MmetControl.md). The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology result buffer.

---

### `M_OUTPUT_FRAME`

Sets the output reference frame that Aurora Imaging Library uses to return feature results. All results are returned relative to the output reference frame that you set. The output reference frame is reset to [`M_GLOBAL_FRAME`](../../Reference/met/MmetControl.md) after each call to [`MmetCalculate`](../../Reference/met/MmetCalculate.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL_FRAME` *(default)* | Specifies that Aurora Imaging Library returns feature results relative to the origin of the global frame. If the target image is not calibrated, the global frame's default origin (0,0) is aligned with the center of the top-left corner pixel of the target image. If the target image is calibrated, the global frame's default origin is aligned with the origin of the relative (world) coordinate system. |
| `M_IMAGE_FRAME` | Specifies that Aurora Imaging Library returns feature results relative to the origin of the target image's reference frame. If the target image is not calibrated, the origin of the target image's reference frame is at the center of the top-left corner pixel of the target image. If the target image is calibrated, the origin of the target image reference frame is the same as the relative coordinate system of the camera calibration. |
| `M_REFERENCE_FRAME` | Specifies that Aurora Imaging Library returns feature results relative to the reference frame of each feature. By default, a feature's reference frame is the global frame. |
| `Value > 0` | Specifies the label of an existing local frame feature to use to return feature results. |

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`MmetGetResult`](../../Reference/met/MmetGetResult.md) generates an error if the result was not calculated on a calibrated image. |

### For deleting features and tolerances

The following [`ControlType`](../../Reference/met/MmetControl.md) and corresponding [`ControlValue`](../../Reference/met/MmetControl.md) parameter settings are used to delete features and tolerances from the context. The [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter can be set to one of the following: [`M_ALL_FEATURES`](../../Reference/met/MmetControl.md), [`M_ALL_TOLERANCES`](../../Reference/met/MmetControl.md), an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**, or an existing tolerance label or index using **M_TOLERANCE_LABEL()** or **M_TOLERANCE_INDEX()**. The [`MetId`](../../Reference/met/MmetControl.md) parameter must be set to a metrology context.

---

### `M_DELETE`

Deletes a specific feature, a specific tolerance, all features, or all tolerances. To delete all features, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter needs to be set to [`M_ALL_FEATURES`](../../Reference/met/MmetControl.md). When deleting a feature, all its derived features and associated geometric tolerances are also deleted. To delete all tolerances, the [`LabelOrIndex`](../../Reference/met/MmetControl.md) parameter needs to be set to [`M_ALL_TOLERANCES`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Deletes a specific feature, a specific tolerance, all features, or all tolerances. |

If you use the default for all coefficients (_A_ = 0, _B_ = 1, and _C_ = 0), Aurora Imaging Library constructs a line that runs along the X-axis of the reference frame.

If you use the default for all positions ([`M_POSITION_START_X`](../../Reference/met/MmetControl.md) = -1.0, [`M_POSITION_START_Y`](../../Reference/met/MmetControl.md) = 0.0, [`M_POSITION_END_X`](../../Reference/met/MmetControl.md) = 1.0, and [`M_POSITION_END_Y`](../../Reference/met/MmetControl.md) = 0), Aurora Imaging Library constructs a segment with a length of 2 (-1.0 to 1.0); the segment is centered on the origin of the reference frame and runs along the reference frame's X-axis.

This setting applies to constructed edgel features that are built by copying the edgels of the specified base features ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_COPY_FEATURE_EDGELS`](../../Reference/met/MmetAddFeature.md)).

This setting applies to all physically measured features and constructed edgel features.

This setting only has an effect if [`M_EDGEL_DENOISING_MODE`](../../Reference/met/MmetControl.md) is set to a value other than its default (which is [`M_NULL`](../../Reference/met/MmetControl.md)).

This setting only has an effect if [`M_EDGEL_RESAMPLING_MODE`](../../Reference/met/MmetControl.md) is set to a value other than its default (which is [`M_NULL`](../../Reference/met/MmetControl.md)).

This setting applies to constructed edgel features built by calling [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_GROUP_EDGELS`](../../Reference/met/MmetAddFeature.md).

This setting can alter the precision of the edge that is ultimately fit; however, it might not reliably discard outliers (outlier edgels/points) when they significantly impact the initial fit (the first iteration of the fit). When dealing with a lot of outliers, use a robust best fit ([`M_ROBUST_FIT`](../../Reference/met/MmetControl.md)).
