---
doctype: UserGuide
part: "Miscellaneous"
chapter: The_AIL_function_development_module
section: The_AIL_function_development_module
module_tag: func
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / function / The AIL function development module"
---

# Aurora Imaging Library Function Development module

The Aurora Imaging Library Function Development module allows programmers to define custom (user-defined) Aurora Imaging Library functions to extend Aurora Imaging Library's functionality. Using this module, you can implement functions or scripts, and integrate them directly into Aurora Imaging Library, where they behave like standard Aurora Imaging Library functions (for example, respecting error handling and tracing). The Aurora Imaging Library Function Development module also allows programmers to group related user-defined Aurora Imaging Library functions together into user-defined modules. This is useful to create high-level packages on top of Aurora Imaging Library and to extend Aurora Imaging Library's function set (for example, by adding new functions with specialized algorithms).

Using a user-defined Aurora Imaging Library function, you can integrate functions or scripts written in C, C++, C#, or Python into Aurora Imaging Library. You can also integrate functions/scripts written in other  .NET languages or language versions (for Python) but you will need to develop an additional DLL to use these other languages. User-defined Aurora Imaging Library functions based on C or C++ functions (embedded in a DLL) are referred to as **C-based user-defined Aurora Imaging Library functions**. User-defined Aurora Imaging Library functions based on a function or script that require an interpreter or the Microsoft .NET framework to be present at runtime are referred to as **script-based user-defined Aurora Imaging Library functions**.

To allow for the remote execution of user-defined Aurora Imaging Library functions and the ability to run scripts, user-defined Aurora Imaging Library functions are composed of two parts: a caller function and a callee function/script. The caller function performs the parameter registration and transfers the information to the callee function/script. The callee function/script receives the parameter information and performs the data processing operations of the user-defined Aurora Imaging Library function. When a remote processor is available, the callee function/script of a user-defined Aurora Imaging Library function that meets certain criteria can be executed remotely. For the purposes of this chapter, the processor that executes the caller function is referred to as the caller processor and the processor which executes the callee function/script is referred to as the callee processor. When no remote processor is available, the caller and callee processors are the Host processor.

The Aurora Imaging Library Function Development module also provides a framework in which you can define your own objects and associate them with Aurora Imaging Library identifiers. Such associations can be useful when designing objects, within a user-defined Aurora Imaging Library function, whose data members should not be accessed directly and whose data structure has specific requirements.

When designing a function, or set of functions, the Aurora Imaging Library Function Development module puts at your disposal all the tools necessary to create custom error codes and error messages that are treated as regular Aurora Imaging Library errors which can in turn be managed using the [`Mapp...`](../../Reference/app/MappAlloc.md) functions.
