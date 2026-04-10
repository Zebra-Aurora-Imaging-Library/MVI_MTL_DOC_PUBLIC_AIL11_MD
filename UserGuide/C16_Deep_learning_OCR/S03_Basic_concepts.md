---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Deep_learning_OCR
section: Basic_concepts
module_tag: dlocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / Deep_learning_OCR / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library Deep Learning OCR module

The basic concepts and vocabulary conventions for the Aurora Imaging Library Deep Learning OCR module are:

- **Character**. A symbol, such as a digit, letter (upper and lowercase) or punctuation mark (disabled by default).
- **Constraint**. A restriction that limits the characters that can be read at a position in a string. This includes the type of character (letters, digits) or a list of accepted characters.
- **Dataset**. A type of database containing a labeled series of entries (images) with which to fine-tune a classifier.
- **Deep learning (DL)**. A method in machine learning that uses learning techniques related to neural networks.
- **Deep Learning OCR fine-tuning context**. An Aurora Imaging Library object that stores the training engine and settings required to fine-tune a classifier.
- **Deep Learning OCR read context**. An Aurora Imaging Library object that stores the prediction engine, all settings, constraints, and string models with which to read text.
- **Fine-tuning**. The process of taking a pretrained classifier and further training on a dataset containing valid read results.
- **String**. A linear sequence of aligned characters. If the distance between two successive characters is greater than the maximum space allowed, the characters are considered part of two strings. Characters on separate lines are always in separate strings.
- **String model**. A data structure, within a Deep Learning OCR context, that stores settings and positional constraints of a string to read in the target image.
- **String occurrence**. A string, read from the target image, that respects the specifications of a Deep Learning OCR context, including string model settings and positional constraints.
- **Target image**. The image in which to read strings.
