---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Distributed_AIL_overview
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Distributed AIL overview"
---

# Distributed Aurora Imaging Library overview

Distributed Aurora Imaging Library (DAIL) allows an Aurora Imaging Library application running on one computer to access or use the resources of other computers, for example, to distribute processing, to grab remotely and display locally, or to monitor their buffers and other objects.

There are two Distributed Aurora Imaging Library configurations:

- **Controlling configuration.** A Distributed Aurora Imaging Library setup comprised of a single Aurora Imaging Library controlling application that uses Distributed Aurora Imaging Library to allocate Aurora Imaging Library systems on remote computers to share in processing and other Aurora Imaging Library operations; the Distributed Aurora Imaging Library remote systems can collaborate with each other and with local systems. You would typically use this to distribute the processing of an Aurora Imaging Library application among several computers.
- **Monitoring configuration.** A Distributed Aurora Imaging Library setup comprised of a single Aurora Imaging Library monitoring application that uses Distributed Aurora Imaging Library to monitor published Aurora Imaging Library objects (for example, buffers) of one or more independent Aurora Imaging Library publishing applications; the Aurora Imaging Library publishing applications use Distributed Aurora Imaging Library to publish designated Aurora Imaging Library objects. You would typically use this to create an Aurora Imaging Library application on a local computer that monitors Aurora Imaging Library applications running on remote computers.

In both configurations, a group of computers interacting using Distributed Aurora Imaging Library is called a cluster. A cluster can have up to 128 computers. Regardless of the configuration or whether running locally or running remotely, all Aurora Imaging Library functions in a Distributed Aurora Imaging Library cluster generally behave as they do when running on a single computer without Distributed Aurora Imaging Library.

All information in this chapter applies to both configurations, unless otherwise specified.

## Setup and installation

To develop or run a Distributed Aurora Imaging Library application, Aurora Imaging Library/Aurora Imaging Library Lite must be installed on all computers in the cluster. All computers should have the same version of Aurora Imaging Library installed, including Service Packs and Distributed Aurora Imaging Library Updates, with appropriate licenses; see [Licensing issues](S03_Preparing_computers_for_Distributed_AIL.md).

The operating systems of computers in a cluster can be a mix of any Aurora Imaging Library supported operating systems. If exchanging buffers across the network, it is recommended that the computers have at least a Gigabit Ethernet interface (1000BaseT). The connection between computers is generally made using TCP/IP with a client-server architecture.

## Client-server architecture

Both Distributed Aurora Imaging Library configurations use a client-server (equivalent to controller-worker) architecture. In a client-server architecture, a client makes requests to a server, which accepts the requests and returns a service. When you install Distributed Aurora Imaging Library, the Distributed Aurora Imaging Library server is installed.

In the controlling configuration, the controlling application is the client, and it allocates the Distributed Aurora Imaging Library remote systems on computers running a Distributed Aurora Imaging Library server. You must explicitly ensure that the Distributed Aurora Imaging Library server is running on the remote computers; although by default, the Distributed Aurora Imaging Library server is automatically running. For more information on managing the Distributed Aurora Imaging Library server in the controlling configuration, see [Setting up the Distributed Aurora Imaging Library server on remote computers](S03_Preparing_computers_for_Distributed_AIL.md).

In the monitoring configuration, the monitoring application is the client and the publishing applications have Distributed Aurora Imaging Library servers loaded in them. In this configuration, you do not directly manage the Distributed Aurora Imaging Library server; it is transparently managed by the Aurora Imaging Library functions that establish the connection between the monitoring and publishing applications. For more information on which functions manage the connections in a monitoring configuration, see [Monitoring configuration](S05_Monitoring_configuration.md).

> **Note:** Typically, you use Distributed Aurora Imaging Library to access or use the resources of other computers. However, there are occasions when it is useful to run a Distributed Aurora Imaging Library configuration only using your local computer. For example, when developing or debugging a Distributed Aurora Imaging Library cluster, it might be useful to test your cluster completely locally before deploying it across several computers, where you have to worry about possible connection issues. For more information, see [Distributed Aurora Imaging Library on your local computer](S07_Distributed_AIL_on_your_local_computer.md).
