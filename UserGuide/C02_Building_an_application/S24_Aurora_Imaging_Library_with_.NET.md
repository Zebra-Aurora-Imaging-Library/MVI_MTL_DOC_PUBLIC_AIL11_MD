---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Aurora_Imaging_Library_with_.NET
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Aurora Imaging Library with .NET"
---

# Aurora Imaging Library with

Several wrappers are included with the installation of Aurora Imaging Library. These are the Aurora Imaging Library .NET wrapper and the Aurora Imaging Library Python wrapper. These wrappers allow you to make use of functions while writing applications in an Aurora Imaging Library-supported  .NET language (C#), or in Python.

Note that there is an object-oriented API for Aurora Imaging Library available in C# and Python. Aurora Imaging Object Library allows you to develop applications identical to those created with the classic API, but with the benefits of object-oriented programming. For more information, see the [Aurora Imaging Object Library](../../AIOL/index.md) documentation.

## Aurora Imaging Library with

The Aurora Imaging Library .NET wrapper is a set of classes, structures, and delegates that allows you to use Aurora Imaging Library in a C# .NET managed environment. To work in a managed .NET environment, you must first add the Zebra Aurora Imaging Library NuGet package in Visual Studio and then include the library in your code.

Using Aurora Imaging Library with managed C# is very similar to using Aurora Imaging Library with unmanaged C or C++, as long as you keep in mind the characteristics of managed code, specifically the common language runtime and its garbage collector. Managed code handles the automatic release of unused resources like  .NET strings objects. In addition, the required size for a StringBuilder parameter is automatically established when used, and will be automatically resized if needed. When using the Aurora Imaging Library .NET wrapper, all Aurora Imaging Library functions and constants are exposed as methods and constants respectively of a class called Aurora Imaging Library. For more information on writing C# code that calls Aurora Imaging Library functions, see [Aurora Imaging Library with.NET overview](../C60_Using_AIL_with_NET/S01_AIL_with_NET_overview.md).

## Aurora Imaging Library with Python

The Aurora Imaging Library Python wrapper is a Python library (module) that allows you to use Aurora Imaging Library functions while writing code in Python language. To use Aurora Imaging Library functions and constants in Python, you must reference the Aurora Imaging Library wrapper within your application as described in [](../C59_Using_AIL_with_Python/S02_Installation_and_distribution_in_Python.md).

Using Aurora Imaging Library with Python is very similar to using Aurora Imaging Library with C or C++ with a few exceptions. When using the Aurora Imaging Library Python wrapper, all Aurora Imaging Library functions and constants are defined in the Aurora Imaging Library Python library (module), and when used, must be preceded with "AIL." throughout your Python code. For more information on writing Python code that calls Aurora Imaging Library functions, see [Aurora Imaging Library with Python overview](../C59_Using_AIL_with_Python/S01_AIL_with_Python_overview.md).
