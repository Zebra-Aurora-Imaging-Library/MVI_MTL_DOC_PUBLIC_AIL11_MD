---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Improving_search_speed
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Improving search speed"
---

# Improving search speed

To ensure the fastest possible read or verify operation:

- Use as clean a target image as possible when using an [`M_GENERAL`](../../Reference/ocr/MocrAllocFont.md) OCR font context.
- Calibrate the OCR font context's character size and target spacing, to match the characters in the target images.
- Preprocess once all the processing controls and constraints are set and before the application's first read/verify operation.
- Reduce the area to read/verify in the target image by creating a child buffer around the target string using [`MbufChild...`](../../Reference/buf/MbufChild1d.md); the search time is roughly proportional to the area searched.
- Set character constraints using [`MocrSetConstraint`](../../Reference/ocr/MocrSetConstraint.md).
- Skip the location step, when possible.
- Set the speed and reliability of the algorithm by setting the robustness factor ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_SPEED`](../../Reference/ocr/MocrControl.md)). For instance, when reading larger characters, robustness can be sacrificed for speed.
- Disable the [`M_BLANK_CHARACTERS`](../../Reference/ocr/MocrControl.md), [`M_BROKEN_CHAR`](../../Reference/ocr/MocrControl.md), and [`M_TOUCHING_CHAR`](../../Reference/ocr/MocrControl.md) control types unless absolutely necessary.
- Adjust your image to minimize the angular search range of the read/verify operation.

The more you know about the target image, the faster it can be read/verified. This includes:

- Knowing where the string is located within the target image.
- Knowing the height, width, and the inter-character spacing of the string within the target image.
- Knowing the exact number of lines to be read from the target image.
- Knowing the exact string length or at least that all the strings are of similar length.
- Having good examples of each character to be found in the target image so a high-quality Aurora Imaging Library OCR font can be made.

Aurora Imaging Library OCR can help determine some of this information, for example, [`MocrCalibrateFont`](../../Reference/ocr/MocrCalibrateFont.md) can automatically calibrate the OCR font context's character size to match the characters in the sample target image, and the string location step with [`MocrControl`](../../Reference/ocr/MocrControl.md) can find a string within the image, barring any problems.
