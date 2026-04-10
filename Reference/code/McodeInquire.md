---
doctype: Reference
module: code
function: McodeInquire
product: Aurora-Imaging-Library-reference
version: 1100
path: "Reference / code / McodeInquire"
---

# McodeInquire

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

> Inquire about a code context, code model, or code result setting.

## Syntax

```c
AIL_INT McodeInquire(
    AIL_ID    CodeId,       //in
    AIL_INT64 InquireType,  //in
    void *    UserVarPtr    //out
)
```

## Description

This function inquires about a setting of the specified code context, code model, or code result buffer.

## Parameters

### `CodeId` *(in, AIL_ID)*

Specifies the identifier of the code context, code model, or code result buffer.

### `InquireType` *(in, AIL_INT64)*

Specifies the setting about which to inquire.

### `UserVarPtr` *(out, *void)*

Specifies the address in which to return the value of the inquired setting. Since the [`McodeInquire`](../../Reference/code/McodeInquire.md) function also returns the requested information, you can set this parameter to `M_NULL`.

## Parameter Associations

### For inquiring about code context or result buffer settings

To inquire about general code context settings or code result buffer settings, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code context or code result buffer.

---

### `M_OWNER_SYSTEM`

Inquires the identifier of the system on which the code context or code result buffer has been allocated.

| Value | Description |
| --- | --- |
| `M_DEFAULT_HOST` | Specifies the default Host system of the current Aurora Imaging Library application. |
| `System identifier` | Specifies a valid system identifier, previously allocated using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md). |

---

### `M_SUPPORTED_CODE_TYPES`

Inquires all code types supported by the module.

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

---

### `M_SUPPORTED_CODE_TYPES_1D`

Inquires all 1D code types supported by the module.

| Value | Description |
| --- | --- |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |

---

### `M_SUPPORTED_CODE_TYPES_2D`

Inquires all 2D code types supported by the module.

| Value | Description |
| --- | --- |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |

---

### `M_SUPPORTED_CODE_TYPES_DETECT`

Inquires all code types supported by [`McodeDetect`](../../Reference/code/McodeDetect.md).

| Value | Description |
| --- | --- |
| *(see [`AcceptedCodeTypes`](Reference/code/McodeDetect.md))* |  |

---

### `M_SUPPORTED_CODE_TYPES_POSTAL`

Inquires all postal code types supported by the module.

| Value | Description |
| --- | --- |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |

### Combination Constants — For determining the required array size (number of elements) to store the returned values

> *Optional, cannot be used alone.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the required array size (number of elements) to store the returned values.

#### `M_NB_ELEMENTS`

Retrieves the required array size (number of elements) to store the returned values.

### For inquiring about the number of code models in a code context

To inquire about the number of code models in a code context, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to the following value. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code context.

---

### `M_NUMBER_OF_CODE_MODELS`

Inquires the number of code models in the code context.

| Value | Description |
| --- | --- |
| `Value >= 0` | Specifies the number of code models in the code context. |

### For inquiring about code context settings that affect an

To inquire about general code context settings that affect an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code context.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although the settings of all these inquire types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the inquire types to determine if their corresponding control type can be activated for training; you can also use filters to limit the inquire types in the table to those whose control type can be trained.

---

### `M_INITIALIZATION_MODE`

Inquires the initialization mode of the code context.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_IMPROVED_RECOGNITION` | Specifies a code context that might provide a more robust [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation. |
| `M_TYPICAL_RECOGNITION` *(default)* | Specifies a code context that might provide a quicker [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, but might potentially produce less robust results. |

---

### `M_MINIMUM_CONTRAST`

Inquires the minimum possible contrast between the foreground and background in the target image.  This inquire type is only available for 1D code types (excluding [`M_PLANET`](../../Reference/code/McodeModel.md) and [`M_POSTNET`](../../Reference/code/McodeModel.md)), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and 2D cross-row code types.  Note that training the corresponding control type ([`M_TRAIN`](../../Reference/code/McodeInquire.md)) is only supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is 50. |
| `0 <= Value <= 255` | Specifies the minimum contrast. |

---

### `M_SCANLINE_HEIGHT`

Inquires the scanline height (or thickness).  This inquire type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies a default scanline height that is based on the code type. |
| `Value` | Specifies the scanline height, relative to the input coordinate system specified using [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_SCANLINE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_SCANLINE_STEP`](../../Reference/code/McodeControl.md) and [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md) inquire types.  This inquire type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_SCANLINE_STEP`

Inquires the scanline step (or interval).  This inquire type is only available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies a default scanline step that is based on the value of [`M_SPEED`](../../Reference/code/McodeInquire.md). |
| `Value` | Specifies the scanline step, relative to the input coordinate system specified using [`M_SCANLINE_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_SEARCH_ANGLE_MODE`

Inquires whether the search angular range algorithm for the code context is enabled.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that the search angular range algorithm is not used. |
| `M_ENABLE` *(default)* | Specifies that the search angular range algorithm is used. |

---

### `M_SPEED`

Inquires the search speed.  This inquire type is available for all code types except [`M_DOTCODE`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the initialization mode. |
| `M_HIGH` | Specifies a high search speed. |
| `M_LOW` | Specifies a low search speed. |
| `M_MEDIUM` | Specifies a medium search speed. |
| `M_VERY_HIGH` | Specifies a very high search speed. |
| `M_VERY_LOW` | Specifies a very low search speed. |

---

### `M_THRESHOLD_MODE`

Inquires the threshold mode used to internally binarize the source image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_ADAPTIVE` | Specifies to use a fast dynamic local threshold. |
| `M_GLOBAL_SEGMENTATION` | Specifies to use a global threshold value. |
| `M_GLOBAL_WITH_LOCAL_RESEGMENTATION` | Specifies that the source image will be globally thresholded and then the edges in the binarized image are resegmented according to the intensities of the surrounding bars and spaces in the original source image. |

---

### `M_THRESHOLD_VALUE`

Inquires the threshold value used to internally binarize the source image, depending on the threshold mode.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_THRESHOLD_MODE`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_COMPUTE` *(default)* | Specifies the threshold value automatically. |
| `0 <= Value <= 255` | Specifies the threshold value. |

---

### `M_TIMEOUT`

Inquires the maximum decoding time for an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, in msec.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies that there is no maximum decoding time. |
| `Value >= 0` *(default)* | Specifies the maximum decoding time, in msec. |

---

### `M_TOTAL_NUMBER`

Inquires the total number of codes to be read in one source image. Note that this number is limited by the maximum number of occurrences of each code model to read or grade ([`M_NUMBER`](../../Reference/code/McodeInquire.md)).  Only 1D code types (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md) code types), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type support searching for multiple occurrences.  Note that for [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) and [`M_DOTCODE`](../../Reference/code/McodeModel.md) code types, [`M_TOTAL_NUMBER`](../../Reference/code/McodeInquire.md) and [`M_NUMBER`](../../Reference/code/McodeInquire.md) always return the same value because a code context can only contain multiple code models of the above-mentioned 1D code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ALL` *(default)* | Specifies to read/grade all code occurrences in the source image, up to the maximum number of occurrences to read/grade of each code model ([`M_NUMBER`](../../Reference/code/McodeInquire.md)). |
| `Value > 0` | Specifies the maximum number of codes to read/grade in one source image. |

### For inquiring about code context settings that affect an

To inquire about general code context settings that affect an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code context.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

---

### `M_ABSOLUTE_APERTURE_SIZE`

Inquires the absolute size (diameter) of the aperture.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value >= 0` *(default)* | Specifies the absolute aperture size, relative to the input coordinate system specified using [`M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeInquire.md) inquire type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. |

---

### `M_APERTURE_MODE`

Inquires the way in which the aperture size is determined.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSOLUTE` | Specifies to use the absolute aperture size, set using [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeInquire.md). |
| `M_DISABLE` | Specifies to disable the aperture. |
| `M_RELATIVE` *(default)* | Specifies to use a relative aperture size, based on the cell size (using [`M_CELL_SIZE...`](../../Reference/code/McodeInquire.md)) and the relative aperture factor (using [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeInquire.md)). |

---

### `M_EXTENDED_AREA_REFLECTANCE_CHECK`

Inquires whether the grading must perform an additional reflectance check over an area containing the code occurrence and its extended area (that is, 20 times the cell size beyond the quiet zone on all sides).  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the additional reflectance check. |
| `M_ENABLE` | Specifies to enable the additional reflectance check. |

---

### `M_GRADE_QUIET_ZONE`

Inquires whether to include the quiet zone when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  This inquire type is only available for [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` | Specifies to exclude the quiet zone. |
| `M_ENABLE` *(default)* | Specifies to include the quiet zone. |

---

### `M_GRADING_STANDARD`

Inquires the grading standard used when performing an[`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ISO_DPM_GRADING` | Specifies  an ISO/IEC 29158 specification. |
| `M_ISO_GRADING` *(default)* | Specifies  an ISO/IEC 15416 or ISO/IEC 15415 specification. |
| `M_SEMI_T10_GRADING` | Specifies  a Semi T10 specification. |

---

### `M_INSPECTION_BAND_RATIO`

Inquires the height of the inspection band as a percentage of the average bar height.  This inquire type is only available for 1D code types, 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default percentage. |
| `0.0 < Value < 100.0` | Specifies the percentage of the average bar height used as the height of the inspection band. |

---

### `M_LIGHTING_CONFIGURATION`

Inquires the tilted coaxial lighting and camera angle position (TCL) relative to the marked object.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_90_DEGREE` | Specifies the TCL is 90 degrees relative to the marked object. |
| `M_UNSPECIFIED` *(default)* | Specifies that the TCL is none of the other values. |

---

### `M_MAXIMUM_CALIBRATED_REFLECTANCE`

Inquires the maximum possible grayscale value in the target image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the maximum calibrated reflectance. |

---

### `M_MEAN_LIGHT_CALIBRATION`

Inquires the expected mean light (MLcal). This corresponds to the expected mean intensity of the centers of the white elements of the code occurrence.  This value is used during the target grading phase of the ISO/IEC 29158 specification.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is equal to the setting of [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeInquire.md). |
| `0 <= Value <= 255` | Specifies the mean intensity. |

---

### `M_MINIMUM_CALIBRATED_REFLECTANCE`

Inquires the minimum possible grayscale value in the target image.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `0 <= Value <= 255` *(default)* | Specifies the minimum calibrated reflectance. |

---

### `M_NUMBER_OF_SCANLINES`

Inquires the number of scanlines inside the inspection band to inspect during the [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, after the code occurrence has been located.  This inquire type is only available for 1D code types, 2D cross-row code types, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value. |
| `M_ALL` | Specifies to use all possible scanlines inside the inspection band. |
| `Value > 0` | Specifies the number of scanlines to inspect. |

---

### `M_PIXEL_SIZE_IN_MM`

Inquires the scale between a pixel and its physical measurement, in millimeters per pixel units.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_UNKNOWN` *(default)* | Specifies that the scale between a pixel and its physical measurement is not known, in mm per pixel units. |
| `Value > 0` | Specifies the scale between a pixel and its physical measurement, in mm per pixel units. |

---

### `M_REFLECTANCE_CALIBRATION`

Inquires the expected reflectance value (Rcal). This value is used during the target grading phase of the ISO/IEC 29158 specification.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value; the default value is equal to the setting of [`M_MAXIMUM_CALIBRATED_REFLECTANCE`](../../Reference/code/McodeInquire.md). |
| `0 <= Value <= 255` | Specifies the reflectance value. |

---

### `M_RELATIVE_APERTURE_FACTOR`

Inquires the relative aperture factor.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the aperture factor is chosen according to the ISO/IEC 15416 specification for all supported code types except 2D matrix code types; for the latter, the aperture factor is chosen according to the ISO/IEC 15415 or ISO/IEC 29158 specification (depends on [`M_GRADING_STANDARD`](../../Reference/code/McodeInquire.md)). |
| `0 <= Value <= 2` | Specifies the aperture factor. |

---

### `M_SYSTEM_RESPONSE_CALIBRATION`

Inquires the System Response value derived during the reflectance calibration phase (SRcal) of the ISO/IEC 29158 specification.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the System Response value for the reference code. |

---

### `M_SYSTEM_RESPONSE_TARGET`

Inquires the System Response value derived during the target grading phase (SRtarget) of the ISO/IEC 29158 specification.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `Value > 0.0` *(default)* | Specifies the System Response value for the target code. |

### For inquiring about code context settings that affect an

To inquire about general code context settings that affect an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code context.

---

### `M_TRAIN_TIMEOUT`

Inquires the maximum training time for an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to disable the timeout. |
| `Value >= 0` | Specifies the timeout limit, in msec. |

### For inquiring about a code model's code type

To inquire about the code type of a code model, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to the following value. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code model.

---

### `M_CODE_TYPE`

Inquires the code type of the specified model.

| Value | Description |
| --- | --- |
| `M_ANY` | Matches all code types. |
| `M_4_STATE` | Specifies a 4-state code type. |
| `M_BC412` | Specifies a BC412 code type. |
| `M_CODABAR` | Specifies a Codabar code type. |
| `M_CODE39` | Specifies a Code 39 code type. |
| `M_CODE93` | Specifies a Code 93 code type. |
| `M_CODE128` | Specifies a Code 128 code type. |
| `M_EAN8` | Specifies an EAN 8 code type. |
| `M_EAN13` | Specifies an EAN 13 code type. |
| `M_EAN14` | Specifies an EAN 14 code type. |
| `M_GS1_128` | Specifies a GS1-128 code type. |
| `M_GS1_DATABAR` | Specifies a GS1 Databar code type. |
| `M_IATA25` | Specifies an IATA 2 of 5 code type. |
| `M_INDUSTRIAL25` | Specifies an Industrial 2 of 5 (standard 2 of 5) code type. |
| `M_INTERLEAVED25` | Specifies an Interleaved 2 of 5 (ITF-14) code type. |
| `M_PHARMACODE` | Specifies a Pharmacode code type. |
| `M_PLANET` | Specifies a Planet code type. |
| `M_POSTNET` | Specifies a Postnet code type. |
| `M_UPC_A` | Specifies a UPC-A code type. |
| `M_UPC_E` | Specifies a UPC-E code type. |
| `M_AZTEC` | Specifies an Aztec code type. |
| `M_DATAMATRIX` | Specifies a Data Matrix code type. |
| `M_DOTCODE` | Specifies a DotCode code type. |
| `M_MAXICODE` | Specifies a Maxicode code type. |
| `M_MICROPDF417` | Specifies a MicroPDF417 code type. |
| `M_MICROQRCODE` | Specifies a Micro QR code type. |
| `M_PDF417` | Specifies a PDF417 code type. |
| `M_QRCODE` | Specifies a QR code type. |
| `M_TRUNCATED_PDF417` | Specifies a Truncated PDF417 code type. |
| `M_COMPOSITECODE` | Specifies a composite code type. |

### For inquiring about code model settings that affect an

To inquire about code model settings that affect an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), or [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code model.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although the settings of all these inquire types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the inquire types to determine if their corresponding control type can be activated for training; you can also use filters to limit the inquire types in the table to those whose control type can be trained.

---

### `M_BEARER_BAR`

Inquires whether bearer bars run along the top and bottom of the code occurrences to read (such as, the edge of a sticker).  This value is available only for 1D code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ABSENT` *(default)* | Specifies that no bearer bars are above and below the code. |
| `M_PRESENT` | Specifies that there are bearer bars above and below the code. |

---

### `M_CELL_NUMBER_X_MAX`

Inquires the maximum number of cells for which to search in the X-direction.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_CELL_NUMBER_X`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. |

---

### `M_CELL_NUMBER_X_MIN`

Inquires the minimum number of cells for which to search in the X-direction.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_CELL_NUMBER_X`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. |

---

### `M_CELL_NUMBER_Y_MAX`

Inquires the maximum number of cells for which to search in the Y-direction.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_CELL_NUMBER_Y`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the maximum number of cells for which to search. |

---

### `M_CELL_NUMBER_Y_MIN`

Inquires the minimum number of cells for which to search in the Y-direction.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_CELL_NUMBER_Y`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for code occurrences with any number of cells. |
| `Value > 0` | Specifies the minimum number of cells for which to search. |

---

### `M_CELL_SIZE_MAX`

Inquires the maximum cell size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to automatically select an appropriate size, which is dependent on the code type and the initialization mode. |
| `Value` | Specifies the maximum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_CELL_SIZE_MIN`

Inquires the minimum cell size.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default cell size, in pixels, which is dependent on the operation and code type. |
| `Value` | Specifies the minimum cell size, relative to the input coordinate system specified using [`M_CELL_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_CHECK_FINDER_PATTERN`

Inquires whether checking for a false Data Matrix pattern is enabled.  This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that the Code module will not check for false [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code types. |
| `M_ENABLE` | Specifies that the Code module will check for false [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code types. |

---

### `M_CHECK_QUIET_ZONE`

Inquires whether the presence of the quiet zone is necessary for a successful [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation of this code type.  > **Note:** Note that the specifications of a code's quiet zone are dependent upon the code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `M_DISABLE` | Specifies that a quiet zone is not necessary. |
| `M_ENABLE` | Specifies that a quiet zone is necessary. |

---

### `M_CODE_FLIP`

Inquires whether code occurrences need to be flipped or read in the opposite direction to be read properly.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the initialization mode. |
| `M_ANY` | Allows Aurora Imaging Library to decide whether a code occurrence needs to be flipped or read in the opposite direction to be read properly. |
| `M_FLIP` | Specifies that code occurrences need to be flipped or read in the opposite direction to be read properly. |
| `M_NO_FLIP` | Specifies that code occurrences don't need to be flipped or read in the opposite direction. |

---

### `M_CODE_SEARCH_MODE`

Inquires the behavior of the decoding algorithm.  This inquire type is available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `M_BEST` | Specifies to search and return the best quality code occurrences. |
| `M_FAST` | Specifies to search and return the first code occurrences decoded. |

---

### `M_DATAMATRIX_SHAPE`

Inquires the shape of the Data Matrix code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type can be any shape. |
| `M_RECTANGLE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a rectangular shape. |
| `M_SQUARE` | Specifies that the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code has a square shape. |

---

### `M_DECODE_ALGORITHM`

Inquires the decoding algorithm used to read the code occurrences.  This inquire type is available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_PDF417`](../../Reference/code/McodeModel.md), and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types.  Note that training the corresponding control type ([`M_TRAIN`](../../Reference/code/McodeInquire.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_CODE_DEFORMED` | Specifies to use the algorithm to decode deformed code occurrences. |
| `M_CODE_NOT_DEFORMED` | Specifies to use the algorithm to decode non-deformed code occurrences. |
| `M_SCANLINE_AT_ANGLE` | Specifies to use the algorithm to decode [`M_PDF417`](../../Reference/code/McodeModel.md) or [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code occurrences using rotated scanlines, without any localization. |

---

### `M_DOT_SIZE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_DOT_SIZE_MAX`](../../Reference/code/McodeControl.md) and [`M_DOT_SIZE_MIN`](../../Reference/code/McodeControl.md) inquire types.  This inquire type is only available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_DOT_SIZE_MAX`

Inquires the maximum diameter of the dots for which to search.  This inquire type is only available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode. |
| `M_AUTO` | Specifies to select an appropriate size, automatically. |
| `Value > 0` | Specifies the maximum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_DOT_SIZE_MIN`

Inquires the minimum diameter of the dots for which to search.  This inquire type is only available for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` | Specifies to select an appropriate size, automatically. |
| `Value > 0` *(default)* | Specifies the minimum dot size, relative to the input coordinate system specified using [`M_DOT_SIZE_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_DOT_SPACING_INPUT_UNITS`

Inquires the units with which to interpret the [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md) and [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md)inquire types.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies what convention is used, automatically. |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. |

---

### `M_DOT_SPACING_MAX`

Inquires the maximum distance between 2 dots in a matrix code type composed of dots.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode. |
| `-256 <= Value <= 256` | Specifies the distance, relative to the input coordinate system specified using [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_DOT_SPACING_MIN`

Inquires the minimum distance between 2 dots in a matrix code type composed of dots.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_MAXICODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies the default value, which is dependent on the initialization mode. |
| `-256 <= Value <= 256` | Specifies the distance, relative to the input coordinate system specified using [`M_DOT_SPACING_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_ECC_CORRECTED_NUMBER`

Inquires whether [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md)are forced to perform a more robust operation to minimize the number of errors to correct.  This inquire type is only available for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies not to perform a more robust operation. |
| `M_ENABLE` | Specifies to perform a more robust operation. |

---

### `M_FINDER_PATTERN_EXHAUSTIVE_SEARCH`

Inquires whether to search for the L-shaped finder pattern (the gray boxed area in the following image) to help localize the Data Matrix code.  This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies to not perform an exhaustive search. |
| `M_ENABLE` | Specifies to perform an exhaustive search. |

---

### `M_FINDER_PATTERN_INPUT_UNITS`

Inquires the units with which to interpret the [`M_FINDER_PATTERN_EXHAUSTIVE_SEARCH`](../../Reference/code/McodeControl.md) and [`M_FINDER_PATTERN_MINIMUM_LENGTH`](../../Reference/code/McodeControl.md) inquire types.  This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_FINDER_PATTERN_MAX_GAP`

Inquires the maximum tolerable gap in the finder pattern of a Data Matrix code.  This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the maximum tolerable gap in the finder pattern is 6 times the minimum cell size specified using [`M_CELL_SIZE_MIN`](../../Reference/code/McodeInquire.md). |
| `Value` | Specifies the maximum gap allowed, in input units specified using [`M_FINDER_PATTERN_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_FINDER_PATTERN_MINIMUM_LENGTH`

Inquires the shortest acceptable length of either "arm" of the finder pattern of a Data Matrix code occurrence.  This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies that the minimum acceptable finder pattern "arm" length is 6 times the minimum cell size specified using [`M_CELL_SIZE_MIN`](../../Reference/code/McodeInquire.md). |
| `Value` | The minimum acceptable finder pattern "arm" length, in input units specified using [`M_FINDER_PATTERN_INPUT_UNITS`](../../Reference/code/McodeInquire.md). |

---

### `M_NUMBER`

Inquires the maximum number of codes to be read for the specified code model. This number is also limited by [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md).  Only 1D code types (excluding [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md), [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md) code types), the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type, and the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type support searching for multiple occurrences.  Note that training the corresponding control type ([`M_TRAIN`](../../Reference/code/McodeInquire.md)) is only supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_ALL` | Specifies that all code model occurrences are read/graded up to the maximum number limited by [`M_TOTAL_NUMBER`](../../Reference/code/McodeInquire.md). |
| `Value >= 0` *(default)* | Specifies the maximum number of code occurrences of the specified code model to read or grade. |

---

### `M_POSITION_ACCURACY`

Inquires the accuracy of positional results. Accuracy depends on the settings of the code context and its models.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_HIGH` | Specifies to report the positional results of the code operation with high accuracy. |
| `M_LOW` | Specifies to report the positional results of the code operation with low accuracy. |

---

### `M_SEARCH_ANGLE`

Inquires the nominal search angle.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_REGION` | Specifies that the nominal angle is set to the angle of the target image's ROI ([`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md)); recall that for [`McodeGrade`](../../Reference/code/McodeGrade.md) and [`McodeRead`](../../Reference/code/McodeRead.md), the ROI must be rectangular. |
| `0.0 <= Value <= 360.0` *(default)* | Specifies the nominal angle, in degrees, relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md). |

---

### `M_SEARCH_ANGLE_DELTA_NEG`

Inquires the negative angular range of the search.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `0.0 <= Value <= 180.0` | Specifies a negative angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md). |

---

### `M_SEARCH_ANGLE_DELTA_POS`

Inquires the positive angular range of the search.  Note that the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `0.0 <= Value <= 180.0` | Specifies a positive angular range, in degrees, relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md). |

---

### `M_SEARCH_ANGLE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) inquire type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the value in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the value in world units, with respect to the relative coordinate system. |

---

### `M_SEARCH_ANGLE_STEP`

Inquires the angle increment/decrement used when searching for codes through an angular range.  This inquire type is available for 1D code types (excluding [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PLANET`](../../Reference/code/McodeModel.md), and [`M_POSTNET`](../../Reference/code/McodeModel.md)). It is also available for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types when [`M_DECODE_ALGORITHM`](../../Reference/code/McodeInquire.md) is set to [`M_SCANLINE_AT_ANGLE`](../../Reference/code/McodeInquire.md).  Note that for [`M_PDF417`](../../Reference/code/McodeModel.md) and [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) code types, the corresponding control type can be automatically activated for training; to inquire if it has been activated for training, use [`M_SEARCH_ANGLE`](../../Reference/code/McodeInquire.md) with [`M_TRAIN`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_DISABLE` *(default)* | Specifies that no explicit increment/decrement is used. |
| `0.1 <= Value <= 180.0` | Specifies the explicit angle increment/decrement, in degrees. |

---

### `M_STRING_SIZE_MAX`

Inquires the maximum size (number of characters) of the string encoded in a code occurrence.  For [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code types, the maximum string size is ignored when performing [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `M_ANY` | Specifies that there is no maximum string size. |
| `0 <= Value <= 65535` | Specifies the maximum string size. |

---

### `M_STRING_SIZE_MIN`

Inquires the minimum size (number of characters) of the string encoded in the code.  For [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code types, the minimum string size is ignored when performing [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `M_ANY` | Specifies that there is no minimum string size. |
| `0 <= Value <= 65535` | Specifies the minimum string size. |

---

### `M_SUB_TYPE`

Inquires the particular code sub-types for which to search.  This value is available only for [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) and [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for all of the code sub-types that can be specified for [`M_SUB_TYPE`](../../Reference/code/McodeInquire.md). |
| `M_EAN8` | Specifies that the EAN 8 code sub-type is enabled. |
| `M_EAN13` | Specifies that the EAN 13 code sub-type is enabled. |
| `M_GS1_128` | Specifies that the GS1-128 code sub-type is enabled. |
| `M_GS1_DATABAR_EXPANDED` | Specifies that the GS1 Databar Expanded code sub-type is enabled. |
| `M_GS1_DATABAR_EXPANDED_STACKED` | Specifies that the GS1 Databar Expanded Stacked code sub-type is enabled. |
| `M_GS1_DATABAR_LIMITED` | Specifies that the GS1 Databar Limited code sub-type is enabled. |
| `M_GS1_DATABAR_OMNI` | Specifies that the GS1 Databar Omnidirectional code sub-type is enabled. |
| `M_GS1_DATABAR_STACKED` | Specifies that the GS1 Databar Stacked code sub-type is enabled. |
| `M_GS1_DATABAR_STACKED_OMNI` | Specifies that the GS1 Databar Stacked Omnidirectional code sub-type is enabled. |
| `M_GS1_DATABAR_TRUNCATED` | Specifies that the GS1 Databar Truncated code sub-type is enabled. |
| `M_UPC_A` | Specifies that the UPC-A code sub-type is enabled. |
| `M_UPC_E` | Specifies that the UPC-E code sub-type is enabled. |

---

### `M_USE_PRESEARCH`

Inquires whether the localization operation is performed prior to the decoding step of an operation.  This inquire type is available for 2D code types, excluding the[`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_DISABLE` | Specifies that the operation is not performed. |
| `M_FINDER_PATTERN_BASE` | Specifies that the localization operation is only performed on the base pattern of the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code (an "L" starting at the top-most left corner, and ending on the bottom-most right corner of the code). |
| `M_STAT_BASE` | Specifies to localize the code within the image with the statistical characteristics of a 2D bar code (for example, local variance and the presence of a lot of edges). |

### For inquiring about code model settings that affect an

To inquire about code model settings that affect an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, the [`InquireType`](../../Reference/code/McodeInquire.md) parameter can be set to one of the following values. In this case, set the [`CodeId`](../../Reference/code/McodeInquire.md) parameter to a code model.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.  Although the settings of all these inquire types affect the results of [`McodeTrain`](../../Reference/code/McodeTrain.md), see the description of the inquire types to determine if their corresponding control type can be activated for training; you can also use filters to limit the inquire types in the table to those whose control type can be trained.

---

### `M_CELL_NUMBER_X`

Inquires the number of cells to be used in the X-direction.  This inquire type is only available for 2D code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for a code with any number of cells, when performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. |
| `Value > 0` | Specifies the number of cells. |

---

### `M_CELL_NUMBER_Y`

Inquires the number of cells to be used in the Y-direction.  This inquire type is only available for 2D code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ANY` *(default)* | Specifies to search for a code with any number of cells, when performing an [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. |
| `Value > 0` | Specifies the number of cells. |

---

### `M_CELL_SIZE_INPUT_UNITS`

Inquires the units with which to interpret the [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) inquire types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_PIXEL` *(default)* | Specifies to interpret the values in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies to interpret the values in world units, with respect to the relative coordinate system. |

---

### `M_ENCODING`

Inquires the type of encoding scheme.  Note that training the corresponding control type ([`M_TRAIN`](../../Reference/code/McodeInquire.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default encoding scheme, which is dependent on the code type and the initialization mode. |
| `M_ANY` | Specifies any type of encoding scheme. |
| `M_ENC_ALPHA` | Specifies an encoding scheme that supports uppercase alphabetical characters, along with the space. |
| `M_ENC_ALPHANUM` | Specifies an encoding scheme that supports alphanumeric characters, as well as the space. |
| `M_ENC_ALPHANUM_PUNC` | Specifies a similar encoding scheme to [`M_ENC_ALPHANUM`](../../Reference/code/McodeInquire.md), except it also supports the following characters: (,), (-), (/) and (.). |
| `M_ENC_ASCII` | Specifies an encoding scheme that supports ASCII characters. |
| `M_ENC_AUSTRALIA_MAIL_C` | Specifies an encoding scheme for a 4-state format used with the C encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_N` | Specifies an encoding scheme for a 4-state format used with the N encoding table by the Australian Mail service. |
| `M_ENC_AUSTRALIA_MAIL_RAW` | Specifies an encoding scheme for a 4-state format used by the Australian Mail service. |
| `M_ENC_AZTEC_COMPACT` | Specifies an encoding scheme for a compact Aztec code. |
| `M_ENC_AZTEC_FULL_RANGE` | Specifies an encoding scheme for a full-range (not compact) Aztec code. |
| `M_ENC_AZTEC_RUNE` | Specifies an encoding scheme for an Aztec rune (the smallest version of an Aztec code). |
| `M_ENC_EAN8` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 8 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN8_ADDON` | Specifies an encoding scheme for an EAN 8 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_EAN13` | Specifies an encoding scheme for a composite code whose 1D portion uses an EAN 13 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_EAN13_ADDON` | Specifies an encoding scheme for an EAN 13 format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_GS1_128_MICROPDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_GS1_128_PDF417` | Specifies an encoding scheme for a composite code whose 1D portion uses a GS1 128 format and whose 2D portion uses a PDF417 format. |
| `M_ENC_GS1_DATABAR_EXPANDED` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_EXPANDED_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Expanded Stacked format. |
| `M_ENC_GS1_DATABAR_LIMITED` | Specifies an encoding scheme that uses a GS1 Databar Limited format. |
| `M_ENC_GS1_DATABAR_OMNI` | Specifies an encoding scheme that uses a GS1 Databar format. |
| `M_ENC_GS1_DATABAR_STACKED` | Specifies an encoding scheme that uses a GS1 Databar Stacked format. |
| `M_ENC_GS1_DATABAR_STACKED_OMNI` | Specifies an encoding scheme that uses a GS1 Databar Stacked Omnidirectional format. |
| `M_ENC_GS1_DATABAR_TRUNCATED` | Specifies an encoding scheme that uses a GS1 Databar Truncated format. |
| `M_ENC_ISO8` | Specifies a similar encoding scheme as [`M_ENC_ASCII`](../../Reference/code/McodeInquire.md), but supports the extended ASCII character set. |
| `M_ENC_KOREA_MAIL` | Specifies an encoding scheme for a 4-state format used by the Korean Mail service. |
| `M_ENC_MODE2` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE3` | Specifies an encoding scheme that requires a Structured Carrier Message. |
| `M_ENC_MODE4` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE5` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_MODE6` | Specifies an encoding scheme that requires a Free Format Message. |
| `M_ENC_NUM` | Specifies an encoding scheme that only supports numbers. |
| `M_ENC_QRCODE_MODEL1` | Specifies an encoding scheme that uses an older version of the QR code format. |
| `M_ENC_QRCODE_MODEL2` | Specifies an encoding scheme that uses a newer version of the QR code format. |
| `M_ENC_STANDARD` | Specifies different types of encoding schemes, depending on what code type is used. |
| `M_ENC_UK_MAIL` | Specifies an encoding scheme for a 4-state format used by the UK Mail service. |
| `M_ENC_UPCA` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-A format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCA_ADDON` | Specifies an encoding scheme for an UPC-A format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_UPCE` | Specifies an encoding scheme for a composite code whose 1D portion uses an UPC-E format and whose 2D portion uses a MicroPDF417 format. |
| `M_ENC_UPCE_ADDON` | Specifies an encoding scheme for an UPC-E format with a supplemental 2 or 5 digit add-on. |
| `M_ENC_US_MAIL` | Specifies the Intelligent Mail Barcode encoding scheme for a 4-state format used by the US Mail service. |
| `5 <= Value <= 95` | Specifies the minimum amount of the symbol that contains error correction information, as a percentage. |

---

### `M_ERROR_CORRECTION`

Inquires the type of error correction.  Note that training the corresponding control type ([`M_TRAIN`](../../Reference/code/McodeInquire.md)) is not supported for the [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` *(default)* | Specifies to use the default error correction scheme for the code type. |
| `M_ANY` | Specifies that the error correction type for [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations is detected automatically. |
| `M_ECC_4STATE` | Specifies to use the Reed-Solomon-based algorithm or a check digit type of error correction scheme, depending on the specification of the encoding. |
| `M_ECC_200` | Specifies to use a Reed-Solomon-based algorithm as an error correction scheme. |
| `M_ECC_CHECK_DIGIT` | Specifies to use an additional digit to check whether there is an error or not. |
| `M_ECC_COMPOSITE` | Specifies to use the default error correction scheme for the 1D and 2D portions of the composite code. |
| `M_ECC_H` | Specifies to use the highest-level error correction scheme. |
| `M_ECC_L` | Specifies to use the lowest-level error correction scheme. |
| `M_ECC_M` | Specifies to use a medium-low level error correction scheme. |
| `M_ECC_NONE` | Specifies no error correction. |
| `M_ECC_Q` | Specifies to use a medium-high level error correction scheme. |
| `M_ECC_REED_SOLOMON` | Specifies to use a Reed-Solomon type of error correction scheme. |
| `M_ECC_REED_SOLOMON_n` | Specifies to use a Reed-Solomon type of error correction scheme. |
| `5 <= Value <= 95` | Specifies the minimum percentage of the symbol that contains error correction information. |

---

### `M_FOREGROUND_VALUE`

Inquires the foreground color of the code.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type and the initialization mode. |
| `M_FOREGROUND_ANY` | Specifies the foreground color as black or white. |
| `M_FOREGROUND_BLACK` | Specifies that the foreground color is black. |
| `M_FOREGROUND_WHITE` | Specifies that the foreground color is white. |

### Combination Constants — For inquiring whether the corresponding control type is activated for training

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether the corresponding control type is activated for training.

#### `M_TRAIN`

Inquires whether the corresponding control type is activated for training.  You can establish the default activation setting of the corresponding control type, regardless of its current activation setting, using [`M_TRAIN`](../../Reference/code/McodeInquire.md) + [`M_DEFAULT`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default activation; the default activation depends on the control type and code model type. For a given trainable control type, if an unsupported code model is included in the training, the default value will be [`M_DISABLE`](../../Reference/code/McodeInquire.md), otherwise it will be [`M_ENABLE`](../../Reference/code/McodeInquire.md). |
| `M_DISABLE` | Specifies that the control type has not been activated for training. |
| `M_ENABLE` | Specifies that the control type has been activated for training. |

### For inquiring about code model settings that affect an

To inquire about code model settings that affect an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to one of the following values. In this case, the [`CodeId`](../../Reference/code/McodeInquire.md) parameter must be set to the identifier of a code model.  > **Note:** Note that besides code type restrictions listed explicitly in the values below, [`McodeGrade`](../../Reference/code/McodeGrade.md) does not support [`M_4_STATE`](../../Reference/code/McodeModel.md), [`M_PHARMACODE`](../../Reference/code/McodeModel.md), [`M_POSTNET`](../../Reference/code/McodeModel.md), and [`M_PLANET`](../../Reference/code/McodeModel.md) code types.

---

### `M_GRADING_STANDARD_EDITION`

Inquires the grading standard edition used when performing an [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.  This inquire type is available for all code types except [`M_MAXICODE`](../../Reference/code/McodeModel.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies  the most recent grading standard edition that is supported, which is dependent on the code type. |
| `M_ISO_15415_2011_15416_2000` | Specifies  the ISO/IEC 15415:2011 and ISO/IEC 15416:2000 specifications. |
| `M_ISO_15415_2011_15416_2016` | Specifies  the ISO/IEC 15415:2011 and ISO/IEC 15416:2016 specifications. |
| `M_ISO_15416_2000` | Specifies  the ISO/IEC 15416:2000 specification. |
| `M_ISO_15416_2016` | Specifies  the ISO/IEC 15416:2016 specification. |
| `M_ISO_29158_2011` | Specifies  the ISO/IEC 29158:2011 specification. |
| `M_ISO_29158_2020` | Specifies  the ISO/IEC 29158:2020 specification. |
| `M_SEMI_T10_0701` | Specifies  the Semi T10-0701 specification. |

### For inquiring about code model settings that affect an

To inquire about code model settings that affect an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to one of the following values. In this case, the [`CodeId`](../../Reference/code/McodeInquire.md) parameter must be set to the identifier of a code model.  > **Note:** Note that you cannot set [`CodeId`](../../Reference/code/McodeInquire.md) to the identifier of a code context when using the following inquire types.

---

### `M_CELL_SIZE_MODE`

Inquires how to establish the cell size to use when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO` *(default)* | Specifies to use the largest possible cell size such that the code will be resized as to just fit into the target image of the operation. |
| `M_USER_DEFINED` | Specifies that the cell size corresponds to the value of [`M_CELL_SIZE_VALUE`](../../Reference/code/McodeInquire.md). |

---

### `M_CELL_SIZE_VALUE`

Inquires the cell size to use when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation and [`M_CELL_SIZE_MODE`](../../Reference/code/McodeInquire.md) is set to [`M_USER_DEFINED`](../../Reference/code/McodeInquire.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `Value > 0.0` | Specifies the cell size. |

---

### `M_DOT_SHAPE`

Inquires the shape of the dots used to draw foreground cells when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `M_CIRCLE` | Specifies that the foreground cells will be drawn with circular dots. |
| `M_SQUARE` | Specifies that the foreground cells will be drawn with square dots. |

---

### `M_DOT_SIZE`

Inquires the size of the dot (or square) used to draw foreground cells when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.  This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md),[`M_DOTCODE`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and[`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default value, which is dependent on the code type. |
| `Value > 0.0` | Specifies the size of the dot or square. |

### For inquiring about result buffer settings dealing with an

To inquire about code result buffer settings dealing with an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeDetect`](../../Reference/code/McodeDetect.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to one of the following values.

---

### `M_RESULT_OUTPUT_UNITS`

Inquires the units with which to interpret the results.

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_ACCORDING_TO_CALIBRATION` *(default)* | Specifies that results are returned in world units if the result was calculated on an image associated with a camera calibration context; otherwise, specifies that results are returned in pixel units. |
| `M_PIXEL` | Specifies that results are returned in pixel units, with respect to the pixel coordinate system. |
| `M_WORLD` | Specifies that results are returned in world units, with respect to the relative coordinate system. |

### For inquiring about result buffer settings dealing with an

To inquire about code result buffer settings dealing with an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, set the [`InquireType`](../../Reference/code/McodeInquire.md) parameter to one of the following values.

---

### `M_STRING_FORMAT`

Inquires the format in which to return the string, retrieved using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STRING`](../../Reference/code/McodeGetResult.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` |  |
| `M_AUTO_FORMAT` *(default)* | Specifies that the string is returned in the string format associated with the code type. |
| `M_GS1_HUMAN_READABLE` | Specifies that the string is returned in a format that is human-readable. |
| `M_JAPANESE` | Specifies that the string is returned in the Japanese (Windows-932) encoding. |
| `M_KOREAN` | Specifies that the string is returned in the Korean (Windows-949) encoding. |
| `M_LATIN` | Specifies that the returned string uses Latin (Windows-1252) encoding. |
| `M_RAW_DATA` | Specifies that the string is returned in a raw data format. |
| `M_SIMPLIFIED_CHINESE` | Specifies that the returned string uses Simplified Chinese (Windows-936) encoding. |
| `M_UPCE_COMPRESSED` | Specifies that the returned string is in the UPCE compressed string format. |
| `M_UTF8` | Specifies that the returned string is in the UTF-8 string format. |

### Combination Constants — For inquiring whether an inquire type is supported

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine whether an inquire type is supported.

#### `M_SUPPORTED`

Inquires whether the specified inquire type is supported.

| Value | Description |
| --- | --- |
| `M_FALSE` | Specifies that the inquire type is not supported. |
| `M_TRUE` | Specifies that the inquire type is supported. |

### Combination Constants — For inquiring about the default value of an inquire type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to determine the default value of an inquire type, regardless of the current value of the inquire type.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Inquires the default value of the specified inquire type. |

### Combination Constants — For specifying the data type

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to cast the requested information to a required data type.

#### `M_TYPE_AIL_DOUBLE`

Casts the requested data to an _AIL_DOUBLE_.

#### `M_TYPE_AIL_ID`

Casts the requested data to an _AIL_ID_.

#### `M_TYPE_AIL_INT`

Casts the requested data to an _AIL_INT_.

#### `M_TYPE_AIL_INT32`

Casts the requested data to an _AIL_INT32_.

#### `M_TYPE_AIL_INT64`

Casts the requested data to an _AIL_INT64_.

## Return Value

**Type:** `AIL_INT`

The returned value is the requested information, cast to an _AIL_INT_. If the requested information does not fit into an _AIL_INT_, this function will return `M_NULL`or truncate the information.

This inquire type is only available for 2D code types.

This inquire type is only available for the [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type.

This inquire type is only available for [`M_AZTEC`](../../Reference/code/McodeModel.md), [`M_DATAMATRIX`](../../Reference/code/McodeModel.md), [`M_QRCODE`](../../Reference/code/McodeModel.md), and [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code types.

Note that the corresponding control type can be trained; to inquire if it has been activated for training, combine this inquire type with [`M_TRAIN`](../../Reference/code/McodeInquire.md).
