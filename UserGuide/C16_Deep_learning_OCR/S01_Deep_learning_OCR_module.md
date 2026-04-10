---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Deep_learning_OCR_module
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Deep learning OCR module"
---

# Aurora Imaging Library Deep Learning OCR module

The Aurora Imaging Library Deep Learning OCR module is a set of functions that allow you to perform optical character recognition using a deep learning architecture. Unlike the other Aurora Imaging Library string reading modules, the Deep Learning OCR module does not require fonts or string models to read text in an image. By default, the Deep Learning OCR module can read all text in an image within the default character height bounds. You can change these bounds to include larger or smaller text if needed. To improve robustness or to select a subset of strings to read, a string model can be defined. You can define the model from a read string occurrence at a location in an image, or you can define the model explicitly. You can also finetune a classifier to improve read results on data from your specific use case.

String models are used to limit the strings being read. You can use a string model to read only strings that start or end with a specified string (textual anchor). String models also allow you to set positional constraints. You can restrict positions in string models to a particular character type or to a list of specified characters. For example, to read the tenth, the first, or the twenty-fifth uppercase letter of the alphabet at the specified position, use the string "JAY".

The Deep Learning OCR module performs best when reading uppercase letters and digits. The Deep Learning OCR module can also read lowercase letters. Punctuation and special characters (!#$%&( )*+,-./:;&lt;=>?@[]^_`&#123;|&#125;~\"'\\€£¥¢) are supported but are disabled by default. Dot-matrix text is supported by the Deep Learning OCR module however, it is recommended to use the SureDotOCR module in cases where dots are well segmented.

While the Deep Learning OCR module does not use fonts, it tends to perform better with some fonts. Some recommended fonts include:

- Bahnschrift.
- Bierstadt.
- Trebuchet.
- Mairyo UI.
- Tahoma.
- Verdana.
- Cascadia Code.
