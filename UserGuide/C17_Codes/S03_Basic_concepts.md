---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Codes
section: Basic_concepts
module_tag: code
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / codes / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Code module

The following are terms commonly used in code operations:

- **One-dimensional (1D) code types**. 1D code types are linear bar codes containing vertical bars and spaces that typically span one row; stacked versions of 1D codes span multiple rows. They store data in one dimension. Note that postal code types encode data in the height of the bars rather than in the width of the bars and spaces. The following are examples of 1D code types:
  *[Image: codes_1d_4_state.png]*
  *[Image: codes_1d_bc412.png]*
  *[Image: codes_1d_upca.png]*
  *[Image: codes_1d_stacked.png]*
- **Two-dimensional (2D) code types**. 2D code types contain blocks that can span multiple rows and columns. They store data both horizontally and vertically. The following are examples of 2D code types:
  *[Image: codes_1d_aztec.png]*
  *[Image: codes_2d_datamatrix.png]*
  *[Image: codes_2d_maxicode.png]*
- **Aperture**. A measurement of the diameter of the circular smoothing filter used by the code grade operation to avoid minor defects that might influence the grades.
- **Bearer bars**. Bars that run along both the top and bottom of a code, two examples of which are illustrated in the diagram below.
  *[Image: bearer_bars.png]*
- **Cell**. The narrowest (thinnest) dark or white space into which data is encoded. Cells have different shapes, depending on the code type. Cells in 1D code types are the thinnest bars or spaces. Some 2D codes have cells that are approximately-square blocks, while Maxicode cells are hexagonal. In 1D and 2D codes, bars can occupy several contiguous cells, as can spaces.
  > **Note:** When dealing with 1D code types (except 4-state, Postnet and Planet) and 2D cross-row codes (MicroPDF, PDF417, and Truncated PDF), industry uses the term module to refer to a cell.
- **Cell size**. Width of the cell, the code's narrowest unit. It is the size of the cell in X.
  The cell size of a Maxicode is defined as the distance between the center points of two hexagons in the same row.
  *[Image: maxicode_cellsize.png]*
  > **Note:** The industry refers to the cell size as either x-width or module size.
- **Checksum number**. Number included as part of a 1D or 2D code, which is calculated based on the other characters in the code; it is a simple form of error detection. Typically during a read operation, the characters that make up the string are put through a checksum calculation whose results are compared against the checksum number.
- **Check digit**. A character included in a code for the purpose of performing a mathematical check on the value of the decoded code to ensure its accuracy.
- **Codeword**. A specific pattern of dark and light cells (such as lines and spaces) that encodes one or more specific digits, depending on the encoding scheme. Each code contains several types of codewords, and the purpose of the codeword is dependent on its type. For example, a data codeword contains part of the encoded information (typically, multiple data codewords are used to encode information), and error codewords are used in error-detection.
- **Code context**. An Aurora Imaging Library object that stores all code models. The code context also contains general code operation settings.
- **Code model**. A data structure that stores all the control settings to read, grade, train, or write a particular code type.
- **Column**. A series of cells along the Y-axis. Columns are only referred to when dealing with codes that have more than one row (such as 2D codes).
- **Composite code**. Composite codes combine a 1D code type and a 2D code type into one code. Composite codes are most often used to encode large strings of data. The following is an example of a composite code:
  *[Image: codes_composite.png]*
- **Extended Channel Interpretation (ECI)**. A piece of information embedded in a code that indicates that non-standard characters, such as Arabic or Mandarin characters, are used.
- **Encoding scheme**. An encoding scheme is a set of rules governing how a string is encoded. Not all code types support all encoding schemes.
- **Error correction**. Error correction schemes prevent read operations from returning incorrect results. During read operations, knowing the error correction scheme allows for error detection and error recovery. Error detection discovers bit errors (when a 1 is read as a 0 or vice versa). Error recovery both detects and corrects bit errors; depending on the error correction scheme used, questionable data can be corrected based on the remaining data.
- **Erasure**. An erasure is a missing or unreadable codeword at a known position. All erasures are errors, but not all errors are erasures.
- **Extended area**. The extended area is a region of a 2D code 20 times the cell size beyond the code's quiet zone. The extended area should contain no marks and a constant symbol contrast (reflectance). Marks can impede the read operation significantly.
- **Finder pattern and clock pattern**. The finder pattern and clock pattern are used for symbol identification, orientation, and cell location. For a Data Matrix code, the finder pattern and clock pattern are on the perimeter of the code. The finder pattern consists of two solid lines that touch at a point and the clock pattern consists of two lines of alternating light and dark cells on the opposite side of the finder pattern.
  *[Image: FinderPattern.png]*
  Aztec codes use a finder pattern and a orientation pattern in the center of the code. The finder pattern consists of concentric squares, while the orientation pattern is along the outer-corners of the finder pattern.
  *[Image: mcode_type_aztec.png]*
  Note that DotCode codes have no finder pattern; instead the fixed patterns are the interstitial dot positions and the 3-dot wide quiet zone not available for printing.
- **Module**. See cell.
- **Quiet zone**. Quiet zones are areas with no marks and are used to aid the scanning process. For 1D codes, the quiet zone immediately precedes the start character and immediately follows the stop character. For 2D matrix codes, the quiet zone must be present on all four sides of the code; for 2D cross-row codes, the quiet zone must be present on 2 sides. Even hand-written scribbles in the quiet zone will impede the read operation significantly. The size of the quiet zone is dependent on the code type.
- **Reflectance**. The grayscale value of a cell.
- **Scan reflectance profile**. A scan reflectance profile of a code is a record of values measured along a line across the width of the code and its quiet zone (scan path).
- **Scanline**. A scan path is the region of the scanline that spans the code and its quiet zone. The rest of the image's width is ignored. The scan path can be in any direction, but must be straight. Multiple scan paths are used to grade the code. A single scan path is used when generating a single scan profile as part of the scan reflectance profile.
- **Start and stop characters**. Characters which inform the reader or scanner of the beginning and end of a code, analogous to the start and stop bits which are specified when transferring data across a modem. Some code types, such as Code 39, use identical stop and start characters, while others use different stop and start characters. The PDF417 code type refers to these characters as start and stop patterns.
  *[Image: barcode_structure.png]*
  *[Image: barcode_2d_structure.png]*

## Technical code information

For detailed information about specific code types, refer to one of the following sources.

| Code type | Source |
| --- | --- |
| 1D Code types | All 1D codes except for Pharamcode, Planet code and Postnet. | *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2000, 2000, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2016, 2016, ISO/IEC Information technology) |
| BC412 | *Semi T1-95 (Reapproved 0303) Specification for back surface bar code marking of silicon wafers* (SEMI 1993, 2003, 2003, Semiconductor Equipment and Matrerials International (SEMI)) |
| Code 128 | *Automatic identification and data capture techniques - Code 128 bar code symbology specification* (International Organization for Standardization (ISO-IEC) 15417:2007, 2007, ISO/IEC Information technology) |
| Code 39 | *Automatic identification and data capture techniques - Code 39 bar code symbology specification* (International Organization for Standardization (ISO-IEC) 16388:2007, 2007, ISO/IEC Information technology) |
| EAN 8, EAN 13, UPC-A, UPC-E | *Automatic identification and data capture techniques - Bar code symbology specification - EAN-UPC* (International Organization for Standardization (ISO-IEC) 15420:2000, 2000, ISO/IEC Information technology) |
| Interleaved 2 of 5 | *Automatic identification and data capture techniques - Interleaved 2 of 5 bar code symbology specification* (International Organization for Standardization (ISO-IEC) 16390:2000, 2000, ISO/IEC Information technology) |
| GS1 Databar | *Automatic identification and data capture techniques - Reduced Space Symbology (RSS) bar code symbology specification* (International Organization for Standardization (ISO-IEC) 24724:2000, 2000, ISO/IEC Information technology) |
| Code 93 | *AIM Uniform symbology specification - Code 93* (ANSI/AIM-BC5-2000, 2000, American National Standards Institute, Inc) |
| Codabar | *AIM Uniform symbology specification - Codabar* (ANSI/AIM-BC3-2000, 2000, American National Standards Institute, Inc) |
| Pharmacode | For technical information about Pharmacode, see *The Pharmacode Guide* (Laetus am Sandberg Geratebau GmbH, 1997, Laetus am Sandberg Geratebau GmbH) by Laetus |
| Planet code | This code type is no longer actively supported by the USPS, but you can find the technical information about it using the Wayback Machine ([web.archive.org/web/20060505214851/https://mailtracking.usps.com/mtr/resources/documents/Guide.pdf](https://web.archive.org/web/20060505214851/https://mailtracking.usps.com/mtr/resources/documents/Guide.pdf)) |
| Postnet | This code type is no longer actively supported by the USPS, but you can find the technical information about it using the Wayback Machine ([web.archive.org/web/20100805234724/http://pe.usps.com/cpim/ftp/manuals/dmm300/708.pdf](https://web.archive.org/web/20100805234724/http://pe.usps.com/cpim/ftp/manuals/dmm300/708.pdf)) |
| 2D code types | All 2D codes except for MaxiCode. | *Automatic identification and data capture techniques — Bar code print quality test specification — Two-dimensional symbols* (International Organization for Standardization (ISO-IEC) 15415:2011(E), 2011, ISO/IEC Information technology) |
| Aztec | *Automatic identification and data capture techniques - Aztec Code bar code symbology specification* (International Organization for Standardization (ISO-IEC)       24778:2008(E), 2008, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC TR 29158:2011, 2011, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC 29158:2020, 2020, ISO/IEC Information technology) |
| Cross-row | *Automatic identification and data capture techniques - PDF417 bar code symbology specification* (International Organization for Standardization (ISO-IEC)       15438:2006(E), 2006, ISO/IEC Information technology) *Automatic identification and data capture techniques" MicroPDF417 bar code symbology specification* (International Organization for Standardization (ISO-IEC)       24728:2006, 2006, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2000, 2000, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2016, 2016, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Two-dimensional symbols* (International Organization for Standardization (ISO-IEC)       15415:2004(E), 2004, ISO/IEC Information technology) |
| Data matrix | *AIM Uniform symbology specification - Data matrix* (ANSI/AIM-BC11-1997, 2000, American National Standards Institute, Inc) *Automatic identification and data capture techniques - Data Matrix bar code symbology specification* (International Organization for Standardization (ISO-IEC) 16022:2007, 2006, ISO/IEC Information technology) *Automatic identification and data capture techniques - Extended Rectangular Data Matrix (DMRE) bar code symbology specification* (ISO/IEC 21471:2020, 2020, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC TR 29158:2011, 2011, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC 29158:2020, 2020, ISO/IEC Information technology) *Specification for marking of wafers with a two-dimensional matrix code symbol* (SEMI T2-0298E 1993, 2000, 2000, Semiconductor Equipment and Matrerials International (SEMI)) *Specification for back surface marking of double-side polished wafers with a two-dimensional matrix code symbol* (SEMI T7-0303 1997, 2003, 2003, Semiconductor Equipment and Matrerials International (SEMI)) |
| DotCode | *Information technology - Automatic identification and data capture techniques - Bar code symbology specification - DotCode* (Association for Automatic Identification and Mobility (AIM), 2019, American National Standards Institute, Inc) |
| MaxiCode | *AIM Uniform symbology specification - MaxiCode* (ANSI/AIM-BC10, 2000, American National Standards Institute, Inc) |
| QR and Micro QR | *Automatic identification and data capture techniques - QR Code 2005 bar code symbology specification* (ISO/IEC FDIS 18004:2006(E), 2006, ISO/IEC Information technology) *Automatic identification and data capture techniques - QR Code 2005 bar code symbology specification* (International Organization for Standardization (ISO-IEC) 18004:2006, 2006, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC TR 29158:2011, 2011, ISO/IEC Information technology) *Direct Part Mark (DPM) Quality Guideline* (ISO/IEC 29158:2020, 2020, ISO/IEC Information technology) |
| Composite codes | General | *Automatic identification and data capture techniques - EAN.UCC Composite bar code symbology specification* (International Organization for Standardization (ISO-IEC)       24723:2006(E), 2006, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2000, 2000, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Linear symbols* (International Organization for Standardization (ISO-IEC) 15416:2016, 2016, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Two-dimensional symbols* (International Organization for Standardization (ISO-IEC)       15415:2004(E), 2004, ISO/IEC Information technology) *Automatic identification and data capture techniques — Bar code print quality test specification — Two-dimensional symbols* (International Organization for Standardization (ISO-IEC) 15415:2011(E), 2011, ISO/IEC Information technology) |
