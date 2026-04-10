---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Retrieving_results
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Retrieving results"
---

# Retrieving results

To retrieve results for all occurrences of all code models (or in the case of an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, for all code models), use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_ALL`](../../Reference/code/McodeGetResult.md) and the result type for the result that you want returned. To retrieve a result for a specific code model occurrence (or in the case of an [`McodeTrain`](../../Reference/code/McodeTrain.md) operation, for a specific code model), use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with the index of the occurrence. To increase efficiency when browsing result types, use filters to limit table values to those of interest based on operation type and code type.

To retrieve the success of an [`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), [`McodeTrain`](../../Reference/code/McodeTrain.md), or [`McodeWrite`](../../Reference/code/McodeWrite.md) operation, use [`M_STATUS`](../../Reference/code/McodeGetResult.md). A positive status is returned only when all conditions, set for all the code models, are met. Note that when reading multiple code occurrences and more than one [`McodeRead`](../../Reference/code/McodeRead.md) operation fails, the error returned is the one you should probably fix first. For example, if you try to read two codes and one fails because the code was not found ([`M_STATUS_NOT_FOUND`](../../Reference/code/McodeGetResult.md)) and the other fails because the encoding type was unknown ([`M_STATUS_ENC_UNKNOWN`](../../Reference/code/McodeGetResult.md)), the latter is returned.

Every occurrence of every code model is associated with a unique index within the result buffer after a successful code operation. When using [`McodeGetResult`](../../Reference/code/McodeGetResult.md), you must specify the index of the occurrence for which to retrieve results.

The following table presents an example of a code result buffer, showing the result occurrence index, code model index, angle, and string for 6 occurrences of 4 models in a context. If, for instance, you wanted to get additional results about only one of these occurrences, you should pass the result occurrence index to [`McodeGetResult`](../../Reference/code/McodeGetResult.md) along with the required result type. Results are ordered in the sequence that they are found.

| Result type | Result occurrence index |
| --- | --- |
| [`M_CODE_MODEL_INDEX`](../../Reference/code/McodeGetResult.md) | 3 | 3 | 1 | 0 | 2 | 0 |
| [`M_ANGLE`](../../Reference/code/McodeGetResult.md) | 9.0 | 7.0 | 2.0 | 4.0 | 0.0 | 0.0 |
| [`M_STRING`](../../Reference/code/McodeGetResult.md) | First occurrence of code model 3 | Second occurrence of code model 3 | Only occurrence of code model 1 | First occurrence of code model 0 | Only occurrence of code model 2 | Second occurrence of code model 0 |

The result occurrence index is not explicitly returned by any function in the Aurora Imaging Library Code module, instead it must be deduced from the order in which the results are returned by [`McodeGetResult`](../../Reference/code/McodeGetResult.md); results are always indexed in ascending order starting from zero. To retrieve the number of occurrences in a code result buffer, use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_NUMBER`](../../Reference/code/McodeGetResult.md).

For each code occurrence read, you can retrieve the decoded string or the length of the string, using [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_STRING`](../../Reference/code/McodeGetResult.md) or [`M_STRING_SIZE`](../../Reference/code/McodeGetResult.md), respectively. Combining [`M_ESCAPE_SEQUENCE`](../../Reference/code/McodeGetResult.md) with [`M_STRING`](../../Reference/code/McodeGetResult.md) will return the string with unprintable characters represented by their ASCII code (in hexadecimal notation, prefixed by \x). Combining [`M_ESCAPE_SEQUENCE`](../../Reference/code/McodeGetResult.md) with [`M_STRING_SIZE`](../../Reference/code/McodeGetResult.md) will return the length of the decoded string with the ASCII characters used to represent unprintable characters included in the count.

## Drawing results

The [`McodeDraw`](../../Reference/code/McodeDraw.md) function provides several operations for drawing results in any specified image buffer or 2D graphics list. By drawing into the display's overlay buffer or associating the 2D graphics list with the display, you can also annotate an image non-destructively (see [Annotating the displayed image non-destructively](../C25_Displaying_an_image/S08_Annotating_the_displayed_image_nondestructively.md)).

You can use a previously allocated 2D graphics context (see [Generating graphics](../C26_Generating_graphics/ChapterInformation.md)) to control the drawing color, or use the default 2D graphics context ([`M_DEFAULT`](../../Reference/code/McodeDraw.md)).

Supported  operations include drawing the bounding box of the specified code occurrence (using [`M_DRAW_BOX`](../../Reference/code/McodeDraw.md)) or a cross-like symbol at the mid-point of the code occurrence (using [`M_DRAW_POSITION`](../../Reference/code/McodeDraw.md)). [`McodeDraw`](../../Reference/code/McodeDraw.md) can draw the code with its quiet zone (using [`M_DRAW_QUIET_ZONE`](../../Reference/code/McodeDraw.md)) and (optionally) the extended area (using [`M_DRAW_EXTENDED_AREA`](../../Reference/code/McodeDraw.md)). [`McodeDraw`](../../Reference/code/McodeDraw.md) can draw the scan path (using [`M_DRAW_SCAN_PROFILES`](../../Reference/code/McodeDraw.md)) or the scan reflectance profiles of the code ([`M_DRAW_REFLECTANCE_PROFILE`](../../Reference/code/McodeDraw.md)), as analyzed by the grade operation. The scan reflectance profile is generated from sampling the code along a single scan path using a specific aperture. The following is an example of a scan reflectance profile of the code.

*[Image: McodeDrawScanReflectanceProfile.png]*

Unless otherwise specified, all scan paths will be drawn. To draw the path of a specific scan reflectance profile, specify the index of the scan reflectance profile using the [`ResultIndex`](../../Reference/code/McodeDraw.md) parameter of [`McodeDraw`](../../Reference/code/McodeDraw.md). The following is an example of 10 scan paths drawn over the existing image of the code.

*[Image: McodeDrawScanProfile.png]*

When drawing composite codes, you can draw either the 2D ([`M_2D_COMPONENT`](../../Reference/code/McodeDraw.md)) or the 1D ([`M_LINEAR_COMPONENT`](../../Reference/code/McodeDraw.md)) component of the code.
