---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Basic_concepts
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Basic concepts"
---

# Basic concepts for Distributed Aurora Imaging Library

The basic concepts and vocabulary conventions for Distributed Aurora Imaging Library are:

- **Board-type system**. A board-type system consists of a Zebra board (or third-party board that is compatible with Aurora Imaging Library), the Host CPU and memory, and any available graphics controller.
- **Cluster**. A cluster is a group of computers interacting in a Distributed Aurora Imaging Library setup. In the controlling configuration, a cluster is all the computers that are being used by a single controlling application. In this configuration, a remote computer can belong to one or multiple clusters, depending on how many controlling applications are connected to it. In the monitoring configuration, all connected computers, whether running monitoring or publishing applications, are part of a single cluster.
- **Cluster manager**. A cluster manager is a computer designated to manage application identifiers in a monitoring configuration cluster.
- **Controlling configuration**. A Distributed Aurora Imaging Library setup with one controlling application that allocates one or more Distributed Aurora Imaging Library remote systems, typically on remote computers. The controlling configuration is used to distribute a single, typically processing heavy, Aurora Imaging Library application across multiple computers. The controlling application manages the Aurora Imaging Library systems allocated on all the computers in the cluster.
- **Distributed Aurora Imaging Library**. Distributed Aurora Imaging Library is an Aurora Imaging Library feature which allows multiple computers to interact across a network, using a client-server architecture. There are two Distributed Aurora Imaging Library configurations: the controlling configuration and the monitoring configuration.
- **Distributed Aurora Imaging Library server**. The Distributed Aurora Imaging Library server is a program running on a computer that allows the computer to handle requests made by a client application, which is either the controlling application or the monitoring application, depending on the configuration.
- **Distributed Aurora Imaging Library remote system**. A Distributed Aurora Imaging Library remote system is an Aurora Imaging Library system typically allocated on a remote computer. Specifically, a Distributed Aurora Imaging Library remote system is an Aurora Imaging Library system allocated in the Distributed Aurora Imaging Library server's address space, which is separate from the main application's address space.
- **Host-type system**. A Host-type system consists of the Host CPU and memory, and any available graphics controller.
- **Local computer**. The local computer is the computer that is running the controlling application in the controlling configuration, or the monitoring application in the monitoring configuration.
- **Aurora Imaging Library system**. An Aurora Imaging Library system is a set of hardware components used to execute Aurora Imaging Library operations. An Aurora Imaging Library system can be either a **board-type system** or a **Host-type system**. A board-type system consists of a Zebra board (or third-party board that is compatible with Aurora Imaging Library), the Host CPU and memory, and any available graphics controller. A Host-type system consists of the Host CPU and memory, and any available graphics controller. In the controlling configuration, Aurora Imaging Library systems on a remote and local computer can collaborate within an Aurora Imaging Library application using Distributed Aurora Imaging Library.
- **Monitoring application**. A monitoring application is an Aurora Imaging Library application which is connected to a publishing application and is able to access the Aurora Imaging Library objects the publishing application publishes.
- **Monitoring configuration**. A Distributed Aurora Imaging Library setup that involves two or more separate Aurora Imaging Library applications, usually on separate computers, where one application, the monitoring application, accesses designated Aurora Imaging Library objects from other applications, the publishing applications.
- **Publishing application**. A publishing application is an Aurora Imaging Library application which is set up to publish its own Aurora Imaging Library objects to a monitoring application.
- **Remote computer**. A remote computer is a computer separate from the local computer. In the controlling configuration, you typically allocate a Distributed Aurora Imaging Library remote system on a remote computer. In the monitoring configuration, the monitoring application typically connects to publishing applications on remote computers.
