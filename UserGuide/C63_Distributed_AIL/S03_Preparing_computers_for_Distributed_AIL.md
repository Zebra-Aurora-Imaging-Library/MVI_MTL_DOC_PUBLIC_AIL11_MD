---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Preparing_computers_for_Distributed_AIL
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Preparing computers for Distributed AIL"
---

# Preparing computers for Distributed Aurora Imaging Library

To use Distributed Aurora Imaging Library, you must prepare all computers with the appropriate software. All computers that you want to use in a Distributed Aurora Imaging Library configuration must have Aurora Imaging Library/Aurora Imaging Library Lite installed with the Distributed Aurora Imaging Library option selected.

During Aurora Imaging Library installation, select the **Distributed Aurora Imaging Library** option from the tree in the **Aurora Imaging Library optional components** pane of the **Aurora Imaging Setup** dialog box. You will then choose how to start the Distributed Aurora Imaging Library server, which is discussed below. Once installation is complete, the Distributed Aurora Imaging Library components, including the Distributed Aurora Imaging Library server, are installed on the computer, along with Aurora Imaging Library/Aurora Imaging Library Lite. You must make sure that all the computers in the cluster have the appropriate licenses.

> **Note:** If exchanging image buffers across the network, it is recommended that the computers have at least a GigE Ethernet interface (1000BaseT).

## Setting up the Distributed Aurora Imaging Library server on remote computers

A computer must be running the Distributed Aurora Imaging Library server to accept new incoming connections and to create new outgoing connections. The Distributed Aurora Imaging Library server manages all of a remote computer's connections; the server transparently opens and closes the required connections as necessary.

Distributed Aurora Imaging Library typically manages the Distributed Aurora Imaging Library server, but you can control when it starts and stops running. In the monitoring configuration, the Aurora Imaging Library functions will completely manage the server regardless of any explicit attempts to control it. In the controlling configuration, you must ensure that the server is running on any computer on which you will allocate a Distributed Aurora Imaging Library remote system. During installation (and afterwards with the Aurora Imaging Configurator utility), you can select to start the server as a service, start the server at logon, or start the server manually. You will typically want to start the Distributed Aurora Imaging Library server as a service. This will cause the server to start automatically when you boot the computer, and to recover automatically if the server crashes; you won't need to logon to the computer to start the server.

The Distributed Aurora Imaging Library server cannot be running as a service on a remote computer if you plan to:

- Present a display on that computer.
- Access that computer's file system with current user credentials.

In these cases, you must select to start the server at logon. To set it to run at logon, open the Aurora Imaging Configurator utility and select **Run at every logon with user credentials** in the **Server Settings** pane, found under the **Distributed Aurora Imaging Library** item. Then, before a Distributed Aurora Imaging Library application can access the computer, you must logon to the computer; once logged on, the server will start automatically.

When developing a distributed application using Distributed Aurora Imaging Library, you might need to share files between computers. You can specify an application context on a remote computer as a source or destination for [`MappFileOperation`](../../Reference/app/MappFileOperation.md); this allows you to easily transfer files between Aurora Imaging Library applications with a single function call. You can also use [`MappFileOperation`](../../Reference/app/MappFileOperation.md) to execute a program on the remote computer. For more information see [Working with files](../C02_Building_an_application/S20_Working_with_files.md).

## Managing network connections and ports

You don't need to manage all the separate connections required between the computers in your cluster. However, you must establish a port on the remote computers to which their Distributed Aurora Imaging Library server should listen for new connection requests, and that your application should access to initiate new connections.

By default, the Distributed Aurora Imaging Library server listens to port 57010 or 57020 for new connection requests when in the controlling configuration or the monitoring configuration, respectively. If the remote computer uses this port for other purposes, you can specify a different listening port number using the Aurora Imaging Configurator utility on this computer. When you specify a non-default port for the server, you should specify that your controlling or monitoring application uses the same port to connect to the remote computer. You can change the default server connection port, which the local computer uses to connect to a remote computer, using the Aurora Imaging Configurator utility on the local computer. If some of your remote computers don't use this default port, your controlling or monitoring application must explicitly specify the port to access on these remote computers. For an example of how to allocate a Distributed Aurora Imaging Library remote system on a specific port, see [Allocating Distributed Aurora Imaging Library remote systems](S04_Controlling_configuration.md). For an example of how to connect to a publishing computer on a specific port, see [Monitoring application](S05_Monitoring_configuration.md).

> **Note:** Note that your application remains more portable if your application does not explicitly specify the port to access during system allocation.

The connection between computers is typically made using TCP/IP.

It is also possible, but not required, to specify a passkey server side (on the remote computer). This ensures that only Aurora Imaging Library applications which specify the passkey during connection are given access. To set up the passkey, specify it in the Aurora Imaging Configurator utility on the remote computer in the **Server Settings** pane; the passkey is a string of up to 16 case-sensitive alphanumeric characters. The controlling/monitoring computer must pass the passkey along with the system name, port, and system type to the remote system(s) or publishing application(s) when establishing a connection to the remote computer(s) using [`MsysAlloc`](../../Reference/sys/MsysAlloc.md) or [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md) (depending on if you are using an Aurora Imaging Library controlling configuration or an Aurora Imaging Library monitoring configuration respectively). The remote computer will then validate the authenticity of the passkey which was passed to it from the controlling/monitoring application. Each remote computer in a Distributed Aurora Imaging Library application can have a different passkey.

> **Note:** Note that you cannot specify a _passkey_ for a Distributed Aurora Imaging Library default system.

### Cluster modes and port range

In the controlling configuration, the Distributed Aurora Imaging Library server can exist in one of two cluster modes: single or multiple. By default, the server is set to single cluster mode, which specifies that the server will only be connected to a single controlling application. By specifying multiple cluster mode, the server is able to be connected to several independent controlling applications. When specifying how to start the Distributed Aurora Imaging Library server, you can choose to start the server as a service in single cluster mode or in multiple cluster mode. You specify the cluster mode in the Aurora Imaging Configurator utility.

*[Image: DAIL_simple_and_multiple_cluster.png]*

If the Distributed Aurora Imaging Library server is in the default single cluster mode, the controlling application connects to the remote computer using the default or explicitly specified server connection port. If the Distributed Aurora Imaging Library server is in multiple cluster mode, only the initial connection is made using the default or explicitly specified server connection port. You must establish a range of ports on the remote computer which the Distributed Aurora Imaging Library server could use for subsequent connections. You can either use the default range or specify a range using the Aurora Imaging Configurator utility on the remote computer. The connection to a remote computer in multiple cluster mode happens as follows:

1. The controlling application connects to the remote computer using the default or explicitly specified server connection port.
2. The remote computer spawns a new instance of the Distributed Aurora Imaging Library server.
3. The new instance of the Distributed Aurora Imaging Library server selects a connection port from the range of specified ports on the remote computer.
4. The remote computer informs the controlling application of the newly selected port.
5. The controlling application disconnects from the remote computer.
6. The controlling application reconnects to the remote computer using the port selected in step 3.

In this process, the remote computer dynamically selects the port for its connections depending on which ports in its specified range are available; this port management technique allows for the computer to be part of multiple clusters.

When running your application, you can confirm that connections have been set up properly by monitoring the Distributed Aurora Imaging Library server's output using the Aurora Imaging Configurator utility on the remote computer.

## Firewall configuration

When using the TCP/IP communication layer, you will need to ensure that your networking equipment (for example, firewalls and proxy servers) has been configured properly so that it allows Distributed Aurora Imaging Library to create all of its required connections.

All computers must be able to accept data from each other on the specified TCP/IP port and port range. When dealing with computers that have Microsoft Windows Firewall enabled, a pop-up will appear during the execution of an Aurora Imaging Library application whenever there is a need to use a port blocked by the firewall. Through this pop-up, you can configure the Microsoft Windows Firewall to allow the use of a specific port by a specific application.

To optimize performance for computers behind a Microsoft Windows Firewall, you should unblock these ports using the Aurora Imaging Configurator utility. On the **Server Settings** pane, accessed using the **Distributed Aurora Imaging Library - Controlling** item, select the **Microsoft Windows Firewall exception** option. Alternatively, you can unblock ports directly using Microsoft Windows by navigating to the Control Panel and adding exceptions to the Microsoft Windows Firewall.

Instead of unblocking the above-mentioned ports, you can disable the firewall for these ports using the Microsoft Windows Firewall interface. Disabling the firewall will stop the firewall software from watching the specified ports. Note that disabling the firewall on specific ports is not recommended if your computers are connected to a large network (such as, the internet).

## Licensing considerations

To develop or run a Distributed Aurora Imaging Library application, you must have the appropriate licenses:

| Product | Mode of operation | Licenses Required |
| --- | --- | --- |
| Aurora Imaging Library | Developing | Aurora Imaging Library development license. | Runtime licenses for Distributed Aurora Imaging Library and any Aurora Imaging Library module that the computer actually uses. |
| Running | Runtime licenses for Distributed Aurora Imaging Library and any Aurora Imaging Library module that the computer actually uses. |
| Aurora Imaging Library Lite | Developing | Supplemental licenses for Distributed Aurora Imaging Library and any other functionality actually used that requires a supplemental license. | Supplemental licenses for Distributed Aurora Imaging Library and any other functionality actually used that requires a supplemental license. |
| Running |

All computers in the cluster need a Distributed Aurora Imaging Library license to either send or receive a Distributed Aurora Imaging Library command; an Aurora Imaging Library development license includes Distributed Aurora Imaging Library. In addition, each computer needs the licenses for any Aurora Imaging Library modules/functionality that the computer actually uses. For example, if the controlling application on the local computer calls a Model Finder function using objects allocated on a remote computer, the operation will actually be performed on the remote computer; therefore, only the remote computer needs a Model Finder license.

> **Note:** Note that when all remote computers in a controlling configuration are Zebra computers or smart cameras (for example, Zebra 4Sight EV7 and Zebra Iris GTX), the local computer does not need a Distributed Aurora Imaging Library license.

For more information on licensing, see [Distribution and licensing](../C69_Distribution_and_licensing/ChapterInformation.md).
