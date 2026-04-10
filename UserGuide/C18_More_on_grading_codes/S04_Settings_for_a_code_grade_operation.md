---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Settings_for_a_code_grade_operation
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Settings for a code grade operation"
---

# Settings for a code grade operation

This section contains settings you might have to change before performing a [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

## Control types that are commonly set

The following is a list of control types in [`McodeControl`](../../Reference/code/McodeControl.md) that you should typically set (if their default settings don't apply to your code occurrences) before grading a code occurrence:

| Control type | Notes |
| --- | --- |
| [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md) | Specifies the foreground color of the code occurrence. This control type must be set if the foreground color is not black; the code occurrence will not be graded if the foreground value is not correctly set. |
| [`M_ENCODING`](../../Reference/code/McodeControl.md) | Specifies the type of encoding scheme for the code occurrence. This control type must be set if the default encoding scheme for the code type differs from the one used by the code occurrence or if [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) | Specifies the type of error correction scheme for the code occurrence. This control type must be set if the default error correction scheme for the code type differs from the one used by the code occurrence or if [`M_ANY`](../../Reference/code/McodeControl.md) is not supported. |
| [`M_STRING_SIZE...`](../../Reference/code/McodeControl.md). | Specifies the maximum and minimum size of the string (number of characters) encoded in each code occurrence. These control types must be set if [`M_STRING_SIZE_MIN`](../../Reference/code/McodeControl.md) and/or [`M_STRING_SIZE_MAX`](../../Reference/code/McodeControl.md) cannot be set to [`M_ANY`](../../Reference/code/McodeControl.md). |
| [`M_SEARCH_ANGLE...`](../../Reference/code/McodeControl.md). | Specifies the nominal search angle and angular search range. These control types must be set if the code occurrence is at an angle greater or less than 0.0 ±5 degrees. |

For more information on these settings, see [](../C17_Codes/S06_Supported_encoding_schemes_by_code_type.md) and [](../C17_Codes/S09_Customizing_read_operation_settings.md).

## Specifying the search region

When specifying the search region, it is important to include the quiet zone of the code. The quiet zone is an area around the code with no markings in it. For 1D codes, the quiet zone immediately precedes the start character and follows the stop character. For 2D codes, the quiet zone must be present on all four sides of the code. The quiet zone must be large enough to meet the code type specifications. For example, the Data Matrix code type requires that the quiet zone be at least one cell wide. The DotCode code type requires a quiet zone of three cells wide. 1D code types require a quiet zone of 2 to 10 times the width of the thinnest bar, depending on the type.

## Setting the aperture

The aperture is the diameter of the circular smoothing filter used by the code grade operation to avoid minor defects that might influence the grading of a code occurrence. Ideally, the aperture size is set large enough so that the smoothing effect eliminates insignificant defects and is set small enough so that the contrast between the foreground and the background remains strong. If the aperture is not set correctly, the grading results will not be accurate. In most cases, the [`M_DEFAULT`](../../Reference/code/McodeControl.md) setting is suitable. However, you have several options depending on how you set the aperture mode ([`M_APERTURE_MODE`](../../Reference/code/McodeControl.md)).

- **Relative aperture setting.** To have Aurora Imaging Library calculate the aperture size as a multiplicative factor of the cell size, use [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) set to [`M_RELATIVE`](../../Reference/code/McodeControl.md) (same as [`M_DEFAULT`](../../Reference/code/McodeControl.md)).
- **Absolute aperture setting.** To set your aperture size as a fixed value, use [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) set to [`M_ABSOLUTE`](../../Reference/code/McodeControl.md).

The aperture size can be set for all code types.

### Relative aperture setting

To set the aperture size to a relative value, use [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) set to [`M_RELATIVE`](../../Reference/code/McodeControl.md). To convert the value you have entered in [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) to real-world units, specify the scale value with [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md). Note that specifying the aperture factor with a pixel size in millimeters is typically used when configuring your aperture to be calculated according to the exact guideline from the ISO industry-standardized print-quality specification.

The table below shows the calculation used based on how [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) and [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) are set.

| Data to use | Set [`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) | Set [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) | Calculation used |
| --- | --- | --- | --- |
| Use standardized factors without specifying pixel size. | [`M_AUTO`](../../Reference/code/McodeControl.md) | [`M_UNKNOWN`](../../Reference/code/McodeControl.md) | `_Aperture Factor_ x [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md)`, where _Aperture Factor_ is a range from 0.15 to 0.8, depending on the code type and applicable standard. |
| Use standardized factors and specify the pixel size. | [`M_AUTO`](../../Reference/code/McodeControl.md) | Pixel size in mm | For EAN/UPC code types, `0.15 / [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md)`. For all other codes, `_Reference number _/ [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md)`, where _Reference number _is a range from 0.075 to 0.5, and is based on the cell size in millimeters ([`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md) x [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md)) and the [ISO/IEC 15416](ISO/IEC 15416) specification. > **Note:** Note that, if the cell size in millimeters is less than 0.1 millimeters, the following formula is used: `0.5 x [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md)` for 1D codes, and the aperture size is `0.8 x [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md)` for matrix codes. |
| Use the specified aperture factor and the pixel size. | Aperture factor | Pixel size in mm | `[`M_RELATIVE_APERTURE_FACTOR`](../../Reference/code/McodeControl.md) x [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md)` > **Note:** Note that the cell size should be the smallest cell size among all of the code occurrences of the model in your images. |

To assist Aurora Imaging Library in calculating your aperture setting, specify the pixel size of your code in millimeters. To determine the pixel size of your code in millimeters ([`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md)), perform the following:

1. Physically measure the entire code with a ruler. Note that this result must be in millimeters.
2. Read the code from a typical image of the code, using [`McodeRead`](../../Reference/code/McodeRead.md).
3. Determine the length of the code in pixels. This can be done in three different ways:
   - For Data Matrix, DotCode, and QR codes, multiply the cell size ([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_CELL_SIZE`](../../Reference/code/McodeGetResult.md)) by the number of cells across the code, as defined by the specification for your code type.
     For 1D codes, use [`McodeGetResult`](../../Reference/code/McodeGetResult.md)with [`M_SIZE_X`](../../Reference/code/McodeGetResult.md), making sure that [`M_RESULT_OUTPUT_UNITS`](../../Reference/code/McodeControl.md)is set to [`M_PIXEL`](../../Reference/code/McodeControl.md)if the image was calibrated.
   - Use the Aurora Imaging Library Measurement module to set two point markers to identify the start and end of your code, using [`MmeasSetMarker`](../../Reference/meas/MmeasSetMarker.md)with [`M_POSITION`](../../Reference/meas/MmeasSetMarker.md). Then, calculate the distance between two markers using [`MmeasCalculate`](../../Reference/meas/MmeasCalculate.md).
   - Use an interactive tool, such as the Aurora Imaging Library Code Reader Interactive Utility.
4. Divide the measurement in millimeters by the length of the code in pixels.
5. Set [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) to this result.

> **Note:** Note that if the setup for acquiring the image of the code is significantly changed (making the acquired code either significantly larger or smaller than the code used to perform the pixels in millimeters calculation), the calculation should be performed again and [`M_PIXEL_SIZE_IN_MM`](../../Reference/code/McodeControl.md) set to the new value.

### Absolute aperture setting

To set the aperture size to a fixed value (so that it will not change regardless of the cell size of your code occurrence), first set the aperture mode to absolute, using [`M_APERTURE_MODE`](../../Reference/code/McodeControl.md) set to [`M_ABSOLUTE`](../../Reference/code/McodeControl.md). Then, set the absolute aperture size to a fixed value, using [`M_ABSOLUTE_APERTURE_SIZE`](../../Reference/code/McodeControl.md). The aperture size is set in units specified by [`M_ABSOLUTE_APERTURE_SIZE_INPUT_UNITS`](../../Reference/code/McodeControl.md).
