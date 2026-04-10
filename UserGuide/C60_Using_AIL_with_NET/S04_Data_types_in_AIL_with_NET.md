---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIL_with_NET
section: Data_types_in_AIL_with_NET
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / NET / Data types in AIL with NET"
---

# Data types in Aurora Imaging Library with

Aurora Imaging Library uses many custom data types, but only _M_ID_ and _M_INT_ are included in both the AIL.NET wrapper and Aurora Imaging Library. For every other data type, an equivalent data type needs to be used instead.

## M_ID and M_INT

The _M_ID_ and _M_INT_ data types have been included in the AIL.NET wrapper, and so are usable when writing C# code. Other data types require a language-specific equivalent. The _M_ID_ and _M_INT_ data types implement the IComparable and the IConvertible interfaces. Please refer to the .NET framework's documentation for more information on these interfaces.

The _M_INT_ type is a structure provided to enable writing portable code. The _M_INT_ type implements explicit conversion operators for the basic .NET types. This allows you to perform basic arithmetic and logical operations between an _M_INT_ and any other numeric type, such as a _long_, _double_, or _byte_.

> **Note:** Note that in a 64-bit operating system, _M_INT_ behaves like a _long_.

## Data type equivalents

The following chart explains the data type equivalents between Aurora Imaging Library custom types, .NET types, and Visual C# types. When writing Aurora Imaging Library code in C#, replace the Aurora Imaging Library custom data type with the language specific equivalent data type in the chart. Note that _M_ID_and _M_INT_ do not require an equivalent and can be used directly in C# code. Also note that the .NET type can be used in any .NET language, including C#.

There are no .NET equivalents for the **AIL_TEXT** macro. When a function's parameter takes one of the macros in C, pass what you would to the macro directly to the function's parameter in  .NET, without any reference to the macro itself. An example can be found in [Aurora Imaging Library with .NET example](S10_AIL_with_NET_example.md).

| Aurora Imaging Library Custom Type | .NET type | Visual C# type |
| --- | --- | --- |
| _M_ID_ | _M_ID_[^_1] | _M_ID_[^_1] |
| _M_INT_ | _M_INT_[^_1] | _M_INT_[^_1] |
| _M_UINT (32-bit) _ | System.UInt32 | uint |
| _M_UINT (64-bit)_ | System.UInt64 | ulong |
| _M_DOUBLE_ | System.Double | double |
| _M_INT8_ | System.SByte | sbyte |
| _M_UINT8_ | System.Byte | byte |
| _M_INT16_ | System.Int16 | short |
| _M_UINT16_ | System.UInt16 | ushort |
| _M_INT32_ | System.Int32 | int |
| _M_UINT32_ | System.UInt32 | uint |
| _M_INT64_ | System.Int64 | long |
| _M_UINT64_ | System.UInt64 | ulong |
| _M_TEXT_PTR_ | System.String | string |
| _M_FLOAT_ | System.Single | float |
| _M_BUFFER_INFO_ | System.IntPtr | System.IntPtr |
| _M_FPGA_CONTEXT_ | System.IntPtr | System.IntPtr |

[^_1]: This data type can be used in this language only with the AIL.NET wrapper.
