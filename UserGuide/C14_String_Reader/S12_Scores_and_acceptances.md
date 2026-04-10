---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Scores_and_acceptances
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Scores and acceptances"
---

# Scores and acceptances

When String Reader performs a read operation, it analyzes the target image and localizes potential character candidates for each string model. The best candidates that meet the minimum definition of a string (a linear sequence of regular characters) and all user-specified constraints will be read.

Every character candidate, and the string candidate to which it belongs, is given a score between 0 and 100. The higher the score, the better the candidate. These scores quantify:

- The similarity between the character in the target image and the corresponding character in the font.
- The similarity between the character in the target image and the other characters of the string in the target.
- The number of unread characters within the read-region of the string in the target image.

String Reader allows you to set various acceptance values, which act as a threshold for the candidate's scores. If the candidate's score is equal to or above its respective acceptance value, the candidate can be read; otherwise, it cannot be read. Note that the string candidates with the highest scores above all the acceptance values will be read, up to the maximum number of strings specified. For more information on setting the maximum number of strings to read, see [Number of strings to read](S08_Number_of_strings_to_read.md).

String Reader also allows you to set various string certainty values. Any string candidate with scores greater than or equal to the certainty levels is considered a certain match. Therefore, if the read operation localizes the required number of string candidates that meet all the certainty values, the read operation stops, without verifying the rest of the image for candidates with higher scores.

The acceptance level represents a threshold for a score that is adequate, though you would like to check the rest of the image for better scores. The certainty level represents a threshold for a score that is so good, you need not verify that there is a better one. Since lowering the certainty usually results in less image analysis, it can speed up the read operation. However, you should set the certainty carefully, since a high certainty can stop [`MstrRead`](../../Reference/str/MstrRead.md) prematurely, and allow the best candidates to remain unread.

Acceptance and certainty values can be set for the characters, the string, and the read-region of the string in the target image. Characters also have a similarity and homogeneity setting.

The default value for each of these settings is typically sufficient. However, to fit the specific needs of your application, it is recommended that you try experimenting with the values. For example, trying various acceptance values might be necessary if you have an image with an unusual amount of text that you want to ignore. Note that, although each value represents a kind of threshold for a specific score, when used in concert, they provide a great deal of versatility for accepting and rejecting strings as a whole.

## String acceptance, certainty, and score

The string score is the average score of all the characters in the string. To set the acceptance threshold for the string score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_ACCEPTANCE`](../../Reference/str/MstrControl.md). To set the certainty threshold for the string score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_CERTAINTY`](../../Reference/str/MstrControl.md). To get the actual score of the string, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_STRING_SCORE`](../../Reference/str/MstrGetResult.md).

The following animation illustrates an image used to define a font, and a target image where the string 'ZEY 877' must be read. In this example, the string has a score of 91.9%. Therefore, if [`M_STRING_ACCEPTANCE`](../../Reference/str/MstrControl.md) is set to 91.9 or below, the string 'ZEY 877' will be accepted by the read operation.

*[Image: StringReaderExample]*

## Character acceptance and score

The character score is calculated as the average value of the character's similarity and homogeneity scores. To set the acceptance threshold for the character score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CHAR_ACCEPTANCE`](../../Reference/str/MstrControl.md). To get the actual score of the character, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_CHAR_SCORE`](../../Reference/str/MstrGetResult.md).

Note that characters with character scores below the character acceptance value will not be considered part of the string, and therefore might cause the string not to be read. Note that if a character does not meet either the character similarity or homogeneity acceptance thresholds, these scores are ignored when calculating the string score ([`M_STRING_SCORE`](../../Reference/str/MstrGetResult.md)).

### Character similarity

The character's similarity score quantifies how closely the character in the target image resembles the character in the font. This resemblance is based solely on how the character "looks," disregarding variations in contrast, size, aspect ratio, scale, and baseline. For example, an 'A' is still an 'A', even if the 'A' in the target image is twice as big as the 'A' in the font.

*[Image: SimilarityExample.png]*

To set the acceptance threshold for the character's similarity score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CHAR_SIMILARITY_ACCEPTANCE`](../../Reference/str/MstrControl.md). To get the actual similarity score of the character, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_CHAR_SIMILARITY_SCORE`](../../Reference/str/MstrGetResult.md).

### Character homogeneity

The character's homogeneity score quantifies the similarity between the character in the target image and the other characters of the string in the target image. To calculate the character's homogeneity score, many character traits are taken into account, such as variations in contrast, size, aspect ratio, scale, and baseline. If each of these characteristics (and others not mentioned) is the same between the character and all other characters of the string in the target image, the resulting homogeneity score for that character would be 100. Note, however, that this is seldom the case.

To set the acceptance threshold for the character's homogeneity score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_CHAR_HOMOGENEITY_ACCEPTANCE`](../../Reference/str/MstrControl.md). To get the actual homogeneity score of the character, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_CHAR_HOMOGENEITY_SCORE`](../../Reference/str/MstrGetResult.md).

Since the character's homogeneity is calculated by how well the character compares to the other characters in the string, rather than how well it compares to the font, it can solve problems that cannot be resolved solely by similarity with the font. For example, in a target image, you might have characters that have high similarity scores and that, individually, are all acceptable. However, if their characteristics cause them to be out of context with the other characters in the string, you might want to reject them, based on their homogeneity score.

*[Image: HomogeneityExample.png]*

## String target acceptance, certainty, and score

String Reader computes a score for the read-region of the string in the target image. This region can be defined as the string's box, which includes the maximum number of spaces before and after the string. This distance is calculated using the space width ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_WIDTH`](../../Reference/str/MstrControl.md)) and the maximum number of consecutive spaces ([`MstrControl`](../../Reference/str/MstrControl.md) with [`M_SPACE_MAX_CONSECUTIVE`](../../Reference/str/MstrControl.md)). For more information, see [Space size](S04_Creating_and_customizing_the_fonts_for_a_font_based_context.md).

Any unread character within the read-region in the target image will lower the string target score. An unread is any character that would ordinarily be read, if not for user-specified constraints (such as scale and grammar restrictions). The greater the number of unread readable characters in the read-region of the string in the target image, the lower the string target score will be.

To get the string target score, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_STRING_TARGET_SCORE`](../../Reference/str/MstrGetResult.md). To set the acceptance threshold for the string target score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_TARGET_ACCEPTANCE`](../../Reference/str/MstrControl.md). To set the certainty threshold for the string target score, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_TARGET_CERTAINTY`](../../Reference/str/MstrControl.md).

For example, to read the string "SAM VOX", you have set 'S', 'A', 'M' as the first three character constraints, and 'V', 'O', 'X' as the last three character constraints. In the following target image, it is likely that "SAM VOX" would be read.

*[Image: Samantha.png]*

However, you might want the extra letters at the beginning and in the middle of the read-region of the string to cause this string not to be read. To do so, set an adequate acceptance level for the string target score (for example, 50.0). The extra letters in the string's read-region will cause this string to have a very low string target score.
