---
doctype: UserGuide
part: "2D processing and analysis"
chapter: SureDotOCR
section: Character_names
module_tag: dmr
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / SureDotOCR / Character names"
---

# Character names

SureDotOCR supports Unicode environments.

You can specify character names using their native representation or in hexadecimal format beginning with "\x". For example, you can specify the character name of the letter A as "A" (its native name) or you can specify it as "\x0041". Characters between 0 and 127 are considered Basic Latin. You can also specify characters beyond the Basic Latin range, such as the smiley face character (☺, or "\x263A" in hexadecimal).

After a read operation, you can use [`M_CHAR_NAME`](../../Reference/dmr/MdmrGetResult.md) to retrieve the name of a character in the string that was read.

When retrieving results in a Unicode environment, character representations are returned (for example, ☺). You can, however, explicitly specify that the character name in hexadecimal format is returned instead using [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrGetResult.md) or [`M_HEX_UTF16_FOR_ALL`](../../Reference/dmr/MdmrGetResult.md) (for example, to obtain "\x263A" instead of the actual smiley face). This is not typically done since characters beyond the Basic Latin range can exist natively in a Unicode string. However, retrieving character names in hexadecimal might be useful if you want to convert your data into ASCII, for example.

Note that the character name representations affect the string size. For example, the string "abc☺" nominally consists of four characters plus the null terminating character. Accordingly, a string size result (retrieved with, for example, [`M_STRING`](../../Reference/dmr/MdmrGetResult.md) + [`M_STRING_SIZE`](../../Reference/dmr/MdmrGetResult.md)) will return 5 as the string size. For the same string mentioned above ("abc☺"), when [`M_HEX_UTF16_FOR_ALL`](../../Reference/dmr/MdmrGetResult.md) is specified, you will obtain "\x0041\x0042\x0043\x263A" and a string size of 25. When [`M_HEX_UTF16_FOR_NON_BASIC_LATIN`](../../Reference/dmr/MdmrGetResult.md) is specified, you will obtain "abc\x263A" and a string size of 10.
