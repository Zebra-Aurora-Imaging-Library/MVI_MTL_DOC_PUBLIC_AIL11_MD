---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Supported_code_types_and_noteworthy_characteristics
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Supported code types and noteworthy characteristics"
---

# Supported code types

Whether you plan on reading, grading, training, or writing codes, you typically need to specify the code type when adding a code model to your code context using [`McodeModel`](../../Reference/code/McodeModel.md). If your operation is not successful, ensure that you have added a code model of the appropriate code type. Alternatively, you can automatically detect the code type of most 1D code occurrences in an image, using [`McodeDetect`](../../Reference/code/McodeDetect.md), and then automatically add a code model for each detected code type to your code context, using [`McodeModel`](../../Reference/code/McodeModel.md) with [`M_RESET_FROM_DETECTED_RESULTS`](../../Reference/code/McodeModel.md). For information, see [Automatically detecting the code type](S07_Automatically_detecting_the_code_type.md).

> **Note:** A code context can contain multiple code models of 1D code types (excluding GS1 databar, Planet, Postnet, and 4-state); for other code types, a code context can contain at most one code model. Although a code context can contain only one 2D code model, you can specify to perform a read, grade, or train operation on more than one occurrence of a Data Matrix code model (or any 1D code model that is not a GS1 Databar, Planet, Postnet, and 4-state code model), using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_NUMBER`](../../Reference/code/McodeControl.md).

If setting up a code context for reading codes that meet an ISO-compatible SEMI-specification, you can use a predefined code context, distributed with Aurora Imaging Library, using [`McodeRestore`](../../Reference/code/McodeRestore.md).

This section lists the supported code types, and their Aurora Imaging Library predefined constants. It also lists the predefined SEMI code contexts.

## 1D code types

The following is a list of the supported 1D code types.

| Family of code | Code type | Sample | Aurora Imaging Library constant | [`McodeDetect`](../../Reference/code/McodeDetect.md) supported |
| --- | --- | --- | --- | --- |
| Code 128 family | Code 128 | *[Image: codes_1d_code128.png]* | [`M_CODE128`](../../Reference/code/McodeModel.md) | Yes |
| EAN 14 | *[Image: codes_1d_ean14.png]* | [`M_EAN14`](../../Reference/code/McodeModel.md) | Yes |
| GS1-128 | *[Image: codes_1d_gs1_128.png]* | [`M_GS1_128`](../../Reference/code/McodeModel.md) | Yes |
| Code 2 of 5 | IATA 2 of 5 (International Air Transport Association) | *[Image: codes_1d_iata25.png]* | [`M_IATA25`](../../Reference/code/McodeModel.md) | Yes |
| Industrial 2 of 5 (standard 2 of 5) | *[Image: codes_1d_industrial_25.png]* | [`M_INDUSTRIAL25`](../../Reference/code/McodeModel.md) | Yes |
| Interleaved 2 of 5 (ITF-14) | *[Image: codes_1d_interleaved25.png]* | [`M_INTERLEAVED25`](../../Reference/code/McodeModel.md) | Yes |
| Consumer | EAN 8 | *[Image: codes_1d_ean8.png]* | [`M_EAN8`](../../Reference/code/McodeModel.md) | Yes |
| EAN 13 | *[Image: codes_1d_ean13.png]* | [`M_EAN13`](../../Reference/code/McodeModel.md) | Yes |
| UPC-A | *[Image: codes_1d_upca.png]* | [`M_UPC_A`](../../Reference/code/McodeModel.md) | Yes |
| UPC-E | *[Image: codes_1d_upce.png]* | [`M_UPC_E`](../../Reference/code/McodeModel.md) | Yes |
| Postal | 4-state | *[Image: codes_1d_4_state.png]* | [`M_4_STATE`](../../Reference/code/McodeModel.md) | -- |
| Planet | *[Image: codes_1d_planet.png]* | [`M_PLANET`](../../Reference/code/McodeModel.md) | -- |
| Postnet | *[Image: codes_1d_posnet.png]* | [`M_POSTNET`](../../Reference/code/McodeModel.md) | -- |
| Miscellaneous | BC412 | *[Image: codes_1d_bc412.png]* | [`M_BC412`](../../Reference/code/McodeModel.md) | Yes |
| Codabar | *[Image: codes_1d_codabar.png]* | [`M_CODABAR`](../../Reference/code/McodeModel.md) | Yes |
| Code 39 | *[Image: codes_1d_code39.png]* | [`M_CODE39`](../../Reference/code/McodeModel.md) | Yes |
| Code 93 | *[Image: codes_1d_code93.png]* | [`M_CODE93`](../../Reference/code/McodeModel.md) | Yes |
| GS1 Databar[^GS1Databar] | *[Image: codes_1d_rss.png]* | [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) | -- |
| Pharmacode | *[Image: codes_1d_pharmacode.png]* | [`M_PHARMACODE`](../../Reference/code/McodeModel.md) | -- |

[^GS1Databar]: There are several different sub-types of the GS1 Databar code type; see [Supported encoding schemes and sub-types for the GS1 Databar code type](S06_Supported_encoding_schemes_by_code_type.md).

## 2D code types

The following is a list of the supported 2D code types.

| Family of code | Code type | Sample | Aurora Imaging Library constant |
| --- | --- | --- | --- |
|  |
| Matrix | Aztec | *[Image: codes_1d_aztec.png]* | [`M_AZTEC`](../../Reference/code/McodeModel.md) |
| Data Matrix | *[Image: codes_2d_datamatrix.png]* | [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) |
| DotCode | *[Image: codes_2d_dotcode.png]* | [`M_DOTCODE`](../../Reference/code/McodeModel.md) |
| Maxicode | *[Image: codes_2d_maxicode.png]* | [`M_MAXICODE`](../../Reference/code/McodeModel.md) |
| QR code | *[Image: codes_2d_qrcode.png]* | [`M_QRCODE`](../../Reference/code/McodeModel.md) |
| Micro QR code | *[Image: codes_2d_microqrcode.png]* | [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) |
| Cross-row | PDF417 | *[Image: codes_2d_pdf417.png]* | [`M_PDF417`](../../Reference/code/McodeModel.md) |
| MicroPDF417 | *[Image: codes_2d_micropdf417.png]* | [`M_MICROPDF417`](../../Reference/code/McodeModel.md) |
| Truncated PDF417 | *[Image: codes_2d_truncatedpdf417.png]* | [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) |

## Composite codes

A composite code combines a 1D and 2D code together in a single image. You specify the 1D and 2D components by specifying their corresponding encoding scheme. For more information, refer to [Supported encoding scheme for composite codes](S06_Supported_encoding_schemes_by_code_type.md). Control types that apply to either part of a composite code also apply to the composite code as a whole. For the purposes of this chapter, composite codes will not be discussed explicitly.

The following is an example of a composite code.

| Code type | Sample | Aurora Imaging Library constant |
| --- | --- | --- |
| Composite code | *[Image: codes_composite.png]* | [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) |

## Predefined SEMI code contexts

Aurora Imaging Library is distributed with three ISO-compatible SEMI code contexts, each containing a code model and code model settings that match the specification. These can be used directly, or modified to suit your needs.

| Code context file | Specification | Associated code type |
| --- | --- | --- |
| SEMI_T1-95r0303.mco | SEMI T1-95 (Reapproved 0303) | [`M_BC412`](../../Reference/code/McodeModel.md) |
| SEMI_T2-0298E.mco | SEMI T2-0298E | [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) |
| SEMI_T7-0303.mco | SEMI T7-0303 | [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) |

To use a predefined SEMI code context, restore it using [`McodeRestore`](../../Reference/code/McodeRestore.md) with the appropriate file name. These files are located in the installed Contexts folder (for example, _C:\Program Files\Aurora Imaging Library\11\Contexts_). Once restored, the code context can be modified using the Aurora Imaging Library Code module's functions.
