---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Custom_data_types_extensions_and_portability_functions
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Custom data types extensions and portability functions"
---

# Aurora Imaging Library custom data types, void pointers, extensions, and portability functions

Aurora Imaging Library features data types, constants, and functions that can improve your application's portability and operating system independence. In addition, in C++, Aurora Imaging Library supports the standard string and vector objects to simplify handling of strings and arrays, respectively. In C++ 11 and later you can also use Aurora Imaging Library smart identifiers to automatically manage the lifespan of Aurora Imaging Library objects.

## Aurora Imaging Library custom data types

In addition to the standard data types available in C and C++, Aurora Imaging Library introduces the following predefined data types, to ensure better compatibility and portability of Aurora Imaging Library applications under different operating systems and to manage Aurora Imaging Library objects:

- _AIL_ID_. Specifies a value that is an Aurora Imaging Library identifier. The _AIL_ID_ data type is used to identify objects (for example, buffers, systems, and contexts) which are allocated using Aurora Imaging Library within an application.
- _AIL_UNIQUE_..._ID_ (available only when using in C++ 11 or higher). Specifies a value that is an Aurora Imaging Library smart identifier. The smart identifier data type is used to identify, and manage the lifetime of, objects which are allocated using Aurora Imaging Library within an application. A smart identifier is similar to a_std::unique_ptr_, except that it owns an Aurora Imaging Library object instead of owning allocated memory. For more information about smart identifiers, see [Aurora Imaging Library smart identifiers](S16_Custom_data_types_extensions_and_portability_functions.md).
- _AIL_INT_. Specifies a value that is an integer. The _AIL_INT_ data type is used as a supplement to the more conventional _long_ and _int_ data types. The _AIL_INT_ data type is a 64-bit value on a 64-bit operating system.
- _AIL_UINT_. Specifies a value that is an unsigned integer. Its data depth is the same as that of the AIL_INT data type.
- _AIL_DOUBLE_. Specifies a value that is a double precision, floating-point number.
- _AIL_FLOAT_. Specifies a value that is a single precision, floating-point number.
- _AIL_UUID_. Specifies a universally unique identifier that is used for identification purposes across unrelated systems and applications. For example, an _AIL_UUID_ value is used as an entry key in a dataset context for the Aurora Imaging Library Classification module.

In addition to the _AIL_INT_ and _AIL_UINT_ data types, Aurora Imaging Library also has signed and unsigned, fixed length versions of these data types: AIL_INT8, AIL_UINT8, AIL_INT16, AIL_UINT16, AIL_INT32, AIL_UINT32, AIL_INT64, and AIL_UINT64.

Aurora Imaging Library also introduces the following predefined data types, to ensure better compatibility and portability of applications:

- _AIL_TEXT_CHAR_. Specifies an array of characters. The _AIL_TEXT_CHAR_ data type is used as a replacement for the more conventional _char_ data type.
- _AIL_STRING_. Specifies a string object. This object can be used wherever an array of _AIL_TEXT_CHAR_, address of an _AIL_TEXT_PTR_, or address of an _AIL_CONST_TEXT_PTR_should be used. However, it should be noted that AIL_STRING cannot be used when dealing with an array of multiple strings. The_AIL_STRING_data type is only available in C++. A variable of this type can be manipulated like a standard C++ string (_std::string_).
- _AIL_STRING_STREAM_. Specifies a string stream object. The_AIL_STRING_STREAM_data type is only available in C++. A variable of one of this type can be manipulated like a standard C++ string stream (_std::stringstream_).
- _AIL_TEXT_PTR_. Specifies a pointer to an array of characters. The _AIL_TEXT_PTR_ data type is used as a replacement for the more conventional _char*_ data type.
- _AIL_CONST_TEXT_PTR_. Specifies a pointer to an array of constant characters. The data type is used as a replacement for the more conventional _const char*_ data type.
- _AIL_WINDOW_HANDLE_. Specifies the handle to a window. The data type is used as a replacement for the more conventional _HWND_ data type in Windows and the _Window_ data type when using the _Xlib_ library in Linux.

> **Note:** There is a macro called **AIL_TEXT** which converts the provided string to the proper encoding (ASCII or Unicode) for the application. Its syntax is: `AIL_TEXT("Text to be converted.")`.

> **Note:** There is a macro called **M_TO_STRING** which converts the provided numeric data to a string with the proper encoding (ASCII or Unicode) for the application. Its syntax is: `M_TO_STRING(number)`. This macro is only available when using C++11 (or later), and maps directly to _std::to_string_ or _std::to_wstring_ (depending on your project settings).

Aurora Imaging Library also features several standard objects in C++ to simplify the handling of strings and arrays, respectively. For more information, see [Strings and arrays](S16_Custom_data_types_extensions_and_portability_functions.md).

## Void pointers

In Aurora Imaging Library, there are functions, predominantly [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) and [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md) functions, where one (or more) of their parameters takes a void pointer. This allows for a more general API where one function (within a module) can retrieve or set information of different data types (for example, [`MdispInquire`](../../Reference/disp/MdispInquire.md) can be used to retrieve the width of the display (_AIL_INT_) and the title of the display (array of _AIL_TEXT_CHAR_). The expected data type for these void pointer parameters is determined in one of two ways:

- The data type is intrinsically determined by the setting of another parameter.
  For example, consider the [`MbufInquire`](../../Reference/buf/MbufInquire.md) function. When the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter of this function is set to [`M_ALLOCATION_OVERSCAN_SIZE`](../../Reference/buf/MbufInquire.md), an _AIL_INT_ value is returned, and, therefore, the [`UserVarPtr`](../../Reference/buf/MbufInquire.md) parameter expects the address of a variable of type _AIL_INT_. However, when the [`InquireType`](../../Reference/buf/MbufInquire.md) parameter is set to [`M_ANCESTOR_ID`](../../Reference/buf/MbufInquire.md), the [`UserVarPtr`](../../Reference/buf/MbufInquire.md) parameter expects the address of a variable of type _AIL_ID_.
  > **Code example:** [userguide.building_an_application.expected_data_type01](userguide.building_an_application.expected_data_type01)
  In the Aurora Imaging Library Help, a parameter association table is used to list the independent parameter setting and the expected data type of the address to pass to its dependent void pointer parameter(s); the data type is presented in green text. If the expected data type is not that of an array, the expected data type will be shown inline. However, if it is of an array, or if, given the setting, one of multiple data types might be expected, you will be able to expand the data type information to see in which situation one data type is expected over the other, and in the case of an array, to see how large the array must be.
- The data type is determined by a modifier (for example, [`M_TYPE_AIL_DOUBLE`](../../Reference/3dmap/M3dmapGetResult.md)) added to the setting of another parameter.
  For example, consider the [`MblobInquire`](../../Reference/blob/MblobInquire.md) function. The [`InquireType`](../../Reference/blob/MblobInquire.md) parameter specifies type of information about which to inquire. The [`UserVarPtr`](../../Reference/blob/MblobInquire.md) parameter specifies the address at which to write the requested information. The [`UserVarPtr`](../../Reference/blob/MblobInquire.md) parameter is a void pointer, and by default requires that you pass it the address of an _AIL_DOUBLE_ value. However, if [`M_TYPE_AIL_INT`](../../Reference/blob/MblobInquire.md) is added to the setting of [`InquireType`](../../Reference/blob/MblobInquire.md), the [`UserVarPtr`](../../Reference/blob/MblobInquire.md) parameter expects the address of a variable of type _AIL_INT_.
  > **Code example:** [userguide.building_an_application.expected_data_type02](userguide.building_an_application.expected_data_type02)
  The ability to modify the expected data type is typically only available for processing or analysis modules' [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) functions and [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md) functions. See the function(s)' reference description to establish if the data type can be changed using a modifier.

The general description of a void pointer parameter includes a complete list of the possible data types that the parameter can take. This list also includes the data types that are expected when using a modifier ([`M_TYPE_...`](../../Reference/im/MimInquire.md)). For more information concerning void parameters in languages other than C/C++, see the appropriate language-specific chapter.

## Aurora Imaging Library smart identifiers

Optionally, when you allocate an Aurora Imaging Library object, you can specify to return an Aurora Imaging Library smart identifier (_AIL_UNIQUE_..._ID_) instead of a standard Aurora Imaging Library identifier (_AIL_ID_). To do so, pass[`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md) to the allocation function instead of the address of a variable in which to write the identifier. The Aurora Imaging Library smart identifier owns the object and manages its lifetime; it frees the Aurora Imaging Library object automatically when the smart identifier goes out of scope, is destroyed, or is assigned a new value. A smart identifier can be passed to all Aurora Imaging Library functions as though it were a standard identifier (_AIL_ID_) except for freeing functions (such as [`MbufFree`](../../Reference/buf/MbufFree.md)) or parameters that take an array of Aurora Imaging Library identifiers (such as the [`DestContainerOrImageBufArrayPtr`](../../Reference/dig/MdigProcess.md)parameter of[`MdigProcess`](../../Reference/dig/MdigProcess.md)).

> **Note:** Smart identifiers are only available when using C++11 (or later).

Unlike a standard Aurora Imaging Library identifier (_AIL_ID_), which can refer to any type of Aurora Imaging Library object, you use different types of Aurora Imaging Library smart identifiers for different types of objects. For example, when you allocate a system using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md), you must store the returned value in an _AIL_UNIQUE_SYS_ID_. Attempting to assign a smart identifier to a variable of the incorrect type will cause a compile time error. To learn what type of Aurora Imaging Library smart identifier to use for an Aurora Imaging Library object, refer to the [`M_UNIQUE_ID`](../../Reference/sys/MsysAlloc.md)setting in the function that allocates the object (for example, [`MbufAllocColor`](../../Reference/buf/MbufAllocColor.md) for a color buffer).

> **Note:** Note that using an Aurora Imaging Library smart identifier does not guarantee type safety. For example, passing an _AIL_UNIQUE_SYS_ID_ smart identifier to a function that requires a buffer (such as [`MbufInquire`](../../Reference/buf/MbufInquire.md)) does not cause a compile time error, because the_AIL_UNIQUE_SYS_ID_is implicitly cast to the data type_AIL_ID_ (which can refer to any type of Aurora Imaging Library object).

The following code snippet shows two functions that grab and save an image. The first function uses_AIL_ID_ variables; the second function uses _AIL_UNIQUE_..._ID_ variables.

> **Code example:** [userguide.building_an_application.ail_unique_ids](userguide.building_an_application.ail_unique_ids)

To extend the lifespan of an Aurora Imaging Library object owned by an Aurora Imaging Library smart identifier and use the object beyond the scope of the function in which you allocated it, return the smart identifier from your function. Ownership of the Aurora Imaging Library object is transferred to the smart identifier in which you store the returned value.

If you assign a new value to an Aurora Imaging Library smart identifier, the Aurora Imaging Library object that the smart identifier previously referred to is automatically freed. For example, if you use an _AIL_UNIQUE_BUF_ID_variable to store the identifier of a 1-band buffer, and you need to reallocate that buffer with different dimensions, you only need to store the identifier returned by[`MbufAlloc2d`](../../Reference/buf/MbufAlloc2d.md) in the same variable; this frees the existing buffer automatically.

> **Note:** To free an existing Aurora Imaging Library object without allocating a new Aurora Imaging Library object, assign the value`nullptr`to the smart identifier.

In addition to [`M...Alloc`](../../Reference/3dmap/M3dmapAlloc.md) functions, most other functions that allocate an Aurora Imaging Library object can return an Aurora Imaging Library smart identifier (for example, [`M...AllocResult`](../../Reference/3dmap/M3dmapAllocResult.md),[`M...Copy/CopyResult`](../../Reference/3dmap/M3dmapCopy.md), and [`M...Restore`](../../Reference/3dmap/M3dmapRestore.md) functions). An exception to this rule is [`M...Stream`](../../Reference/3dmap/M3dmapStream.md) with [`M_RESTORE`](../../Reference/3dmap/M3dmapStream.md); this operation cannot return a smart identifier.

> **Note:** Note that Aurora Imaging Library inquire functions that can return an _AIL_ID_ never return a smart identifier. For example, you can associate a LUT buffer with a display by passing its smart identifier to [`MdispLut`](../../Reference/disp/MdispLut.md). However, if you later inquire the Aurora Imaging Library identifier of the associated LUT buffer (using[`MdispInquire`](../../Reference/disp/MdispInquire.md)with [`M_LUT_ID`](../../Reference/disp/MdispInquire.md)), the underlying standard Aurora Imaging Library identifier (_AIL_ID_) of the LUT buffer is returned. You must not pass this Aurora Imaging Library identifier to a freeing function.

## Strings and arrays

Strings in Aurora Imaging Library are typically represented using arrays of type _AIL_TEXT_CHAR_, or an _AIL_TEXT_PTR_or _AIL_CONST_TEXT_PTR_. In C++, you can instead use an _AIL_STRING_.

_AIL_STRING_ is resized automatically when calling an [`M...Inquire`](../../Reference/3dmap/M3dmapInquire.md) or [`M...GetResult`](../../Reference/3dmap/M3dmapGetResult.md)function that returns a string. This makes it easier to inquire or get such a result, because you don't need to inquire the string length, allocate an array of this size, and then inquire/get the string.

It should be noted that AIL_STRING cannot be used when dealing with an array of strings, that is, if the function is returning multiple null-terminated strings into an array.

In C++, all Aurora Imaging Library functions that take an input or output array as a parameter have overloaded versions that allow you to pass a reference to a standard C++ vector (_std::vector_) to that parameter instead. Some output parameters allow you to pass a combination constant (for example; [`M_TYPE_...`](../../Reference/im/MimInquire.md)) to cast the returned data to a required data type. When using a vector, don't pass the combination constant; just pass a vector of the required-type. Aurora Imaging Library will automatically return the data cast to the type of the vector.

Note that for these output parameters, the vector can be the expected data type of the parameter or one of the listed cast types for that output parameter (for example [`M_TYPE_...`](../../Reference/im/MimInquire.md)).

Typically, when using an input vector, Aurora Imaging Library will automatically determine the array size based on the size of the vector, with the exception of cases where only a subset of the vector should be specified (for example, [`MbufPut1d`](../../Reference/buf/MbufPut1d.md)).

> **Note:** When using a vector for an output parameter, it is not necessary to determine the size of the array before calling a function, since Aurora Imaging Library will resize the output vector automatically.

> **Note:** A vector overload function might call more than one Aurora Imaging Library function. Vector overload functions might call more Aurora Imaging Library functions in debug mode than in release mode, to validate parameters. When using vector overload functions in debug mode, you might notice an increased performance slowdown in addition to the typical performance slowdown when running in debug mode.

The following code snippet illustrates how to use a standard vector (_std::vector_) overload function for input and output array parameters.

> **Code example:** [userguide.building_an_application.vector_overloads01](userguide.building_an_application.vector_overloads01)

The following code snippet illustrates how to use a standard vector (_std::vector_) overload function that has a parameter defining the size of the array. In this case, [`MgraDots`](../../Reference/gra/MgraDots.md) is called to draw dots in an image. [`M_DEFAULT`](../../Reference/gra/MgraDots.md) is passed to the [`NumberOfDots`](../../Reference/gra/MgraDots.md) parameter. Aurora Imaging Library will automatically determine the appropriate size based on the number of elements in the vector. Refer to individual function descriptions for more information on what to pass to an array size parameter.

> **Code example:** [userguide.building_an_application.vector_overloads02](userguide.building_an_application.vector_overloads02)

The following code snippet illustrates how to instantiate and use a standard vector (_std::vector_) when calling a function where a casting combination flag would typically be required.

> **Code example:** [userguide.building_an_application.vector_overloads03](userguide.building_an_application.vector_overloads03)

## Aurora Imaging Library directory constants

Aurora Imaging Library provides several string constants for accessing special directories, such as the default Aurora Imaging Library installation, contexts, and images directories. These constants are guaranteed to be correct for the local computer, even if Aurora Imaging Library is not installed in the default location. You can increase the portability of your Aurora Imaging Library application by prepending these constants to strings that contain file paths, instead of using hard-coded paths which might not be correct for all computers on which your application will run.

> **Note:** Typically, these directories are only used when accessing content related to Aurora Imaging Library examples (except for the operating system's temporary directory).

The following constants are provided:

- **M_INSTALL_DIR**. The path to the root directory of the local Aurora Imaging Library installation. This constant is equivalent to using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_AIL_DIRECTORY_INSTALL`](../../Reference/app/MappInquire.md).
  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version Number>\Library_.
  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/library_.
- **M_IMAGE_PATH**. The path to the local Aurora Imaging Library images directory. This constant is equivalent to using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_AIL_DIRECTORY_IMAGES`](../../Reference/app/MappInquire.md).
  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version Number>\Images_.
  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/library/images_.
- **M_CONTEXT_PATH**. The path to the local Aurora Imaging Library contexts directory. This constant is equivalent to using [`MappInquire`](../../Reference/app/MappInquire.md) with [`M_AIL_DIRECTORY_CONTEXTS`](../../Reference/app/MappInquire.md).
  By default on a Windows platform, this path is _C:\Program Files\Aurora Imaging Library\&lt;version Number>\Contexts_.
  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/library/contexts_.
- **M_USER_DLL_DIR**. The path to the Aurora Imaging Library folder that stores the DLLs which contain user-defined Aurora Imaging Library callee functions. For more information on user-defined Aurora Imaging Library functions, see [Aurora Imaging Library Function Development module](../C68_The_AIL_function_development_module/S01_The_AIL_function_development_module.md).
  By default on a Windows platform, this path is _C:\Users\Public\Documents\Aurora Imaging Library\&lt;version Number>\Library\UserDLL _.
  By default on a Linux platform, this path is _/opt/aurora_imaging_library/&lt;version>/library/lib _. This folder is only accessible by users in the Aurora Imaging Library administrator group.
- **M_TEMP_DIR**. The path to the operating system's temporary directory. On a Windows platform, this is equivalent to the environment variable `%TEMP%`.
  By default on a Windows platform, this path is _C:\Users\&lt;USERNAME>\AppData\Local\Temp_.
  By default on a Linux platform, this path is _/tmp_.
  > **Note:** On a Linux platform, the contents of the temporary folder is erased when the computer is restarted.

## M_AIL_USE_SAFE_TYPE extension

When using void pointers it is possible to make a mistake and pass a value with the wrong data type. These errors can cause unexpected behavior such as: stack corruption, array overflow, uninitialized returned data, and segmentation faults. Aurora Imaging Library includes the **M_AIL_USE_SAFE_TYPE** extension to help you find these errors.

> **Note:** Note that the extension is also used when validating or attempting to cast a standard vector (_std::vector_) or _AIL_STRING_type parameter.

When the **M_AIL_USE_SAFE_TYPE** extension is enabled, an error message is returned if the data type of the variable whose address is passed to a void pointer parameter does not match the expected data type. By default, the **M_AIL_USE_SAFE_TYPE** extension is enabled when compiling in debug mode.

> **Note:** Note that the **M_AIL_USE_SAFE_TYPE** extension is only available if you are using a C++ compiler under Windows. It is not available under Linux.

You might want to skip the verification performed by the **M_AIL_USE_SAFE_TYPE** extension if:

- The data type of a value is unknown at the time the function is called. This typically occurs when the Aurora Imaging Library function is wrapped inside another function in the application. For example:
  > **Code example:** [userguide.building_an_application.wrapper_function](userguide.building_an_application.wrapper_function)
- The data type of the value passed to a function is not one of the expected data types accepted by a function (for example, when a custom data type is defined in the application).
  > **Code example:** [userguide.building_an_application.user_defined_data_type](userguide.building_an_application.user_defined_data_type)

To disable the **M_AIL_USE_SAFE_TYPE** extension entirely, include the `#define M_AIL_USE_SAFE_TYPE 0` statement before the `#include &lt;AIL.h>` statement in the application code. When the extension is excluded, all Aurora Imaging Library overloads are called without any data type verification.

## Portability functions

In addition to the functions available in the C/C++ standard library, Aurora Imaging Library introduces the following functions to ensure better compatibility and portability of applications:

| Function in the C/C++ standard library | Portable Aurora Imaging Library version of the function | Description |
| --- | --- | --- |
| `abs` | `AIL_INT MosAbs(AIL_INT SignedValue)` | Calculates the absolute value of "SignedValue". |
| `N/A` | `AIL_INT MosConsoleClear()` | Clears the contents of the console. |
| `N/A` | `AIL_INT MosConsoleGetPosition(AIL_INT *PositionXPtr, AIL_INT *PositionYPtr)` | Retrieves the X- and Y-positions of the console's cursor. |
| `N/A` | `AIL_INT MosConsoleInit()` | Initializes resources necessary for console-related functions. |
| `N/A` | `AIL_INT MosConsoleRefresh()` | Refreshes the console display. |
| `N/A` | `AIL_INT MosConsoleRelease()` | Releases resources used by console-related functions. |
| `N/A` | `AIL_INT MosConsoleResize(AIL_INT NumColumns, AIL_INT NumLines)` | Resizes the console and its display area. |
| `N/A` | `AIL_INT MosConsoleScroll(AIL_BOOL Scrollable)` | Enables or disables console scrolling. |
| `N/A` | `AIL_INT MosConsoleSetPosition(AIL_INT PositionX, AIL_INT PositionY)` | Sets the X- and Y-positions of the console's cursor. |
| `getch` | `AIL_INT MosGetch()` | Reads a character or keystroke from the standard input (keyboard). After a keystroke is read, the application instantly resumes. |
| `getchar` | `AIL_INT MosGetchar()` | Reads a character from the standard input (keyboard). After a keystroke is read, the application waits for the `Enter` key to be pressed before resuming. |
| `kbhit` | `AIL_INT MosKbhit()` | Returns a non-zero value once a keystroke is read. |
| `main` | `AIL_INT MosMain()` | Specifies the first function to execute. |
| `printf` | `AIL_INT MosPrintf(AIL_CONST_TEXT_PTR FormatString, Arg1, Arg2, …) ` | Prints formatted data to the console. The `FormatString` parameter is an _AIL_TEXT_ string that can optionally contain format specifiers that will be replaced with the subsequent arguments (`Arg1, Arg2, ...`). Format specifiers are introduced by the "%" character within the `FormatString` value. For example, the following code: `MosPrintf(AIL_TEXT("A character: %c and an integer: %d"), 'b', 523);` outputs the following sentence to the console: `A character: b and an integer: 523`. |
| `sleep` | ` void MosSleep(AIL_INT WaitTime)` | Suspends an application for the specified period of time (in milliseconds). |
| `sprintf_s` | `AIL_INT MosSprintf(array of type AIL_TEXT_CHAR DstArrayPtr, AIL_INT Size, AIL_CONST_TEXT_PTR FormatString, Arg1, Arg2, …)` | Writes formatted data to a string. The `DstArrayPtr` parameter specifies the array of characters in which to store the formatted data as a string. The `Size`is the maximum size of the array of characters, including the NULL-termination character. The `FormatString` parameter is an _AIL_TEXT_ string that can optionally contain format specifiers that will be replaced with the subsequent arguments (`Arg1, Arg2, ...`). Format specifiers are introduced by the "%" character within the `FormatString` value. For example, the following code: `MosSprintf(String, AIL_TEXT("A character: %c and an integer: %d."), 'b', 523);` saves the following sentence in `DstArrayPtr`: `A character: b and an integer: 523.` |
| `strcat_s` | `AIL_TEXT_PTR MosStrcat(AIL_TEXT_PTR DstString, AIL_INT Size, AIL_CONST_TEXT_PTR SrcString)` | Joins strings. |
| `strcmp` | `AIL_INT MosStrcmp(AIL_CONST_TEXT_PTR String1, AIL_CONST_TEXT_PTR String2)` | Compares two strings. |
| `strcpy_s` | `AIL_TEXT_PTR MosStrcpy(AIL_TEXT_PTR DstString, AIL_INT Size, AIL_CONST_TEXT_PTR SrcString)` | Copies characters of one string to another. |
| `strlen` | `AIL_INT MosStrlen(AIL_CONST_TEXT_PTR String)` | Calculates and returns the length of a string. |
| `strlwr` | `AIL_TEXT_PTR MosStrlwr(AIL_TEXT_PTR String)` | Converts a string to lowercase. |
| `strupr` | `AIL_TEXT_PTR MosStrupr(AIL_TEXT_PTR String)` | Converts a string to uppercase. |
| `UuidCreate` | `AIL_INT64 MosUuidCreate(AIL_UUID* AilUuidPtr)` | Creates a new UUID. |
| `UuidFromString` | `AIL_INT64 MosUuidFromString(AIL_CONST_TEXT_PTR InputUuidTextPtr, AIL_UUID* OutputUuidPtr)` | Converts a string to a UUID. |
| `UuidToString` | `AIL_INT64 MosUuidToString(const AIL_UUID Uuid, AIL_TEXT_PTR OutputTextPtr)` | Converts a UUID to a string. |

For more information on the above-mentioned functions of the C/C++ standard library, refer to MSDN.
