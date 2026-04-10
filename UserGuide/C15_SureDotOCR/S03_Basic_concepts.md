---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: Basic_concepts
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library SureDotOCR module

The basic concepts and vocabulary conventions for the Aurora Imaging Library SureDotOCR module are:

- **Character**. A symbol, such as a digit, letter, or punctuation mark, represented by a dot-matrix.
- **Constraint**. A restriction that limits the strings to read. Constraints typically refer to permitted character constraints. If you override the constraints for a specific position, the position is said to be explicitly constrained; otherwise, it is said to be implicitly constrained.
- **Dot-matrix**. A two-dimensional grid of dots that represents a character.
- **Dot-matrix template**. The dot-matrix with which every character in a font must be represented. If a font's dot-matrix template is 5 columns by 7 rows, the dot-matrix that represents the number 1 can contain a dot in every cell of its center column.
- **Empty font**. A font with no characters.
- **Font**. A data structure, within a SureDotOCR context, that stores a list of characters, with each character's name and corresponding dot-matrix representation.
- **Permitted character constraint**. A restriction that limits the characters to read at a string model position according to a font and character type.
- **Punctuation character**. Typically categorized as characters that are not letters or digits, such as the hyphen ('-').
- **Rank**. The order in which to read strings. SureDotOCR uses rank to establish the total number of strings to read.
- **Regular character**. A character that is either a letter or a digit.
- **Space**. An area within which character information is absent. Spaces occur between dots in the same character, between characters in the same string, and between strings on the same line. Strings on separate lines are always separate strings.
- **String**. A linear sequence of generally aligned characters. If the distance between two successive characters is greater than the maximum space allowed, the characters are considered part of two strings. Characters on separate lines are always in separate strings.
- **String model**. A data structure, within a SureDotOCR context, that stores a string to read in the target image.
- **String model positions**. Subdivisions of a string model representing the characters to read.
- **String results**. Dot-matrix strings, read from the target image, that respect the specifications of a SureDotOCR context, including string model and font settings therein.
- **SureDotOCR context**. An Aurora Imaging Library object that stores all settings, fonts, and string models with which to read dot-matrix text. Use the SureDotOCR console-based utility,_DmrContextViewer_ (accessible from the Aurora Imaging Example Launcher), to view the settings of a context, and the settings of the fonts and string models therein.
  *[Image: MdmrContext.png]*
- **SureDotOCR font definition file **. A text file, with an MDMRF extension, that represents a font and its dot matrix characters. Use this file to import a font into, and to export a font out of, a SureDotOCR context.
- **Target image**. The image in which to read dot-matrix strings.
- **Text block**. The area in the image buffer (or in the buffer's associated region of interest) that includes all dot-matrix text.
