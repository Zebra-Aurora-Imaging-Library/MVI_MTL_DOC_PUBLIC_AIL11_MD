---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: AIWL_overview
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / AIWL overview"
---

# Aurora Imaging Web Library overview

You can monitor your Aurora Imaging Library application from one or more remote computers by configuring it to be an Aurora Imaging Web Library server application. When Aurora Imaging Web Library functionality is enabled for your application, you can make some Aurora Imaging Library objects (such as 2D and 3D displays) available over the network. Published Aurora Imaging Library objects can be accessed by an Aurora Imaging Web Library client application, which you write using the provided Aurora Imaging Web Library JavaScript or C/C++ API.

The Aurora Imaging Web Library JavaScript API is suitable for writing a client application that can run in a compatible web browser on a variety of devices (including computers, phones, and tablets) under a variety of operating systems (including Windows, Linux, MacOS, Android, iOS, and others). The Aurora Imaging Web Library C/C++ API is suitable for writing a standalone client application that can run under Windows or Linux. Regardless of the chosen API, the remote computer is not required to have Aurora Imaging Library installed, have an Aurora Imaging Library license, nor contain Zebra hardware to run an Aurora Imaging Web Library client application.

*[Image: WebOverview.png]*

You can create a single Aurora Imaging Web Library client application that simultaneously connects to multiple Aurora Imaging Web Library server applications. You can also create more than one client application for connecting to the same server application. For example, it can be useful to have one client application used to monitor a single vision controller in an HMI, and another used to monitor several vision controllers simultaneously from another location. You do not need to use the same Aurora Imaging Web Library API (JavaScript or C/C++) for each client application you create.

## Capabilities of Aurora Imaging Web Library client applications

Both the Aurora Imaging Web Library C/C++ API and Aurora Imaging Web Library JavaScript API are designed for you to create an Aurora Imaging Web Library client application that performs the following tasks:

- Show 2D displays published by the Aurora Imaging Web Library server application (including annotations).
  For example, you might select the last processed image to a display in your Aurora Imaging Library application, and show that display in your Aurora Imaging Web Library client application.
- Manipulate displays of the Aurora Imaging Web Library server application (including annotations).
  For example, you might design your Aurora Imaging Web Library client application to allow the user to interactively pan, zoom, and annotate a display of your Aurora Imaging Library application.
- Receive other information from the Aurora Imaging Web Library server application in the form of messages.
  For example, you might design your Aurora Imaging Library application to send a message to your Aurora Imaging Web Library client application indicating whether the last image passed or failed an inspection process. You can then show this pass or failure result to the user in your client application.
- Send information to the Aurora Imaging Web Library server application in the form of messages.
  For example, your server application might have some operational settings that can be changed at runtime (such as which digitizer to use for grabbing images, or the parameter settings for image processing functions). You can design your Aurora Imaging Web Library client so that the user can select new settings from an interface; your client can then send the updated setting in a message, which is received and applied by your Aurora Imaging Library application.
  > **Note:** Since you can send any type of information in a message, you can control any aspect of your Aurora Imaging Library application using messages by establishing your own protocol. For example, you might design your Aurora Imaging Web Library server application to grab an image every time an Aurora Imaging Web Library client application sends the number 12345 in a message. For more information, see[Aurora Imaging Web Library messages](S03_Using_AIWL_to_monitor_your_application.md).
