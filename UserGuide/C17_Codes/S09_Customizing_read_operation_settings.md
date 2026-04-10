---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Customizing_read_operation_settings
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Customizing read operation settings"
---

# Customizing read and grade operation settings

This section provides information to consider and describes settings that you might have to change for [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

You can also train many of these settings using [`McodeTrain`](../../Reference/code/McodeTrain.md) with a sample set of your images. To establish which can be trained, refer to their description in [`McodeControl`](../../Reference/code/McodeControl.md) in the Aurora Imaging Library Reference; to increase efficiency when browsing control types, use filters to limit table values to those of that can be trained. For information on training, see [Training read and grade operation settings](S08_Training_read_and_grade_operation_settings.md).

## Reading and grading multiple occurrences

The Aurora Imaging Library Code module is designed to read and grade one or many code occurrences in an image; the module only supports searching for multiple occurrences of linear code types (excluding 4-State, GS1 Databar, Planet, and Postnet code types), the Data Matrix code type, and the DotCode code type. When reading or grading multiple code occurrences, multiple results (one set of results for each occurrence of each code model) can be retrieved using [`McodeGetResult`](../../Reference/code/McodeGetResult.md).

If you want to read/grade more than one code occurrence in the image, set the number of code occurrences to read for each code model using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md). To set the total number of code occurrences to read, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md); the default value is [`M_ALL`](../../Reference/code/McodeControl.md), which finds the expected number of occurrences specified for each code model.

For example, if an application must read one code occurrence in an image, you should add a code model to the code context for each possible code type that the code occurrence can be. Since only one code occurrence must be read for any image, you should set [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md) to 1, and set [`M_NUMBER`](../../Reference/code/McodeControl.md) for each code model to 1.

The following table further demonstrates the relationship between [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md) and [`M_NUMBER`](../../Reference/code/McodeControl.md), by showing an example of different combinations of possible results:

| Code context element | Number of occurrences set | Number of occurrences read or graded |
| --- | --- | --- |
| Context | 5 (maximum) | 5 | 5 | 5 | 5 | 5 | 5 | 5 | 5 |
| Model 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 | 0 | 1 |
| Model 1 | 3 | 0 | 0 | 1 | 1 | 2 | 2 | 3 | 3 |
| Model 2 | [`M_ALL`](../../Reference/code/McodeControl.md) | 5 | 4 | 4 | 3 | 3 | 2 | 2 | 1 |

When reading multiple occurrences, it is recommended to set [`M_POSITION_ACCURACY`](../../Reference/code/McodeControl.md) to [`M_HIGH`](../../Reference/code/McodeControl.md) to increase the accuracy of the read.

## Child buffers

For [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md), it is recommended to use a child buffer, especially if your images contain more code occurrences than your code context is configured to read/grade; otherwise, Aurora Imaging Library will select which code occurrences to read/grade, up to the specified maximum number of code occurrences ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TOTAL_NUMBER`](../../Reference/code/McodeControl.md)). Using child buffers is also recommended because it allows for a faster and more robust operation if your images contain other information that might be misinterpreted as a code occurrence.

For best results, if a code occurrence has a quiet zone, your child buffer must be large enough to contain the quiet zone. Generally, the minimum quiet zone for a linear code type is ten times the width of the smallest bar and space. For a 2D code type, the width is generally a factor of the size of the cell. Aurora Imaging Library uses the quiet zone to identify the beginning/end of the code occurrence. Aurora Imaging Library can, in some cases, successfully read a code occurrence that does not meet the minimum requirements for the quiet zone; however, it is strongly recommended that code occurrences have adequate quiet zones to perform a robust [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation and return accurate results.

The extended quiet zone, referred to as the extended area of a code occurrence, is an optional part of the specification for the Aztec, Data Matrix, DotCode, QR, and Micro QR code types. It is an area 20 times the cell size beyond the quiet zone on all sides. If you are grading code occurrences including their extended area ([`M_EXTENDED_AREA_REFLECTANCE_CHECK`](../../Reference/code/McodeControl.md)), your child buffer must be large enough to contain their extended area.

## Regions of interest

Another way to reduce the amount of processing to be performed on your image is to use an image with a region of interest (ROI). Similar to a child buffer, an ROI allows for a fast and robust [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation.

Only a rectangular [`M_VECTOR`](../../Reference/buf/MbufInquire.md) or [`M_VECTOR_AND_RASTER`](../../Reference/buf/MbufInquire.md)ROI, will be considered. To create such an ROI, use [`MbufSetRegion`](../../Reference/buf/MbufSetRegion.md) with the [`ImageOrGraphicListId`](../../Reference/buf/MbufSetRegion.md) parameter set to the Aurora Imaging Library identifier of a 2D graphics list, and the [`Operation`](../../Reference/buf/MbufSetRegion.md) parameter set to either [`M_RASTERIZE`](../../Reference/buf/MbufSetRegion.md) or [`M_NO_RASTERIZE`](../../Reference/buf/MbufSetRegion.md).

## Presearching

There are some situations where you cannot create a child buffer or an ROI for your[`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation. For example, when you cannot guarantee that the target is not in the same location each time it is read or graded by your application. In this case, you could use the Aurora Imaging Library Code module's presearch algorithm. This helps Aurora Imaging Library locate 2D code occurrences in the image prior to the read or grade operation. The presearch algorithm is only supported for 2D code occurrences, not including DotCode, and is disabled by default for efficiency. To set the presearch option, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_USE_PRESEARCH`](../../Reference/code/McodeControl.md).

## Foreground color

Setting the foreground color can improve the results of [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md). To set the foreground color, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md). The default foreground color is black. In situations where the foreground color might change, set [`M_FOREGROUND_VALUE`](../../Reference/code/McodeControl.md) to [`M_FOREGROUND_ANY`](../../Reference/code/McodeControl.md). Note that this impacts the performance of [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md).

## Setting the search speed

You can specify the speed at which to perform [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md); the faster the speed, the less robust the operation. In general, the larger and more clearly defined the code occurrence, the better chance it has of being found at a speed higher than the default speed ([`M_MEDIUM`](../../Reference/code/McodeControl.md)). Specify the search speed using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SPEED`](../../Reference/code/McodeControl.md). If you are having problems finding the code occurrence, you might want to search at a speed lower than the default.

## Timing out your search

In certain cases, [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) might take longer than necessary for your purposes. In this event, you have two options to reduce the reading or grading time. You can either specify a maximum decoding time for the operation using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_TIMEOUT`](../../Reference/code/McodeControl.md), or you can call [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_STOP_READ`](../../Reference/code/McodeControl.md) to stop the current [`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) operation when required. For the latter option, the call to [`McodeControl`](../../Reference/code/McodeControl.md) must be done from another thread.

## Cell size and number

In most cases, you should not have to specify the cell size or number of cells when reading or grading code occurrences; Aurora Imaging Library can search for the specified code type and automatically determine the cell size and number of cells within the code occurrence. However, for some linear code types and 2D code types, such as Data Matrix and Maxicode,[`McodeRead`](../../Reference/code/McodeRead.md) or [`McodeGrade`](../../Reference/code/McodeGrade.md) can perform a more robust operation if you specify the cell size (that is, the size of the cell in X). To increase both the speed and robustness of the operation for 2D code types, you can also specify the number of cells in the X and Y-direction; for [`M_PDF417`](../../Reference/code/McodeModel.md), [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), and [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code types, instead of specifying the number of cells in the Y-direction, you specify the number of rows.

> **Note:** Aurora Imaging Library might have difficulty reading linear code occurrences if the cell size is less than 2 pixels, and 2D code occurrences if the cell size is less than 3 pixels, even if the size is specified.

You specify the cell size as a range using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_CELL_SIZE_MAX`](../../Reference/code/McodeControl.md). Aurora Imaging Library will search for code occurrences with cells that fall within this range. If the cell size is not within the specified range, the code occurrence will not be found.

> **Note:** It is highly recommended that you specify the best cell size range possible. This is especially important when using an adaptive threshold, discussed later in [Thresholding](S09_Customizing_read_operation_settings.md). To obtain the best results from a presearch when a code occurrence's cell size is less than 6 or greater than 10, you must specify the cell size minimum and maximum.

You specify the number of cells using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) and [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md). Note that specifying the number of cells is mostly useful when reading Aztec, PDF417, Truncated PDF417, Data Matrix, DotCode, and QRCode code types; the Maxicode code type has a fixed number of cells. For the DotCode code type, the sum of the number of rows and number of columns must be an odd number. For the PDF417 code type, [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) must equal `17_c_ + 35`, where `_c_` is the number of columns, and 35 represents the number of cells required for the start and stop patterns. For the Truncated PDF417 code type, [`M_CELL_NUMBER_X`](../../Reference/code/McodeControl.md) must equal `17_c_ + 18`, where `_c_` is the number of columns, and 18 represents the number of cells required for the start patterns. When used with [`M_PDF417`](../../Reference/code/McodeModel.md), [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md), and [`M_MICROPDF417`](../../Reference/code/McodeModel.md) code types, [`M_CELL_NUMBER_Y`](../../Reference/code/McodeControl.md) represents the number of rows.

## Search angular range

By default, [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) read code occurrences if they fall within the angular range of 0±5°. If the code occurrence appears rotated in the image, specify another nominal angle using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md).

If you are uncertain about the code occurrence's exact orientation or you expect a possible deviation of more than ±5° from the nominal angle, increase the angular range relative to [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). Set the lower and upper deviations from the nominal angle using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md). Optionally, for linear, PDF417, and TruncatedPDF417 code types, you can set the step by which to increment or decrement through the search angular range, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md). If you don't explicitly specify this step, the search angular range algorithm will automatically establish a step. The search angular range algorithm will first search at the specified nominal angle. It will then toggle between incrementing and decrementing from the nominal angle, with each iteration moving further away from the initial search angle by the value of [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md) (or the automatically established step). The process will continue until either the code occurrence is found, or the search falls outside of the range defined by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `-` [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) `+` [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md).

Note that [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) are relative to the nominal angle set by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md). While the nominal angle is relative to the input coordinate system specified using [`M_SEARCH_ANGLE_INPUT_UNITS`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) are not affected by the coordinate system used.

For linear code types, as the angular range increases and/or the angular step decreases, the operation speed might decrease. You can sometimes speed up the search by disabling the search angular range algorithm and trying to read the code occurrence at the specified search (nominal) angle. If the code occurrence deviates from the nominal angle by a few degrees, as in the image below, [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) can sometimes successfully read the code occurrence without explicitly searching through a range of angles. To disable the search angular range algorithm, call [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md) set to [`M_DISABLE`](../../Reference/code/McodeControl.md). The angular range does not affect the speed of the operation when searching for 2D code types, and so it is not necessary to disable the search angular range algorithm.

*[Image: codeAngleSearchRange.png]*

For linear code types, the Code module will not use the search angular range algorithm if [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) both have a value less than or equal to 5°, regardless of [`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md). In this instance, [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md) and [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md) have no effect.

For PDF417 and Truncated PDF417 code types, you can specify the angle of the scanlines using [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) with [`M_SCANLINE_AT_ANGLE`](../../Reference/code/McodeControl.md). This enables decoding using scanlines rotated to the orientation specified by [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md), without any localization. Select this algorithm if you have codes that do not completely respect their specification (for example, they are missing their quiet zone). Note that although this improves robustness, this might affect speed. To get the best performance, specify a precise [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md) or use [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md), and ensure [`M_SCANLINE_HEIGHT`](../../Reference/code/McodeControl.md) is not set at a higher value than the row height. You can try disabling[`M_SEARCH_ANGLE_MODE`](../../Reference/code/McodeControl.md)to see if the Code module can still find the required code occurrences; this might improve the speed.

If bearer bars are present and you are reading a bar code at an unknown angle, [`M_BEARER_BAR`](../../Reference/code/McodeControl.md) must be enabled. When bearer bars are caused by the background (such as, the bar code has a dark background that touches only one edge of the lines and spaces), then the speed of the read should be reduced (using [`M_SPEED`](../../Reference/code/McodeControl.md) set to [`M_VERY_LOW`](../../Reference/code/McodeControl.md)).

When the search speed [`M_SPEED`](../../Reference/code/McodeControl.md) is set to [`M_VERY_LOW`](../../Reference/code/McodeControl.md), a more exhaustive search algorithm is enabled that can exceed the specified angle range. In this case, the specified angle range is used as a starting reference for the search instead of as a search restriction.

In some cases, the target code occurrence might be inverted. For linear code types, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_CODE_FLIP`](../../Reference/code/McodeControl.md) set to [`M_FLIP`](../../Reference/code/McodeControl.md) to have linear code occurrences read from right-to-left (as opposed to the standard left-to-right), before trying to read the code occurrence. If you use the setting [`M_ANY`](../../Reference/code/McodeControl.md), Aurora Imaging Library will decide whether the code occurrence needs to be flipped or not. The default value for [`M_CODE_FLIP`](../../Reference/code/McodeControl.md) is [`M_NO_FLIP`](../../Reference/code/McodeControl.md). If [`M_CODE_FLIP`](../../Reference/code/McodeControl.md) does not support the code type of a code occurrence that is inverted, you can flip the code occurrence vertically or horizontally using [`MimFlip`](../../Reference/im/MimFlip.md).

For 2D code types, the rotation is calculated as the angle between the bottom of the code and the X-axis of the input coordinate system. The bottom of the code is found reading the finder pattern to determine the orientation of the code. The images below show how the angle is affected by the code being rotated or flipped.

*[Image: mcode_2d_rotation.png]*

## Dot spacing

Sometimes 2D matrix code occurrences are composed of dots, such as when a code occurrence is punched into an object. In an ideal situation, the cell containing the dot (and its surrounding border of white space) matches the expected cell size for your code type. However, often in these cases, the dots have some spacing between them, which can make the code occurrence more difficult to read. For best results, set the dot spacing using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md)and [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md). Set each to a range that covers half the pixel distance between dots. If a 2D code occurrence composed of dots is printed rather than punched, and the cells overlap due to printing artifacts or ink spread, set [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md) to the minimum negative half the width of the overlap. Set [`M_DOT_SPACING_MAX`](../../Reference/code/McodeControl.md) to a value greater than [`M_DOT_SPACING_MIN`](../../Reference/code/McodeControl.md), but keep the difference between the two relatively small.

Note that for the DotCode code type, the dot spacing control types are ignored; for this code type, specify the dot size instead, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DOT_SIZE`](../../Reference/code/McodeControl.md).

It is recommended to only use a negative dot spacing when the dots are much larger than the expected cell size (typically, when the dots overlap). In the image below, the gray circle represents the actual cell size, while the black dot represents the expected cell size. In this case, specifying a negative dot spacing will assist with future [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

*[Image: code_dotspacing_bleeding2.png]*

Alternatively, when successive dots are not touching, setting a positive dot spacing will increase the overall size of the cells (that is, the space taken by the dots and the surrounding border of white space for each dot) until they reach the expected cell size. In the image below, there is an abundance of space between the dots. In this case, setting a positive dot spacing equal to half the distance between the dots will assist with future [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

*[Image: code_dotspacing_regular.png]*

The following is an example of a code occurrence that might need you to specify the dot spacing since its dots are too far apart. By changing the dot spacing better results might be obtained. The image on the right shows how Aurora Imaging Library interprets the image after applying positive dot spacing.

*[Image: mcode-positive-dotspacingex.png]*

If the dots are too close together, specifying a negative dot spacing might result in better results. The image on the right shows how Aurora Imaging Library interprets the image after applying negative dot spacing.

*[Image: mcode-negative-dotspacingex.png]*

## String size

You can optionally specify the size of the string encoded in the code occurrence to locate, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_STRING_SIZE_MIN`](../../Reference/code/McodeControl.md) and [`M_STRING_SIZE_MAX`](../../Reference/code/McodeControl.md).

Note that for [`M_QRCODE`](../../Reference/code/McodeModel.md), [`M_MICROQRCODE`](../../Reference/code/McodeModel.md), [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md), and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) code types, the minimum and maximum string size is ignored when performing [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) operations.

If you have set [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md) to [`M_ECC_CHECK_DIGIT`](../../Reference/code/McodeControl.md), [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) assume that there is a check digit at the end of the string. The specified string size should not include the check digit, since the check digit is not returned as part of the string.

The following table lists code types that require a minimum and maximum string size to be specified:

| Code type | String size |
| --- | --- |
| [`M_POSTNET`](../../Reference/code/McodeModel.md) | 5, 9, or 11 | 5, 9 or 11 |
| [`M_PLANET`](../../Reference/code/McodeModel.md) | 11 or 13 | 11 or 13 |
| [`M_CODABAR`](../../Reference/code/McodeModel.md) | 3 | – |
| [`M_EAN8`](../../Reference/code/McodeModel.md) | 8 | 8 |
| [`M_EAN13`](../../Reference/code/McodeModel.md) | 13 | 13 |
| [`M_UPC_A`](../../Reference/code/McodeModel.md) and [`M_UPC_E`](../../Reference/code/McodeModel.md) | 12 | 12 |

### Reading EAN 14 code types

Strings encoded with the EAN 14 code type typically start with "(01)", which helps to identify the encoding type of the string. The string can be either 13 or 14 digits in length. Note that the string length should not include "(01)". The optional fourteenth digit is a check digit calculated based on the first 13-digits according to Modulo 10.

### Reading GS1-128 code types

Strings encoded with the GS1-128 code type typically start with a two-digit number inside parentheses (for example, (13)). The parentheses surrounding the number will not be read as part of the encoded string, as long as the number matches one of the valid application identifiers for GS1-128 (UCC/EAN-128). If this number is invalid, the parentheses are read as normal characters.

## Distortion and deterioration

If the code occurrence is distorted and/or deteriorated, [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) might not be able to read it. In this case, you can apply a distortion compensation algorithm that increases the robustness of the operation. There are different algorithms that you can use, depending on the code type and the type of distortion present in the code occurrence.

### Aztec, Data Matrix, DotCode, and QR code types

For an Aztec, Data Matrix, DotCode, or QR code occurrence that has different column widths or row heights, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) set to [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md). This algorithm can also work in the presence of perspective distortion. The[`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md)value is only available for these code types.

This algorithm can be used for Data Matrix code occurrences that are skewed but still maintain parallelism, and those that are skewed beyond parallelism. When there is noise, damage, or distortion in the finder pattern, you can limit how much deviation to tolerate. Use [`M_FINDER_PATTERN_MAX_GAP`](../../Reference/code/McodeControl.md) to set the maximum allowable size of a gap (unintended space) in the finder pattern. Use [`M_FINDER_PATTERN_MINIMUM_LENGTH`](../../Reference/code/McodeControl.md) to set the minimum acceptable length of one of the "arms" of the finder pattern.

*[Image: Datamatrix_uneven_rows.png]*

*[Image: datamatrix_distortion.png]*

Except for the DotCode type, in cases where the clock pattern/fixed pattern is too noisy but the code occurrence does not have any distortion (or only a very small amount of distortion), try setting [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) to [`M_CODE_NOT_DEFORMED`](../../Reference/code/McodeControl.md).

> **Note:** Note that the use of a distortion compensation algorithm can lead to longer processing times for Aztec, Data Matrix, and QR code types; for optimal speed and robustness, it is best to adjust your setup instead. For the DotCode code type, [`M_CODE_DEFORMED`](../../Reference/code/McodeControl.md) is more efficient and robust than [`M_CODE_NOT_DEFORMED`](../../Reference/code/McodeControl.md).

### PDF417 and TruncatedPDF417 code types

You can read deteriorated and distorted PDF417 and TruncatedPDF417 code types using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_DECODE_ALGORITHM`](../../Reference/code/McodeControl.md) set to [`M_SCANLINE_AT_ANGLE`](../../Reference/code/McodeControl.md). For example, you can read PDF417 codes that are missing their first and/or last bar or are degraded.

*[Image: PDF417_deteriorated.png]*

To use this functionality, you must use the controls [`M_SEARCH_ANGLE`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_NEG`](../../Reference/code/McodeControl.md), [`M_SEARCH_ANGLE_DELTA_POS`](../../Reference/code/McodeControl.md), and [`M_SEARCH_ANGLE_STEP`](../../Reference/code/McodeControl.md). When decoding, [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) will rotate the scanlines using the values set by these control types and try to decode without assuming the complete integrity of the codes.

## Thresholding

[`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) internally binarize the source image so as to separate code occurrences from the background. By default, the threshold value is automatically chosen and is suitable in most cases. However, if you think that a different thresholding mode and/or value would result in a better separation (and therefore in a more efficient operation), you can manually adjust these, using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md) and [`M_THRESHOLD_VALUE`](../../Reference/code/McodeControl.md), respectively. If the lighting is not uniform across the code occurrence (see the image below for an example) and dealing with 2D matrix code types, MicroPDF417, or linear code types (excluding 4-state, Planet and Postnet), use [`M_THRESHOLD_MODE`](../../Reference/code/McodeControl.md) with [`M_ADAPTIVE`](../../Reference/code/McodeControl.md). This adaptive threshold mode computes, for each pixel, a threshold value based on the pixel's neighborhood.

*[Image: NonUniform_Datamatrix.png]*

When dealing with linear code types (excluding Planet and Postnet), the DotCode code type, or the MicroPDF417 code type, and using adaptive thresholding, you must specify the minimum contrast ([`McodeControl`](../../Reference/code/McodeControl.md) with [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md)) between the foreground and background in the target image so that [`McodeRead`](../../Reference/code/McodeRead.md) and [`McodeGrade`](../../Reference/code/McodeGrade.md) reads the code occurrence properly. Valid values are between 1 and 255. The default value is 50; note that some internal steps might use a value other than 50 when using the default minimum contrast, so it is not equivalent to setting [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) to 50.

Generally, increasing the minimum contrast will make  more robust to non-uniform lighting or noise so that the code occurrence is readable; however, in some instances, it could make a readable code occurrence unreadable. For example, if the contrast between a valid foreground pixel and its background is 25, and you set the minimum contrast to 50, the pixel will not be considered as a foreground pixel. Therefore, it is important to make sure that the minimum contrast value is not causing the operation to ignore code features. In most cases, you can probably estimate an appropriate minimum contrast by looking at your image.

When dealing with 2D code types (excluding DotCode and MicroPDF417), the minimum contrast is automatically determined, so the adaptive threshold mode ignores the [`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) setting.

If you are having problems reading a code, try adjusting the minimum contrast using[`M_MINIMUM_CONTRAST`](../../Reference/code/McodeControl.md) with [`McodeControl`](../../Reference/code/McodeControl.md).

## Determining whether your code type supports GS1

While some code types have GS1 in their constant names (such as [`M_GS1_128`](../../Reference/code/McodeModel.md) and [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md)), many that can support GS1 do not. It could be instead specified by its subtype and/or its encoding scheme. To determine these values, use [`McodeInquire`](../../Reference/code/McodeInquire.md) with [`M_SUB_TYPE`](../../Reference/code/McodeInquire.md) and [`M_ENCODING`](../../Reference/code/McodeInquire.md), respectively. For a listing of encoding and subtypes, see [Supported encoding scheme for composite codes](S06_Supported_encoding_schemes_by_code_type.md).

The simplest way to determine whether your code type follows the industry standard for a GS1 Databar code (for linear codes and composite codes) or a GS1 symbology (for 2D codes), use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_IS_GS1`](../../Reference/code/McodeGetResult.md). This result type is available for Aztec, Data Matrix, DotCode, Code 128, EAN-14, GS1-128, GS1-Databar, QR codes, and composite codes.

If [`M_IS_GS1`](../../Reference/code/McodeGetResult.md) returns true, you can set [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_STRING_FORMAT`](../../Reference/code/McodeControl.md) to [`M_GS1_HUMAN_READABLE`](../../Reference/code/McodeControl.md) to specify that the returned string is human-readable (that is, contains GS1 Application Identifiers or separator strings, in brackets), or [`M_RAW_DATA`](../../Reference/code/McodeControl.md) to retrieve the string in a raw data format with separator characters.

In the following GS-128 code sample, the raw format returns the following string: è010123456789012815051231. The human-readable version of this string, is returned as follows: (01)01234567890128(15)051231.

*[Image: GS-128SampleCode.png]*
