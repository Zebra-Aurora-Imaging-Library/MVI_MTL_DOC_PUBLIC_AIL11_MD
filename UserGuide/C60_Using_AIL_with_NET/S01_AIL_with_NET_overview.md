---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: AIL_with_NET_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / AIL with NET overview"
---

# Aurora Imaging Library with

The Aurora Imaging Library .NET (AIL.NET) wrapper allows you to use Aurora Imaging Library with C# in a .NET environment. Using Microsoft's Platform Invoke technology, the .NET wrapper exposes the Aurora Imaging Library's functionally to .NET languages such as C#. Essentially, the .NET wrapper is a set of classes, structures, and delegates that exposes all the Aurora Imaging Library functions as methods, and all the Aurora Imaging Library constants as constants of the class.

Typically, the term **function** is used in reference to C/C++, and the term **method** is used in reference to object-oriented languages like C#. However, they imply essentially the same thing; methods are simply functions defined in a class and that typically are used on objects of that class. For consistency with the rest of the Aurora Imaging Library documentation, we use the term **function** throughout, regardless of the language and context being discussed.

.NET is a managed environment, which means that it has certain characteristics, such as garbage collection and the Common Language Runtime, that make it different from programming in standard C/C++. However, programming using Aurora Imaging Library in a managed  .NET environment is very similar to programming with Aurora Imaging Library in C/C++, as long as you keep the concepts of writing managed code in mind.

Note that this chapter does not expand upon the concepts inherent to managed code, nor does it detail the syntax of C#. For information about these topics, refer to [learn.microsoft.com](https://learn.microsoft.com/).

Note that there is an object-oriented API for Aurora Imaging Library available in C#. Aurora Imaging Object Library allows you to develop applications identical to those created with the classic API, but with the benefits of object-oriented programming. For more information, see the [Aurora Imaging Object Library](../../AIOL/index.md) documentation.
