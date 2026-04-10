---
doctype: UserGuide
part: "Miscellaneous"
chapter: HTTP_servers
section: Hosting_files_on_a_HTTP_server
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / HTTP_servers / Hosting files on a HTTP server"
---

# Hosting files on an Aurora Imaging Library HTTP server

You can host files on the local computer using an Aurora Imaging Library HTTP server. The HTTP server makes the contents of a specified folder (and its subfolders) readable using any HTTP/1.1-compatible web browser on a connected remote computer. If the folder contains valid HTML files, they will be viewable as a web page. To create an Aurora Imaging Library HTTP server, you need to allocate an Aurora Imaging Library HTTP server object in your Aurora Imaging Library application using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md)with [`M_HTTP_SERVER`](../../Reference/obj/MobjAlloc.md).

*[Image: HTTPServer_Diagram.png]*

> **Note:** Aurora Imaging Library HTTP servers are intended to be run on a segmented LAN that has no internet/WAN access; Aurora Imaging Library HTTP servers offer no internal security mechanisms. When an Aurora Imaging Library HTTP server is enabled, you should typically ensure that your network administrator configures your network to prevent incoming connections from the internet to the computer running your Aurora Imaging Library application (and from any other devices on your network that do not need access). If this is not possible, at a minimum, you should block incoming connections from the internet to the listening port of the HTTP server. You can inquire which port is used for this purpose, using [`MobjInquire`](../../Reference/obj/MobjInquire.md) with [`M_HTTP_PORT`](../../Reference/obj/MobjInquire.md).

Using an Aurora Imaging Library HTTP server within your Aurora Imaging Library application is typically more convenient for simple file hosting than installing and configuring third-party server software. Additionally, allocating an Aurora Imaging Library HTTP server within your Aurora Imaging Library application ensures that the HTTP server only runs while your Aurora Imaging Library application is active. Note that an Aurora Imaging Library HTTP server is only capable of hosting files; it cannot execute programs or scripts (for example, using CGI or PHP). It is therefore not suitable for hosting complex websites.

While an Aurora Imaging Library HTTP server is typically used to host the HTML and JavaScript files of an Aurora Imaging Web Library client application (discussed in[Using Aurora Imaging Web Library to monitor your application](../C58_Using_AIWL_to_monitor_your_application/ChapterInformation.md)), it can also be used to host other web pages or types of files. You can allocate and use an Aurora Imaging Library HTTP server regardless of whether Aurora Imaging Web Library server functionality is enabled for your application (and vice versa).

The following steps provide a typical methodology to host the contents of a folder with an Aurora Imaging Library HTTP server:

1. Allocate an HTTP server using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with [`M_HTTP_SERVER`](../../Reference/obj/MobjAlloc.md).
2. Set the full path to the folder (on the local computer) that contains the files and subfolders to host on the HTTP server, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_HTTP_ROOT_DIRECTORY`](../../Reference/obj/MobjControl.md).
   > **Note:** All files and subfolders in this folder will be readable by other computers on your network while the HTTP server is enabled. Therefore, you should typically ensure that there are no sensitive files in this folder (or its subfolders).
3. Optionally, specify the listening port on which the HTTP server receives incoming connections, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_HTTP_PORT`](../../Reference/obj/MobjControl.md). This must be a port not used by any other application running on the local computer. The default port is 8080.
4. Optionally, specify the web address (URL) that remote computers will use to access the HTTP server, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_HTTP_ADDRESS`](../../Reference/obj/MobjControl.md). This must consist of`http://`, followed by the hostname or local static IP address of the local computer (for example,`http://VisionController42` or `http://192.168.1.58`).
   The default URL (`http://localhost`) is only accessible from a web browser running on the local computer.
   > **Note:** You must have administrator permissions on the local computer to make the HTTP server accessible to remote computers.
   If your local computer is running more than one HTTP server application (either multiple Aurora Imaging Library HTTP servers, or other HTTP server software in addition to your Aurora Imaging Library application), you must append to the URL a colon (:) followed by the listening port (for example, `http://192.168.1.58:8080`).
5. Start the Aurora Imaging Library HTTP server using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_HTTP_START`](../../Reference/obj/MobjControl.md).

Once you run your Aurora Imaging Library application, you can access the hosted files using any web browser on a computer that has access to the HTTP server. To access a file hosted by Aurora Imaging Library HTTP server, type the URL in the address bar of your web browser. The URL must include the path to the file, relative to the root directory specified using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_HTTP_ROOT_DIRECTORY`](../../Reference/obj/MobjControl.md) (for example,`http://192.168.1.58:8080/xfolder/xfile.fox`).

If you are accessing the html files used for a web page, you only need to manually access the web page's entry point html file; your web browser will automatically retrieve any additional required files. By convention, this file is usually named_index.htm_or_index.html_, but it can have any name.

> **Note:** Directory listing is not enabled for Aurora Imaging Library HTTP servers; the HTTP server does not publish a list of available files and folders. Therefore, you must already know the path to the required file to access it.
