---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Using_NET_strings_with_AIL
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Using NET strings with AIL"
---

# Using

Strings are handled differently in managed code than in unmanaged code. In managed code, strings are immutable (read-only) objects. Some Aurora Imaging Library functions, such as [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) or [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md), have a parameter that requires a pointer to a modifiable string. Since strings are immutable in .NET, additional steps must be taken to use these functions. In these cases, you must create a StringBuilder object, and pass that StringBuilder object, rather than a pointer to a string, to the Aurora Imaging Library function. The following example demonstrates how this is done.

> **Code example:** [userguide.mil_in_net.strings.cs](userguide.ail_in_net.strings.cs)

When the call to the Aurora Imaging Library function returns, the StringBuilder object contains a sequence of characters that ends with a null terminating character ('\0'). Then, you can retrieve the string object by calling the ToString function of the StringBuilder instance.
