---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distribution_and_licensing
section: Additional_licensing_notes_License_server
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / licensing / Additional licensing notes License server"
---

# Miscellaneous user guide information for license server

This section includes additional user guide information for license server. The information found in this section might be a reiteration of content previously documented.

Network licensing employs a client-server architecture; you can connect a network license dongle to a central server to provide licenses to one or more clients. The central server can run on a computer without Aurora Imaging Library installed or on the same computer as one of the clients.

## Server-side setup

To install the central server service on Windows, run:

```
CentralServer_Setup.exe
```

Whereas, on Linux, execute the following command:

```
sudo bash centralserversetup.run
```

Under Linux, Aurora Imaging Library and the central server cannot be installed on the same PC.

## Controlling the central server service

To control the central server service, use the `serverctl` tool. To do so, open a command prompt as an administrator and change the current working directory to where centralserver is installed.

It is typically located in the tools folder (for example, _C:\Program Files\Aurora Imaging Library\11\Tools\centralserver_ on Windows and _/opt/aurora_imaging_library/11/tools/centralserver _ on Linux).

You can run `serverctl` without parameters for a complete list of supported commands.

### Port

To change the port used by the server on Windows, execute the following command:

```
serverctl setport NN
```

Likewise, on Linux, execute the following command:

```
sudo /opt/aurora_imaging_library/11/tools/serverctl setport NN
```

Replace NN with a valid port number. If you are not using the default port, every client must use the specified port.

### Debug

You can run the`serverctl` executable in a terminal console to view additional debugging information. Before doing so, ensure that the server service is stopped since only one instance of the server can be run at once.

To stop the server service on Windows, execute the following command:

```
serverctl stop
```

Likewise, on Linux, execute the following command:

```
sudo /opt/aurora_imaging_library/11/tools/serverctl stop
```

### Log

To enable server logs on Windows, execute the following command:

```
serverctl enablelog
```

The log files will be created under the log folder (for example, _%ProgramData%\Aurora Imaging Library\11\log_). To disable the server logs, execute the following command:

```
serverctl disablelog
```

Likewise, to enable server logs on Linux, execute the following command:

```
sudo /opt/aurora_imaging_library/11/tools/serverctl enablelog
```

The log files will be created under the _/var/lib/aurora_imaging_ folder. To disable the server logs, execute the following command:

```
sudo /opt/aurora_imaging_library/11/tools/serverctl disablelog
```

Log files can become quite large, so you should disable them when no longer needed.

You can generate a dump of the server information to the _%TEMP%\CentralServerDump_ folder on Windows or _/tmp/CentralServerDump_ folder on Linux. To do so, execute the following command on Windows:

```
serverctl serverinfo
```

Likewise, on Linux, execute the following command:

```
sudo /opt/aurora_imaging_library/11/tools/serverctl serverinfo
```

This folder can then be zipped and forwarded to your Zebra technical support representative.

## Client-side setup

To set up the client-side on Windows, run the following command:

```
ailconfig -activatelicenseserver [servername[:port]]
```

Whereas, on Linux, execute the following command:

```
 ailconfig --activatelicenseserver [servername[:port]]
```

The port only needs to be specified if the [port number](S11_Additional_licensing_notes_License_server.md) has been changed on the central server.

> **Note:** Once you set up the license server's client side, multiple versions of Aurora Imaging Library are supported side-by-side because the server is compatible with all versions of the library.

### Licensing Features

The license status of all Aurora Imaging Library modules is listed in Aurora Imaging Configurator's **Licensing Status** page.

In the **Licensing License Server** page, you can perform the following:

- Change the name of the central server.
- Change the connection port.
- Set a description string to help administrators in identifying the different clients connected to the server.
- Automatically reserve a license when the client PC boots up, which speeds up application allocation.
- View the connected clients and the available licenses using the **License Server Status Tool**.
