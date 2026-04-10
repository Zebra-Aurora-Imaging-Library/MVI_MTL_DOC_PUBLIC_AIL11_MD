---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Code_module
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Code module"
---

# Aurora Imaging Library code module

Many industries label products using symbols from symbologies such as UPC-A bar codes and PDF417 cross-row codes. This is done for identification purposes during different stages of production and distribution. Each **symbology**, known as a **code type** in Aurora Imaging Library, follows a different set of rules to encode the data into light and dark patterns.

Aurora Imaging Library can both read symbols from and write symbols to images. To read symbols, Aurora Imaging Library searches for specified code types in an image and decodes them. The decoded strings can then be used to identify the object, or objects, in the image. To write a symbol, Aurora Imaging Library encodes a string into a symbol using the specified code type and encoding scheme. The resulting image of the symbol can then be rotated, combined with text on a logo, and then printed on a physical medium (such as a labeling sticker, bag, or package). In Aurora Imaging Library, **symbols** are known as **codes**.

Aurora Imaging Library also allows you to grade the quality of the printed codes so that you can ensure there were no errors in the print process and the codes are readable by most types of imagers (such as, bar code scanners) that support your code type. To grade a code, Aurora Imaging Library computes the quality-grade of the code in the specified source image.

Aurora Imaging Library supports 1D, 2D, and composite code types:

- **One-dimensional (1D) code types**. These are linear bar codes used to represent data with vertical bars and spaces. These code types are typically used to represent short strings (for example, the identification number for a grocery store item, or a stock room part number).
- **Two-dimensional (2D) code types**. These are cross-row and matrix codes used to represent data with blocks stacked within a predetermined grid (that is, in or potentially spanning 1 or more rows and/or columns). 2D code types can store more information than 1D code types. These code types are typically used to represent longer strings (for example, postage and paper boarding passes).
- **Composite code types**. These are a combination of specific 1D and 2D code types. The 1D code type component encodes the item's primary identification, while the 2D code type component encodes additional data (for example, batch number or expiration date).

For examples of supported code types, see [Supported code types](S05_Supported_code_types_and_noteworthy_characteristics.md).

The Aurora Imaging Library Code module can read multiple code occurrences of most 1D code types, the DotCode code type, and the Data Matrix code type from an image.

Some code types support several methods of encoding, known as **encoding schemes**. This means a code type might, for example, support an encoding scheme of alpha characters only, numeric characters only, or both alpha and numeric characters. In addition, some code types can support any number of characters, while others need a fixed number.

If the bar code is degraded, codes can still be read if an **error correction** scheme was used when the code was generated. Error correction is essentially redundant data included in the encoding scheme of some code types. Some error correction schemes are used only for error detection, while others are used for error detection and recovery. For more information, see [Supported code types](S05_Supported_code_types_and_noteworthy_characteristics.md).

To help setup for reading or grading, the Code module can detect the type of most 1D code occurrences in an image. In addition, the module can automatically optimize the control settings of a read or grade operation by training them using a set of sample images.

The Aurora Imaging Library Code module only supports 8-bit unsigned 1-band image buffers. Performing a code operation on buffers in other formats will produce an error.

> **Note:** Note that Aurora Imaging Library supports the reading, grading, or training of codes encoded with Extended Channel Interpretation (ECI).
