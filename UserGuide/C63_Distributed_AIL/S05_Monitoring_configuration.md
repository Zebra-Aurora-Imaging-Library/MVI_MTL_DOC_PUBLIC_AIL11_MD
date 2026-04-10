---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Monitoring_configuration
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Monitoring configuration"
---

# Monitoring configuration

When you have one or more Aurora Imaging Library applications running locally or on remote computers, it can be convenient to monitor or manipulate some of the applications' Aurora Imaging Library objects, such as buffers or contexts, from an application on your local computer. Distributed Aurora Imaging Library can allow one Aurora Imaging Library application on the local computer, known as the monitoring application, to monitor Aurora Imaging Library objects of separate Aurora Imaging Library applications, typically on separate computers, known as the publishing applications. The monitoring application is able to access only those Aurora Imaging Library objects explicitly designated by the publishing applications. All other processes and Aurora Imaging Library objects of the publishing application are not accessible to the monitoring application.

When initially allocating a monitoring or publishing Aurora Imaging Library application context, you must specify a computer as a cluster manager. The cluster manager keeps all the application identifiers unique within the cluster. The default cluster manager is specified using the Aurora Imaging Configurator utility.

The publishing applications can designate a permission level, either read only or read/write, for each of their own Aurora Imaging Library objects; by default, all Aurora Imaging Library objects' permission levels are set to non-accessible. When you publish an object, the publishing application opens the Distributed Aurora Imaging Library server, which allows the monitoring application to then connect to the publishing application. Once connected, the monitoring application inquires a list of the Aurora Imaging Library objects that the publishing application has designated as read only or read/write, and then interacts with these Aurora Imaging Library objects according to their permission level. In a monitoring configuration cluster, both monitoring and publishing applications run independently, except for the explicit monitoring mentioned above.

A simple monitoring configuration cluster is illustrated below:

*[Image: DAIL_monitoring_configuration.png]*

## Steps to create a Distributed Aurora Imaging Library monitoring configuration

The following steps provide a basic methodology for creating a Distributed Aurora Imaging Library monitoring configuration cluster:

1. Set a computer as the cluster manager.
2. Setup the publishing applications.
3. Setup the monitoring application.

Before you can run a Distributed Aurora Imaging Library application, you must prepare all the computers with the appropriate software and licenses. See [Preparing computers for Distributed Aurora Imaging Library](S03_Preparing_computers_for_Distributed_AIL.md).

## Cluster manager

An Aurora Imaging Library cluster manager manages the identifiers of Aurora Imaging Library application contexts in a Distributed Aurora Imaging Library monitoring configuration cluster. This assures that application context identifiers are unique; duplicate identifiers can cause a conflict.

The Aurora Imaging Library cluster manager is a designated computer that has a valid version of Aurora Imaging Library/Aurora Imaging Library Lite installed with the Distributed Aurora Imaging Library option selected. The Aurora Imaging Library cluster manager can be on a computer running the monitoring application, a publishing application, or any other stand alone Aurora Imaging Library application. It can also be a computer not currently running any Aurora Imaging Library application.

When allocating an Aurora Imaging Library application context, you must specify an Aurora Imaging Library cluster manager that is currently active. You can activate an Aurora Imaging Library cluster manager using the Aurora Imaging Configurator utility on the computer designated as the cluster manager. The activation prompt is found in the **Cluster Manager** pane, accessible from the **Distributed **Aurora Imaging Library item. To ensure the uniqueness of Aurora Imaging Library application context identifiers across the cluster, all computers within a cluster must specify the same cluster manager.

## Publishing application

A publishing application allows individual Aurora Imaging Library objects to be accessed by a monitoring application. This is called publishing an Aurora Imaging Library object. A publishing application can only be connected to a single monitoring application at a time.

To create a publishing application:

1. Allocate an Aurora Imaging Library application context using [`MappAlloc`](../../Reference/app/MappAlloc.md) with its [`ServerDescription`](../../Reference/app/MappAlloc.md) parameter set to the name or IP address of the computer designated as the cluster manager.
2. Optionally, specify a non-default listening port for the publishing application using [`M_DAIL_CONNECTION_PORT`](../../Reference/app/MappControl.md). To use the default listening port, set using the Aurora Imaging Configurator utility, you do not need to explicitly specify it.
   > **Note:** Note that the port must be unique if more than one Aurora Imaging Library application will publish objects from the same computer.
3. Set the global permission level for the application's objects to read/write or read only, using [`MappControl`](../../Reference/app/MappControl.md) with [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) set to either [`M_DAIL_CONTROL`](../../Reference/app/MappControl.md) or [`M_DAIL_MONITOR`](../../Reference/app/MappControl.md), respectively.
   This activates the Distributed Aurora Imaging Library server and allows the monitoring application to connect to this publishing application.
4. Set the access to a particular Aurora Imaging Library object, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_DAIL_PUBLISH`](../../Reference/obj/MobjControl.md) set to either [`M_READ_WRITE`](../../Reference/obj/MobjControl.md) or [`M_READ_ONLY`](../../Reference/obj/MobjControl.md). Any Aurora Imaging Library object with an Aurora Imaging Library identifier can be published in this manner, except for Aurora Imaging Library displays ([`MdispAlloc`](../../Reference/disp/MdispAlloc.md)).
   An Aurora Imaging Library object cannot have greater permission than the global permission level for the application. For instance, if the application permission level is set to read only, no individual Aurora Imaging Library object in the application can be set to read/write. If the application permission level is set to disable ([`MappControl`](../../Reference/app/MappControl.md) with [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) set to [`M_DISABLE`](../../Reference/app/MappControl.md)), no individual Aurora Imaging Library object in the application can be set to either read only or read/write.
5. Optionally, name the published Aurora Imaging Library objects, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md) set to any string. If an object is named, you can more easily identify it from the monitoring application using either [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md) or [`MobjInquire`](../../Reference/obj/MobjInquire.md).

For additional details, you can run/view the _publishingapplication.cpp_ example in Aurora Imaging Example Launcher.

## Monitoring application

A monitoring application can access Aurora Imaging Library objects published by a publishing application. This access can be read/write or read only. A monitoring application can be connected to multiple publishing applications at the same time.

To create a monitoring application:

1. Allocate an Aurora Imaging Library application context using [`MappAlloc`](../../Reference/app/MappAlloc.md) with its [`ServerDescription`](../../Reference/app/MappAlloc.md) parameter set to the name or IP address of the computer designated as the cluster manager.
2. Open a connection to a remote or local computer that is running a publishing application, using [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md). You must specify the monitoring application's connection port if its default connection port, set using the Aurora Imaging Configurator utility, is different from the publishing application's listening port.
   Note that the publishing application must have called [`MappControl`](../../Reference/app/MappControl.md) with [`M_DAIL_CONNECTION`](../../Reference/app/MappControl.md) set to either [`M_DAIL_CONTROL`](../../Reference/app/MappControl.md) or [`M_DAIL_MONITOR`](../../Reference/app/MappControl.md) before a connection can take place.
3. Inquire the Aurora Imaging Library object's _AIL_ID_ from the publishing application, using [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md).
   - If you know the name that the publishing application assigned to the object (using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_OBJECT_NAME`](../../Reference/obj/MobjControl.md)), inquire that Aurora Imaging Library object's Aurora Imaging Library identifier using [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md) set to [`M_DAIL_PUBLISHED_NAME`](../../Reference/app/MappInquireConnection.md).
   - Inquire a list of published Aurora Imaging Library identifiers using [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md) with [`InquireType`](../../Reference/app/MappInquireConnection.md) set to [`M_DAIL_PUBLISHED_LIST`](../../Reference/app/MappInquireConnection.md). You can limit this list to all published Aurora Imaging Library identifiers of a single Aurora Imaging Library object type by also specifying the required type of object. For example, you can inquire the list of all image buffers published by a specific publishing computer using [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md) with [`M_DAIL_PUBLISHED_LIST`](../../Reference/app/MappInquireConnection.md) and [`M_IMAGE`](../../Reference/app/MappInquireConnection.md).
4. Repeat step 2 and step 3 for each publishing application you want the monitoring application to connect to.

For additional details, you can run/view the _monitoringapplication.cpp_ example in Aurora Imaging Example Launcher.

## Using published Aurora Imaging Library identifiers

A monitoring application uses the inquired Aurora Imaging Library identifier from a publishing application as it would a local Aurora Imaging Library identifier. The only difference is that it uses the Aurora Imaging Library identifier of a published object rather than a local object.

A monitoring application will typically process its functions on the local computer. For example, the monitoring application could call [`MimResize`](../../Reference/im/MimResize.md) and use an image buffer published by a publishing application as the source image. For the destination image, the function could specify a local image buffer or a published image buffer that is set to read/write. Regardless of the destination, the function would be processed on the local computer.

In the process of using a published object, the monitoring application creates a local temporary copy of the published object, and then quickly deletes the local copy after using it. Despite deleting the local copy, the monitoring application retains a link to the original published object.

## Displaying a published image buffer

The monitoring application can display an image buffer from a publishing application, provided that the published image buffer had originally been allocated with the [`M_DISP`](../../Reference/buf/MbufAlloc1d.md) attribute. Note that the monitoring application cannot use a display from a publishing application. Publishing a display object will result in an error.

To display a published image buffer, the monitoring application must allocate a display using [`MdispAlloc`](../../Reference/disp/MdispAlloc.md). It must then select the published displayable image buffer using [`MdispSelect`](../../Reference/disp/MdispSelect.md).

Any changes to the published image buffer in the publishing application will trigger the monitoring application to get the new version of the image buffer and automatically update its display. The publishing application does not have to display its own displayable image buffer for the monitoring application to display the image buffer.

> **Note:** Note that if both the monitoring and publishing applications are displaying the same image buffer, there might be a small difference in the rate at which images grabbed or altered in this buffer are displayed on both computers, due to bandwidth limitations.
