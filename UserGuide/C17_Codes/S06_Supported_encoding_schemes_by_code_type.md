---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Supported_encoding_schemes_by_code_type
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Supported encoding schemes by code type"
---

# Supported encoding schemes, sub-types, and error correction schemes by code type

In many cases, each of the supported code types can use one of several encoding schemes and error correction schemes. In addition, composite codes and GS1 Databar 1D codes are families of codes, with each member being a sub-type and typically having a different encoding scheme.

## Supported encoding schemes and sub-types

To read, grade, or write most code types, you must ensure that the proper encoding scheme is selected for your code model using [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ENCODING`](../../Reference/code/McodeControl.md); in some cases, [`M_ANY`](../../Reference/code/McodeControl.md) is supported. [`McodeDetect`](../../Reference/code/McodeDetect.md) can help you establish the encoding scheme if it supports the code type; for information, see [Automatically detecting the code type](S07_Automatically_detecting_the_code_type.md). You can also train the encoding scheme using [`McodeTrain`](../../Reference/code/McodeTrain.md) with a sample set of your images. For information, see [Training read and grade operation settings](S08_Training_read_and_grade_operation_settings.md).

> **Note:** Note that, for best results, specify the encoding scheme when reading a Code 39 code type.

When reading or grading composite code types, both [`M_ENCODING`](../../Reference/code/McodeControl.md) and [`M_SUB_TYPE`](../../Reference/code/McodeControl.md)are used. [`M_SUB_TYPE`](../../Reference/code/McodeControl.md) allows you to specify multiple sub-types so that you can limit the number of sub-types that Aurora Imaging Library tries to read or grade. For example, when dealing with a composite code, if you specify a sub-type of [`M_GS1_DATABAR_EXPANDED`](../../Reference/code/McodeControl.md) + [`M_EAN13`](../../Reference/code/McodeControl.md), Aurora Imaging Library reads your code more quickly because it will only look for these two family members as opposed to all the available sub-types.

### Supported encoding schemes for 1D code types (except GS1 Databar)

The following table lists the encoding schemes supported by each 1D code type (except [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md)):

| Encoding scheme ([`M_ENCODING`](../../Reference/code/McodeControl.md)) | 1D code types (except GS1 Databar) |
| --- | --- |
| [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | — | Yes[^CODE128ANDEAN14ONLY] | Yes | — | — | — | — | — | — |
| [`M_ENC_EAN8_ADDON`](../../Reference/code/McodeControl.md) | — | — | — | — | Yes | — | — | — | — |
| [`M_ENC_EAN13_ADDON`](../../Reference/code/McodeControl.md) | — | — | — | — | — | Yes | — | — | — |
| [`M_ENC_UPCA_ADDON`](../../Reference/code/McodeControl.md) | — | — | — | — | — | — | Yes | — | — |
| [`M_ENC_UPCE_ADDON`](../../Reference/code/McodeControl.md) | — | — | — | — | — | — | — | Yes | — |
| [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | — | — | — | Yes | Yes | Yes | Yes | Yes | — |
| [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | Yes | Yes | — | — | — | — | — | — | — |
| [`M_ENC_KOREA_MAIL`](../../Reference/code/McodeControl.md), [`M_ENC_US_MAIL`](../../Reference/code/McodeControl.md)[^INTELLIGENTMAIL], and [`M_ENC_UK_MAIL`](../../Reference/code/McodeControl.md) | — | — | — | — | — | — | — | — | Yes |

[^CODE128ANDEAN14ONLY]: Note that [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) is only available for [`M_CODE128`](../../Reference/code/McodeModel.md) and [`M_EAN14`](../../Reference/code/McodeModel.md).

[^INTELLIGENTMAIL]: Note that this is the encoding type for Intelligent Mail Barcodes.

### Supported encoding schemes and sub-types for the GS1 Databar code type

The following table lists the encoding schemes and sub-types supported by the GS1 Databar code type ([`M_GS1_DATABAR`](../../Reference/code/McodeModel.md)):

| Encoding scheme ([`M_ENCODING`](../../Reference/code/McodeControl.md)) | Sub-type ([`M_SUB_TYPE`](../../Reference/code/McodeControl.md)) | Image |
| --- | --- | --- |
| [`M_ENC_GS1_DATABAR_EXPANDED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_EXPANDED`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Expanded.png]* |
| [`M_ENC_GS1_DATABAR_EXPANDED_STACKED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_EXPANDED_STACKED`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Stacked_Expanded.png]* |
| [`M_ENC_GS1_DATABAR_LIMITED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_LIMITED`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Limited.png]* |
| [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Omni.png]* |
| [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Stacked.png]* |
| [`M_ENC_GS1_DATABAR_STACKED_OMNI`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_STACKED_OMNI`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Stacked_Omni.png]* |
| [`M_ENC_GS1_DATABAR_TRUNCATED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_TRUNCATED`](../../Reference/code/McodeControl.md) | *[Image: GS1_Databar_Truncated.png]* |

### Supported encoding schemes for 2D code types

The following table lists the encoding schemes supported by each 2D code type:

| Encoding scheme ([`M_ENCODING`](../../Reference/code/McodeControl.md)) | 2D code types |
| --- | --- |
| [`M_ANY`](../../Reference/code/McodeControl.md) | Yes | Yes | — | Yes | Yes | Yes | — |
| [`M_ENC_ALPHA`](../../Reference/code/McodeControl.md) | — | Yes | — | — | — | — | — |
| [`M_ENC_ALPHANUM`](../../Reference/code/McodeControl.md) | — | Yes | — | — | — | — | — |
| [`M_ENC_ALPHANUM_PUNC`](../../Reference/code/McodeControl.md) | — | Yes | — | — | — | — | — |
| [`M_ENC_ASCII`](../../Reference/code/McodeControl.md) | — | Yes | — | — | — | — | — |
| [`M_ENC_AZTEC...`](../../Reference/code/McodeControl.md) | Yes | — | — | — | — | — | — |
| [`M_ENC_ISO8`](../../Reference/code/McodeControl.md) | — | Yes | Yes | — | — | — | — |
| [`M_ENC_MODE2`](../../Reference/code/McodeControl.md) | — | — | — | Yes | — | — | — |
| [`M_ENC_MODE3`](../../Reference/code/McodeControl.md) | — | — | — | Yes | — | — | — |
| [`M_ENC_MODE4`](../../Reference/code/McodeControl.md) | — | — | — | Yes | — | — | — |
| [`M_ENC_MODE5`](../../Reference/code/McodeControl.md) | — | — | — | Yes | — | — | — |
| [`M_ENC_MODE6`](../../Reference/code/McodeControl.md) | — | — | — | Yes | — | — | — |
| [`M_ENC_NUM`](../../Reference/code/McodeControl.md) | — | Yes | — | — | — | — | — |
| [`M_ENC_QRCODE_MODEL1`](../../Reference/code/McodeControl.md) | — | — | — | — | — | Yes | — |
| [`M_ENC_QRCODE_MODEL2`](../../Reference/code/McodeControl.md) | — | — | — | — | — | Yes | — |
| [`M_ENC_STANDARD`](../../Reference/code/McodeControl.md) | — | — | — | — | Yes | — | Yes |

### Supported encoding scheme for composite codes

The following table lists the encoding schemes, 1D part, 2D part, and sub-types supported by composite codes. The supported composite codes have a 2D part that is either MicroPDF (CC-A or CC-B) or PDF417 (CC-C). The sub-type relates to the 1D part of the composite code, while the encoding scheme relates to both the 1D and 2D parts.

| Encoding scheme ([`M_ENCODING`](../../Reference/code/McodeControl.md)) | [`M_COMPOSITECODE`](../../Reference/code/McodeModel.md) |
| --- | --- |
| [`M_ENC_EAN8`](../../Reference/code/McodeControl.md) | [`M_EAN8`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_EAN13`](../../Reference/code/McodeControl.md) | [`M_EAN13`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_128_MICROPDF417`](../../Reference/code/McodeControl.md) | [`M_GS1_128`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_128_PDF417`](../../Reference/code/McodeControl.md) | [`M_GS1_128`](../../Reference/code/McodeControl.md) | — | Yes |
| [`M_ENC_UPCA`](../../Reference/code/McodeControl.md) | [`M_UPC_A`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_UPCE`](../../Reference/code/McodeControl.md) | [`M_UPC_E`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_EXPANDED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_EXPANDED`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_EXPANDED_STACKED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_EXPANDED_STACKED`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_LIMITED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_LIMITED`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_OMNI`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_STACKED`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_STACKED_OMNI`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_STACKED_OMNI`](../../Reference/code/McodeControl.md) | Yes | — |
| [`M_ENC_GS1_DATABAR_TRUNCATED`](../../Reference/code/McodeControl.md) | [`M_GS1_DATABAR_TRUNCATED`](../../Reference/code/McodeControl.md) | Yes | — |

## Supported error correction schemes

In most cases, Aurora Imaging Library can automatically detect the error correction scheme for read and grade operations. Similarly, Aurora Imaging Library can automatically choose the best error correction scheme for most write operations. If your code type supports more than one error correction scheme and does not support [`M_ANY`](../../Reference/code/McodeControl.md), you must specify the error correction scheme.

To specify the error correction scheme, use [`McodeControl`](../../Reference/code/McodeControl.md) with [`M_ERROR_CORRECTION`](../../Reference/code/McodeControl.md). If you specify a scheme that is not supported by your code type, Aurora Imaging Library returns an error. If your code type uses a checksum, when a discrepancy exists between the calculated and stored checksum, the read operation will yield no results. You can also train the error correction scheme using [`McodeTrain`](../../Reference/code/McodeTrain.md) with a sample set of your images. For information, see [Training read and grade operation settings](S08_Training_read_and_grade_operation_settings.md).

The following table lists the error correction schemes supported by each 1D code type:

| Code type | Error correction scheme |
| --- | --- |
|  |
| [`M_4_STATE`](../../Reference/code/McodeModel.md) | Yes | — | Yes |
| [`M_BC412`](../../Reference/code/McodeModel.md) | Yes | Yes | — |
| [`M_CODABAR`](../../Reference/code/McodeModel.md) | — | Yes | — |
| [`M_CODE39`](../../Reference/code/McodeModel.md) | Yes | Yes | — |
| [`M_CODE93`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_CODE128`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_EAN8`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_EAN13`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_EAN14`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_GS1_128`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_GS1_DATABAR`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_INTERLEAVED25`](../../Reference/code/McodeModel.md) | Yes | Yes | — |
| [`M_PHARMACODE`](../../Reference/code/McodeModel.md) | — | Yes | — |
| [`M_PLANET`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_POSTNET`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_UPC_A`](../../Reference/code/McodeModel.md) | Yes | — | — |
| [`M_UPC_E`](../../Reference/code/McodeModel.md) | Yes | — | — |

The following table lists the error correction schemes supported by each 2D code type:

| Code type | Error correction scheme |
| --- | --- |
| [`M_AZTEC`](../../Reference/code/McodeModel.md) | Yes[^AZTEC_ERROR_CORRECTION_ANY] | — | — | — | — |
| [`M_DATAMATRIX`](../../Reference/code/McodeModel.md) | Yes[^ERROR_CORRECTION_ANY] | Yes | — | — | — |
| [`M_DOTCODE`](../../Reference/code/McodeModel.md) | — | — | — | Yes | Yes |
| [`M_MAXICODE`](../../Reference/code/McodeModel.md) | — | — | — | Yes | — |
| [`M_MICROPDF417`](../../Reference/code/McodeModel.md) | — | — | — | Yes | — |
| [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) | Yes | — | Yes[^MicroQR] | — | — |
| [`M_PDF417`](../../Reference/code/McodeModel.md) | Yes | — | — | — | Yes |
| [`M_QRCODE`](../../Reference/code/McodeModel.md) | Yes | — | Yes | — | — |
| [`M_TRUNCATED_PDF417`](../../Reference/code/McodeModel.md) | Yes | — | — | — | Yes |

[^AZTEC_ERROR_CORRECTION_ANY]: Note that [`M_ANY`](../../Reference/code/McodeControl.md) sets the percentage of the code to be used for error checking to 23%.

[^ERROR_CORRECTION_ANY]: Note that [`M_ANY`](../../Reference/code/McodeControl.md) is not available for a Data Matrix code type when performing an [`McodeWrite`](../../Reference/code/McodeWrite.md) operation.

[^MicroQR]: Note that [`M_MICROQRCODE`](../../Reference/code/McodeModel.md) does not support [`M_ECC_H`](../../Reference/code/McodeControl.md).

## Extended Channel Interpretation (ECI) encoding

[`McodeRead`](../../Reference/code/McodeRead.md), [`McodeGrade`](../../Reference/code/McodeGrade.md), and [`McodeTrain`](../../Reference/code/McodeTrain.md) support occurrences of 2D code types (except [`M_MICROQRCODE`](../../Reference/code/McodeModel.md)) with an Extended Channel Interpretation (ECI) encoding. This is a piece of information embedded in a code occurrence that indicates that non-standard characters, such as Arabic or Mandarin characters, are used. To retrieve whether a code occurrence has an ECI encoding, use [`McodeGetResult`](../../Reference/code/McodeGetResult.md) with [`M_IS_ECI`](../../Reference/code/McodeGetResult.md).
