---
doctype: Reference
module: app
function: MappFileOperation
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappFileOperation"
---

# MappFileOperation

> Perform a file operation or file search on a local or a remote computer.

## Syntax

```c
void MappFileOperation(
    AIL_ID             Comp1ContextAppId,  //in
    AIL_CONST_TEXT_PTR Comp1FileName,      //in
    AIL_ID             Comp2ContextAppId,  //in
    AIL_CONST_TEXT_PTR Comp2FileName,      //in
    AIL_INT64          Operation,          //in
    AIL_INT64          OperationFlag,      //in
    void *             OperationDataPtr    //out
)
```

## Description

This function can copy or move a file from one computer to another, including your local computer as either source or destination. This function can also remotely delete a file, create or delete a directory, search a directory for files with a specific file name (wildcards are supported), or execute a program on another computer. When executing a program, you can choose to wait for the remotely-run program to finish before proceeding with the local program, or start the remote program and immediately continue with the local program.

This function is typically used to perform operations on remote computers, but also works on the local computer.

When using this function to manipulate files (for example, moving, copying, or executing them), you must set the [`Comp1FileName`](../../Reference/app/MappFileOperation.md)parameter (and optionally the[`Comp2FileName`](../../Reference/app/MappFileOperation.md)parameter) to a file name or full file path. If you specify only a file name or partial file path (without drive letter), the working directory of the application is assumed as the location of the file (or as the prefix of the partial file path).

When using this function to search for files ([`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md) and [`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md) + [`M_NB_ELEMENTS`](../../Reference/app/MappFileOperation.md)), you must specify a file name search pattern. The search might find more than one matching file. You must specify which file name to return by setting the [`OperationFlag`](../../Reference/app/MappFileOperation.md) parameter to the file's index in the search results. You can retrieve the total number of search results using [`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md) + [`M_NB_ELEMENTS`](../../Reference/app/MappFileOperation.md).

## Parameters

### `Comp1ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the Aurora Imaging Library application context to use. The computer on which this application context has been allocated is referred to as either computer 1, or the source computer.

*For specifying the application context on Computer 1*

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application context identifier` | Specifies the identifier of an application context.

Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappFileOperation.md). |

### `Comp1FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the file on the target or source computer to use for the file operation, or the file name search pattern to use.

*For specifying the file on Computer 1 or the file name search pattern to find files on Computer 1*

| Value | Description |
| --- | --- |
| `"FileName"` | Specifies the file name, and optionally the full file path drive and directory, of the file on the target or source computer to use for the operation. If you specify only the file name, the file is assumed to be in the working directory of the application. If the file is elsewhere on the computer, a path is required. For example, _"myFile"_ or _"C:\myDirectory\myFile"_.

The file name is always local to computer 1.

Optionally, the file name and/or path can be followed by executable arguments (command-line parameters) for the [`M_FILE_EXECUTE`](../../Reference/app/MappFileOperation.md) and [`M_FILE_DISPATCH`](../../Reference/app/MappFileOperation.md) operations. For example, _"C:\myDirectory\myProgram -c -r"_. |
| `"FileNameSearchPattern"` | Specifies the file name search pattern to use. This must include the path to the root folder of the search, and can also include the asterisk (`*`) and question mark (`?`) wildcard characters. An asterisk (`*`) matches 0 or more characters, while a question mark (`?`) matches exactly 1 character.

For example, the search string _"C:\DirectoryToSearch\Year_188?\*Night*.bmp"_ specifies to search only the immediate subdirectories of _C:\DirectoryToSearch\_that have the name _Year188_ plus exactly one additional character. Examples of files that would be found with this string include:

- _C:\DirectoryToSearch\Year_1889\The_Starry_Night.bmp_.
- _C:\DirectoryToSearch\Year_1888\Starry_Night_Over_the_Rhone.bmp_.
- _C:\DirectoryToSearch\Year_188W\Night.bmp_.

> **Note:** The string must not end in a backslash (`\`). To find all file names in a directory, follow the path with an asterisk (`*`) (for example,_"C:\DirectoryToSearch\*"_).

The file name search pattern is always local to computer 1. |

### `Comp2ContextAppId` *(in, AIL_ID)*

Specifies the identifier of the application context that determines where to copy or move the file specified using [`Comp1FileName`](../../Reference/app/MappFileOperation.md), during an [`M_FILE_COPY`](../../Reference/app/MappFileOperation.md), [`M_FILE_COPY_AIL_DLL`](../../Reference/app/MappFileOperation.md), [`M_FILE_COPY_AIL_USER_DLL`](../../Reference/app/MappFileOperation.md), or [`M_FILE_MOVE`](../../Reference/app/MappFileOperation.md)operation. The computer on which this application context has been allocated is referred to as either computer 2, or the destination computer.

### `Comp2FileName` *(in, AIL_CONST_TEXT_PTR)*

Specifies the directory (and optionally a new file name) on computer 2 to which to copy the file specified using [`Comp1FileName`](../../Reference/app/MappFileOperation.md). If this parameter is set to a file name and not a full path, the file is copied or moved to the working directory of the destination application.

### `Operation` *(in, AIL_INT64)*

Specifies the operation to perform on the specified file, or the type of information to return from the search.

### `OperationFlag` *(in, AIL_INT64)*

Specifies additional settings for the operation or search. Set this parameter to `M_DEFAULT` if not used.

### `OperationDataPtr` *(out, *void)*

Specifies the address of the user data that is used by some operations. Set this parameter to `M_NULL` if not used.

## Parameter Associations

### For specifying the file operation

The following [`Operation`](../../Reference/app/MappFileOperation.md) and corresponding [`Comp2ContextAppId`](../../Reference/app/MappFileOperation.md)and [`Comp2FileName`](../../Reference/app/MappFileOperation.md)parameter settings can be specified to perform a file operation. For these settings, you must set [`Comp1FileName`](../../Reference/app/MappFileOperation.md) to an explicit file name, partial path, or full path.  Set unused parameters to [`M_NULL`](../../Reference/app/MappFileOperation.md), except for [`OperationFlag`](../../Reference/app/MappFileOperation.md)which must be set to[`M_DEFAULT`](../../Reference/app/MappFileOperation.md).

---

### `M_FILE_COPY`

Specifies to copy the file from the source computer to the destination computer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application Context Identifier` | Specifies the identifier of an application context.  Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappFileOperation.md). |
| `"Drive\Path\[FileName]"` | Specifies the drive, directory, and optionally the new file name, on the destination computer in which the file specified using [`Comp1FileName`](../../Reference/app/MappFileOperation.md) will be copied. For example, _"C:\myDirectory\"_ or _"C:\myDirectory\myNewFileName"_.  When specifying a file, the file name is always local to computer 2.  When specifying a directory, the string must end in a directory separator (for example, _"C:\myDirectory\"_).  If no destination file name is specified, the source file name is used. |

---

### `M_FILE_COPY_AIL_DLL`

Specifies to attempt to copy a file from the source computer to the runtime library subfolder of the installation folder on the destination computer; the specified application might not have write-access to this folder (in which case the operation fails). This is primarily useful for copying a compiled library that contains custom functions, created using the Function Development module ([`Mfunc...`](../../Reference/func/MfuncAlloc.md)).  You can check whether the file already exists in this folder using [`M_FILE_EXISTS_AIL_DLL`](../../Reference/app/MappFileOperation.md).

#### System specific

| Board(s) | Note |
|---|---|
| System specific | This is the _DLL_ subfolder of your installation folder (for example,_C:\Program Files\Aurora Imaging Library\&lt;version number>\Library\DLL_).  This folder might not be write-accessible if the destination application, specified by the [`Comp2ContextAppId`](../../Reference/app/MappFileOperation.md) parameter, was launched without administrative privileges. To copy the DLL to a folder that does not require administrative privileges (but where it can still be loaded by the application), use [`M_FILE_COPY_AIL_USER_DLL`](../../Reference/app/MappFileOperation.md) instead. |
| System specific | This is the _library_ subfolder of your installation folder (for example,_/opt/aurora_imaging_library/&lt;version>/library_).  This folder might not be write-accessible if the destination application, specified by the [`Comp2ContextAppId`](../../Reference/app/MappFileOperation.md) parameter, was launched by a user who is not part of the permissions group.  > **Note:** Before accessing the copied DSO, you must update the runtime library bindings using ldconfig. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application Context Identifier` | Specifies the identifier of an application context.  Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappFileOperation.md). |
| `"FileName"` | Specifies a new file name for the copied DLL. |

---

### `M_FILE_COPY_AIL_USER_DLL`

Specifies to copy a file from the source computer to the user runtime library folder on the destination computer; the specified application typically has write-access to this folder (under Windows). This is primarily useful for copying a compiled library that contains custom functions, created using the Function Development module ([`Mfunc...`](../../Reference/func/MfuncAlloc.md)).  You can check whether the file already exists in this folder using [`M_FILE_EXISTS_AIL_USER_DLL`](../../Reference/app/MappFileOperation.md).

#### System specific

| Board(s) | Note |
|---|---|
| System specific | The path to this folder is_C:\Users\Public\Documents\Aurora Imaging Library\&lt;version number>\Library\UserDLL_. |
| System specific | This setting is the same as [`M_FILE_COPY_AIL_DLL`](../../Reference/app/MappFileOperation.md). The folder is not guaranteed to be write-accessible.  > **Note:** Before accessing the copied DSO, you must update the runtime library bindings using ldconfig. |

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application Context Identifier` | Specifies the identifier of an application context.  Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappFileOperation.md). |
| `"FileName"` | Specifies a new file name for the copied DLL. |

---

### `M_FILE_DELETE`

Specifies to delete the specified file on Computer 1.

---

### `M_FILE_DELETE_DIR`

Specifies to delete a directory on Computer 1. The directory must already be empty, unless [`OperationFlag`](../../Reference/app/MappFileOperation.md)is set to[`M_RECURSIVE`](../../Reference/app/MappFileOperation.md).

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the default behavior. The directory is only deleted if it is empty. |
| `M_RECURSIVE` | Specifies to delete the directory and its contents. |

---

### `M_FILE_DISPATCH`

Specifies to execute the specified program on Computer 1 without waiting for the specified program to terminate on the source computer. When set to [`M_FILE_DISPATCH`](../../Reference/app/MappFileOperation.md), Computer 1 cannot return an exit code.  > **Note:** Note that when set to [`M_FILE_DISPATCH`](../../Reference/app/MappFileOperation.md), the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter can include executable arguments (command-line parameters).  > **Note:** The capability to execute programs on a remote computer has security implications. You are responsible for ensuring that the program executed by the remote Distributed Aurora Imaging Library application is in fact the program that you requested. [`MappFileOperation`](../../Reference/app/MappFileOperation.md)cannot execute a program on a remote computer unless that computer is running a connected Distributed Aurora Imaging Library application.

---

### `M_FILE_EXECUTE`

Specifies to execute the specified program on Computer 1. The computer initiating the operation will wait for the program to terminate. Optionally, you can retrieve the exit code of the program using [`OperationDataPtr`](../../Reference/app/MappFileOperation.md).  > **Note:** Note that when set to [`M_FILE_EXECUTE`](../../Reference/app/MappFileOperation.md), the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter can include executable arguments (command-line parameters).  > **Note:** The capability to execute programs on a remote computer has security implications. You are responsible for ensuring that the program executed by the remote Distributed Aurora Imaging Library application is in fact the program that you requested. [`MappFileOperation`](../../Reference/app/MappFileOperation.md)cannot execute a program on a remote computer unless that computer is running a connected Distributed Aurora Imaging Library application.

---

### `M_FILE_EXISTS`

Specifies to determine whether the file specified by [`Comp1FileName`](../../Reference/app/MappFileOperation.md)exists on computer 1.

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the file does not exist. |
| `M_YES` | Specifies that the file does exist. |

---

### `M_FILE_EXISTS_AIL_DLL`

Specifies to determine whether the file specified by [`Comp1FileName`](../../Reference/app/MappFileOperation.md)exists in the runtime library subfolder of the installation folder on computer 1; the specified application might not have write-access to this folder.

#### System specific

| Board(s) | Note |
|---|---|
| System specific | This is the _DLL_ subfolder of your installation folder (for example,_C:\Program Files\Aurora Imaging Library\&lt;version number>\Library\DLL_). |
| System specific | This is the _library_ subfolder of your installation folder (for example,_/opt/aurora_imaging_library/&lt;version>/library_). |

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the file does not exist. |
| `M_YES` | Specifies that the file does exist. |

---

### `M_FILE_EXISTS_AIL_USER_DLL`

Specifies to determine whether the file specified by [`Comp1FileName`](../../Reference/app/MappFileOperation.md)exists in the user runtime library folder on computer 1; the specified application typically has write-access to this folder (under Windows).

#### System specific

| Board(s) | Note |
|---|---|
| System specific | The path to this folder is_C:\Users\Public\Documents\Aurora Imaging Library\&lt;version number>\Library\UserDLL_. |
| System specific | This is the same as[`M_FILE_EXISTS_AIL_DLL`](../../Reference/app/MappFileOperation.md). |

| Value | Description |
| --- | --- |
| `M_NO` | Specifies that the file does not exist. |
| `M_YES` | Specifies that the file does exist. |

---

### `M_FILE_MAKE_DIR`

Specifies to create a directory on Computer 1.

---

### `M_FILE_MOVE`

Specifies to move the file from the source computer to the destination computer.  > **Note:** The file is only deleted from the source computer after it is successfully copied to the destination computer.

| Value | Description |
| --- | --- |
| `M_DEFAULT` | Specifies the application context of the current process. |
| `Application Context Identifier` | Specifies the identifier of an application context.  Typically, specifying an application context identifier is used to specify a remote application on a remote computer. However, it can be used for a remote application on the local computer, or the current process. When you explicitly specify the identifier of the current process, it is equivalent to specifying [`M_DEFAULT`](../../Reference/app/MappFileOperation.md). |
| `"Drive\Path\[FileName]"` | Specifies the drive, directory, and optionally the new file name, on the destination computer in which the file specified using [`Comp1FileName`](../../Reference/app/MappFileOperation.md) will be moved. For example, _"C:\myDirectory\"_ or _"C:\myDirectory\myNewFileName"_.  When specifying a file, the file name is always local to computer 2.  When specifying a directory, the string must end in a directory separator (for example,_"C:\myDirectory\"_).  If no destination file name is specified, the source file name is used. |

### For specifying to perform a file search

The following [`Operation`](../../Reference/app/MappFileOperation.md) parameter settings can be specified to perform a file search. For these settings, you must set [`Comp1FileName`](../../Reference/app/MappFileOperation.md) to a file name search pattern.  Set unused parameters to [`M_NULL`](../../Reference/app/MappFileOperation.md), except for [`OperationFlag`](../../Reference/app/MappFileOperation.md)which must be set to[`M_DEFAULT`](../../Reference/app/MappFileOperation.md).

---

### `M_FILE_NAME_FIND`

Specifies to determine the name of a file found with the search pattern specified by the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter. Since the search might find more than one file, you must specify which file name to return by setting the [`OperationFlag`](../../Reference/app/MappFileOperation.md) parameter to the file's index in the search results. You can determine the total number of files that match the search pattern using [`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md) + [`M_NB_ELEMENTS`](../../Reference/app/MappFileOperation.md).  The search is not recursive, and does not include the names of folders that match the search pattern. Only the file name is output, and not the full path.  > **Note:** Search results are not cached. If a file is added or deleted between calls to [`MappFileOperation`](../../Reference/app/MappFileOperation.md), the indexing of the search results might change. Therefore, if you want to iterate through the files found by a search and delete some of them, you should first iteratively call [`MappFileOperation`](../../Reference/app/MappFileOperation.md) with [`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md)to store all of the search results in an array (or other data structure), and then delete the files by iterating through the stored results.

| Value | Description |
| --- | --- |
| `0 <= Value < M_FILE_NAME_FIND + M_NB_ELEMENTS` | Specifies which file name to return, by index in the search results. |

---

### `M_FOLDER_NAME_FIND`

Specifies to determine the name of a folder found with the search pattern specified by the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter. Since the search might find more than one folder, you must specify which folder name to return by setting the [`OperationFlag`](../../Reference/app/MappFileOperation.md) parameter to the folder's index in the search results. You can determine the total number of folders that match the search pattern using [`M_FOLDER_NAME_FIND`](../../Reference/app/MappFileOperation.md) + [`M_NB_ELEMENTS`](../../Reference/app/MappFileOperation.md).  The search is not recursive, and does not include the names of files that match the search pattern. Only the folder name is output, and not the full path.  > **Note:** Search results are not cached. If a folder is added or deleted between calls to [`MappFileOperation`](../../Reference/app/MappFileOperation.md), the indexing of the search results might change. Therefore, if you want to iterate through the folders found by a search and delete some of them, you should first iteratively call [`MappFileOperation`](../../Reference/app/MappFileOperation.md) with [`M_FILE_NAME_FIND`](../../Reference/app/MappFileOperation.md)to store all of the search results in an array (or other data structure), and then delete the folders by iterating through the stored results.

| Value | Description |
| --- | --- |
| `0 <= Value < M_FOLDER_NAME_FIND + M_NB_ELEMENTS` | Specifies which folder name to return, by index in the search results. |

### Combination Constants — For getting the string size

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the string's length.

#### `M_STRING_SIZE`

Retrieves the length of the string, including the terminating null character ("\0").

### Combination Constants — For getting the number of elements found with the search pattern

> *Optional.*

> **Usage:** You can add one of the following values to the above-mentioned values to get the number of elements found with the search pattern specified by the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter.

#### `M_NB_ELEMENTS`

Retrieves the number of elements found with the search pattern specified by the [`Comp1FileName`](../../Reference/app/MappFileOperation.md) parameter.  The search is not recursive, and the result will only include search pattern matches for the specified element type.
