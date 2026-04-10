---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Fixing_read_problems_using_String_Expert
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Fixing read problems using String Expert"
---

# Fixing read problems using String Expert

Although the String Reader module is easy to use, there are many subtleties that can occasionally cause you to receive unexpected results. For example, a string might not be read or might not be read the way you expect it to be read. If this occurs, you can use the String Expert functionality in the String Reader interactive utility, which makes a diagnosis of your String Reader settings and reports configuration problems. An alternative to using the String Reader utility is to use the String Expert functionality available using [`MstrExpert`](../../Reference/str/MstrExpert.md) from within your application.

> **Note:** Note that the String Expert functionality is not available for a fontless context.

To use the String Expert functionality in the String Reader utility, enter your String Reader settings and the string that you want to read and start String Expert. String Expert will then give a report of the errors and/or warnings that have been detected. Errors inform you why the string is not being read or why the string being read might not be what you expect. Warnings inform you that, although the string is being read, it is at the limit of one or more of your settings. You can choose to have String Expert only produce error reports or only warning reports but the default setting is to produce both error and warning reports. String Expert will also show you the string model associated with the specified error and/or warning.

String Expert will give reports on a wide range of problems, such as an improperly set font or string model setting, as well as incorrect characters in your font or improper character constraints. For example, you might want to read a license plate that must consist of 3 letters and 3 digits, such as the license plates shown below. However, when you try to read the string from a correct source image, the string cannot be located. To establish the problem, enter the string that you expect to read (in this case, 'ZEY877') and start String Expert. When you get the error report, you might get a message saying that the string model reference scale is too high. In this case, you should lower the string scale and perform the read operation again. Now, it should finally read your string.

*[Image: StringExpertExamplePart2.png]*

Another example might be that you want to read the string 'YRH900', but did not add the correct characters to the font. However, based on your string model acceptance levels, you have a valid read operation, although the string read is not what it should be. Depending on your settings and the actual characters in your font, an 'R' can be read as an 'A', or the numerical '0' character can be read as the alphabetical 'O' character.

*[Image: StringExpertIncorrectRead.png]*

By using the String Expert functionality and getting the error report for this situation, you will receive a message saying that the target string contains one or more characters that are not defined in your font. Upon adding these characters to the font, the read operation will then produce the results you expect.

*[Image: StringExpertCorrectRead.png]*

As mentioned previously, an alternative to using the String Expert functionality in the String Reader utility is using [`MstrExpert`](../../Reference/str/MstrExpert.md). You might want to use this approach if you wanted to debug from within your application. To produce the error/warning report, pass [`MstrExpert`](../../Reference/str/MstrExpert.md) the string to read and the target image. You can also choose to only obtain an error report or only a warning report.

After calling [`MstrExpert`](../../Reference/str/MstrExpert.md), you can get the errors and/or warnings that have been detected, using [`MstrGetResult`](../../Reference/str/MstrGetResult.md). To get the actual error or warning message, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_REPORT_ERRORS`](../../Reference/str/MstrGetResult.md) or [`M_REPORT_WARNINGS`](../../Reference/str/MstrGetResult.md). To retrieve the total number of errors or warnings, use [`MstrGetResult`](../../Reference/str/MstrGetResult.md) with [`M_REPORT_NUMBER_OF_ERRORS`](../../Reference/str/MstrGetResult.md) or [`M_REPORT_NUMBER_OF_WARNINGS`](../../Reference/str/MstrGetResult.md). To retrieve the string associated with either a general error or warning, use [`MstrInquire`](../../Reference/str/MstrInquire.md) with [`M_REPORT_STRING`](../../Reference/str/MstrInquire.md).
