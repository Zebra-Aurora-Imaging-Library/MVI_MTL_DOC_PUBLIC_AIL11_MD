---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: AIL_with_NET_example
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / AIL with NET example"
---

# Aurora Imaging Library with .NET example

The two snippets below perform identical procedures, but the first is written in C++, while the second is written in C#. The comments in the C# code identify the Aurora Imaging Library-specific differences.

> **Code example:** [userguide.ail_in_net.customtypes_c](userguide.ail_in_net.customtypes_c)

> **Code example:** [userguide.mil_in_net.customtypes.cs](userguide.ail_in_net.customtypes.cs)

## Compiling Aurora Imaging Library C# examples under Linux

Compiling Aurora Imaging Library C# examples under Linux requires that .NET is installed in the_/usr/share/dotnet_ directory or that the `DOTNET_ROOT` environment variable is exported.

To compile all C# examples, enter the following commands:

```
 cd $AILDIR11/examples
 make all_dotnet
```
