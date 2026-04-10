---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Basic_concepts
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library OCR module

The basic concepts and vocabulary conventions for the Aurora Imaging Library OCR module are:

- **Acceptance level**. The user-defined level against which the match score is compared to determine if the match is valid.
- **Character representations**. The grayscale bitmap (image) or ASCII representation (drawing) of a character used by Aurora Imaging Library OCR.
- **Entire text**. One or many strings contained within the target image.
- **Fixed size font**. A series of character representations that have same character _Y_ size and character _X_ size. Characters of a fixed size font tend to be all upper-case.
  *[Image: dimen.png]*
- **Match score**. The result of a read/verify operation that shows the level of correlation between the character representations in the OCR font and the characters found within the image, taking into account character constraints.
- **OCR font**. The Aurora Imaging Library OCR font contains the character representations.
- **OCR font context**. An Aurora Imaging Library object that stores the OCR font information, target image character size and spacing, constraints, and processing controls.
- **Target string(s)**. The text to be found within the target image. This can be as little as a single character or multiple lines of characters.
