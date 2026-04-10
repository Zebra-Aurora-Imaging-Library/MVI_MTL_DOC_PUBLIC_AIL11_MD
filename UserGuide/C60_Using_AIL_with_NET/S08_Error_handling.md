---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Error_handling
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Error handling"
---

# Error handling

By default, when an error occurs in an Aurora Imaging Library function, a message box is displayed. Aurora Imaging Library allows you to control printing errors to the screen using [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md).

An option is available in .NET languages to throw exceptions instead of displaying a message box. Once enabled, when an error occurs in a function, a .NET exception is thrown instead of a message box being displayed. Exceptions thrown by Aurora Imaging Library functions are fully compatible with try/catch blocks. To enable exceptions, use [`MappControl`](../../Reference/app/MappControl.md) with [`M_ERROR`](../../Reference/app/MappControl.md) set to [`M_THROW_EXCEPTION`](../../Reference/app/MappControl.md). The following example demonstrates how to enable exceptions, and how to use them with a try/catch block.

> **Code example:** [userguide.mil_in_net.errors.cs](userguide.ail_in_net.errors.cs)
