---
doctype: UserGuide
part: "2D processing and analysis"
chapter: String_Reader
section: Number_of_strings_to_read
module_tag: str
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D processing and analysis / string / Number of strings to read"
---

# Number of strings to read

You can set the maximum number of strings to read in the target image for both the String Reader context and for each individual string model in the String Reader context.

For each individual string model, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md), specifying the index of the particular string model. The default value is 1. To read all strings that adhere to a string model, set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) to [`M_ALL`](../../Reference/str/MstrControl.md).

The number set for the String Reader context specifies the maximum total of all strings that can be read, for all string models in the context together. To set the number for a String Reader context, use [`MstrControl`](../../Reference/str/MstrControl.md) with [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md), specifying [`M_CONTEXT`](../../Reference/str/MstrControl.md) as the index. The default value is [`M_ALL`](../../Reference/str/MstrControl.md), which reads the maximum number of strings specified for each string model.

Note that if the context's [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) value is reached, the read operation stops, regardless of any individual string model's outstanding [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) setting. Similarly, once every individual string model's [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) value is reached, the read operation stops, regardless of the context's outstanding [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) setting.

For example, if you are writing an application that reads three name tags, you need three string models, one for each name. If you set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) for the context to 1, and you set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) for each string model to 1, then as soon as one of the name tags is read, both string number settings will be satisfied. However, if you set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) for the context to 3 and you set [`M_STRING_NUMBER`](../../Reference/str/MstrControl.md) for each string model to 1, then the string number settings will only be satisfied when one of each name tag is read.
