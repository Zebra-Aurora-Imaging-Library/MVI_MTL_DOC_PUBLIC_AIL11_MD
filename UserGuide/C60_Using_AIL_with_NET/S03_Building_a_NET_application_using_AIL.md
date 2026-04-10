---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Building_a_NET_application_using_AIL
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Building a NET application using AIL"
---

# Building a

For Aurora Imaging Library functions and constants to be available in a managed .NET application, you must first add the Zebra Aurora Imaging Library NuGet package in Visual Studio; this gives the code access to all of Aurora Imaging Library. Once this is done, your code can use the language-specific call to include the library. Note that all Aurora Imaging Library modules are contained in a single namespace, Zebra.AuroraImagingLibrary. The following details the procedure for including Aurora Imaging Library in C# in a managed .NET environment.

## Aurora Imaging Library inclusion procedure for

To use Aurora Imaging Library functions and constants, you must first add the Zebra Aurora Imaging Library NuGet package in Visual Studio, and optionally, include the library in your code. To start using Aurora Imaging Library in an existing .NET project:

1. In Visual Studio, add the Zebra Aurora Imaging Library NuGet package using the NuGet Package Manager.
2. Add the following to your code to include the functions and constants in the Zebra.AuroraImagingLibrary namespace:
   | Language | Reference statement |
   | --- | --- |
   | Visual C# | using Zebra.AuroraImagingLibrary; |
   > **Note:** Note you could optionally avoid including the entire library and preface each Aurora Imaging Library function call with `[[Zebra.AuroraImagingLibrary.]AIL.AilFunction(...)]`.
3. Add Aurora Imaging Library code in your application, keeping in mind the differences of programing with the AIL.NET wrapper, which are discussed throughout this chapter.

When using the AIL.NET wrapper, you must compile using the 64-bit configuration.

Typically, you should use custom Aurora Imaging Library data types (for example, _M_ID_ and _M_INT_), as opposed to language-specific data types. This allows for a more portable application. See [Data types in Aurora Imaging Library with .NET](S04_Data_types_in_AIL_with_NET.md) for more information on _M_ID_ and _M_INT_.

## Calling Aurora Imaging Library functions and constants in

Once the Zebra Aurora Imaging Library NuGet package is added and accessible by your code, calling Aurora Imaging Library functions is almost identical in .NET as it is in unmanaged C/C++. Aurora Imaging Library is in an AIL.NET wrapper, and all Aurora Imaging Library functions and constants are static members of the class. The only difference in calling a function or using a constant lies in prefacing each Aurora Imaging Library function and each Aurora Imaging Library constant with "AIL.", assuming you have included the library in your code as outlined in step 4 of the [Aurora Imaging Library inclusion procedure for .NET](S03_Building_a_NET_application_using_AIL.md) subsection.

> **Code example:** [userguide.ail_in_net.simplecpp](userguide.ail_in_net.simplecpp)

> **Code example:** [userguide.mil_in_net.simplesharp1.cs](userguide.ail_in_net.simplesharp1.cs)

## Disabling compiler warnings in

If you recompile an Aurora Imaging Library application with deprecated constants and functions, compiler warnings will now typically be generated. Deprecated constants and functions should no longer be used since they could be removed in a future major release of Aurora Imaging Library (for example, Aurora Imaging Library 11). All code that produces warnings about deprecated features should be updated to the code's current equivalent. For more information on how to update deprecated constants and functions to the code's current equivalent, see [Compiler warnings](../C02_Building_an_application/S05_Compiling_and_linking.md).

Deprecated Aurora Imaging Library functions and constants in the  .NET environment are denoted with the **Obsolete** attribute. The **Obsolete** attribute is a  .NET feature that marks a program entity as one that is no longer recommended for use. Whenever you use a program entity marked as **Obsolete**, the compiler will generate a warning or an error. You can disable warnings about **Obsolete** features by following the procedures below. Note, however, that code with deprecated features might not compile with a future major release of Aurora Imaging Library if you disable warnings and do not replace the code with the current standard.

### Procedure for disabling compiler warnings in C#

There are two possible ways to disable compiler warnings when building an application in C#: one that disables compiler warnings caused by the **Obsolete** attribute for an encapsulated piece of code, and one that disables compiler warnings caused by the **Obsolete** attribute across the entire project.

To disable compiler warnings for a portion of your C# application (for example, a block of code that should be updated to the code's current equivalent), you can encapsulate your code with the `#pragma warning` directive. This is preferable to disabling the compiler warnings globally, since it will disable the compiler warnings caused by the **Obsolete** attribute for only a block of Aurora Imaging Library code rather than your entire project. Encapsulate your code between the following preprocessor directives:

> **Code example:** [userguide.mil_in_net.simplesharp2.cs](userguide.ail_in_net.simplesharp2.cs)

To disable compiler warnings caused by the **Obsolete** attribute across the entire project, perform the following steps. Note that this will disable all compiler warnings caused by the **Obsolete** attribute, including constants and functions not related to Aurora Imaging Library.

1. Open the **Project Properties** dialog box in Microsoft Visual Studio, by either selecting the **'projectName' Properties** item in the **Project** menu, or by right-clicking on the project's name in the **Solution Explorer** pane and selecting **Properties**.
2. In the **Build** tab of the **Project Properties** dialog box, add **0612;0618** to the **Suppress warnings** text box.
3. Repeat step 2 for each build configuration and platform combination.
