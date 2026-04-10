---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: String_Reader_module
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / String Reader module"
---

# Aurora Imaging Library String Reader module

The Aurora Imaging Library String Reader module is a set of powerful functions that allow you to perform feature-based character recognition. Unlike the Aurora Imaging Library Optical Character Recognition module, which is template-based, String Reader's feature-based technology makes it invariant to changes in scale, aspect ratio, and contrast. It is also considerably tolerant towards variations in perspective and angle, and allows you to quickly determine why your application is not reading the string you expect. In addition, String Reader is designed to support multiple user-defined grammar rules, multi-font definitions, and fontless string reading, making applications simple to write and maintain.

Given its feature-based technology, String Reader proves itself most useful when solving problems that have a limited amount of information, or information that tends to vary. For example, when writing an Automatic Number Plate Recognition (ANPR) application, you will probably have to contend with bad lighting, poor contrast, and variations in scale, perspective, and font. String Reader provides you with the tools to solve such problems.

*[Image: ANPRExample.png]*

String Reader uses a set of specified string models to locate and read strings. Each string model serves as a template, defining the rules a string must follow for it to be read. String Reader reads strings in grayscale images, and provides numerous results, such as the string's score and the character's value. String Reader also offers many settings that allow you to tailor the read algorithm to meet your specific needs. For example, by adjusting the character's acceptance, you can have precise control over which strings are accepted by the read operation. You can also specify a font that the read string must match to be valid or you can specify that the read strings do not have to match any specific font.

> **Note:** Note that if the strings you must read are made up of dot-matrix characters, use the [Aurora Imaging Library SureDotOCR module](../C15_SureDotOCR/S01_SureDotOCR_module.md).
