---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Basic_concepts
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library String Reader module

The basic concepts and vocabulary conventions for the Aurora Imaging Library String Reader module are:

- **Character**. Any ASCII or Unicode symbol, such as a letter or a digit. A group of characters is used to define a font. Note that a space is not considered a character.
- **Punctuation character**. Typically categorized as characters that are not letters or numbers, such as the hyphen ('-'). To be considered part of the string, a punctuation character must fall within the range of at least one regular character's Y-size.
- **Read region**. A region in the target image enclosing the string that has been read. This region can be defined as the string's box, which includes the maximum number of spaces before and after the string.
- **Regular character**. A character that is either a letter or a number.
- **Source image**. The image from which a font can be defined. Note that fonts can also be defined from fonts installed on your system (for example, Times New Roman and Arial).
- **Space**. The space is used to delimit multiple strings that are on the same line. Strings on separate lines are automatically considered separate strings. Note that String Reader does not consider a space a normal character. A space is therefore never read.
- **String**. A linear sequence of regular characters. If the distance between two successive characters is greater than the maximum space allowed, these two characters are considered part of two separate strings. Note that characters on separate lines can never be part of the same string.
- **String model**. A data structure, within the String Reader context, that stores all the settings and constraints that control how to read one or many specific strings in the target image.
- **String Reader context**. An Aurora Imaging Library object that stores all string models and fonts. The String Reader context also stores global read settings that apply to the character recognition algorithm. Based on the settings of the context, string models, and fonts, strings in the target image are either accepted or rejected by the read operation.
- **String Reader font**. A data structure, within the String Reader context, that stores a list of characters (ASCII or Unicode) with each character's corresponding representation. In a given font, each character must be unique. Only strings containing characters that adhere to a String Reader font can be read.
- **String result**. A string, read from the target image, that respects all the settings and constraints of a string model.
- **Target image**. The image in which to read the strings.
