---
doctype: UserGuide
part: "2D processing and analysis"
chapter: More_on_grading_codes
section: Attributes_that_can_be_graded
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / more_on_grading_codes / Attributes that can be graded"
---

# Attributes that can be graded

To grade a code, [`McodeGrade`](../../Reference/code/McodeGrade.md) scans the code multiple times along different paths (also referred to as scanlines). To grade a 1D code, [`McodeGrade`](../../Reference/code/McodeGrade.md) performs scans at regular intervals along the Y-axis of the code; the set of grayscale values along a scanline (in the area of the code and its quiet zones) is called a scan reflectance profile. Note that to read 1D codes, [`McodeRead`](../../Reference/code/McodeRead.md) also scans the code at regular intervals along the Y-axis, but it only performs as many scans as needed to successfully read the code; these scans are referred to as decoded scanlines. The image below shows in red example paths along which [`McodeGrade`](../../Reference/code/McodeGrade.md) extracts scans reflectance profiles and shows in blue the example paths along which [`McodeRead`](../../Reference/code/McodeRead.md) decodes scans. For 2D codes, the entire code is analyzed in both operations. For composite codes, the 1D portion is graded with multiple scanlines and the entire 2D portion is analyzed.

*[Image: mcode_scan_profiles.png]*

The following table lists the common code attributes that can be graded using Aurora Imaging Library, including the attributes of the scan reflectance profiles.

| Attribute | Notes | Available results |
| --- | --- | --- |
| Axial nonuniformity | *[Image: Code_axial_non_uniformity.png]* | This attribute is a measure of how spacing between sampling points differs between the X- and Y-axis (width to height). | [`M_AXIAL_NONUNIFORMITY_GRADE`](../../Reference/code/McodeGetResult.md) |
| Contrast | *[Image: Code_cell_contrast.png]* | This attribute is a measure of the contrast in the cells of the code occurrence or in the entire code occurrence. The higher the contrast, the better it is for scanning purposes and the better the grade. | [`M_SYMBOL_CONTRAST_GRADE`](../../Reference/code/McodeGetResult.md) |
| Decode | *[Image: Code_decode.png]* | This attribute is a measure of the success or failure of the decoding algorithm and is used to determine if the code is readable. | [`M_DECODE_GRADE`](../../Reference/code/McodeGetResult.md) [`M_SCAN_DECODE_GRADE`](../../Reference/code/McodeGetResult.md) |
| Defects | *[Image: Code_defects.png]* | This attribute is a measure of the defects, or local deviations, in the scan reflectance profiles of the code. | [`M_SCAN_DEFECTS_GRADE`](../../Reference/code/McodeGetResult.md) |
| Fixed-pattern damage | *[Image: Code_fixed_pattern_damage.png]* | This attribute is used to grade the damage relating to the finder pattern and/or clock pattern of Aztec, Data Matrix, QR, and MicroQR codes. This attribute is also used to grade the damage relating to the dot positions not available for printing of a DotCode code. | See [](S05_Attributes_that_can_be_graded.md) |
| Grid non-uniformity | *[Image: Code_grid_non_uniformity.png]* | This attribute is calculated as the amount of deviation of the code occurrence's actual grid from an ideal grid. | [`M_GRID_NONUNIFORMITY_GRADE`](../../Reference/code/McodeGetResult.md) |
| Minimum edge contrast | *[Image: Code_minimum_edge_contrast.png]* | This attribute is calculated as the contrast between two adjacent regions (for example, a bar and an adjacent space). | [`M_SCAN_EDGE_CONTRAST_MINIMUM_GRADE`](../../Reference/code/McodeGetResult.md) |
| Print growth | *[Image: Code_print_growth.png]* | This attribute is a measure of the extent in which the boundaries of the black and white markings of the code occurrence are within their cell's boundaries. | [`M_PRINT_GROWTH_GRADE`](../../Reference/code/McodeGetResult.md) |
| Start/stop pattern | *[Image: Code_start_pattern_damage.png]* | This attribute is used to grade the quality of the pattern that marks the end points of the code occurrence. | [`M_START_STOP_PATTERN_GRADE`](../../Reference/code/McodeGetResult.md) |
| Unused Error Correction | *[Image: Code_unused_error_correction.png]* | This is a measure of the extent in which regional or spot damage in the code occurrence has eroded the reading safety margin that error correction provides. | [`M_UNUSED_ERROR_CORRECTION_GRADE`](../../Reference/code/McodeGetResult.md) |

Note that an overall symbol grade can also be returned, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_OVERALL_SYMBOL_GRADE`](../../Reference/code/McodeGetResult.md). For 2D codes, this is a measure of the worst grade for the results of a specific code occurrence. For 1D codes, it is a measure of the average grade for the results of a specific code occurrence.

> **Note:** Other code attributes can also be evaluated with the grading operation, but they return a value as opposed to a grade.

## Understanding fixed pattern damage

The fixed pattern (also known as the fixed orientation pattern and the finder pattern) of a printed code (symbol) enables scanning equipment to identify and locate the code. Damage in the finder pattern of a code can make it unreadable. [`McodeGrade`](../../Reference/code/McodeGrade.md) can establish a grade for multiple aspects of the fixed pattern of a code occurrence in an image.

| Code type (and encoding scheme if applicable) | Fixed pattern | Available results |
| --- | --- | --- |
| [`M_AZTEC`](../../Reference/code/McodeModel.md) code type with [`M_ENC_AZTEC_COMPACT`](../../Reference/code/McodeControl.md) or [`M_ENC_AZTEC_RUNE`](../../Reference/code/McodeControl.md) | *[Image: AztecFixedPatternSegments_compact.png]* Has a 2-ring bull's eye (A segment), whereby each corner of the center bull's eye uses a different, fixed orientation pattern for determining symbol orientation. | - [`M_FIXED_PATTERN_DAMAGE_A_GRADE`](../../Reference/code/McodeGetResult.md) |
| [`M_AZTEC`](../../Reference/code/McodeModel.md) code type with [`M_ENC_AZTEC_FULL_RANGE`](../../Reference/code/McodeControl.md) | *[Image: AztecFixedPatternSegments_full.png]* Has a 3-ring bull's eye (A segment), whereby each corner of the center bull's eye uses a different, fixed orientation pattern for determining symbol orientation. In addition, it has a fixed reference grid along every 16th row and column (B segments) to help map the data layers. | - [`M_FIXED_PATTERN_DAMAGE_A_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_B_GRADE`](../../Reference/code/McodeGetResult.md) |
| [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) code type | *[Image: DataMatrixClockTrackAndSolid_L_QZL.png]* Has L1, L2, QZL1, and QZL2 segments. In addition, it has clock track and solid (clock pattern and adjacent solid area) segments. | - [`M_FIXED_PATTERN_DAMAGE_AVERAGE_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_L1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_L2_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_QZL1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_QZL2_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_CLOCKTRACK_SOLID_GRADE`](../../Reference/code/McodeGetResult.md) |
| [`M_DOTCODE`](../../Reference/code/McodeModel.md) code type | *[Image: DotCodeFixedPatternSegments.png]* There are no fixed patterns within the data dots of a DotCode code type. Instead, the fixed patterns are those dot positions not made available for printing: the interstitial dot positions and the 3-dot wide quiet zone on all 4 sides. | - [`M_FIXED_PATTERN_DAMAGE_INTERSTITIAL_DOTS_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_QUIET_ZONES_GRADE`](../../Reference/code/McodeGetResult.md) |
| [`M_QRCODE`](../../Reference/code/McodeModel.md) code type | *[Image: QrCodeFixedPatternSegments.PNG]* Has A1, A2, A3, B1, B2, and C segments. | - [`M_FIXED_PATTERN_DAMAGE_A1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_A2_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_A3_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_B1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_B2_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_C_GRADE`](../../Reference/code/McodeGetResult.md) |
| [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) code type | *[Image: MicroQrCodeFixedPatternSegments.png]* Has A1, B1, and B2 segments. | - [`M_FIXED_PATTERN_DAMAGE_A1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_B1_GRADE`](../../Reference/code/McodeGetResult.md)<br/>- [`M_FIXED_PATTERN_DAMAGE_B2_GRADE`](../../Reference/code/McodeGetResult.md) |

## Understanding modulation versus reflectance margin with 2D codes

To analyze whether a given cell is considered part of the foreground or the background, you can grade its modulation and/or reflectance margin. Both modulation and reflectance margin rely on your code occurrence's threshold. The difference is that the modulation is computed without regard to whether the cell should be light or dark; whereas, the reflectance margin takes the expected state of any cell (that is, whether it is supposed to be light or dark) into account.

When reading a 2D code occurrence, if the grayscale value of a cell (also called its reflectance value) is above the threshold value, it is considered light. Values close to the threshold are considered ambiguous, while values further away are considered unambiguous. For example, consider a given 2D matrix code occurrence that has a symbol contrast of 255 and a computed threshold of 128. In this case, if a cell has a grayscale value of 255, it is a light cell. This is considered unambiguous since 255 is very far from 128. If a cell's grayscale value is 132, however, the cell is still considered light (since it is over the computed threshold), but the difference between the grayscale value of the cell and the threshold is low enough that the result is considered ambiguous by most grading specifications. In the following image, the circled cells are examples within the code occurrence that are considered ambiguous because those cells are closer to the threshold value than an unambiguous dark cell.

*[Image: Code_modulation.png]*

The modulation of a code occurrence is based on the difference between the computed threshold and the grayscale value of each cell (its reflectance), divided by the symbol contrast. A greater difference results in a greater modulation. The modulation can be calculated for each cell in the code occurrence and uses the following calculation:

*[Image: Code_UG_ModulationEq.png]*

The modulation is computed without regards to whether any cell should be light or dark. Instead, the modulation relies on the difference between the grayscale value of each cell and the code occurrence's threshold value. The greater this difference, the greater the modulation, and by association, the unambiguous claim that a cell should be either light or dark, respectively.

The following is a list of the different parts of a code occurrence for which you can retrieve the modulation grade:

- To retrieve the modulation of the code occurrence, as a grade calculated using the ISO standard, use [`M_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the modulation of the code occurrence calculated using the ISO/IEC 29158 standard, use [`M_CELL_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the modulation of each codeword as a grade, use [`M_CODEWORD_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the modulation of the start/stop patterns of 2D cross-row and composite code types, as a grade, use [`M_SCAN_MODULATION_GRADE`](../../Reference/code/McodeGetResult.md).

The reflectance margin is similar to the modulation in that it is calculated using the threshold value and the grayscale value of the cell. The difference is that when any cell is supposed to be light and its grayscale value is below the threshold value, its reflectance margin is 0.

The following is a list of the different parts of a code occurrence for which you can retrieve the cell's grayscale value (its reflectance) and its reflectance margin grade:

- To retrieve the reflectance margin grade of a code occurrence, use [`M_REFLECTANCE_MARGIN_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the minimum reflectance (grayscale value of the cell in the code occurrence), use [`M_MINIMUM_REFLECTANCE_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the reflectance margin of each codeword, use [`M_CODEWORD_REFLECTANCE_MARGIN_GRADE`](../../Reference/code/McodeGetResult.md).

> **Note:** Note that, the reflectance of other code attributes can also be evaluated with the grading operation, but they return a value as opposed to a grade.

## Understanding decodability with codes

For 1D, 2D cross-row, and composite code types, you can retrieve a measure of decodability using [`M_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md) or [`M_SCAN_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md) (depending on code type). Decodability is a measure of how well the Aurora Imaging Library Code module can read the code occurrence. By contrast, the decode grade is a grade denoting the success or failure of the decoding operation (A or F). You can retrieve the decode grade using [`M_SCAN_DECODE_GRADE`](../../Reference/code/McodeGetResult.md).

The range of decodability can sometimes result in a relatively low decodability that still results in a passing decode grade, or the opposite (a relatively high decodability that results in a failing decode grade). For example, with Code 93 code types, each letter is represented by a specific number of bars and spaces, each of a predetermined width. If the code occurrence has a very low decodability grade, this should result in a failing decode grade (although a string might be decoded or partially decoded, depending on the success or failure of the related error checks). However, sometimes a passing decode grade is obtained while the decodability is relatively low. In such cases, the decodability of other parts of the code occurrence should be examined (such as the decodabiliy of the scan profile or codewords).

*[Image: Code_Decodability.png]*

The following is a list of the different parts of a code occurrence for which you can retrieve the decodability grade:

- To retrieve the decodability grade of the entire code occurrence, use [`M_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the decodability grade of the scan reflectance profile, use [`M_SCAN_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the decodability grade for each codeword, use [`M_CODEWORD_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).
- To retrieve the decodability grade of the start/stop patterns of the code occurrence, use [`M_SCAN_DECODABILITY_GRADE`](../../Reference/code/McodeGetResult.md).

Note that [`McodeGrade`](../../Reference/code/McodeGrade.md) uses a decoding algorithm specified by the selected standard. This algorithm is less flexible than the Aurora Imaging algorithms that are used by [`McodeRead`](../../Reference/code/McodeRead.md). For poor quality codes, it is possible that [`McodeRead`](../../Reference/code/McodeRead.md) succeeds ([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STATUS`](../../Reference/code/McodeGetResult.md)), but [`McodeGrade`](../../Reference/code/McodeGrade.md) returns an F for the decode grade ([`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_DECODE_GRADE`](../../Reference/code/McodeGetResult.md)).
