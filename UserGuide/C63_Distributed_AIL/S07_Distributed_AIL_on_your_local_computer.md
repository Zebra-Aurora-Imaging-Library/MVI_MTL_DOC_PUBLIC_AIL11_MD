---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Distributed_AIL_on_your_local_computer
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Distributed AIL on your local computer"
---

# Distributed Aurora Imaging Library on your local computer

Typically, Distributed Aurora Imaging Library is used among one local computer and one or more remote computers. This could be to improve the performance of processing heavy applications or to allow a local computer to use or monitor a remote computer (for example, on the factory floor or in another country). However, you can use Distributed Aurora Imaging Library to create a cluster consisting only of your local computer (for example, to test a Distributed Aurora Imaging Library setup that will eventually be run over several computers). You can create this Distributed Aurora Imaging Library cluster on your local computer whether in controlling or monitoring configuration, although the procedure is slightly different.

## Developing and debugging a Distributed Aurora Imaging Library application

When developing or debugging an application that will use a Distributed Aurora Imaging Library cluster, it can be more efficient to set up a cluster on your local computer. For instance, when testing, you don't have to worry about connection issues, such as internet problems and network failures. You can focus solely on the logic of the application(s). Also, when transferring files from your local computer to the remote computer's folder structure during testing, it is faster, especially with large folders of test images. Finally, by setting up your Distributed Aurora Imaging Library cluster on your local computer, you are able to develop the entire cluster before you have access to the actual remote computers. When testing a Distributed Aurora Imaging Library cluster on your local computer, however, measuring load times and overall processing speed is useless.

## Steps to use Distributed Aurora Imaging Library on your local computer

The steps to run a Distributed Aurora Imaging Library setup on your local computer are slightly different depending on the configuration:

- To run a controlling application that allocates a Distributed Aurora Imaging Library remote system on your local computer, you must:
  1. Ensure that your local computer is running the Distributed Aurora Imaging Library server. For more information on running a Distributed Aurora Imaging Library server on your local computer, see [Setting up the Distributed Aurora Imaging Library server on remote computers](S03_Preparing_computers_for_Distributed_AIL.md). Note that the section is about running the Distributed Aurora Imaging Library server on a remote computer, but the procedures are the same for running a Distributed Aurora Imaging Library server on your local computer.
  2. Allocate a Distributed Aurora Imaging Library remote system on your local computer, using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) with [`SystemDescriptor`](../../Reference/sys/MsysAlloc.md) set to [`AIL_TEXT("dailshm://[Passkey:]localhost[:Port]/AILSystemType")`](../../Reference/sys/MsysAlloc.md), where _[Passkey]_ is replaced with the passkey specified in the server settings page of the Aurora Imaging Configurator utility on the local computer, and _AILSystemType_ is replaced with the type of Aurora Imaging Library system (often a Zebra board) that you need to allocate.
- To run both a publishing application and a monitoring application on your local computer, you must:
  1. Publish Aurora Imaging Library objects from one or more Aurora Imaging Library applications running on your computer. These are now the publishing applications.
  2. Connect a monitoring application to the publishing application(s), using [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md) with [`ConnectionDescriptor`](../../Reference/app/MappOpenConnection.md) set to [`AIL_TEXT("dailshm://[Passkey:]localhost[:Port]")`](../../Reference/app/MappOpenConnection.md), where _[Passkey]_ is replaced with the passkey specified in the server settings page of the Aurora Imaging Configurator utility of the local computer.

> **Note:** The previous steps outlined running a Distributed Aurora Imaging Library setup on your local computer using the SHM protocol. You can also use the TCP/IP protocol when using Distributed Aurora Imaging Library on your local computer, but the SHM protocol will provide faster performance. If you use the TCP/IP protocol, replace "dailshm" with "dailtcp" and either leave "localhost" as is or replace it with 127.0.0.1.

> **Note:** For more information concerning setting the Passkey, please refer to [Managing network connections and ports](S03_Preparing_computers_for_Distributed_AIL.md).
