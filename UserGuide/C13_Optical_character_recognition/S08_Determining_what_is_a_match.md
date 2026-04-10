---
doctype: UserGuide
part: "2D processing and analysis"
chapter: Optical_character_recognition
section: Determining_what_is_a_match
module_tag: ocr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / ocr / Determining what is a match"
---

# Determining what is a match

You can control what is considered a match between characters in an image and the provided OCR font context by adjusting acceptance levels. A character must meet a specified acceptance level to be considered a recognized character. If a character is unrecognized, a symbol can be returned at the position of the unrecognized character in the string.

## Acceptance levels

You can set the acceptance level of a successful read/verify operation:

- For each character ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_ACCEPTANCE`](../../Reference/ocr/MocrControl.md)).
  If the correspondence (also known as the match score) between a character in an image and a character and its constraints in an OCR font context is less than the specified acceptance level, that character is considered invalid and the associated validity flag for that character is set to false.
- For the entire string of characters and the entire text ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_STRING_ACCEPTANCE`](../../Reference/ocr/MocrControl.md)).
  If the match score for the entire string passes the acceptance level set for the string, the string is considered valid and its validity flag is set to true. The match score for the entire string is determined by taking the average of the match scores for all characters in that string. The match score for the entire text is the average of the match scores for all the read/verified strings.

A perfect match has a match score of 100%, and no correlation has a match score of 0%. If your images have a lot of noise or distortion, set a lower acceptance level. Poor-quality images increase the chance of false readings and will probably increase the time required to read/verify the character.

> **Note:** Perfect matches are highly improbable due to noise obtained during image acquisition.

## Unrecognized characters

You can specify the symbol for unrecognized (or invalid) characters ([`MocrControl`](../../Reference/ocr/MocrControl.md) with [`M_CHAR_INVALID`](../../Reference/ocr/MocrControl.md)). If a character's match score does not reach the specified acceptance level, you can force a specified symbol to be returned at that position in the string. If no symbol is specified (default), the character with the closest match will be returned. For example:

|   |   |
| --- | --- |
| Target string: | "HELLO" |
| Invalid character: | "*" |
| Acceptance level: | 30% 30% 30% 30% 30% |
| Match scores: | 70% 15% 53% 24% 80% |
| Result: | H*L*O |

Since the match scores of the characters in the second and fourth positions are less than the specified acceptance level, these characters are replaced by asterisks in the result.
