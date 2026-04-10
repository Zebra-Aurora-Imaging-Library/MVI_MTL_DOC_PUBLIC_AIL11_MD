---
doctype: Reference
module: met
function: MmetInquire
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / met / MmetInquire"
---

# MmetInquire

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

> Inquire information about a metrology context, a derived metrology region object, features, tolerances, or a result buffer.

## Syntax

```c
AIL_INT MmetInquire(
    AIL_ID    MetId,         //in
    AIL_INT   LabelOrIndex,  //in
    AIL_INT64 InquireType,   //in
    void *    UserVarPtr     //out
)
```

## Description

This function inquires information about a metrology context, a derived metrology region object, features, tolerances, or a result buffer. To obtain metrology result buffer values, use [`MmetGetResult`](../../Reference/met/MmetGetResult.md).

If the inquired setting is set to [`M_DEFAULT`](../../Reference/met/MmetControl.md)(for example, in [`MmetControl`](../../Reference/met/MmetControl.md)), [`MmetInquire`](../../Reference/met/MmetInquire.md) will return [`M_DEFAULT`](../../Reference/met/MmetControl.md). To inquire the actual default value, add [`M_DEFAULT`](../../Reference/met/MmetInquire.md) to the [`InquireType`](../../Reference/met/MmetInquire.md) parameter.

## Parameters

### `MetId` *(in, AIL_ID)*

Specifies the identifier of the metrology context, the derived metrology region object, or the result buffer about which to inquire information. The metrology context or derived metrology region object must have been previously allocated on the required system using [`MmetAlloc`](../../Reference/met/MmetAlloc.md). The metrology result buffer must have been previously allocated on the required system using [`MmetAllocResult`](../../Reference/met/MmetAllocResult.md).

### `LabelOrIndex` *(in, AIL_INT)*

Specifies that information will be inquired about the metrology context, the derived metrology region object, features, tolerances, or the result buffer. This parameter should be set to one of the following values.

*For specifying what to inquire*

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the index value of an existing individual feature. |
| `M_FEATURE_LABEL` | Specifies the label value of an existing individual feature about which to inquire. |
| `M_TOLERANCE_INDEX` | Specifies the index value of an existing individual tolerance. |
| `M_TOLERANCE_LABEL` | Specifies the label value of an existing individual tolerance. |
| `M_CONTEXT` *(default)* | Inquires information about the metrology context, which has been set using the [`MetId`](../../Reference/met/MmetInquire.md) parameter. |
| `M_DERIVED_GEOMETRY_REGION` | Inquires information about the derived metrology region object, which has been set using the [`MetId`](../../Reference/met/MmetInquire.md) parameter. |
| `M_GENERAL` | Inquires information about a setting of a metrology result buffer. This setting applies to all features and tolerances in the metrology result buffer. When using [`M_GENERAL`](../../Reference/met/MmetInquire.md), [`MetId`](../../Reference/met/MmetInquire.md) must specify a metrology result buffer. |
| `M_GLOBAL_FRAME` | Inquires information about the global frame of the metrology template. |

### `InquireType` *(in, AIL_INT64)*

Specifies the type of setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to write the requested information. Since the [`MmetInquire`](../../Reference/met/MmetInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring general context settings

To inquire about general metrology context settings, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. To inquire general context settings, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to [`M_CONTEXT`](../../Reference/met/MmetInquire.md).

---

### `M_ASSOCIATED_CALIBRATION`

Inquires the camera calibration context associated with the template reference of the metrology context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Removes the association to the camera calibration. |
| `Calibration context identifier` | Specifies the identifier of the camera calibration context that is associated with the template reference of the metrology context. |
| `M_NULL` | Specifies that there is no camera calibration context associated with the template reference. |

---

### `M_MODIFICATION_COUNT`

Inquires the current value of the modification counter. The modification counter is increased by one each time settings for the context are modified.  Although you cannot identify the modification counter's contents, you can compare them throughout your application to know if the context has been altered. If the modification counter has changed you can, for example, prompt the user to save before closing the application.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the object modification count. |

---

### `M_NUMBER`

Inquires the number of external edgels/points.  External edgels/points come from [`MmetPut`](../../Reference/met/MmetPut.md), which puts them into the constructed external edgel feature ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_EDGEL`](../../Reference/met/MmetAddFeature.md) and[`M_EXTERNAL_FEATURE`](../../Reference/met/MmetAddFeature.md)).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of external edgels/points. |

---

### `M_NUMBER_OF_CONSTRUCTED_FEATURES`

Inquires the number of constructed features in the template of the metrology context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of constructed features. |

---

### `M_NUMBER_OF_FEATURES`

Inquires the total number of features in the template of the metrology context. This total includes both measured features and constructed features.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the total number of features. |

---

### `M_NUMBER_OF_MEASURED_FEATURES`

Inquires the number of measured features in the template of the metrology context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of measured features. |

---

### `M_NUMBER_OF_TOLERANCES`

Inquires the number of geometric tolerances in the template of the metrology context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of geometric tolerances. |

---

### `M_TEMPLATE_REFERENCE_ID`

Inquires the identifier of the template reference (image) that was added to the context.  Note that this is the identifier of an internal copy of the buffer specified when calling [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_TEMPLATE_REFERENCE_ID`](../../Reference/met/MmetControl.md). You should not try to free this internal buffer using [`MbufFree`](../../Reference/buf/MbufFree.md). If you want to disassociate it from the metrology context, call [`MmetControl`](../../Reference/met/MmetControl.md) with [`M_TEMPLATE_REFERENCE_ID`](../../Reference/met/MmetControl.md) set to [`M_NULL`](../../Reference/met/MmetControl.md).

| Value | Description |
| --- | --- |
| `M_NULL` | Releases the template reference buffer. |
| `Image identifier` | Specifies the identifier of the buffer. |
| `M_NULL` | Specifies that there is no template reference image associated with the context. |

---

### `M_TEMPLATE_REFERENCE_SIZE_BAND`

Inquires the number of bands in the template reference image.

| Value | Description |
| --- | --- |
| `1 <= Value <= 3` | Specifies the number of bands. |

---

### `M_TEMPLATE_REFERENCE_SIZE_X`

Inquires the width of the template reference.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the template reference's width. This value must be in pixel units. |

---

### `M_TEMPLATE_REFERENCE_SIZE_Y`

Inquires the height of the template reference.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the template reference's height. This value must be in pixel units. |

---

### `M_TEMPLATE_REFERENCE_TYPE`

Inquires the data type and the data depth (per band) of the template reference.

| Value | Description |
| --- | --- |
| `Value` | Specifies the template reference's data type and data depth. |

---

### `M_TIMEOUT`

Inquires the maximum measurement-and-validation time for [`MmetCalculate`](../../Reference/met/MmetCalculate.md), in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies an infinite amount of measurement and validation time. |
| `Value > 0.0` | Specifies the maximum measurement and validation time, in msec. |

### For inquiring about the system

To inquire about the system on which either the metrology context or metrology result buffer has been allocated, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to the value below.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which either the metrology context or metrology result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

### For inquiring global processing settings

To inquire about the processing settings for a measured feature, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. To inquire global processing settings, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to an existing measured feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**.

---

### `M_CHAIN_ALL_NEIGHBORS`

Inquires whether edge chains are built with as much edgel information as possible.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that edge chains are built with the least amount of edgel information possible. |
| `M_ENABLE` *(default)* | Specifies that edge chains are built with as much edgel information as possible. |

---

### `M_EXTRACTION_SCALE`

Inquires the image scale at which to extract the edges.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the extraction scale. |

---

### `M_FILL_GAP_ANGLE`

Inquires the aperture angle where the feature extraction searches for edge extremity candidates when filling gaps in features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the aperture angle, in degrees. |

---

### `M_FILL_GAP_DISTANCE`

Inquires the maximum distance radius where the feature extraction searches for edge extremity candidates when filling gaps in features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_INFINITE` | Specifies an infinite maximum distance radius. |
| `Value >= 0.0` | Specifies the maximum distance radius, in pixels. |

---

### `M_FILTER_SMOOTHNESS`

Inquires the degree of smoothness (strength of denoising) of the edge extraction filter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the smoothness value. |

---

### `M_FILTER_TYPE`

Inquires the type of filter used when performing the edge extraction.

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

Inquires the mode in which to perform the edge extraction filter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edge extractions are not forced to be performed using floating-point precision calculations. |
| `M_ENABLE` | Specifies that all edge extractions are forced to be performed using floating-point precision calculations. |

---

### `M_MAGNITUDE_TYPE`

Inquires the type of magnitude value used to calculate the magnitude of the edge at each edgel position.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NORM` | Specifies that the magnitude will be used. |
| `M_SQR_NORM` *(default)* | Specifies that the square of the magnitude will be used. |

---

### `M_REGION_ACCURACY_HIGH`

Inquires the accuracy with which you define the metrology region associated with a feature when dealing with a calibrated image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that high accuracy is not used when defining a metrology region. |
| `M_ENABLE` *(default)* | Specifies that high accuracy is used when defining a metrology region. |

---

### `M_THRESHOLD_MODE`

Inquires the threshold mode of the edge extraction. Note that lower threshold values result in a more sensitive edgel detection.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies no threshold. |
| `M_HIGH` *(default)* | Specifies a high threshold. |
| `M_LOW` | Specifies a low threshold. |
| `M_MEDIUM` | Specifies a medium threshold. |
| `M_USER_DEFINED` | Specifies that the threshold values will be user-defined. |
| `M_VERY_HIGH` | Specifies a very high threshold. |

---

### `M_THRESHOLD_TYPE`

Inquires the type of the hysteresis threshold used when performing the edge extraction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FULL_HYSTERESIS` | Specifies that the lower bound threshold value is 0.0. |
| `M_HYSTERESIS` *(default)* | Specifies that both the lower bound threshold value and the upper bound threshold value will be used. |
| `M_NO_HYSTERESIS` | Specifies that the lower bound threshold value is equal to the upper bound threshold value. |

---

### `M_THRESHOLD_VALUE_HIGH`

Inquires the user-defined upper bound of the hysteresis threshold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the upper bound of the hysteresis threshold. |

---

### `M_THRESHOLD_VALUE_LOW`

Inquires the user-defined lower bound of the hysteresis threshold.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the lower bound of the hysteresis threshold. |

### For inquiring about features

To inquire about the features of a metrology context, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. For inquiring feature settings, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to one of the following: an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**, or [`M_GLOBAL_FRAME`](../../Reference/met/MmetInquire.md).

---

### `M_ACTIVE`

Inquires whether the feature is enabled for calculation.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies not to calculate the feature. |
| `M_ENABLE` | Specifies to calculate the feature. |

---

### `M_ANGLE_END`

Inquires the end angle of a constructed arc. The angle is relative to the reference frame's axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_ANGLE_START`

Inquires the start angle of a constructed arc. The angle is relative to the reference frame's axis.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value < 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_DEPENDENCIES`

Inquires whether other features or tolerances have been constructed or validated based on the specified feature.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that other features or tolerances have not been constructed or validated based on the specified feature. |
| `M_TRUE` | Specifies that other features or tolerances have been constructed or validated based on the specified feature. |

---

### `M_EDGEL_ANGLE_RANGE`

Inquires the angular range within which an edgel's gradient angle must fall for Aurora Imaging Library to consider it an active edgel.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angular range, in degrees. |

---

### `M_EDGEL_DENOISING_MODE`

Inquires how to denoise edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies no denoising. |
| `M_GAUSSIAN` | Specifies a Gaussian type of denoising. |
| `M_MEAN` | Specifies a mean type of denoising. |
| `M_MEDIAN` | Specifies a median type of denoising. |

---

### `M_EDGEL_DENOISING_RADIUS`

Inquires the radius by which to denoise edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the radius, in pixels. |

---

### `M_EDGEL_DENOISING_USE_ORDER`

Inquires whether Aurora Imaging Library should use and maintain the order of the edgels when denoising them.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library should not use and maintain the order of the edgels when denoising them. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library should use and maintain the order of the edgels when denoising them. |

---

### `M_EDGEL_RELATIVE_ANGLE`

Inquires the relative angle from which to measure the gradient angle range ([`M_EDGEL_ANGLE_RANGE`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_REVERSE` | Specifies that the gradient angle range is measured relative to the reverse angle (orientation) of the region of interest (+ 180°). |
| `M_SAME` *(default)* | Specifies that the gradient angle range is measured relative to the same angle (orientation) as the region of interest. |
| `M_SAME_OR_REVERSE` | Specifies that the gradient angle range is measured relative to either the same or the reverse angle (orientation) of the region of interest. |

---

### `M_EDGEL_SELECTION_RANK`

Inquires which edgels to select in each column of the metrology region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that all edgels are selected. |
| `M_LAST` | Specifies the last edgel in each column, relative to the metrology region. |
| `Value > 0` | Specifies which edgels are selected, based on their rank, relative to the metrology region. |

---

### `M_EDGEL_TYPE`

Inquires the type of edgels that will be used for constructed edgel features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACTIVE_EDGELS` *(default)* | Specifies to use the active edgels of the measured feature's metrology region. |
| `M_ALL_EDGELS` | Specifies to use all the edgels of the measured feature's metrology region. |
| `M_FITTED_EDGELS` | Specifies to use the fitted edgels of the measured feature's metrology region. |

---

### `M_FEATURE_GEOMETRY`

Inquires the geometry of the specified feature.

| Value | Description |
| --- | --- |
| *(see [`Geometry`](Reference/met/MmetAddFeature.md))* |  |

---

### `M_FEATURE_TYPE`

Inquires the type of the specified feature.

| Value | Description |
| --- | --- |
| *(see [`FeatureType`](Reference/met/MmetAddFeature.md))* |  |

---

### `M_FIT_COVERAGE_MIN`

Inquires the approximate portion of the feature that must be covered by fitted edgels for a successful fit.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 100.0` *(default)* | Specifies the minimum coverage, as a percentage. |

---

### `M_FIT_DISTANCE_MAX`

Inquires the greatest possible gap between an active edgel and an iterative fit for the active edgel to be considered during the next iteration.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_INFINITE` *(default)* | Specifies no maximum distance. |
| `Value` | Specifies the maximum distance, in pixel units. |

---

### `M_FIT_DISTANCE_OUTLIERS`

Inquires whether to automatically determine the distance that establishes outliers (edgels/points), or to use a user-defined distance. Outliers are excluded from the robust best fit. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetInquire.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically determines the distance. |
| `M_USER_DEFINED` | Specifies that the distance is set with [`M_FIT_DISTANCE_OUTLIERS_VALUE`](../../Reference/met/MmetInquire.md). |

---

### `M_FIT_DISTANCE_OUTLIERS_VALUE`

Inquires the explicit distance value that Aurora Imaging Library uses to establish outliers (outlier edgels/points), which are excluded from the robust best fit. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetInquire.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetInquire.md), and [`M_FIT_DISTANCE_OUTLIERS`](../../Reference/met/MmetInquire.md) must be set to [`M_USER_DEFINED`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the distance; value must be greater than 0.0. |

---

### `M_FIT_INTERNAL_NUMBER_OF_POINTS`

Inquires the number of points from which Aurora Imaging Library performs the robust fit to build the feature. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md); also, [`M_FIT_TYPE`](../../Reference/met/MmetInquire.md) must be set to [`M_ROBUST_FIT`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library automatically determines the number of points. |
| `Value` | Specifies the number of points. |

---

### `M_FIT_ITERATIONS_MAX`

Inquires the maximum number of fit iterations used to compute the feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum number of fit iterations will be automatically determined. |
| `Value >= 1` | Specifies the maximum number of fit iterations. |

---

### `M_FIT_TYPE`

Inquires the type of best fit with which to build the feature. This only applies to features built using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_FIT`](../../Reference/met/MmetAddFeature.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ROBUST_FIT` | Specifies a robust best fit. |
| `M_STANDARD_FIT` *(default)* | Specifies a standard best fit. |

---

### `M_FIT_VARIATION_MAX`

Inquires the maximum allowable difference in the value of the feature's underlying coefficients, from one fit iteration to the next.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum variation will be determined automatically. |
| `0.0 <= Value <= 100.0` | Specifies the maximum variation, as a percentage. |

---

### `M_LINE_A`

Inquires the coefficient _A_ of the equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient's value. |

---

### `M_LINE_B`

Inquires the coefficient _B_ of the equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the coefficient's value. |

---

### `M_LINE_BISECTOR_TYPE`

Inquires how to construct a line feature when building it as a bisector. This only applies when using [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_CONSTRUCTED`](../../Reference/met/MmetAddFeature.md) set to [`M_LINE`](../../Reference/met/MmetAddFeature.md) and [`M_BISECTOR`](../../Reference/met/MmetAddFeature.md), and specifying two lines as the base features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_BIGGEST_ANGLE` | Specifies that the newly constructed line is built at the middle of the biggest angle formed by the two lines specified as base features. |
| `M_FEATURE_ORDER` *(default)* | Specifies that the newly constructed line is built according to the order in which you specified the two line base features; that is, at the middle of the angle formed between the first and second specified lines. |
| `M_SMALLEST_ANGLE` | Specifies that the newly constructed line is built at the middle of the smallest angle formed by the two lines specified as base features. |

---

### `M_LINE_C`

Inquires the coefficient _C_ of the equation for a parametrically constructed line.  The line equation is: `_A_ _x_ + _B_ _y_ + _C_ = 0`.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `-0.5 * AIL_INT32_MAX <= Value <= 0.5 * AIL_INT32_MAX` *(default)* | Specifies the coefficient's value. |

---

### `M_NUMBER_MAX`

Inquires the maximum number of subfeatures to calculate in a multiple feature (for point features only).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum number of subfeatures. |

---

### `M_NUMBER_MIN`

Inquires the minimum number of subfeatures to calculate in a multiple feature (for point features only).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the minimum number of subfeatures. |

---

### `M_OCCURRENCE`

Inquires the point to use if there is more than one candidate when adding a constructed point feature built at the intersection of two base features ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with [`M_POINT`](../../Reference/met/MmetAddFeature.md) and [`M_INTERSECTION`](../../Reference/met/MmetAddFeature.md) or [`M_EXTENDED_INTERSECTION`](../../Reference/met/MmetAddFeature.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the index value. |

---

### `M_OPERATION`

Inquires the operation used to build the specified feature.

| Value | Description |
| --- | --- |
| *(see [`BuildOperation`](Reference/met/MmetAddFeature.md))* |  |

---

### `M_POSITION`

Inquires the position, along the contour of the specified base feature, at which to construct the point feature ([`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) with with [`M_POINT`](../../Reference/met/MmetAddFeature.md) and [`M_POSITION_ABSOLUTE`](../../Reference/met/MmetAddFeature.md) or [`M_POSITION_RELATIVE`](../../Reference/met/MmetAddFeature.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the position value. |

---

### `M_POSITION_END_X`

Inquires the X-coordinate of the end point of a parametrically constructed segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_END_Y`

Inquires the Y-coordinate of the end point of a parametrically constructed segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_POSITION_START_X`

Inquires the X-coordinate of the start point of a parametrically constructed segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_START_Y`

Inquires the Y-coordinate of the start point of a parametrically constructed segment.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_POSITION_X`

Inquires the X-coordinate of the center of a constructed circle or arc, or the X-coordinate of the position of a constructed local frame or point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-coordinate, in pixel or world units. |

---

### `M_POSITION_Y`

Inquires the Y-coordinate of the center of a constructed circle or arc, or the Y-coordinate of the position of a constructed local frame or point.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-coordinate, in pixel or world units. |

---

### `M_RADIUS`

Inquires the radius of a constructed circle or arc.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0.0` *(default)* | Specifies the radius, in pixel or world units. |

---

### `M_REFERENCE_FRAME`

Inquires the reference frame for any feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies that the reference frame is the global frame. |
| `Value > 0` | Specifies the label of the reference frame. |

---

### `M_SORT`

Inquires the sorting order that Aurora Imaging Library applies to the index of subpoints, when retrieving results of multiple point features.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_SORT_DOWN` | Specifies that results of multiple point features are sorted in descending order, according to the subpoint's index. |
| `M_SORT_UP` *(default)* | Specifies that results of multiple point features are sorted in ascending order, according to the subpoint's index. |

### For inquiring about constructed edgel features

To inquire about constructed edgel features, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. In this case, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to the label or index of the feature, using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**, and the [`MetId`](../../Reference/met/MmetInquire.md) parameter must be set to a metrology context.

---

### `M_EDGEL_ANGLE`

Inquires the gradient angles (edgel direction) of the edgels of the edgel feature, constructed from externally established edgels/points (using [`MmetPut`](../../Reference/met/MmetPut.md)).  If the edgels of the edgel feature were constructed from externally established points ([`MmetPut`](../../Reference/met/MmetPut.md) with only X and Y coordinates) and their gradient angle was not calculated ([`M_INTERPOLATE_ANGLE`](../../Reference/met/MmetPut.md) was not specified), the angle of these edgels is returned as [`M_DISABLE`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that no angle was specified. |
| `Value` | Specifies the angles. |

---

### `M_EDGEL_POSITION_X`

Inquires the X-coordinates of the edgels of the edgel feature, constructed from externally established edgels/points (using [`MmetPut`](../../Reference/met/MmetPut.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the X-coordinates, in pixel or world units. |

---

### `M_EDGEL_POSITION_Y`

Inquires the Y-coordinates of the edgels of the edgel feature, constructed from externally established edgels/points (using [`MmetPut`](../../Reference/met/MmetPut.md)).

| Value | Description |
| --- | --- |
| `Value` | Specifies the Y-coordinates, in pixel or world units. |

---

### `M_EDGEL_PROVIDED_ORDER`

Inquires whether the externally established egdels/points used to construct the edgel feature are listed in a meaningful order.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies unordered edgels/points. |
| `M_SEQUENTIAL` | Specifies ordered edgels/points. |

---

### `M_EDGEL_RESAMPLING_LINE_ANGLE`

Inquires the angle of a line along which to resample edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable resampling along a line. |
| `Value >= 0.0` | Specifies the angle of the line along which to resample edgels. |

---

### `M_EDGEL_RESAMPLING_MODE`

Inquires how to resample edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NULL` *(default)* | Specifies no resampling. |
| `M_MEAN` | Specifies a mean type of resampling. |
| `M_MEDIAN` | Specifies a median type of resampling. |

---

### `M_EDGEL_RESAMPLING_RADIUS`

Inquires the radius by which to resample edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the radius. |

---

### `M_EDGEL_RESAMPLING_USE_ORDER`

Inquires whether Aurora Imaging Library should use and maintain the order of the edgels when resampling them.

| Value | Description |
| --- | --- |
| `M_DISABLE` | Specifies that Aurora Imaging Library should not use and maintain the order of the edgels then resampling them. |
| `M_ENABLE` *(default)* | Specifies that Aurora Imaging Library should use and maintain the order of the edgels when resampling them. |

---

### `M_GROUP_EDGEL_GAP_SIZE`

Inquires the maximum allowable gap (distance) between edgels for Aurora Imaging Library to consider them part of the same group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that Aurora Imaging Library should automatically determine the gap size. |
| `Value >= 0.0` | Specifies the gap size, in pixels. |

---

### `M_GROUP_MIN_NUMBER_OF_EDGELS`

Inquires the minimum number of edgels a group must have.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_NONE` *(default)* | Specifies that there is no minimum number of edgels that a group must have. |
| `Value > 0` | Specifies the minimum number of edgels that a group must have. |

---

### `M_GROUP_SELECTION_MODE`

Inquires how to group edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_CLOSEST_TO_POINT` | Specifies to select one group, based on its proximity to a point or the local frame's origin. |
| `M_OCCURRENCE` *(default)* | Specifies to select one or all group(s), based on the index of the group of edgels. |

---

### `M_GROUP_SELECTION_OCCURRENCE`

Inquires whether to use one or all edgels to establish the group.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to use all groups of edgels. |
| `Value > 0` | Specifies the index of the group of edgels to select. |

---

### `M_GROUP_SELECTION_POINT`

Inquires the point from which Aurora Imaging Library takes the distance to group edgels.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL_FRAME` *(default)* | Specifies a virtual point at the location (origin) of the global frame. |
| `Value > 0` | Specifies the label of an existing point feature or local frame feature. |

### For inquiring the geometry of the metrology region

To inquire the geometry of the metrology region, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to the following value. In this case, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to an existing measured feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()** (for an explicitly-defined or 2D graphics list metrology region) or [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetInquire.md) (for a derived metrology region), and the [`MetId`](../../Reference/met/MmetInquire.md) parameter can be set to a metrology context or a derived metrology region object.

---

### `M_REGION_GEOMETRY`

Inquires the geometry of the metrology region.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Same as [`M_INFINITE`](../../Reference/met/MmetInquire.md). |
| `M_ARC` | Specifies an arc region. |
| `M_INFINITE` | Specifies an infinite region. |
| `M_RECTANGLE` | Specifies a rectangle region. |
| `M_RING` | Specifies a ring-shaped region. |
| `M_RING_SECTOR` | Specifies a ring-sector region. |
| `M_SEGMENT` | Specifies a linear segment region. |

### For inquiring the geometry data of the metrology region

To inquire the geometry data required to define a metrology region, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. In this case, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to an existing measured feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()** (for an explicitly-defined or 2D graphics list metrology region) or [`M_DERIVED_GEOMETRY_REGION`](../../Reference/met/MmetInquire.md) (for a derived metrology region), and the [`MetId`](../../Reference/met/MmetInquire.md) parameter can be set to a metrology context or a derived metrology region object, unless otherwise specified.

---

### `M_REGION_ANGLE`

Inquires the angle of the metrology region. [`M_REGION_ANGLE`](../../Reference/met/MmetInquire.md) applies to [`M_INFINITE`](../../Reference/met/MmetInquire.md) and [`M_RECTANGLE`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value` | Specifies the angle.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_ANGLE`](../../Reference/met/MmetInquire.md) returns an explicit angle value, in degrees.  For a derived metrology region, use [`M_REGION_ANGLE_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_ANGLE`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the angle, or an explicit angle value, in degrees. |

---

### `M_REGION_ANGLE_END`

Inquires the end angle of the metrology region. [`M_REGION_ANGLE_END`](../../Reference/met/MmetInquire.md) applies to [`M_ARC`](../../Reference/met/MmetInquire.md) and [`M_RING_SECTOR`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value` | Specifies the end angle.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_ANGLE_END`](../../Reference/met/MmetInquire.md) returns an explicit angle value, in degrees.  For a derived metrology region, use [`M_REGION_ANGLE_END_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_ANGLE_END`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the angle, or an explicit angle value, in degrees. |

---

### `M_REGION_ANGLE_END_TYPE`

Inquires whether the end angle of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_ANGLE_END`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_ANGLE_START`

Inquires the start angle of the metrology region. [`M_REGION_ANGLE_START`](../../Reference/met/MmetInquire.md) applies to [`M_ARC`](../../Reference/met/MmetInquire.md) and [`M_RING_SECTOR`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value` | Specifies the start angle.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_ANGLE_START`](../../Reference/met/MmetInquire.md) returns an explicit angle value, in degrees.  For a derived metrology region, use [`M_REGION_ANGLE_START_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_ANGLE_START`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the angle, or an explicit angle value, in degrees. |

---

### `M_REGION_ANGLE_START_TYPE`

Inquires whether the start angle of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_ANGLE_START`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_ANGLE_TYPE`

Inquires whether the angle of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_ANGLE`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_END`

Inquires the derived metrology region's end point according to the specified feature. [`M_REGION_END`](../../Reference/met/MmetInquire.md) applies to derived metrology regions with an [`M_SEGMENT`](../../Reference/met/MmetInquire.md) geometry.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the end point of the metrology region. |

---

### `M_REGION_END_TYPE`

Inquires whether the end point of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_END`](../../Reference/met/MmetInquire.md) or [`M_REGION_END_X`](../../Reference/met/MmetInquire.md) and [`M_REGION_END_Y`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_END_X`

Inquires the X-coordinate of the metrology region's end point. [`M_REGION_END_X`](../../Reference/met/MmetInquire.md) applies to [`M_SEGMENT`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value` | Specifies the end point's X-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_END_X`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_END_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_END_X`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the end point, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_END_Y`

Inquires the Y-coordinate of the metrology region's end point. [`M_REGION_END_Y`](../../Reference/met/MmetInquire.md) applies to [`M_SEGMENT`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value` | Specifies the end point's Y-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_END_Y`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_END_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_END_Y`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the end point, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_HEIGHT`

Inquires the height of the metrology region. [`M_REGION_HEIGHT`](../../Reference/met/MmetInquire.md) applies to [`M_RECTANGLE`](../../Reference/met/MmetInquire.md) metrology regions.

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the height.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_HEIGHT`](../../Reference/met/MmetInquire.md) returns an explicit height value, in pixel or world units.  For a derived metrology region, use [`M_REGION_HEIGHT_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_HEIGHT`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the height, or an explicit height value, in pixel or world units. |

---

### `M_REGION_HEIGHT_TYPE`

Inquires whether the height of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_HEIGHT`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_POSITION`

Inquires the derived metrology region's position according to the specified feature. [`M_REGION_POSITION`](../../Reference/met/MmetInquire.md) applies to derived metrology regions with any geometry except [`M_SEGMENT`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the position of the metrology region. |

---

### `M_REGION_POSITION_TYPE`

Inquires whether the position of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_POSITION`](../../Reference/met/MmetInquire.md) or [`M_REGION_POSITION_X`](../../Reference/met/MmetInquire.md) and [`M_REGION_POSITION_Y`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_POSITION_X`

Inquires the X-coordinate of the metrology region's position. [`M_REGION_POSITION_X`](../../Reference/met/MmetInquire.md) applies to any geometry except [`M_SEGMENT`](../../Reference/met/MmetInquire.md), unless otherwise specified.

| Value | Description |
| --- | --- |
| `Value` | Specifies the position's X-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_POSITION_X`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_POSITION_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_POSITION_X`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the position, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_POSITION_Y`

Inquires the Y-coordinate of the metrology region's position. [`M_REGION_POSITION_Y`](../../Reference/met/MmetInquire.md) applies to any geometry except [`M_SEGMENT`](../../Reference/met/MmetInquire.md), unless otherwise specified.

| Value | Description |
| --- | --- |
| `Value` | Specifies the position's Y-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_POSITION_Y`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_POSITION_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_POSITION_Y`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the position, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_RADIUS`

Inquires the radius of the metrology region. [`M_REGION_RADIUS`](../../Reference/met/MmetInquire.md) applies to [`M_ARC`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the radius.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_RADIUS`](../../Reference/met/MmetInquire.md) returns an explicit radius value, in pixel or world units.  For a derived metrology region, use [`M_REGION_RADIUS_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_RADIUS`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the radius, or an explicit radius value, in pixel or world units. |

---

### `M_REGION_RADIUS_END`

Inquires the end radius defining the metrology region. [`M_REGION_RADIUS_END`](../../Reference/met/MmetInquire.md) applies to [`M_RING`](../../Reference/met/MmetInquire.md) or [`M_RING_SECTOR`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the end radius.  For an explicitly-defined metrology region, [`M_REGION_RADIUS_END`](../../Reference/met/MmetInquire.md) returns an explicit radius value, in pixel or world units.  For a derived metrology region, use [`M_REGION_RADIUS_END_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_RADIUS_END`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the end radius, or an explicit radius value, in pixel or world units. |

---

### `M_REGION_RADIUS_END_TYPE`

Inquires whether the end radius of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_RADIUS_END`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_RADIUS_START`

Inquires the start radius defining the metrology region. [`M_REGION_RADIUS_START`](../../Reference/met/MmetInquire.md) applies to [`M_RING`](../../Reference/met/MmetInquire.md) or [`M_RING_SECTOR`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the start radius.  For an explicitly-defined metrology region, [`M_REGION_RADIUS_START`](../../Reference/met/MmetInquire.md) returns an explicit radius value, in pixel or world units.  For a derived metrology region, use [`M_REGION_RADIUS_START_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_RADIUS_START`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the start radius, or an explicit radius value, in pixel or world units. |

---

### `M_REGION_RADIUS_START_TYPE`

Inquires whether the start radius of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_RADIUS_START`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_RADIUS_TYPE`

Inquires whether the radius of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_RADIUS`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_START`

Inquires the derived metrology region's start point according to the specified feature. [`M_REGION_START`](../../Reference/met/MmetInquire.md) applies to derived metrology regions with an [`M_SEGMENT`](../../Reference/met/MmetInquire.md) geometry.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label of a point feature, which metrology uses to set the start point of the metrology region. |

---

### `M_REGION_START_TYPE`

Inquires whether the start point of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_START`](../../Reference/met/MmetInquire.md) or [`M_REGION_START_X`](../../Reference/met/MmetInquire.md) and [`M_REGION_START_Y`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

---

### `M_REGION_START_X`

Inquires the X-coordinate of the metrology region's start point. [`M_REGION_START_X`](../../Reference/met/MmetInquire.md) applies to [`M_SEGMENT`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the start point's X-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_START_X`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_START_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_START_X`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the start point, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_START_Y`

Inquires the Y-coordinate of the metrology region's start point. [`M_REGION_START_Y`](../../Reference/met/MmetInquire.md) applies to [`M_SEGMENT`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value` | Specifies the start point's Y-coordinate.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_START_Y`](../../Reference/met/MmetInquire.md) returns an explicit coordinate value, in pixel or world units.  For a derived metrology region, use [`M_REGION_START_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_START_Y`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the start point, or an explicit coordinate value, in pixel or world units. |

---

### `M_REGION_WIDTH`

Inquires the width of the metrology region. [`M_REGION_WIDTH`](../../Reference/met/MmetInquire.md) applies to [`M_RECTANGLE`](../../Reference/met/MmetInquire.md).

| Value | Description |
| --- | --- |
| `Value >= 0.0` | Specifies the width.  For an explicitly-defined or 2D graphics list metrology region, [`M_REGION_WIDTH`](../../Reference/met/MmetInquire.md) returns an explicit width value, in pixel or world units.  For a derived metrology region, use [`M_REGION_WIDTH_TYPE`](../../Reference/met/MmetInquire.md) to determine whether [`M_REGION_WIDTH`](../../Reference/met/MmetInquire.md) returns the label of the feature used to establish the width, or an explicit width value, in pixel or world units. |

---

### `M_REGION_WIDTH_TYPE`

Inquires whether the width of the derived metrology region was specified with the label of a feature or an explicit value ([`M_REGION_WIDTH`](../../Reference/met/MmetInquire.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_LABEL_VALUE` | Specifies a feature. |
| `M_PARAMETRIC` *(default)* | Specifies an explicit value. |

### For inquiring about geometric tolerances

To inquire about the geometric tolerance settings of a metrology context, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. For inquiring geometric tolerances, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to an existing tolerance label or index using **M_TOLERANCE_LABEL()** or **M_TOLERANCE_INDEX()**. Unless otherwise specified, the following settings apply to any geometric tolerance ([`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md)).

---

### `M_AREA_BETWEEN_CURVES_DEFINE_SIDE`

Inquires the reference frame with which to determine the side of the curve of edgels to use to establish the area, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance, and enable the [`M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`](../../Reference/met/MmetInquire.md) setting.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the label value of a local frame feature. |
| `M_FEATURE_LABEL` | Specifies the label value of a local frame feature. |
| `M_GLOBAL_FRAME` *(default)* | Specifies the global frame. |

---

### `M_AREA_BETWEEN_CURVES_ONE_SIDE_ONLY`

Inquires whether to establish the area by considering only one side of the curve of edgels, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance. This only has an effect if [`M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`](../../Reference/met/MmetInquire.md) is disabled and Aurora Imaging Library only uses positive areas.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not validate the area on only one side of the curve. |
| `M_ENABLE` | Specifies that Aurora Imaging Library validates the area on only one side of the curve. |

---

### `M_AREA_BETWEEN_CURVES_OPPOSITES_SUBTRACT`

Inquires whether Aurora Imaging Library should subtract opposing areas when establishing the area tolerance for [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not subtract opposing areas. |
| `M_ENABLE` | Specifies that Aurora Imaging Library subtracts the areas on one side of the curve (opposing areas) from the areas on the other side of the curve, so that the result is positive. |

---

### `M_AREA_UNDER_CURVE_ALLOW_NEGATIVE`

Inquires whether Aurora Imaging Library allows negative (opposing) areas when establishing the area tolerance for [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md) or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that Aurora Imaging Library does not allow negative areas when establishing the area tolerance. |
| `M_ENABLE` | Specifies that Aurora Imaging Library allows negative areas when establishing the area tolerance. |

---

### `M_CURVE_EDGEL_GAP_SIZE`

Inquires the maximum gap between edgels for Aurora Imaging Library to consider them part of the same curve of edgels, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md), [`M_AREA_UNDER_CURVE_MAX`](../../Reference/met/MmetAddTolerance.md), or [`M_AREA_UNDER_CURVE_MIN`](../../Reference/met/MmetAddTolerance.md) tolerance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL_SIZE_DIAGONAL` *(default)* | Specifies twice the diagonal size of the pixels from which the edgels were taken. |
| `Value > 0.0` | Specifies the size, in pixels. |

---

### `M_CURVE_INFO`

Inquires the information that Aurora Imaging Library uses to establish the curves of edgels, when you specify an [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_FEATURE_INDEX` | Specifies the index of a reference frame feature (local frame). |
| `M_FEATURE_LABEL` | Specifies the label of a reference frame feature (local frame). |
| `M_AUTO_CURVE_CLOSED` | Specifies that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance is validating bounded (closed) curves (edgel features). |
| `M_AUTO_CURVE_OPENED` *(default)* | Specifies that the [`M_AREA_BETWEEN_CURVES`](../../Reference/met/MmetAddTolerance.md) tolerance is validating unbounded (opened) curves (edgel features). |

---

### `M_DISTANCE_MODE`

Inquires how to apply the angle at which to establish the distance between features when adding a distance tolerance.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` *(default)* | Specifies that Aurora Imaging Library establishes the angle by taking the distance at which the features are either the farthest (for [`M_DISTANCE_MAX`](../../Reference/met/MmetAddTolerance.md)) or the closest (for [`M_DISTANCE_MIN`](../../Reference/met/MmetAddTolerance.md)). |
| `M_FERET_AT_ANGLE` | Specifies that Aurora Imaging Library calculates the distance tolerance using the feret at the angle that is set ([`M_ANGLE`](../../Reference/met/MmetInquire.md)). |
| `M_GAP_AT_ANGLE` | Specifies that Aurora Imaging Library calculates the distance tolerance using the gap at the angle that is set ([`M_ANGLE`](../../Reference/met/MmetInquire.md)). |

---

### `M_GAIN`

Inquires the gain Aurora Imaging Library uses to correct systematic bias between multiple imaging vision set-ups.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the gain. |

---

### `M_OFFSET`

Inquires the offset Aurora Imaging Library uses to correct systematic bias between multiple imaging vision set-ups.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the offset. |

---

### `M_TOLERANCE_TYPE`

Inquires the type of the geometric tolerance.

| Value | Description |
| --- | --- |
| `M_ANGULARITY` | Adds an angularity tolerance. |
| `M_AREA_BETWEEN_CURVES` | Adds an area tolerance for the area between two curves of edgels. |
| `M_AREA_CONVEX_HULL` | Adds an area tolerance, based on the convex hull, for the surface of an edgel, point, or circle feature. |
| `M_AREA_SIMPLE` | Adds a simple area tolerance for the surface of an edgel, point, or circle feature. |
| `M_AREA_UNDER_CURVE_MAX` | Adds an area tolerance for the maximum area under a curve of edgels or points. |
| `M_AREA_UNDER_CURVE_MIN` | Adds an area tolerance for the minimum area under a curve of edgels. |
| `M_CONCENTRICITY` | Adds a concentricity tolerance. |
| `M_DISTANCE_MAX` | Adds a maximum distance tolerance. |
| `M_DISTANCE_MIN` | Adds a minimum distance tolerance. |
| `M_LENGTH` | Adds a length tolerance. |
| `M_PARALLELISM` | Adds a parallelism tolerance. |
| `M_PERIMETER_CONVEX_HULL` | Adds a perimeter tolerance, based on the convex hull, for the surface of an edgel, point, or circle feature. |
| `M_PERIMETER_SIMPLE` | Adds a simple perimeter tolerance for the surface of an edgel, point, or circle feature. |
| `M_PERPENDICULARITY` | Adds a perpendicularity tolerance. |
| `M_POSITION_X` | Adds a positioning tolerance, along the X-direction of the specified reference frame. |
| `M_POSITION_Y` | Adds a positioning tolerance, along the Y-direction of the specified reference frame. |
| `M_RADIUS` | Adds a radius tolerance. |
| `M_ROUNDNESS` | Adds a roundness tolerance. |
| `M_STRAIGHTNESS` | Adds a straightness tolerance. |

---

### `M_VALUE_MAX`

Inquires the maximum possible value for a geometric tolerance. If a geometric tolerance value exceeds this maximum threshold, the calculation operation will fail and its status will be [`M_FAIL`](../../Reference/met/MmetGetResult.md). The status of the calculation of a tolerance is found using [`MmetGetResult`](../../Reference/met/MmetGetResult.md) with [`M_STATUS`](../../Reference/met/MmetGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the maximum value. |

---

### `M_VALUE_MIN`

Inquires the minimum required value for a geometric tolerance. If a geometric tolerance value does not meet this minimum requirement, the calculation operation will fail and its status will be [`M_FAIL`](../../Reference/met/MmetGetResult.md). The status of the calculation of a tolerance is found using [`MmetGetResult`](../../Reference/met/MmetGetResult.md)with [`M_STATUS`](../../Reference/met/MmetGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the minimum value. |

---

### `M_VALUE_WARNING_MAX`

Inquires the maximum warning value for a geometric tolerance. Geometric tolerance values that are greater or equal to [`M_VALUE_MIN`](../../Reference/met/MmetControl.md) and lower than [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) will result in a warning. Geometric tolerance values that are lower or equal to [`M_VALUE_MAX`](../../Reference/met/MmetControl.md)and greater than [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md) will also result in a warning.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EQUAL_PASS_MAX` *(default)* | Specifies that the maximum warning value is equal to the maximum pass value ([`M_VALUE_MAX`](../../Reference/met/MmetInquire.md)). |
| `Value` | Specifies the maximum warning value. |

---

### `M_VALUE_WARNING_MIN`

Inquires the minimum warning value for a geometric tolerance. Geometric tolerance values that are greater or equal to [`M_VALUE_MIN`](../../Reference/met/MmetControl.md)and lower than [`M_VALUE_WARNING_MIN`](../../Reference/met/MmetControl.md) will result in a warning. Geometric tolerance values that are lower or equal to [`M_VALUE_MAX`](../../Reference/met/MmetControl.md) and greater than [`M_VALUE_WARNING_MAX`](../../Reference/met/MmetControl.md) will also result in a warning.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_EQUAL_PASS_MIN` *(default)* | Specifies that the minimum warning value is equal to the minimum pass value ([`M_VALUE_MIN`](../../Reference/met/MmetInquire.md)). |
| `Value` | Specifies the minimum warning value. |

### For inquiring label or index information about a feature or geometric tolerance

To inquire label or index information about a feature or geometric tolerance ([`LabelOrIndex`](../../Reference/met/MmetInquire.md)), set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values.

---

### `?`

---

### `?`

---

### `M_BASE_FEATURES_ARRAY_SIZE`

Inquires the number of base features, for the feature or tolerance specified by the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter. This corresponds to the number of elements in the feature array ([`FeatureLabelArrayPtr`](../../Reference/met/MmetAddFeature.md)) in either [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) or [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of elements in the feature array. |

---

### `M_BASE_SUBFEATURES_ARRAY_SIZE`

Inquires the number of base subfeatures, for the feature or tolerance specified by the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter. This corresponds to the number of elements in the subfeature array ([`SubFeatureIndexArrayPtr`](../../Reference/met/MmetAddFeature.md)) in either [`MmetAddFeature`](../../Reference/met/MmetAddFeature.md) or [`MmetAddTolerance`](../../Reference/met/MmetAddTolerance.md).

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of elements in the subfeature array. |

---

### `M_INDEX_FROM_LABEL`

Inquires the index associated with the specified label, if the label is used. Pass the label of either a feature or tolerance using the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter set to **M_FEATURE_LABEL()** or **M_TOLERANCE_LABEL()**, respectively.

| Value | Description |
| --- | --- |
| `M_INVALID` | Specifies that the label is not associated with any feature (**M_FEATURE_LABEL()**) or any tolerance (**M_TOLERANCE_LABEL()**). |
| `Value >= 0` | Specifies the index that is associated with the specified label. |

---

### `M_LABEL_VALUE`

Inquires the label value of a specified feature or tolerance. For inquiring the feature's label, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to **M_FEATURE_INDEX()** or [`M_GLOBAL_FRAME`](../../Reference/met/MmetInquire.md). For inquiring the tolerance's label, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter can be set to **M_TOLERANCE_INDEX()**.

| Value | Description |
| --- | --- |
| `Value > 0` | Specifies the label value of the specified feature or tolerance. |

### For inquiring about features or geometric tolerances

To inquire about features or geometric tolerances of a metrology context, you can set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. In this case, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()** or an existing tolerance label or index using **M_TOLERANCE_LABEL()** or **M_TOLERANCE_INDEX()**, and the [`MetId`](../../Reference/met/MmetInquire.md) parameter must be set to a metrology context.

---

### `M_ANGLE`

Inquires the angle used to construct a feature or define a tolerance, when applicable.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the angle. |

---

### `M_DRAWABLE`

Inquires whether the feature or tolerance will be drawn using [`MmetDraw`](../../Reference/met/MmetDraw.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the feature or tolerance will not be drawn. |
| `M_ENABLE` *(default)* | Specifies that the feature or tolerance will be drawn. |

### For inquiring clone transformations settings

To inquire about the clone transformation settings used for a cloned feature in a metrology result buffer, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. For inquiring clone transformations, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to an existing feature label or index using **M_FEATURE_LABEL()** or **M_FEATURE_INDEX()**.

---

### `M_CLONE_ANGLE`

Inquires the rotation angle of the cloned feature in the counterclockwise direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the angle, in degrees. |

---

### `M_CLONE_OFFSET_X`

Inquires the translation value of the cloned feature, in the X-direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the X-offset for the cloned feature, in pixel or world units. |

---

### `M_CLONE_OFFSET_Y`

Inquires the translation value of the cloned feature, in the Y-direction.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value` *(default)* | Specifies the Y-offset for the cloned feature, in pixel or world units. |

---

### `M_CLONE_SCALE`

Inquires the scale factor of the cloned feature.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the scale factor. |

### For inquiring result buffer settings

To inquire about result buffer settings, set the [`InquireType`](../../Reference/met/MmetInquire.md) parameter to one of the following values. In this case, the [`LabelOrIndex`](../../Reference/met/MmetInquire.md) parameter must be set to [`M_GENERAL`](../../Reference/met/MmetInquire.md).

---

### `M_OUTPUT_FRAME`

Inquires the output reference frame that will be used to return the feature results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_GLOBAL_FRAME` *(default)* | Specifies that Aurora Imaging Library returns feature results relative to the origin of the global frame. |
| `M_IMAGE_FRAME` | Specifies that Aurora Imaging Library returns feature results relative to the origin of the target image's reference frame. |
| `M_REFERENCE_FRAME` | Specifies that Aurora Imaging Library returns feature results relative to the reference frame of each feature. |
| `Value > 0` | Specifies the label of an existing local frame feature to use to return feature results. |

---

### `M_RESULT_OUTPUT_UNITS`

Inquires whether results are returned in pixel or world units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### Combination Constants — For inquiring the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

#### `M_DEFAULT`

Inquires the default value of the specified inquire type.

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether a result is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported for the metrology context.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to the required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested information to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested information to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested information to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested information to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested information to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.
