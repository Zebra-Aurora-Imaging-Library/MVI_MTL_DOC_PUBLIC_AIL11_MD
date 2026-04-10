---
doctype: UserGuide
part: "Getting started"
chapter: Building_an_application
section: Working_with_files
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Getting started / building-application / Working with files"
---

# Working with files

Aurora Imaging Library provides several useful functions for working with files. Using Aurora Imaging Library functions, you can import and export images, 3D data, and Aurora Imaging Library contexts in various formats. You can also move. copy, delete, and search for files on the local computer in an operating-system independent way, and host files on the local computer using HTTP so that they can be accessed by a web browser on a remote computer. In a Distributed Aurora Imaging Library application, you can also share files (including DLLs) between computers, and execute programs on a remote computer.

## Moving, sharing, and searching for files

You can move, copy, delete, and search for files using [`MappFileOperation`](../../Reference/app/MappFileOperation.md). This function is supported for both Windows and Linux, increasing the portability of your Aurora Imaging Library application. You can also use this function to execute a program on the local computer.

[`MappFileOperation`](../../Reference/app/MappFileOperation.md)is particularly useful to manage the files used by a simulated digitizer (which grabs images from a specified folder instead of a camera). You can use this function to confirm which files are present in the images folder, and to add or remove images at runtime. For more information about simulated digitizers, see [Simulated digitizer](../C27_Grabbing_with_your_digitizer/S13_Simulated_Digitizer.md).

A local computer on the network can host files using an Aurora Imaging Library HTTP server, allocated using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)with [`M_HTTP_SERVER`](../../Reference/obj/MobjAlloc.md). Hosted files can then be accessed in a web browser by remote computers and other devices (such as smart phones and tablets). For more information, see [Hosting files on an Aurora Imaging Library HTTP server](../C64_HTTP_servers/S01_Hosting_files_on_a_HTTP_server.md).

### Additional functionality for Distributed Aurora Imaging Library

When developing a distributed application using Distributed Aurora Imaging Library, you might need to share files between computers. You can specify an application context on a remote computer as a source or destination for[`MappFileOperation`](../../Reference/app/MappFileOperation.md); this allows you to easily transfer files between Aurora Imaging Library applications with a single function call. This function is particularly useful for copying custom Aurora Imaging Library functions created using the Function Development module ([`Mfunc...`](../../Reference/func/MfuncAlloc.md)). For example, you can use [`MappFileOperation`](../../Reference/app/MappFileOperation.md)with [`M_FILE_EXISTS_AIL_USER_DLL`](../../Reference/app/MappFileOperation.md)to confirm whether a remote computer has a required user DLL, and if not, you can copy that DLL to the remote computer using[`M_FILE_COPY_AIL_USER_DLL`](../../Reference/app/MappFileOperation.md).

You can also use [`MappFileOperation`](../../Reference/app/MappFileOperation.md) to execute a program on the remote computer.

> **Note:** The capability to execute programs on a remote computer has security implications. You are responsible for ensuring that the program executed by the remote Distributed Aurora Imaging Library application is in fact the program that you requested. [`MappFileOperation`](../../Reference/app/MappFileOperation.md)cannot execute a program on a remote computer unless that computer is running a connected Distributed Aurora Imaging Library application.

## Importing and exporting data

You can save and load most types of Aurora Imaging Library objects to a file, using a function from the module that you used to allocate the object. For example, you can allocate a blob context using[`MblobAlloc`](../../Reference/blob/MblobAlloc.md), save it using[`MblobSave`](../../Reference/blob/MblobSave.md), and restore it using [`MblobRestore`](../../Reference/blob/MblobRestore.md). You can also save or restore the blob context to or from a memory stream using [`MblobStream`](../../Reference/blob/MblobStream.md).

You can save and load image buffers and 3D-processable point cloud containers to and from standard file formats that are supported by a wide variety of applications (for example, PNG and BMP files for images and PLY or STL files for 3D data). You can also save a buffer or container (including 3D-processable depth map containers) with all of its attributes and settings, using the Aurora Imaging Library native file format. For more information about saving and loading buffers, see [Loading a data buffer](../C23_Data_buffers/S07_Managing_data_buffers.md). Most of the information in that section also applies to 3D-processable point cloud and depth map containers.

Typically, when saving and loading files with Aurora Imaging Library, you should use the file extensions listed in [Aurora Imaging Library file extension list](S21_Extensions.md).
