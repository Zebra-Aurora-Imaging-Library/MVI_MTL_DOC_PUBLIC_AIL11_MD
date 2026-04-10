---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: SureDotOCR_module
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / SureDotOCR module"
---

# Aurora Imaging Library SureDotOCR module

The Aurora Imaging Library SureDotOCR module is an innovative set of functions, prefixed with Mdmr, that allow you to perform OCR directly on dot-matrix text (no previous preparations needed).

*[Image: MdmrSureDotOCRLogo.png]*

Dot-matrix text refers to characters represented by a two-dimensional grid of dots. This grid is a dot-matrix. A common dot-matrix has 5 columns and 7 rows.

*[Image: MdmrGridBasic.png]*

To form dot-matrix characters, machines can print the required dots using drops of ink. To represent a virtual equivalent of dot-matrix characters, you can specify the printed dots as foreground values (FF) in a font file, or even an array.

*[Image: MdmrGridBasicOWithForeground.png]*

Numerous industries, like packaging, use dot-matrix text to print product information, such as manufacturing date, expiry date, lot number, and product number. With SureDotOCR, you can design a vision inspection system that directly ensures the validity of this information. For example, to validate an expiration date printed with dot-matrix text, you can use SureDotOCR to read the text and verify that the first printed string is "Exp." and the second printed string is the expected month, day, and year.

*[Image: MdmrGridBasicExpiration316.png]*

You can quickly create a SureDotOCR context that holds the required fonts, string models, and constraints necessary to perform robust read operations on target images. With minimal tuning of defaults, SureDotOCR successfully reads strings in 8-bit grayscale images and provides numerous results, such as the string's score and the character's value.

SureDotOCR is invariant to changes in contrast, and offers full rotational tolerance. The module can also adapt to some variations in scale, aspect ratio, non-uniform dot spacing, uneven backgrounds, deformed or skewed characters, and perspective. SureDotOCR supports multiple grammar-like rules, offering a high degree of flexibility to constrain the characters to read at each position in a string. Multiple font definitions are supported in a single context, and you have access to SureDotOCR's console-based font utilities, as well as numerous fonts using SureDotOCR's own font file format, allowing you to easily manage your fonts. You can quickly create your own, and you can make use of the ones that SureDotOCR provides.

A SureDotOCR context internally handles characters in Unicode. Numerous types of characters are supported, including international character sets, punctuation marks, and, of course, the smiley face.

SureDotOCR uses feature-based technologies specifically designed to accurately interpret and robustly read the unique qualities of dot-matrix strings. For example, the spaces between the dots that make up characters can prove difficult to manage for traditional character recognition solutions, which typically require preparing case-specific images with solid-stroked text. SureDotOCR requires no such preparation. You just need to specify some settings, such as the expected size of the dots and the dimensions of a box framing the dot-matrix text in your image. Reading dot-matrix text has never been simpler, or more effective, than with SureDotOCR.
