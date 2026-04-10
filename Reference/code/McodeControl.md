---
doctype: Reference
module: code
function: McodeControl
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / code / McodeControl"
---

# McodeControl

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

> Control a code context, code model, or code result buffer setting.

## Syntax

```c
void McodeControl(
    AIL_ID     CodeId,       //out
    AIL_INT64  ControlType,  //in
    AIL_DOUBLE ControlValue  //in
)
```

## Description

This function changes the setting of a specified code context, code model, or code result buffer.

## Parameters

### `CodeId` *(out, AIL_ID)*

Specifies the identifier of the code context, code model, or code result buffer.

### `ControlType` *(in, AIL_INT64)*

Specifies the setting to change.

### `ControlValue` *(in, AIL_DOUBLE)*

Specifies the setting's new value.

## Parameter Associations

### Code context settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified for a code context, to control an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. In this case, a code context must be passed to the [`CodeId`](../../Reference/code/McodeControl.md) parameter.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although all these control types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the control types to determine if they can be activated for training; you can also use filters to limit the control types in the table to those that can be trained.

---

### `M_INITIALIZATION_MODE`

Changes the initialization mode of the context; this changes the default of several code context and code model settings. The default value of the setting in each mode is indicated within the description of the control type. When you change the initialization mode, control types that have already been set to some value (other than default) are not changed. Instead, the initialization mode only changes those control values that are set to their default value ([`M_DEFAULT`](../../Reference/code/McodeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMPROVED_RECOGNITION` | Specifies a code context that might provide a more robust [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. This initialization mode is recommended if your target images lack similarity (for example, some but not all include complex or noisy backgrounds), contain distorted code occurrences, or contain code occurrences that are at an angle or even flipped. It is also recommended when searching for 2D matrix codes that are composed of dots.  > **Note:** Note that when selecting this mode, the default of the affected code context and code model settings might change between Aurora Imaging Library releases to improve robustness, given new features or standard recommendations.  This initialization mode is not recommended with [`M_PHARMACODE`](../../Reference/code/McodeModel.md) code types. |
| `M_TYPICAL_RECOGNITION` *(default)* | Specifies a code context that might provide a quicker [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, but might potentially produce less robust results. |

---

### `M_MINIMUM_CONTRAST`

Sets the minimum possible contrast between the foreground and background in the target image for 1D code types (excluding [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_POSTNET`](../../Reference/code/McodeModel.md)), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type when using the [`M_ADAPTIVE`](../../Reference/code/McodeControl.md) threshold mode. For 1D code types (except for [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) and 2D cross-row code types, regardless of the threshold mode, [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) is also used to help the localization.  When using the [`M_ADAPTIVE`](../../Reference/code/McodeControl.md) threshold mode for 1D code types, the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type, increasing the minimum contrast will typically improve [`McodeRead`](../../Reference/code/McodeRead.md) operations, particularly in the presence of noise and non-uniform lighting. However, if the minimum contrast is higher than the contrast of a code, the code will not be read correctly. For 2D code types (excluding [`M_DOTCODE`](../../Reference/code/McodeModel.md) and [`M_MICROPDF417`](../../Reference/code/McodeModel.md)), the minimum contrast is determined automatically, so the adaptive threshold mode does not use the [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) setting.  For 1D code types (excluding [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_POSTNET`](../../Reference/code/McodeModel.md)) and 2D cross-row code types, regardless of threshold mode, you should set this control type if the contrast is less than 50 and there are difficulties decoding the code.  Note that training this control type ([`M_TRAIN`](../../Reference/code/McodeControl.md)) is only supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 50.  > **Note:** Note that setting [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) to [`M_DEFAULT`](../../Reference/code/McodeControl.md) is not equivalent to setting [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) to 50. When [`M_DEFAULT`](../../Reference/code/McodeControl.md) is specified, some internal steps might use a minimum contrast other than 50. |
| `0 <= Value <= 255` | Specifies the minimum contrast. Only integer values are accepted.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than or equal to 10. Only integer values are accepted. |

---

### `M_SCANLINE_HEIGHT`

Sets the height (or thickness) of the scanlines. A scanline is a line along which the code occurrence is read.  When locating a code occurrence, a scanline is usually first taken across the middle of the image or the image's region of interest (ROI), if one is associated. If a code occurrence is not found, a scanline is taken either higher or lower in the image or its ROI.  Usually, setting this control type to a higher value increases the robustness of [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), and [`McodeTrain`](../../Reference/code/McodeTrain.md) operations. This is especially true in noisy images. However, defects in the code can cause the need for a lower value. Using the default value is recommended.  > **Note:** Note that during an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, after locating the code, when taking scanlines within the inspection band, Aurora Imaging Library uses a scanline height of a single pixel regardless of the value of [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md), as per the ISO standard.  This control type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies a default scanline height that is based on the code type. The table below shows the default value for some code types.  | Code type | Default value (in pixel units) | | --- | --- | | Linear bar codes (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) | 5 | | MicroPDF 417 | 1 | | PDF 417/Truncated PDF417 | 3 | | Composite codes | 1 | |
| `Value` | Specifies the scanline height, relative to the input coordinate system specified using [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  When entering a value in pixel units, this value must be between 1 and 256, inclusive. The value entered is rounded to the nearest integer.  The recommended range of values for linear bar codes is: 1 &lt; Value &lt;= (BarcodeHeight / 2). For [`M_MICROPDF417`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), the recommended range of values is: 1 &lt;= Value &lt;= (RowHeight / 2). |

---

### `M_SCANLINE_INPUT_UNITS`

Sets the units with which to interpret the [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md) and [`M_SCANLINE_STEP`](../../Reference/code/McodeControl.md) control types. This essentially sets the input coordinate system to use.  This control type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_SCANLINE_STEP`

Sets the scanline step. The scanline step is the distance, or gap, between scanlines taken in the image or the image's region of interest (ROI), if one is associated.  In general, the higher the value for this control type, the faster the code is read, but the greater the chance of missing a readable line of code. Using the default value is recommended; the default value is dependent upon the search speed.  > **Note:** Note that during an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, after locating the code, when taking scanlines within the inspection band, Aurora Imaging Library uses a scanline step distance based on the height of the inspection band and the required number of scanlines, regardless of the value of [`M_SCANLINE_STEP`](../../Reference/code/McodeControl.md), as per the ISO standard; the ISO standard requires ten evenly spaced scanlines within the inspection band.  This control type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies a default scanline step that is based on the value of [`M_SPEED`](../../Reference/code/McodeControl.md).  When dealing with 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types, the following values are associated with [`M_DEFAULT`](../../Reference/code/McodeControl.md).  | [`M_SPEED`](../../Reference/code/McodeControl.md) value | Default value | | --- | --- | | [`M_LOW`](../../Reference/code/McodeControl.md) | 5 | | [`M_VERY_LOW`](../../Reference/code/McodeControl.md) | 5 | | [`M_MEDIUM`](../../Reference/code/McodeControl.md) | 10 | | [`M_HIGH`](../../Reference/code/McodeControl.md) | 15 | | [`M_VERY_HIGH`](../../Reference/code/McodeControl.md) | 25 |  When dealing with 2D cross-row code types, the default value is 2, regardless of the value of [`M_SPEED`](../../Reference/code/McodeControl.md). |
| `Value` | Specifies the scanline step, relative to the input coordinate system specified using [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  When entering a value in pixel units, this value must be greater than or equal to 1. The value entered is rounded to the nearest integer. |

---

### `M_SEARCH_ANGLE_MODE`

Sets whether to enable the search angular range algorithm for the code context.  When enabled, codes are sought at the angular range defined by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `-` [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) to [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `+` [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md). For 1D codes, the Code module will not use the search angular range algorithm if [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) both have a value of less than 5°, regardless of [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md).  For linear code types, if you expect that the codes sought are close to their nominal angle, you can try disabling the search angular range algorithm to see if the Code module can still find the required code occurrences. Disabling this algorithm might speed up the search depending on the code type and the target image.  For [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), you can set [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) to [`M_SCANLINE_AT_ANGLE`](../../Reference/code/McodeControl.md) if you are having issues reading your code at an angle. Note that although this improves robustness, this might affect the speed of the operation. You can try disabling [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) to see if the Code module can still find the required code occurrences; this might improve the speed.  Note that [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) is automatically activated for training if you activate [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the search angular range algorithm is not used. |
| `M_ENABLE` *(default)* | Specifies that the search angular range algorithm is used. |

---

### `M_SPEED`

Sets the search speed. The faster the search speed, the less robust the operation.  For 1D code types, consider reducing the search speed when the pixel height of the bar code is small.  For 2D code types, consider reducing the search speed when the cell size is small relative to the image size.  This control type is available for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md).  > **Note:** For 1D code types, [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_MICROPDF417`](../../Reference/code/McodeModel.md), you can also affect the search speed by adjusting the value for [`M_SCANLINE_STEP`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is[`M_VERY_LOW`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_MEDIUM`](../../Reference/code/McodeControl.md). |
| `M_HIGH` | Specifies a high search speed. |
| `M_LOW` | Specifies a low search speed. |
| `M_MEDIUM` | Specifies a medium search speed. |
| `M_VERY_HIGH` | Specifies a very high search speed. |
| `M_VERY_LOW` | Specifies a very low search speed. |

---

### `M_STOP_READ`

Stops the current [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation being performed on the code context passed to [`CodeId`](../../Reference/code/McodeControl.md).  You must call [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_STOP_READ`](../../Reference/code/McodeControl.md) from another thread, typically of higher priority.  After making this call, the results returned to the result buffer of the [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation are invalid and discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Implements the default behavior. |

---

### `M_THRESHOLD_MODE`

Sets the threshold mode used to internally binarize the source image.  If the lighting is non-uniform across the code, use [`M_ADAPTIVE`](../../Reference/code/McodeControl.md) mode or process the image before performing the [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation so that the background is strictly darker or strictly lighter than the code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value for [`M_MICROPDF417`](../../Reference/code/McodeModel.md) and linear (1D) code types (except [`M_PHARMACODE`](../../Reference/code/McodeModel.md)) is [`M_GLOBAL_SEGMENTATION`](../../Reference/code/McodeControl.md), and the default value for the remaining code types is [`M_ADAPTIVE`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value for [`M_DOTCODE`](../../Reference/code/McodeModel.md) is [`M_ADAPTIVE`](../../Reference/code/McodeControl.md), and the default value for the remaining code types is [`M_GLOBAL_SEGMENTATION`](../../Reference/code/McodeControl.md). |
| `M_ADAPTIVE` | Specifies to use a fast dynamic local threshold. This mode selects an individual threshold for each pixel based on the range of intensity values in its local neighborhood.  For linear (1D) code types (excluding [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_POSTNET`](../../Reference/code/McodeModel.md)), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type, you must specify the minimum contrast ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md)) between the foreground and background in the target image when using the [`M_ADAPTIVE`](../../Reference/code/McodeControl.md) mode. It is highly recommended that [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) is set to a valid value that is not [`M_DEFAULT`](../../Reference/code/McodeControl.md); if your code is not decoded properly, increase the value of [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md).  For 2D code types (excluding [`M_DOTCODE`](../../Reference/code/McodeModel.md) and [`M_MICROPDF417`](../../Reference/code/McodeModel.md)), the minimum contrast is determined automatically, so the adaptive threshold mode does not use the [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) setting.  2D and 1D code types (excluding [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_POSTNET`](../../Reference/code/McodeModel.md)) are the code types that support [`M_ADAPTIVE`](../../Reference/code/McodeControl.md). |
| `M_GLOBAL_SEGMENTATION` | Specifies to use a global threshold value. Specify the value using [`M_THRESHOLD_VALUE`](../../Reference/code/McodeControl.md). |
| `M_GLOBAL_WITH_LOCAL_RESEGMENTATION` | Specifies that the source image will be globally thresholded and then the edges in the binarized image are resegmented according to the intensities of the surrounding bars and spaces in the original source image. These individual local thresholds, calculated per region, help enhance the edge positions in the code. Specify the global threshold value using [`M_THRESHOLD_VALUE`](../../Reference/code/McodeControl.md).  This binarization method is compatible with the methods described in the various ISO code grading standards. For more information, refer to [Technical code information](../../UserGuide/C17_Codes/S03_Basic_concepts.md).  This control value is available for all 1D code types (except [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md)) and the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type. |

---

### `M_THRESHOLD_VALUE`

Sets the threshold value used to internally binarize the source image, depending on the specified threshold mode.  Note that if [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md) is set to [`M_ADAPTIVE`](../../Reference/code/McodeControl.md), this control type is ignored.  Note that this control type is automatically activated for training if you activate [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_COMPUTE` *(default)* | Specifies the threshold value automatically. |
| `0 <= Value <= 255` | Specifies the threshold value. Only integer values are accepted. |

---

### `M_TIMEOUT`

Specifies the maximum decoding time for an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, in msec.  Note that this control type can still affect[`McodeTrain`](../../Reference/code/McodeTrain.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that there is no maximum decoding time. |
| `Value >= 0` *(default)* | Specifies the maximum decoding time, in msec. |

---

### `M_TOTAL_NUMBER`

Sets the maximum number of code occurrences to read or grade in one source image. Note that this number is limited by the maximum number of occurrences of each code model to read or grade ([`M_NUMBER`](../../Reference/code/McodeControl.md)).  To avoid obtaining false positives when searching for a specific number of occurrences, set [`M_CODE_SEARCH_MODE`](../../Reference/code/McodeControl.md) to [`M_BEST`](../../Reference/code/McodeControl.md). Note that [`M_BEST`](../../Reference/code/McodeControl.md) is not supported for 2D code types other than the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  The following table lists the code types that support searching for multiple occurrences:  | Code type | Multiple occurrences | | --- | --- | | Linear bar codes (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) | Yes | | [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) | Yes | | [`M_DOTCODE`](../../Reference/code/McodeModel.md) | Yes |  Only the code types in the table above support this control type. When dealing with all other code types, [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md) must be set to [`M_DEFAULT`](../../Reference/code/McodeControl.md).  Note that for [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) and [`M_DOTCODE`](../../Reference/code/McodeModel.md) code types, [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md) should be set to the same value as [`M_NUMBER`](../../Reference/code/McodeControl.md) or to [`M_DEFAULT`](../../Reference/code/McodeControl.md) because a code context can only contain multiple code models of the above-mentioned 1D code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to read/grade all code occurrences in the source image, up to the maximum number of occurrences to read/grade of each code model ([`M_NUMBER`](../../Reference/code/McodeControl.md)). |
| `Value > 0` | Specifies the maximum number of codes to read/grade in one source image. Only integer values are accepted. |

### Code context settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified for a code context, to control an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. In this case, a code context must be passed to the [`CodeId`](../../Reference/code/McodeControl.md) parameter.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

---

### `M_ABSOLUTE_APERTURE_SIZE`

Sets the absolute size (diameter) of the aperture. Use this control type when using an absolute aperture mode (using [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) set to [`M_ABSOLUTE`](../../Reference/code/McodeControl.md)).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the absolute aperture size, relative to the input coordinate system specified using [`M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`

Sets the units with which to interpret the [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeControl.md) control type. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeGrade`](../../Reference/code/McodeGrade.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_APERTURE_MODE`

Sets the way in which the aperture size is determined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the absolute aperture size, set using [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeControl.md). |
| `M_DISABLE` | Specifies to disable the aperture. Note that this is provided for backwards compatibility with older code. In this case, the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation's returned results will more closely resemble the results of an [`McodeRead`](../../Reference/code/McodeRead.md) operation. |
| `M_RELATIVE` *(default)* | Specifies to use a relative aperture size, based on the cell size (using [`M_CELL_SIZE...`](../../Reference/code/McodeControl.md)) and the relative aperture factor (using [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md)). |

---

### `M_DPM_CALIBRATION_RESULTS`

Sets the expected reflectance and mean light values derived during the reflectance calibration phase.  This control type is an alternative to separately obtaining the reflectance and mean light values from the result buffer used during the reflectance calibration phase, and setting [`M_REFLECTANCE_CALIBRATION`](../../Reference/code/McodeControl.md) and [`M_MEAN_LIGHT_CALIBRATION`](../../Reference/code/McodeControl.md) to these two values, respectively, in the code context associated with the target code.

| Value | Description |
| --- | --- |
| `Result buffer identifier` | Specifies the result buffer used during the reflectance calibration phase. |

---

### `M_EXTENDED_AREA_REFLECTANCE_CHECK`

Sets whether the grading must perform an additional reflectance check over an area containing the code occurrence and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.  Note that this control type must be set before an [`McodeRead`](../../Reference/code/McodeRead.md) operation if you intend to use the result for an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the additional reflectance check. |
| `M_ENABLE` | Specifies to enable the additional reflectance check.  > **Note:** Note that you can only enable this control type when performing an ISO grading (that is, when [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md)). |

---

### `M_GRADE_QUIET_ZONE`

Sets whether to include the quiet zone when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to exclude the quiet zone. |
| `M_ENABLE` *(default)* | Specifies to include the quiet zone. |

---

### `M_GRADING_STANDARD`

Sets the grading standard to use when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. To specify a particular grading standard edition, use [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md).  The results of the [`McodeGrade`](../../Reference/code/McodeGrade.md)operation are dependent on the standard used; some control types have a different implementation or meaning depending on the standard used. These differences are outlined in the control types' description.  Note that this control type must be set before an [`McodeRead`](../../Reference/code/McodeRead.md) operation if you intend to use the result for an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ISO_DPM_GRADING` | Specifies to use an ISO/IEC 29158 specification. Note that this is the replacement for the AIM DPM-1-2006 standard.  This control value must be used at the beginning of the target grading phase, and should not be used during the reflectance calibration phase.  This control value is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types. |
| `M_ISO_GRADING` *(default)* | Specifies to use an ISO/IEC 15416 or ISO/IEC 15415 specification. |
| `M_SEMI_T10_GRADING` | Specifies to use a Semi T10 specification.  This control value is available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.  This control type requires that [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md), [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md), and [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md) are set to [`M_DEFAULT`](../../Reference/code/McodeControl.md). |

---

### `M_INSPECTION_BAND_RATIO`

Sets the height of the inspection band as a percentage of the average bar height. The inspection band is the area of the code occurrence from which the scanlines to grade the code occurrence are taken, after it has been located.  This control type is only available for 1D code types, 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default percentage. The default value is 80% of average bar height. |
| `0.0 < Value < 100.0` | Specifies the percentage of the average bar height to set as the height of the inspection band. |

---

### `M_LIGHTING_CONFIGURATION`

Sets the tilted coaxial lighting and camera angle position (TCL) relative to the marked object. There is a tolerance of +/- 3 degrees between the angle of the light and the angle of the camera. See the ISO/IEC 29518:2020 specifications for further details.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_90_DEGREE` | Specifies the TCL is 90 degrees relative to the marked object.  If [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_29158_2020`](../../Reference/code/McodeControl.md), this will cause [`M_MINIMUM_REFLECTANCE`](../../Reference/code/McodeGetResult.md) and [`M_MINIMUM_REFLECTANCE_GRADE`](../../Reference/code/McodeGetResult.md) to return [`M_CODE_GRADE_NOT_AVAILABLE`](../../Reference/code/McodeGetResult.md).  If [`M_GRADING_STANDARD_EDITION`](../../Reference/code/McodeControl.md) is set to [`M_ISO_29158_2011`](../../Reference/code/McodeControl.md), this setting will have no effect. |
| `M_UNSPECIFIED` *(default)* | Specifies that the TCL is none of the other values. |

---

### `M_MAXIMUM_CALIBRATED_REFLECTANCE`

Sets the maximum possible grayscale value in the target image. This control type is used in the scan reflectance profile analysis. If this control type does not accurately represent the actual maximum grayscale value from the target image, the resulting values from the scan reflectance profile are invalid. Note that this value must be higher than the value of [`M_MINIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the maximum calibrated reflectance. Only integer values are accepted. |

---

### `M_MEAN_LIGHT_CALIBRATION`

Sets the expected mean light (MLcal). This corresponds to the expected mean intensity of the centers of the white elements of the code occurrence.  This control type is used during the target grading phase of the ISO/IEC 29158 specification.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is equal to the setting of [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md). |
| `0 <= Value <= 255` | Specifies the mean intensity. This value is typically derived during the reflectance calibration phase. |

---

### `M_MINIMUM_CALIBRATED_REFLECTANCE`

Sets the minimum possible grayscale value in the target image. This control type is used in the scan reflectance profile analysis. If this control type does not accurately represent the actual minimum grayscale value from the target image, the resulting values from the scan reflectance profile are invalid. Note that this value must be lower than the value of [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the minimum calibrated reflectance. Only integer values are accepted. |

---

### `M_NUMBER_OF_SCANLINES`

Sets the number of scanlines inside the inspection band to inspect during the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, after the code occurrence has been located. Set the height of the inspection band using [`M_INSPECTION_BAND_RATIO`](../../Reference/code/McodeControl.md).  This control type is only available for 1D code types, 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. The default value is 10 scanlines. |
| `M_ALL` | Specifies to use all possible scanlines inside the inspection band. |
| `Value > 0` | Specifies the number of scanlines to inspect. This value is equivalent to [`M_ALL`](../../Reference/code/McodeControl.md) if it exceeds the inspection band height. Only integer values are accepted. |

---

### `M_PIXEL_SIZE_IN_MM`

Sets the scale between a pixel and its physical measurement in mm. To simplify the computation of this value, measure the length of the entire code and divide it by its pixel length; this yields a good approximation. Alternatively, calculate the scale using the following formula: `(_size of the object_) / (_number of pixels in object_)`. For more information, see point 3 in [Relative aperture setting](../../UserGuide/C18_More_on_grading_codes/S04_Settings_for_a_code_grade_operation.md).  Code specifications generally specify constraints in mm units. To compare values known in Aurora Imaging Library (for example, the cell size in pixels) to the values in the code specifications, this control type is used. Note that this control type can impact other control types, for example, the aperture size selection, and the [`M_SCAN_INTERCHARACTER_GAP_GRADE`](../../Reference/code/McodeGetResult.md) result (retrieved using [`McodeGetResult`](../../Reference/code/McodeGetResult.md)).  Note that this value is needed even when operating on calibrated images because Aurora Imaging Library does not know the relationship between mm and the world units of the calibrated context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the scale between a pixel and its physical measurement is not known, in mm per pixel units. |
| `Value > 0` | Specifies the scale between a pixel and its physical measurement, in mm per pixel units. |

---

### `M_REFLECTANCE_CALIBRATION`

Sets the expected reflectance value (Rcal). This control type is used during the target grading phase of the ISO/IEC 29158 specification.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is equal to the setting of [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeControl.md). |
| `0 <= Value <= 255` | Specifies the reflectance value. This value is typically derived during the reflectance calibration phase. Only integer values are accepted. |

---

### `M_RELATIVE_APERTURE_FACTOR`

Sets the aperture factor to use when [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) is set to [`M_RELATIVE`](../../Reference/code/McodeControl.md). The aperture factor is multiplied by the [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) to determine the aperture's diameter.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the aperture factor is chosen according to the ISO/IEC 15416 specification for all supported code types except 2D matrix code types; for the latter, the aperture factor is chosen according to the ISO/IEC 15415 or ISO/IEC 29158 specification (depends on [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md)).  Note that if the nominal bar width is less than 0.1 mm, the aperture factor is 0.5. When dealing with EAN/UPC code types, the aperture factor is 0.15. When dealing with 2D matrix code types, the aperture factor is 0.8.  Note that the recommended aperture size in the ISO/IEC 15415 specification has changed. The 2004 specification states that the aperture size should be 0.8 times the diameter. The 2011 specification recommends an aperture size between 50% and 80% of the smallest X-dimension encountered. [`M_AUTO`](../../Reference/code/McodeControl.md) complies with the 2004 version of the specification. Since the new specification now permits alternative aperture sizes, using an aperture factor of 0.5 with 2D matrix code types can often lead to better results. |
| `0 <= Value <= 2` | Specifies the aperture factor. |

---

### `M_SYSTEM_RESPONSE_CALIBRATION`

Sets the System Response value derived during the reflectance calibration phase (SRcal) of the ISO/IEC 29158 specification.  The System Response value is a user-defined aggregate of the settings of environmental factors, such as exposure time and/or gain factor, that create the conditions for an acceptable image of the code.  For example, you could define the System Response value as: (`_Exposure time_ * _Gain factor_`). During the reflectance calibration phase, adjust the exposure time and/or gain factor until you obtain an acceptable image of the code, and then multiply the two values together. At the beginning of the target grading phase, set this result as the System Response value of the reflectance calibration phase.  Note that you should use the same formula to calculate the System Response value, in both the reflectance calibration phase and the target grading phase.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the System Response value for the reference code. |

---

### `M_SYSTEM_RESPONSE_TARGET`

Sets the System Response value derived during the target grading phase (SRtarget) of the ISO/IEC 29158 specification.  To set this control type, use the same aggregate of environmental factors that was used to calculate [`M_SYSTEM_RESPONSE_CALIBRATION`](../../Reference/code/McodeControl.md).  For example, if you set the System Response value of the reflectance calibration phase to the multiplication of the exposure time by the gain factor, set the System Response value of the target grading phase to the multiplication of the exposure time by the gain factor used to create the conditions for an acceptable image of the target code. During the target grading phase, you should not alter any environmental factors other than those used to calculate the System Response value.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the System Response value for the target code. |

### Code context settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. In this case, a code context must be passed to the [`CodeId`](../../Reference/code/McodeControl.md) parameter.

---

### `M_RESET_FROM_TRAINED_RESULTS`

Resets the code context using the results in the code train result buffer. Existing code models are discarded and code models used for training are added to the context. If a control type was not trained, it is set to its value used during training; if a control type was trained, it is set to its trained value. The results in the code train result buffer can be applied to any code context.  To reconfigure the code context that was used during training, you might find it useful to selectively change some control types to their recommended setting, established during training. To do so, you can use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) to retrieve their recommended setting, and then use [`McodeControl`](../../Reference/code/McodeControl.md) to set them explicitly. Note that some control types are inter-dependent. Changing the setting of one control type to its trained value might not be optimal if the settings of other control types are not also changed. This is especially true for control types that are automatically activated for training if another control type is activated (for example, [`M_CELL_NUMBER_...`](../../Reference/code/McodeControl.md)).

| Value | Description |
| --- | --- |
| `CodeTrainResultBufId` | Specifies the identifier of the code train result buffer to use to reset the code context. The result buffer must have been previously allocated using[`McodeAllocResult`](../../Reference/code/McodeAllocResult.md) with [`M_CODE_TRAIN_RESULT`](../../Reference/code/McodeAllocResult.md) and must contain results from a successful [`McodeTrain`](../../Reference/code/McodeTrain.md). Use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md) to ensure that the result buffer contains results from a successful operation ([`M_STATUS_TRAIN_OK`](../../Reference/code/McodeGetResult.md)). |

---

### `M_SET_TRAINING_STATE_ALL`

Resets whether all trainable control types will be trained when [`McodeTrain`](../../Reference/code/McodeTrain.md)is called. Using this control type is equivalent to using the [`M_TRAIN`](../../Reference/code/McodeControl.md) combination value with every trainable control type.  After activating or deactivating all trainable control types using[`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md), you can then activate or deactivate the training of specific control types using [`M_TRAIN`](../../Reference/code/McodeControl.md), if necessary.  [`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md) modifies the training status of [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md), [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md), [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md), [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md), [`M_CODE_FLIP`](../../Reference/code/McodeControl.md), [`M_DATAMATRIX_SHAPE`](../../Reference/code/McodeControl.md), [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md), [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md), [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md), [`M_ENCODING`](../../Reference/code/McodeControl.md), [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md), [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md), [`M_SPEED`](../../Reference/code/McodeControl.md), [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md), and [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default activation; the default activation depends on the control type and code model type. For a given trainable control type, if an unsupported code model is included in the training, the default value will be [`M_DISABLE`](../../Reference/code/McodeControl.md), otherwise it will be [`M_ENABLE`](../../Reference/code/McodeControl.md). |
| `M_DISABLE` | Specifies to not train any of the trainable control types. |
| `M_ENABLE` | Specifies to train all trainable control types. |

---

### `M_STOP_TRAIN`

Stops the current [`McodeTrain`](../../Reference/code/McodeTrain.md) operation being performed on the code context passed to [`CodeId`](../../Reference/code/McodeControl.md).  This must be called from another thread, typically of higher priority.  After making this call, the results returned to the result buffer of the [`McodeTrain`](../../Reference/code/McodeTrain.md) operation are invalid and discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

---

### `M_TRAIN_TIMEOUT`

Sets the maximum training time for an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the timeout. |
| `Value >= 0` | Specifies the timeout limit, in msec. |

### Code model settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified for a code model to control an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. If you pass a code context to the [`CodeId`](../../Reference/code/McodeControl.md) parameter, the specified control type setting is applied to all the code models in the context. If none of the code models support the specified control type, an error will occur. If only some code models do not support the specified control type, the control type setting is ignored for these code models.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although all these control types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the control types to determine if they can be activated for training; you can also use filters to limit the control types in the table to those that can be trained.

---

### `M_BEARER_BAR`

Sets whether bearer bars run along the top and bottom of the code occurrences to read (such as, the edge of a sticker). Note that this control type is only taken into account when the search angle is not specified.  This value is available only for 1D code types.  In situations where bearer bars don't run along both the top and the bottom of the code occurrence to read, setting [`M_SPEED`](../../Reference/code/McodeControl.md) to [`M_VERY_LOW`](../../Reference/code/McodeControl.md) will provide more robust results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSENT` *(default)* | Specifies that no bearer bars are above and below the code. |
| `M_PRESENT` | Specifies that there are bearer bars above and below the code. |

---

### `M_CELL_NUMBER_X_MAX`

Sets the maximum number of cells for which to search, in the X-direction of a 2D code.  Note that this control type is automatically activated for training if you activate [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. If a value is specified, it must be larger than [`M_CELL_NUMBER_X_MIN`](../../Reference/code/McodeControl.md), otherwise an error occurs.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4.  Only integer values are accepted. |

---

### `M_CELL_NUMBER_X_MIN`

Sets the minimum number of cells for which to search, in the X-direction of a 2D code.  Note that this control type is automatically activated for training if you activate [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. If a value is specified, it must be smaller than [`M_CELL_NUMBER_X_MAX`](../../Reference/code/McodeControl.md), otherwise an error occurs.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4.  Only integer values are accepted. |

---

### `M_CELL_NUMBER_Y_MAX`

Sets the maximum number of cells for which to search, in the Y-direction of a 2D code.  Note that this control type is automatically activated for training if you activate [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. If a value is specified, it must be larger than [`M_CELL_NUMBER_Y_MIN`](../../Reference/code/McodeControl.md), otherwise an error occurs.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4.  Only integer values are accepted. |

---

### `M_CELL_NUMBER_Y_MIN`

Sets the minimum number of cells for which to search, in the Y-direction of a 2D code.  Note that this control type is automatically activated for training if you activate [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. If a value is specified, it must be smaller than [`M_CELL_NUMBER_Y_MAX`](../../Reference/code/McodeControl.md), otherwise an error occurs.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4.  Only integer values are accepted. |

---

### `M_CELL_SIZE_MAX`

Sets the maximum cell size. Note that setting the maximum and minimum cell size increases both the speed and robustness of the operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to automatically select an appropriate size, which is dependent on the code type and the initialization mode.  For an [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, the default is always 255 when[`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) is set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md). For all code types, the default is always 24 when [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) is set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md). |
| `Value` | Specifies the maximum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  When entering a value in pixel units, this value must be between 1 and 256, inclusive. |

---

### `M_CELL_SIZE_MIN`

Sets the minimum cell size. Note that setting the maximum and minimum cell size increases both the speed and robustness of the operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default cell size, in pixels, which is dependent on the operation and code type. Setting the default ignores the input unit type chosen with [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  For an [`McodeRead`](../../Reference/code/McodeRead.md) operation, [`M_DEFAULT`](../../Reference/code/McodeControl.md) is equivalent to 1, except for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) (which have a default of 3).  For an [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type regardless of the operation, the default is always 2. |
| `Value` | Specifies the minimum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  When entering a value in pixel units, this value must be between 1 and 256, inclusive. |

---

### `M_CHECK_FINDER_PATTERN`

Sets whether checking for a false Data Matrix pattern is enabled.  If this control type is enabled, [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations are more robust, and false Data Matrix code types are not read.  This control type should only be enabled if it is possible that parts of your image could be falsely interpreted as a Data Matrix code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Code module will not check for false [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code types. |
| `M_ENABLE` | Specifies that the Code module will check for false [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code types. |

---

### `M_CHECK_QUIET_ZONE`

Sets whether the presence of the quiet zone is necessary for a successful [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation of this code type.  > **Note:** Note that the specifications of a code's quiet zone are dependent upon the code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. The default value for [`M_DOTCODE`](../../Reference/code/McodeModel.md) is [`M_DISABLE`](../../Reference/code/McodeControl.md). For all other code types, the default is [`M_ENABLE`](../../Reference/code/McodeControl.md). |
| `M_DISABLE` | Specifies that a quiet zone is not necessary. When set to [`M_DISABLE`](../../Reference/code/McodeControl.md), the [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation can be less robust, especially for 1D code types. |
| `M_ENABLE` | Specifies that a quiet zone is necessary. |

---

### `M_CODE_FLIP`

Sets whether code occurrences need to be flipped or read in the opposite direction to be read properly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is[`M_ANY`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_NO_FLIP`](../../Reference/code/McodeControl.md). |
| `M_ANY` | Allows Aurora Imaging Library to decide whether a code occurrence needs to be flipped or read in the opposite direction to be read properly. |
| `M_FLIP` | Specifies that code occurrences need to be flipped or read in the opposite direction to be read properly. When dealing with 1D code types, a code occurrence will be read from left-to-right and from right-to-left. When dealing with 2D code types, a code occurrence will be flipped before being read. |
| `M_NO_FLIP` | Specifies that code occurrences don't need to be flipped or read in the opposite direction. When dealing with 1D code types, a code occurrence will be read from left-to-right. When dealing with 2D code types, a code occurrence will not be flipped. |

---

### `M_CODE_SEARCH_MODE`

Sets the behavior of the decoding algorithm.  Note that the default setting is [`M_FAST`](../../Reference/code/McodeControl.md) in the majority of cases. To avoid obtaining false positives when searching for a specific number of occurrences ([`M_NUMBER`](../../Reference/code/McodeControl.md) and [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md)), set [`M_CODE_SEARCH_MODE`](../../Reference/code/McodeControl.md) to [`M_BEST`](../../Reference/code/McodeControl.md).  This control type is available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type.  The default value is [`M_FAST`](../../Reference/code/McodeControl.md) for all code types except [`M_PHARMACODE`](../../Reference/code/McodeModel.md), in which case, the default value is [`M_BEST`](../../Reference/code/McodeControl.md). |
| `M_BEST` | Specifies to search and return the best quality code occurrences. |
| `M_FAST` | Specifies to search and return the first code occurrences decoded. |

---

### `M_DATAMATRIX_SHAPE`

Sets the shape of the Data Matrix code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type can be any shape. |
| `M_RECTANGLE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a rectangular shape. This shape can only use the [`M_ECC_200`](../../Reference/code/McodeControl.md) error correction scheme. |
| `M_SQUARE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a square shape. The number of cells in X and Y must be equal, and the aspect ratio of cell sizes in the X and Y direction must be near 1:1. |

---

### `M_DECODE_ALGORITHM`

Sets the decoding algorithm to use to read the code occurrences.  This control type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types.  Note that training this control type ([`M_TRAIN`](../../Reference/code/McodeControl.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) is[`M_CODE_NOT_DEFORMED`](../../Reference/code/McodeControl.md), and the default value for the remaining code types is [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_CODE_NOT_DEFORMED`](../../Reference/code/McodeControl.md) for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md), in which case, the default value is [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md). |
| `M_CODE_DEFORMED` | Specifies to use the algorithm to decode deformed code occurrences.  This algorithm is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_DOTCODE`](../../Reference/code/McodeModel.md) code types.  > **Note:** Note that selecting this algorithm might lead to longer processing times. |
| `M_CODE_NOT_DEFORMED` | Specifies to use the algorithm to decode non-deformed code occurrences. |
| `M_SCANLINE_AT_ANGLE` | Specifies to use the algorithm to decode [`M_PDF417`](../../Reference/code/McodeModel.md) or [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code occurrences using rotated scanlines, without any localization.  The angle of the scanlines is determined using [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md).  Select this algorithm if you have codes that do not completely respect their specification (for example, they are missing their quiet zone). To get the best performance, specify a precise [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) or use [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md); also, ensure [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md) is not set to a value higher than the row height.  This algorithm is only available for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types. |

---

### `M_DOT_SIZE_INPUT_UNITS`

Sets the units with which to interpret the [`M_DOT_SIZE_MIN`](../../Reference/code/McodeControl.md)and [`M_DOT_SIZE_MAX`](../../Reference/code/McodeControl.md) control types. This essentially sets the input coordinate system to use.  This control type is only available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_DOT_SIZE_MAX`

Sets the maximum diameter of the dots for which to search.  This control type is only available for the[`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  Note that setting the maximum and minimum dot size increases both the speed and robustness of the operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode. The default is always 255 when[`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) is set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md). Whereas the default is always 24 when [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) is set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md). |
| `M_AUTO` | Specifies to select an appropriate size, automatically. |
| `Value > 0` | Specifies the maximum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_DOT_SIZE_MIN`

Sets the minimum diameter of the dots for which to search.  This control type is only available for the[`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  Note that setting the maximum and minimum dot size increases both the speed and robustness of the operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to select an appropriate size, automatically. |
| `Value > 0` *(default)* | Specifies the minimum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_DOT_SPACING_INPUT_UNITS`

Sets the units with which to interpret the [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md)control types. This essentially sets the input coordinate system to use.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies what convention is used, automatically. |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_DOT_SPACING_MAX`

Sets the maximum distance between 2 dots in a matrix code type composed of dots. Note that the value entered represents half the distance between two dark cells (dots). To set the minimum distance between 2 dots, use [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md).  When the dark cells in a printed code are oversized (for example, due to printing artifacts) and overlap, set [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md)to a negative value equal to half the width of the overlap. The image below illustrates dark cells overlapping, the gray circle represents the actual cell size, while the black dot represents the expected cell size.  *[Image: code_dotspacing_bleeding2.png]*  By contrast, when the dots are too far apart, specify a positive dot spacing equal to half the distance between the two dots.  *[Image: code_dotspacing_regular.png]*  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.  Note that overestimating this value will severely impair reading results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 2.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is the same as [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md). Note that if [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) is also set to [`M_DEFAULT`](../../Reference/code/McodeControl.md), the result is no spacing. |
| `-256 <= Value <= 256` | Specifies the distance, relative to the input coordinate system specified using [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md). Specifying a positive value means that you are specifying half the distance between the edges of the dots. Specifying a negative value means you are specifying the half-depth of the overlap.  If [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md) is set to [`M_WORLD`](../../Reference/code/McodeControl.md), the distance is specified in world units. Aurora Imaging Library transforms the value to pixel units according to the camera calibration of the target image that is specified when reading the code occurrence.  > **Note:** Whether to use a negative or positive dot spacing is a situation best determined by testing. For more information, refer to [Dot spacing](../../UserGuide/C17_Codes/S09_Customizing_read_operation_settings.md). |

---

### `M_DOT_SPACING_MIN`

Sets the minimum distance between 2 dots in a matrix code type composed of dots. Note that the value entered represents half the distance between two dark cells (dots). For more information, refer to [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md).  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.  Note that overestimating this value will severely impair reading results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is the same as -1.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is the same as [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md). Note that if [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md) is also set to [`M_DEFAULT`](../../Reference/code/McodeControl.md), the result is no spacing. |
| `-256 <= Value <= 256` | Specifies the distance, relative to the input coordinate system specified using [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md). Specifying a positive value means that you are specifying half the distance between the edges of the dots. Specifying a negative value means you are specifying the half-depth of the overlap.  If [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md) is set to [`M_WORLD`](../../Reference/code/McodeControl.md), the distance is specified in world units. Aurora Imaging Library transforms the value to pixel units according to the camera calibration of the target image that is specified when reading the code occurrence.  > **Note:** Whether to use a negative or positive dot spacing is a situation best determined by testing. For more information, refer to [Dot spacing](../../UserGuide/C17_Codes/S09_Customizing_read_operation_settings.md).  When entering a value in pixel units, this value must be between -256 and 256, inclusive. |

---

### `M_ECC_CORRECTED_NUMBER`

Sets whether [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md)are forced to perform a more robust operation to minimize the number of errors to correct.  Enabling [`M_ECC_CORRECTED_NUMBER`](../../Reference/code/McodeControl.md) decreases performance, particularly in good quality images or in images where the error count is not important.  This control type is only available for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to perform a more robust operation. |
| `M_ENABLE` | Specifies to perform a more robust operation. |

---

### `M_FINDER_PATTERN_EXHAUSTIVE_SEARCH`

Sets whether to search for the L-shaped finder pattern (the gray boxed area in the following image) to help localize the Data Matrix code. Note that enabling this control type increases the operation time, but is more reliable when a Data Matrix code has little contrast between foreground and background colors or when dealing with complex images.  *[Image: mcode-datamatrix-finder-pattern.png]*  This control type is only available when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) is set to [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md) or when [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md) is set to [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md).  In addition, this control type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not perform an exhaustive search. |
| `M_ENABLE` | Specifies to perform an exhaustive search. |

---

### `M_FINDER_PATTERN_INPUT_UNITS`

Sets the units with which to interpret the [`M_FINDER_PATTERN_MAX_GAP`](../../Reference/code/McodeControl.md) and [`M_FINDER_PATTERN_MINIMUM_LENGTH`](../../Reference/code/McodeControl.md) control types. This essentially sets the input coordinate system to use.  This control type is only available when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) is set to [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md) or when [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md) is set to [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md).  In addition, this control type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_FINDER_PATTERN_MAX_GAP`

Sets the maximum tolerable gap in the finder pattern of a Data Matrix code. A gap is an unintentional space (area of background color), in the solid foreground color area of an [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code's finder pattern. A code occurrence that has a finder pattern with a gap larger than the specified value is not read.  This control type is only available when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) is set to [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md) or when [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md) is set to [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md).  In addition, this control type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum tolerable gap in the finder pattern is 6 times the minimum cell size specified using [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md). |
| `Value` | Specifies the maximum gap allowed, in input units specified using [`M_FINDER_PATTERN_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_FINDER_PATTERN_MINIMUM_LENGTH`

Sets the shortest acceptable length of either "arm" of the finder pattern of an [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code occurrence. If either "arm" is shorter than the minimum, the code occurrence is not read.  This control type is only available when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) is set to [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md) or when [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md) is set to [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md).  In addition, this control type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the minimum acceptable finder pattern "arm" length is 6 times the minimum cell size specified using [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md). |
| `Value` | The minimum acceptable finder pattern "arm" length, in input units specified using [`M_FINDER_PATTERN_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_NUMBER`

Sets the maximum number of code occurrences of the specified code model to read, grade, or train.  Note that this number is limited by the specified maximum total number of code occurrences in the image to read, grade, or train ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md)).  To avoid obtaining false positives when searching for a specific number of occurrences, set [`M_CODE_SEARCH_MODE`](../../Reference/code/McodeControl.md) to [`M_BEST`](../../Reference/code/McodeControl.md). Note that [`M_BEST`](../../Reference/code/McodeControl.md) is not supported for 2D code types other than the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.  The following table lists the code types that support searching for multiple occurrences:  | Code type | Multiple occurrences | | --- | --- | | Linear bar codes (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)) | Yes | | [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) | Yes | | [`M_DOTCODE`](../../Reference/code/McodeModel.md) | Yes |  Only the code types in the table above support this control type. When dealing with all other code types, [`M_NUMBER`](../../Reference/code/McodeControl.md) must be set to 0 or 1 (or [`M_DEFAULT`](../../Reference/code/McodeControl.md)).  Note that training this control type ([`M_TRAIN`](../../Reference/code/McodeControl.md)) is only supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 1 for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md), in which case, the default value is [`M_ALL`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 1. |
| `M_ALL` | Specifies that all code model occurrences are read/graded up to the maximum number limited by [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md). |
| `Value >= 0` *(default)* | Specifies the maximum number of code occurrences of the specified code model to read or grade. Only integer values are accepted.  Only 1D code types (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md) code types), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type support specifying a value greater than 1. |

---

### `M_POSITION_ACCURACY`

Sets the accuracy of positional results. Accuracy depends on the settings of the code context and its models.  It is always recommended to set this control type to [`M_HIGH`](../../Reference/code/McodeControl.md) when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation; for higher accuracy, this is also recommended when performing an [`McodeRead`](../../Reference/code/McodeRead.md) on multiple code occurrences. Note that this can result in longer read times.  This control type must be set to [`M_HIGH`](../../Reference/code/McodeControl.md)before an [`McodeRead`](../../Reference/code/McodeRead.md) operation if you want to use its result for an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_HIGH`](../../Reference/code/McodeControl.md) for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md), in which case, the default value is [`M_LOW`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_LOW`](../../Reference/code/McodeControl.md). |
| `M_HIGH` | Specifies to report the positional results of the code operation with high accuracy. |
| `M_LOW` | Specifies to report the positional results of the code operation with low accuracy. |

---

### `M_SEARCH_ANGLE`

Sets the nominal search angle.  The search is performed between the range of angles defined by: ([`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `-` [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md)) to ([`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `+` [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md)), inclusively, starting with an angle closest to that of [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md).  > **Note:** Note that [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) must be set to [`M_DEFAULT`](../../Reference/code/McodeControl.md) when dealing with the following code types: [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_MICROPDF417`](../../Reference/code/McodeModel.md). Their default angular search range will be used.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` | Specifies that the nominal angle is set to the angle of the target image's ROI ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)); recall that for [`McodeGrade`](../../Reference/code/McodeGrade.md) and [`McodeRead`](../../Reference/code/McodeRead.md), the ROI must be rectangular. If this value is chosen but no ROI has been defined, the angle is set to 0.0°. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal angle, in degrees, relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md).  An angle interpreted with respect to the pixel coordinate system is always measured counter-clockwise. For information on the angle's direction of rotation when interpreting the angle with respect to the relative coordinate system, see [Angle convention in Aurora Imaging Library](../../UserGuide/C28_Calibration/S09_Working_with_realworld_units.md). |

---

### `M_SEARCH_ANGLE_DELTA_NEG`

Sets the negative angular range of the search.  Note that [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) must be set to [`M_DEFAULT`](../../Reference/code/McodeControl.md) when dealing with the following code types: [`M_PHARMACODE`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md). Their default angular search range will be used:  | Code type | Maximum search angle | | --- | --- | | [`M_PHARMACODE`](../../Reference/code/McodeModel.md) | ±5° | | [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) | ±5° |  Note that this control type is automatically activated for training if you activate [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 180.0°. Note that if your code type is [`M_PHARMACODE`](../../Reference/code/McodeModel.md) or [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), the default value is 5.0°.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 5.0°. |
| `0.0 <= Value <= 180.0` | Specifies a negative angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). Note that while the nominal angle is relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) is not affected by the coordinate system used.  For 1D codes, the Code module will not use the search angular range algorithm if [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) both have a value of less than 5°, regardless of [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md). |

---

### `M_SEARCH_ANGLE_DELTA_POS`

Sets the positive angular range of the search.  Note that [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) must be set to [`M_DEFAULT`](../../Reference/code/McodeControl.md) when dealing with the following code types: [`M_PHARMACODE`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md). Their default angular search range will be used:  | Code type | Maximum search angle | | --- | --- | | [`M_PHARMACODE`](../../Reference/code/McodeModel.md) | ±5° | | [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) | ±5° |  Note that this control type is automatically activated for training if you activate [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 180.0°. Note that if your code type is [`M_PHARMACODE`](../../Reference/code/McodeModel.md) or [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), the default value is 5.0°.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is 5.0°. |
| `0.0 <= Value <= 180.0` | Specifies a positive angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). Note that while the nominal angle is relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) is not affected by the coordinate system used.  For 1D codes, the Code module will not use the search angular range algorithm if [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) both have a value of less than 5°, regardless of [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md). |

---

### `M_SEARCH_ANGLE_INPUT_UNITS`

Sets the units with which to interpret the [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) control type. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) generates an error if the operation is not performed on a calibrated image.  > **Note:** If there is distortion in the target image, you should correct the image before performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. |

---

### `M_SEARCH_ANGLE_STEP`

Sets the angle increment/decrement used when searching for codes through an angular range.  When [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md) is enabled, the search angular range algorithm will first search at the specified nominal angle. It will then toggle between incrementing and decrementing from the nominal angle, with each iteration moving further away from the initial search angle by the value of [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md). The process will continue either until the code is found, or the search falls outside of the range defined by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `-` [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `+` [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md).  Note that setting the[`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md) will affect both the speed and robustness of the search.  > **Note:** Note that this control type is ignored if [`M_SPEED`](../../Reference/code/McodeControl.md) is set to [`M_VERY_LOW`](../../Reference/code/McodeControl.md), or [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) is set to [`M_DISABLE`](../../Reference/code/McodeControl.md).  This control type is available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)). It is also available for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) is set to [`M_SCANLINE_AT_ANGLE`](../../Reference/code/McodeControl.md).  Note that for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types, this control type is automatically activated for training if you activate [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no explicit increment/decrement is used. The actual increment/decrement used during the search is determined by the search angular range algorithm. |
| `0.1 <= Value <= 180.0` | Specifies the explicit angle increment/decrement, in degrees. |

---

### `M_STRING_SIZE_MAX`

Sets the maximum size (number of characters) of the string encoded in a code occurrence.  For [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code types, the maximum string size is ignored when performing [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type.  For the [`M_IATA25`](../../Reference/code/McodeInquire.md) code type, the default value is 15.  For all other code types, the default value is [`M_ANY`](../../Reference/code/McodeControl.md). |
| `M_ANY` | Specifies that there is no maximum string size. |
| `0 <= Value <= 65535` | Specifies the maximum string size.  For a 1D code (except [`M_POSTNET`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_CODABAR`](../../Reference/code/McodeModel.md) codes), the range of possible values starts at 1.  For an [`M_POSTNET`](../../Reference/code/McodeModel.md) code, the value must be either 5, 9, or 11.  For an [`M_PLANET`](../../Reference/code/McodeModel.md) code, the value must be 11 or 13.  For an [`M_CODABAR`](../../Reference/code/McodeModel.md) code, the value must be at least 3.  For all 2D codes and cross-row codes (except [`M_AZTEC`](../../Reference/code/McodeModel.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md) set to [`M_ENC_AZTEC_RUNE`](../../Reference/code/McodeControl.md) and [`M_MAXICODE`](../../Reference/code/McodeModel.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md) set to either[`M_ENC_MODE2`](../../Reference/code/McodeControl.md)or [`M_ENC_MODE3`](../../Reference/code/McodeControl.md)) the value can start at 0.  Only integer values are accepted. |

---

### `M_STRING_SIZE_MIN`

Sets the minimum size (number of characters) of the string encoded in a code occurrence.  For [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code types, the minimum string size is ignored when performing [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type.  For the [`M_IATA25`](../../Reference/code/McodeInquire.md) code type, the default value is 15.  For all other code types, the default value is [`M_ANY`](../../Reference/code/McodeControl.md). |
| `M_ANY` | Specifies that there is no minimum string size. |
| `0 <= Value <= 65535` | Specifies the minimum string size.  For a 1D code (except [`M_POSTNET`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_CODABAR`](../../Reference/code/McodeModel.md) codes), the range of possible values starts at 1.  For an [`M_POSTNET`](../../Reference/code/McodeModel.md) code, the value must be either 5, 9, or 11.  For an [`M_PLANET`](../../Reference/code/McodeModel.md) code, the value must be 11 or 13.  For an [`M_CODABAR`](../../Reference/code/McodeModel.md) code, the value must be at least 3.  For all 2D codes and cross-row codes (except [`M_AZTEC`](../../Reference/code/McodeModel.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md) set to [`M_ENC_AZTEC_RUNE`](../../Reference/code/McodeControl.md) and [`M_MAXICODE`](../../Reference/code/McodeModel.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md) set to either[`M_ENC_MODE2`](../../Reference/code/McodeControl.md)or [`M_ENC_MODE3`](../../Reference/code/McodeControl.md)) the value can start at 0.  Only integer values are accepted. |

---

### `M_SUB_TYPE`

Sets the particular code sub-types for which to search. Enabling fewer sub-types will help increase the speed of the operation.  Some versions of [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) codes only differ by their bar height and not their structure. In these cases, the operation is successful even if only one of the sub-types is enabled. For example, GS1 Databar Omnidirectional and GS1 Databar Truncated can be decoded if either [`M_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) or [`M_GS1_DATABAR_TRUNCATED`](../../Reference/code/McodeControl.md) is enabled.  The sub-types are additive. If you set the sub-type to [`M_UPC_A`](../../Reference/code/McodeControl.md) + [`M_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md), you can search for [`M_UPC_A`](../../Reference/code/McodeModel.md) and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) codes. If you enabled other sub-types previously, they are no longer enabled.  This value is available only for [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for all of the code sub-types that can be specified for [`M_SUB_TYPE`](../../Reference/code/McodeControl.md). |
| `M_EAN8` | Specifies that the EAN 8 code sub-type is enabled.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |
| `M_EAN13` | Specifies that the EAN 13 code sub-type is enabled.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |
| `M_GS1_128` | Specifies that the GS1-128 code sub-type is enabled.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |
| `M_GS1_DATABAR_EXPANDED` | Specifies that the GS1 Databar Expanded code sub-type is enabled. |
| `M_GS1_DATABAR_EXPANDED_STACKED` | Specifies that the GS1 Databar Expanded Stacked code sub-type is enabled. |
| `M_GS1_DATABAR_LIMITED` | Specifies that the GS1 Databar Limited code sub-type is enabled. |
| `M_GS1_DATABAR_OMNI` | Specifies that the GS1 Databar Omnidirectional code sub-type is enabled. |
| `M_GS1_DATABAR_STACKED` | Specifies that the GS1 Databar Stacked code sub-type is enabled. |
| `M_GS1_DATABAR_STACKED_OMNI` | Specifies that the GS1 Databar Stacked Omnidirectional code sub-type is enabled. |
| `M_GS1_DATABAR_TRUNCATED` | Specifies that the GS1 Databar Truncated code sub-type is enabled. |
| `M_UPC_A` | Specifies that the UPC-A code sub-type is enabled.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |
| `M_UPC_E` | Specifies that the UPC-E code sub-type is enabled.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |

---

### `M_USE_PRESEARCH`

Sets whether the localization operation is performed prior to the decoding step of an operation.  Note that, when reading multiple occurrences of [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) codes, the setting of this control type is ignored.  This control type is available for 2D code types, excluding the[`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md) and the code type is [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), the default value is [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md). For supported 2D code types other than [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), the default value is [`M_STAT_BASE`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_DISABLE`](../../Reference/code/McodeControl.md). |
| `M_DISABLE` | Specifies that the operation is not performed. |
| `M_FINDER_PATTERN_BASE` | Specifies that the localization operation is only performed on the base pattern of the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code (an "L" starting at the top-most left corner, and ending on the bottom-most right corner of the code). This localization operation is more robust than an [`M_STAT_BASE`](../../Reference/code/McodeControl.md)localization operation. Note that if the surrounding image is very complex, better results might be obtained by setting[`M_FINDER_PATTERN_EXHAUSTIVE_SEARCH`](../../Reference/code/McodeControl.md) to [`M_ENABLE`](../../Reference/code/McodeControl.md).  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) is the only code type that supports this setting. |
| `M_STAT_BASE` | Specifies to localize the code within the image with the statistical characteristics of a 2D bar code (for example, local variance and the presence of a lot of edges). This localization operation is performed faster than an [`M_FINDER_PATTERN_BASE`](../../Reference/code/McodeControl.md) localization operation.  Note that this localization operation might be less efficient when a code occurrence's cell size is greater than a particular value, depending on the code type. To obtain the best results from this operation, you should specify the cell size as a range, using [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md), when the cell sizes of the codes in your images are greater than the cell sizes listed in the table below. If they are less than or equal to these values, you can leave [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md) to their default values.  | Code type | Cell size | | --- | --- | | Aztec | 9 | | MicroPDF417 | 4 | | PDF417/Truncated PDF417 | 7 | | QR code | 8 |  Note that unlisted code types have no known cell size limitations. |

### Code model settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified for a code model to control an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation. If you pass a code context to the [`CodeId`](../../Reference/code/McodeControl.md) parameter, the specified control type setting is applied to all the code models in the context. If none of the code models support the specified control type, an error occurs. If only some code models do not support the specified control type, the control type setting is ignored for these code models.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although all these control types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the control types to determine if they can be activated for training; you can also use filters to limit the control types in the table to those that can be trained.

---

### `M_CELL_NUMBER_X`

Sets the number of cells of a 2D code in the X-direction. Note that setting the number of cells in both the X- and Y-direction, increases the robustness of the operation.  This control type is only used for 2D code types. The number of cells is not required for [`M_MAXICODE`](../../Reference/code/McodeModel.md) because it is a fixed size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for a code with any number of cells, when performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  Fits the string to be encoded into the smallest possible number of cells, when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation. |
| `Value > 0` | Specifies the number of cells. This can be any integer value. Note that for the [`M_PDF417`](../../Reference/code/McodeModel.md) code type, the value that you pass must be equal to `17_c_ + 35`, where _c_ is the required number of columns, and 35 represents the number of cells required for the start and stop characters.  For the [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code type, the value that you pass must be equal to `17_c_ + 18`, where _c_ is the required number of columns, and 18 represents the number of cells required for the start and stop characters.  For the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type, this setting should be set to 38 for a one-column code, 55 for a two-column code, 82 for a three-column code, and 99 for a four-column code. Only integer values are accepted.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4. Only integer values are accepted. |

---

### `M_CELL_NUMBER_Y`

Sets the number of cells of a 2D code in the Y-direction. Note that setting the number of cells in both the X- and Y-direction, increases the robustness of the operation.  This control type is only used for 2D code types. The number of cells is not required for [`M_MAXICODE`](../../Reference/code/McodeModel.md) because it is a fixed size. When used with [`M_PDF417`](../../Reference/code/McodeModel.md), [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), and [`M_MICROPDF417`](../../Reference/code/McodeModel.md), this represents the number of rows.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for a code with any number of cells, when performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  Fits the string to be encoded into the smallest possible number of cells, when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation. |
| `Value > 0` | Specifies the number of cells. For exact values, refer to the standard documentation of the code used.  For the [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types, [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) should have a value between 3 and 90. The value of [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) is used when calculating the value of[`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md). Only integer values are accepted.  For the [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code type, [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) should have a value between 4 and 44. Only integer values are accepted.  For the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, this setting should be set to a value greater than 4. Only integer values are accepted. |

---

### `M_CELL_SIZE_INPUT_UNITS`

Sets the units with which to interpret the [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) control types. This essentially sets the input coordinate system to use.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) generates an error if the operation is not performed on a calibrated image. |

---

### `M_ENCODING`

Sets the type of encoding scheme.  Note that training this control type ([`M_TRAIN`](../../Reference/code/McodeControl.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default encoding scheme, which is dependent on the code type and the initialization mode. The following table lists the default encoding scheme. Note that when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, [`M_DEFAULT`](../../Reference/code/McodeControl.md) is not supported in all circumstances. If your specific code type corresponds to [`M_ANY`](../../Reference/code/McodeControl.md), check the description of [`M_ANY`](../../Reference/code/McodeControl.md) to see if it can be used with an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.  | Code type | [`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md) | [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md) | | --- | --- | --- | | 4-state | [`M_ENC_US_MAIL`](../../Reference/code/McodeControl.md) | [`M_ENC_US_MAIL`](../../Reference/code/McodeControl.md) | | Aztec | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ANY`](../../Reference/code/McodeControl.md) | | BC412 | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | Codabar | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | Code 39 | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | Code 93 | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | | Code 128 | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | | Composite | [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | | Data Matrix | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ANY`](../../Reference/code/McodeControl.md) | | DotCode | [`M_ENC_ISO8`](../../Reference/code/McodeControl.md) | [`M_ENC_ISO8`](../../Reference/code/McodeControl.md) | | EAN 8 | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | EAN 13 | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | EAN 14 | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | | GS1-128 | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | | GS1 Databar | [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | | Industrial 2 of 5 | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | Interleaved 2 of 5 | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | Maxicode | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ANY`](../../Reference/code/McodeControl.md) | | MicroPDF417 | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | Micro QR | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | PDF417 | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | Pharmacode | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | Planet | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | Postnet | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | QR | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ANY`](../../Reference/code/McodeControl.md) | | Truncated PDF | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | | UPC-A | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | | UPC-E | [`M_ANY`](../../Reference/code/McodeControl.md) | [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | |
| `M_ANY` | Specifies any type of encoding scheme.  [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md) and [`M_QRCODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme for [`McodeWrite`](../../Reference/code/McodeWrite.md) operations.  [`M_EAN8`](../../Reference/code/McodeModel.md), [`M_EAN13`](../../Reference/code/McodeModel.md),[`M_UPC_A`](../../Reference/code/McodeModel.md), [`M_UPC_E`](../../Reference/code/McodeModel.md),[`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), and [`M_QRCODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme for [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations. |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space.  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space.  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeControl.md), except it also supports the following characters: (,), (-), (/) and (.).  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters.  [`M_CODE39`](../../Reference/code/McodeModel.md), [`M_CODE93`](../../Reference/code/McodeModel.md), [`M_CODE128`](../../Reference/code/McodeModel.md), [`M_EAN14`](../../Reference/code/McodeModel.md), and [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code.  [`M_AZTEC`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code.  [`M_AZTEC`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code).  [`M_AZTEC`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on.  When a code occurrence is read, the character "|" is used as a separator between the string read from the EAN 8 code and the string read from the EAN 8 add-on.  [`M_EAN8`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on.  When a code occurrence is read, the character "|" is used as a separator between the string read from the EAN 13 code and the string read from the EAN 13 add-on.  [`M_EAN13`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Expanded format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Expanded Stacked format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Limited format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Stacked format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Stacked Omnidirectional format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format.  For a composite code, this encoding scheme specifies that the 1D portion uses a GS1 Databar Truncated format and the 2D portion uses a MicroPDF417 format.  [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeControl.md), but supports the extended ASCII character set.  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) and [`M_DOTCODE`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. This encoding scheme can be used for a numeric postal code.  [`M_MAXICODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. This encoding scheme can be used for an alphanumeric postal code.  [`M_MAXICODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message.  [`M_MAXICODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. This data type has a higher level of error correction than [`M_ENC_MODE4`](../../Reference/code/McodeControl.md).  [`M_MAXICODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. This encoding scheme indicates that the resulting code is used to program a code reader.  [`M_MAXICODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers.  [`M_EAN8`](../../Reference/code/McodeModel.md), [`M_EAN13`](../../Reference/code/McodeModel.md), [`M_INDUSTRIAL25`](../../Reference/code/McodeModel.md), [`M_INTERLEAVED25`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), [`M_UPC_A`](../../Reference/code/McodeModel.md), [`M_UPC_E`](../../Reference/code/McodeModel.md), and [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) are the code types that can use this encoding scheme. If the code type is [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), spaces are also allowed. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format.  [`M_QRCODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. This version can handle more data and is more robust than model 1.  [`M_QRCODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used.  If [`M_CODE39`](../../Reference/code/McodeModel.md) or [`M_CODE93`](../../Reference/code/McodeModel.md) is the code type, [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) supports the alphabet (capital letters), digits 0 - 9, and the (-), space, ($), (/), (+), (.), (%) characters.  If [`M_BC412`](../../Reference/code/McodeModel.md) is the code type, [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) supports numbers and the alphabet (capital letters), except for the letter O.  If [`M_PDF417`](../../Reference/code/McodeModel.md), [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), or [`M_MICROPDF417`](../../Reference/code/McodeModel.md) is the code type, [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) supports ASCII and extended ASCII characters.  If [`M_CODABAR`](../../Reference/code/McodeModel.md) is the code type, [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) supports digits 0 - 9, the (-), ($), (:), (/), (.), and (+) characters, and the a, b, c, and d characters. It uses the latter four characters as start and stop characters. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on.  When a code occurrence is read, the character "|" is used as a separator between the string read from the UPC-A code and the string read from the UPC-A add-on.  [`M_UPC_A`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on.  When a code occurrence is read, the character "|" is used as a separator between the string read from the UPC-E code and the string read from the UPC-E add-on.  [`M_UPC_E`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this encoding scheme. Only integer values are accepted. |

---

### `M_ERROR_CORRECTION`

Sets the type of error correction.  > **Note:** Note that if the size of an [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code is the smallest possible size, 11x11 (M1 version), [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) must be set to either [`M_ANY`](../../Reference/code/McodeControl.md) or [`M_DEFAULT`](../../Reference/code/McodeControl.md) when performing [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.  Note that training this control type ([`M_TRAIN`](../../Reference/code/McodeControl.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default error correction scheme for the code type. The following table lists the default error correction scheme:  |   |   | | --- | --- | | 4-state | [`M_ECC_4STATE`](../../Reference/code/McodeControl.md) | | Aztec | [`M_ANY`](../../Reference/code/McodeControl.md) | | BC412 | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Codabar | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Code39 | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Code93 | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | Code128 | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | Composite | [`M_ECC_COMPOSITE`](../../Reference/code/McodeControl.md) | | Data Matrix | [`M_ANY`](../../Reference/code/McodeControl.md) | | DotCode | [`M_ANY`](../../Reference/code/McodeControl.md) | | Ean8 | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | Ean13 | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | GS1 Databar | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | Industrial 2 of 5 | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Interleaved 2 of 5 | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Maxicode | [`M_ECC_REED_SOLOMON`](../../Reference/code/McodeControl.md) | | MicroPDF417 | [`M_ECC_REED_SOLOMON`](../../Reference/code/McodeControl.md) | | Micro QR | [`M_ANY`](../../Reference/code/McodeControl.md) | | PDF417 | [`M_ANY`](../../Reference/code/McodeControl.md) | | Pharmacode | [`M_ECC_NONE`](../../Reference/code/McodeControl.md) | | Planet | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | Postnet | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | QR | [`M_ANY`](../../Reference/code/McodeControl.md) | | Truncated PDF417 | [`M_ANY`](../../Reference/code/McodeControl.md) | | UPC-A | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | | UPC-E | [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md) | |
| `M_ANY` | Specifies that the error correction type for [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations is detected automatically. This error correction scheme is only supported for write operations ([`McodeWrite`](../../Reference/code/McodeWrite.md)) when dealing with [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), or [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md).  When dealing with [`M_AZTEC`](../../Reference/code/McodeModel.md), this value specifies that error correction will make up 23% of the symbol.  [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md),[`M_QRCODE`](../../Reference/code/McodeModel.md),[`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_4STATE` | Specifies to use the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding.  [`M_4_STATE`](../../Reference/code/McodeModel.md) is the code type that can use this error correction scheme. |
| `M_ECC_200` | Specifies to use a Reed-Solomon-based algorithm as an error correction scheme.  [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) is the code type that can use this error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies to use an additional digit to check whether there is an error or not. There is error detection but not correction.  [`M_4_STATE`](../../Reference/code/McodeModel.md) [`M_BC412`](../../Reference/code/McodeModel.md), [`M_EAN8`](../../Reference/code/McodeModel.md), [`M_EAN13`](../../Reference/code/McodeModel.md), [`M_CODE39`](../../Reference/code/McodeModel.md), [`M_CODE93`](../../Reference/code/McodeModel.md), [`M_CODE128`](../../Reference/code/McodeModel.md), [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_INDUSTRIAL25`](../../Reference/code/McodeModel.md), [`M_INTERLEAVED25`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), [`M_UPC_A`](../../Reference/code/McodeModel.md), and [`M_UPC_E`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_COMPOSITE` | Specifies to use the default error correction scheme for the 1D and 2D portions of the composite code.  [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) is the code type that can use this error correction scheme. |
| `M_ECC_H` | Specifies to use the highest-level error correction scheme.  [`M_QRCODE`](../../Reference/code/McodeModel.md) is the code type that can use this error correction scheme. |
| `M_ECC_L` | Specifies to use the lowest-level error correction scheme.  [`M_QRCODE`](../../Reference/code/McodeModel.md) and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_M` | Specifies to use a medium-low level error correction scheme.  [`M_QRCODE`](../../Reference/code/McodeModel.md) and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction.  [`M_BC412`](../../Reference/code/McodeModel.md), [`M_CODABAR`](../../Reference/code/McodeModel.md), [`M_CODE39`](../../Reference/code/McodeModel.md), [`M_INDUSTRIAL25`](../../Reference/code/McodeModel.md), [`M_INTERLEAVED25`](../../Reference/code/McodeModel.md), and[`M_PHARMACODE`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_Q` | Specifies to use a medium-high level error correction scheme.  [`M_QRCODE`](../../Reference/code/McodeModel.md) and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies to use a Reed-Solomon type of error correction scheme.  [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md) and [`M_MICROPDF417`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies to use a Reed-Solomon type of error correction scheme. _n_ is an integer from 0 to 8. The higher the number, the higher the level of error correction.  [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) are the code types that can use this error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information.  [`M_AZTEC`](../../Reference/code/McodeModel.md) is the only code type that can use this error correction scheme. Only integer values are accepted. |

---

### `M_FOREGROUND_VALUE`

Sets the foreground color of the code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode.  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to[`M_IMPROVED_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_FOREGROUND_ANY`](../../Reference/code/McodeControl.md).  When using a code context with [`M_INITIALIZATION_MODE`](../../Reference/code/McodeControl.md) set to [`M_TYPICAL_RECOGNITION`](../../Reference/code/McodeAlloc.md), the default value is [`M_FOREGROUND_BLACK`](../../Reference/code/McodeControl.md) for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md), in which case, the default value is [`M_FOREGROUND_WHITE`](../../Reference/code/McodeControl.md). |
| `M_FOREGROUND_ANY` | Specifies the foreground color as black or white. Note that using this value will impact the performance of [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations and should be used only when the foreground can change or cannot be known beforehand. When used for [`McodeWrite`](../../Reference/code/McodeWrite.md) operations, the default color ([`M_FOREGROUND_WHITE`](../../Reference/code/McodeControl.md) for [`M_DOTCODE`](../../Reference/code/McodeModel.md) and [`M_FOREGROUND_BLACK`](../../Reference/code/McodeControl.md) for all other code types) is used. |
| `M_FOREGROUND_BLACK` | Specifies that the foreground color is black. |
| `M_FOREGROUND_WHITE` | Specifies that the foreground color is white. |

### Combination Constants — For training a control type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to set whether the control is to be trained.

#### `M_TRAIN`

Sets whether to activate or deactivate a specified control type for training. Note that training one control type might automatically train some other related control types that cannot be combined with [`M_TRAIN`](../../Reference/code/McodeControl.md). For example, activating [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) for training, also automatically activates [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md), and [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) for training.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default activation; the default activation depends on the control type and code model type. For a given trainable control type, if an unsupported code model is included in the training, the default value will be [`M_DISABLE`](../../Reference/code/McodeControl.md), otherwise it will be [`M_ENABLE`](../../Reference/code/McodeControl.md). |
| `M_DISABLE` | Disables the training of the specified control type. |
| `M_ENABLE` | Enables the training of the specified control type. |

### Code model settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. If you pass a code context to the [`CodeId`](../../Reference/code/McodeControl.md) parameter, the specified control type setting is applied to all the code models in the context. If none of the code models support the specified control type, an error will occur. If only some code models do not support the specified control type, the control type setting is ignored for these code models.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

---

### `M_GRADING_STANDARD_EDITION`

Sets the grading standard edition to use when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  Note that the grading standard edition must correspond to the grading standard specified using [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md).  This control type is available for all code types except [`M_MAXICODE`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies to use the most recent grading standard edition that is supported, which is dependent on the code type.  When [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md) is set to [`M_ISO_GRADING`](../../Reference/code/McodeControl.md), the default value for 1D code types supported by [`McodeGrade`](../../Reference/code/McodeGrade.md) is the same as [`M_ISO_15416_2016`](../../Reference/code/McodeControl.md). For supported 2D code types and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types, the default value is the same as [`M_ISO_15415_2011_15416_2016`](../../Reference/code/McodeControl.md).  When [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md)is set to [`M_ISO_DPM_GRADING`](../../Reference/code/McodeControl.md), the default value is the same as[`M_ISO_29158_2020`](../../Reference/code/McodeControl.md).  When [`M_GRADING_STANDARD`](../../Reference/code/McodeControl.md)is set to [`M_SEMI_T10_GRADING`](../../Reference/code/McodeControl.md), the default value is the same as[`M_SEMI_T10_0701`](../../Reference/code/McodeControl.md). |
| `M_ISO_15415_2011_15416_2000` | Specifies to use the ISO/IEC 15415:2011 and ISO/IEC 15416:2000 specifications.  This control value is available for 2D and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types, excluding [`M_MAXICODE`](../../Reference/code/McodeModel.md). |
| `M_ISO_15415_2011_15416_2016` | Specifies to use the ISO/IEC 15415:2011 and ISO/IEC 15416:2016 specifications.  This control value is available for 2D and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types, excluding [`M_MAXICODE`](../../Reference/code/McodeModel.md). |
| `M_ISO_15416_2000` | Specifies to use the ISO/IEC 15416:2000 specification.  This control value is available for 1D code types. |
| `M_ISO_15416_2016` | Specifies to use the ISO/IEC 15416:2016 specification.  This control value is available for 1D code types. |
| `M_ISO_29158_2011` | Specifies to use the ISO/IEC 29158:2011 specification.  This control value is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types. |
| `M_ISO_29158_2020` | Specifies to use the ISO/IEC 29158:2020 specification.  This control value is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types. |
| `M_SEMI_T10_0701` | Specifies to use the Semi T10-0701 specification.  This control value is available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type. |

### Code model settings for an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation. If you pass a code context to the [`CodeId`](../../Reference/code/McodeControl.md) parameter, the specified control type setting is applied to all the code models in the context. If none of the code models support the specified control type, an error will occur. If only some code models do not support the specified control type, the control type setting is ignored for these code models.

---

### `M_CELL_SIZE_MODE`

Sets how to establish the cell size to use when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to use the largest possible cell size such that the code will be resized as to just fit into the target image of the operation. |
| `M_USER_DEFINED` | Specifies that the cell size corresponds to the value of [`M_CELL_SIZE_VALUE`](../../Reference/code/McodeControl.md). |

---

### `M_CELL_SIZE_VALUE`

Sets the cell size to use when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation and [`M_CELL_SIZE_MODE`](../../Reference/code/McodeControl.md) is set to [`M_USER_DEFINED`](../../Reference/code/McodeControl.md). If [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) is set to [`M_WORLD`](../../Reference/code/McodeControl.md), specify the cell size in world units. The specified cell size is transformed to pixel units according to the calibration of the target image used for writing.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. This value is equivalent to 7 pixels for [`M_DOTCODE`](../../Reference/code/McodeModel.md), 10 pixels for [`M_MAXICODE`](../../Reference/code/McodeModel.md), or 4 pixels for all other codes types. |
| `Value > 0.0` | Specifies the cell size.  > **Note:** Note that if the cell size is specified in pixel units, the minimum value that this control type can take is 1. |

---

### `M_DOT_SHAPE`

Sets the shape of the dots used to draw foreground cells when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md)operation.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. The default value for [`M_DOTCODE`](../../Reference/code/McodeModel.md) is [`M_CIRCLE`](../../Reference/code/McodeControl.md). For all other code types that use [`M_DOT_SHAPE`](../../Reference/code/McodeControl.md), the default is [`M_SQUARE`](../../Reference/code/McodeControl.md). |
| `M_CIRCLE` | Specifies that the foreground cells will be drawn with circular dots. |
| `M_SQUARE` | Specifies that the foreground cells will be drawn with square dots. |

---

### `M_DOT_SIZE`

Sets the size of the dot (or square) when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation. The dot size corresponds to the circle diameter if [`M_DOT_SHAPE`](../../Reference/code/McodeControl.md) is set to [`M_CIRCLE`](../../Reference/code/McodeControl.md) or to the length of the square if it is set to [`M_SQUARE`](../../Reference/code/McodeControl.md).  If [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md) is set to [`M_WORLD`](../../Reference/code/McodeControl.md), specify the dot size in world units. The specified dot size is transformed to pixel units according to the calibration of the target image used for writing.  This control type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md),[`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. The default value for [`M_DOTCODE`](../../Reference/code/McodeModel.md) is 7 pixels. For all other code types that use [`M_DOT_SIZE`](../../Reference/code/McodeControl.md), the default value is equivalent to the size of the cell set by [`M_CELL_SIZE_VALUE`](../../Reference/code/McodeControl.md). |
| `Value > 0.0` | Specifies the size of the dot or square. |

### Result buffer settings dealing with an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

---

### `M_RESULT_OUTPUT_UNITS`

Sets whether to return results in pixels or world units. This essentially sets the output coordinate system to use. The setting of this control type will only affect functions within this module which return positional results. This control type can be changed at any time to return results in the required output units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. If world units are specified, calling [`McodeGetResult`](../../Reference/code/McodeGetResult.md) generates an error if the result was not calculated on a calibrated image. |

### Result buffer settings dealing with an

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

---

### `M_STRING_FORMAT`

Sets the format in which to return the string, retrieved using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STRING`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_FORMAT` *(default)* | Specifies that the string is returned in the string format associated with the code type.  Graphical interpretations are assigned depending on corresponding character sets, allowing for the use of character set ECIs. |
| `M_GS1_HUMAN_READABLE` | Specifies that the string is returned in a format that is human-readable. Note that this is the default format for both an [`M_EAN14`](../../Reference/code/McodeModel.md) and [`M_GS1_128`](../../Reference/code/McodeModel.md) code type.  This format is only available when reading the following code types:[`M_CODE128`](../../Reference/code/McodeModel.md), [`M_EAN14`](../../Reference/code/McodeModel.md), [`M_GS1_128`](../../Reference/code/McodeModel.md), [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md). |
| `M_JAPANESE` | Specifies that the string is returned in the Japanese (Windows-932) encoding. |
| `M_KOREAN` | Specifies that the string is returned in the Korean (Windows-949) encoding. |
| `M_LATIN` | Specifies that the returned string uses Latin (Windows-1252) encoding. |
| `M_RAW_DATA` | Specifies that the string is returned in a raw data format. This format will have the FNC1 separator at the beginning of the string (represented by the `è` character) and the group separators (GS) will be included in the string (represented by the `ascii=29` character or `\x1D`) (for example, è01034531200000111708050810ABCD1234\x1D4109501101020917).  This format is only available when reading the following code types:[`M_CODE128`](../../Reference/code/McodeModel.md), [`M_EAN14`](../../Reference/code/McodeModel.md), [`M_GS1_128`](../../Reference/code/McodeModel.md), [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), all 2D code types (except [`M_MICROQRCODE`](../../Reference/code/McodeModel.md)), and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.  When decoding character set ECIs for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md),[`M_MICROPDF417`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), or [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types, encodable ECIs are inserted at the appropriate locations. All back slashes (\) in the string are doubled (\\); while typically unprintable characters, such as Arabic characters, are put in a \xnnnnnn format, where nnnnnn is the hexadecimal value representing the unprintable character. |
| `M_SIMPLIFIED_CHINESE` | Specifies that the returned string uses Simplified Chinese (Windows-936) encoding. |
| `M_UPCE_COMPRESSED` | Specifies that the returned string is in the UPCE compressed string format. Note that this is only available when reading an [`M_UPC_E`](../../Reference/code/McodeModel.md) code. Otherwise, an error will occur. |
| `M_UTF8` | Specifies that the returned string is in the UTF-8 string format. |

### Result buffer settings dealing with

The following [`ControlType`](../../Reference/code/McodeControl.md) and corresponding [`ControlValue`](../../Reference/code/McodeControl.md) parameter settings can be specified when dealing with an [`McodeDetect`](../../Reference/code/McodeDetect.md) operation. In this case, an [`M_CODE_DETECT_RESULT`](../../Reference/code/McodeAllocResult.md) result buffer must be passed to the [`CodeId`](../../Reference/code/McodeControl.md) parameter.

---

### `M_STOP_DETECT`

Stops the current [`McodeDetect`](../../Reference/code/McodeDetect.md) operation being performed on the code result buffer passed to [`CodeId`](../../Reference/code/McodeControl.md).  This must be called from another thread, typically of higher priority.  After making this call, the results returned to the result buffer of the [`McodeDetect`](../../Reference/code/McodeDetect.md) operation are invalid and discarded.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. |

This control type is available for [`M_DATAMATRIX`](../../Reference/code/McodeModel.md),[`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

This control type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

This format is only available when reading the following code types: [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), [`M_MICROPDF417`](../../Reference/code/McodeModel.md), [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), and [`M_DATAMATRIX`](../../Reference/code/McodeModel.md).

This control type is only available for 2D code types.

This control type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

When entering a value in world units, the number that results from the internal transformation of world units to pixel units must still respect the value's pixel unit constraint. To find the range of allowed values in world units, according to your associated camera calibration, use [`McalTransformResult`](../../Reference/cal/McalTransformResult.md).

When [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) is enabled, codes are sought at the angular range defined by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `-` [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) to [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `+` [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md). When [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) is disabled, this setting has no effect.

For the [`M_AZTEC`](../../Reference/code/McodeModel.md),[`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md),[`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md)code types, the angular range does not affect the speed of the operation.

Note that the value of this control type can also be set using [`M_DPM_CALIBRATION_RESULTS`](../../Reference/code/McodeControl.md).

When the search speed [`M_SPEED`](../../Reference/code/McodeControl.md) is set to [`M_VERY_LOW`](../../Reference/code/McodeControl.md), a more exhaustive search algorithm is enabled that can exceed the specified angle range. In this case, the specified angle range is used as a starting reference for the search instead of as a search restriction.

Note that [`McodeControl`](../../Reference/code/McodeControl.md) generates an error if you set both a range of cells for which to search (using [`M_CELL_NUMBER_X_MAX`](../../Reference/code/McodeControl.md), [`M_CELL_NUMBER_X_MIN`](../../Reference/code/McodeControl.md), [`M_CELL_NUMBER_Y_MAX`](../../Reference/code/McodeControl.md), and/or [`M_CELL_NUMBER_Y_MIN`](../../Reference/code/McodeControl.md)) and an explicit number of cells for which to search (using [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) and/or [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md)).

This format is not available with Windows Embedded Compact.

Note that this control type can be trained using [`McodeTrain`](../../Reference/code/McodeTrain.md); activate it for training with the combination constant [`M_TRAIN`](../../Reference/code/McodeControl.md) or with [`M_SET_TRAINING_STATE_ALL`](../../Reference/code/McodeControl.md).
