---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Miscellaneous_AIWL_notes_User_guide
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Miscellaneous AIWL notes User guide"
---

# Miscellaneous user guide information for Aurora Imaging Web Library

This section includes additional user guide information for Aurora Imaging Web Library. The information found in this section might be a reiteration of content previously documented.

## Server application

The server is an Aurora Imaging Library application that publishes objects to the web using the following functions:

[`MappControl`](../../Reference/app/MappControl.md) with [`M_WEB_CONNECTION`](../../Reference/app/MappControl.md).

[`MappInquire`](../../Reference/app/MappInquire.md) with [`M_WEB_CONNECTION_PORT`](../../Reference/app/MappInquire.md).

[`MappHookFunction`](../../Reference/app/MappHookFunction.md) with [`M_WEB_CLIENT_CONNECTED`](../../Reference/app/MappHookFunction.md) and [`M_WEB_CLIENT_DISCONNECTED`](../../Reference/app/MappHookFunction.md).

[`MappGetHookInfo`](../../Reference/app/MappGetHookInfo.md) with [`M_WEB_CLIENT_INDEX`](../../Reference/app/MappGetHookInfo.md).

[`MdispAlloc`](../../Reference/disp/MdispAlloc.md) where the InitFlag parameter is set to [`M_WEB`](../../Reference/disp/MdispAlloc.md).

[`MobjControl`](../../Reference/obj/MobjControl.md) with **M_UPDATE_START**, **M_UPDATE_END**, and [`M_WEB_PUBLISH`](../../Reference/obj/MobjControl.md).

## Deploying web client application

JavaScript applications are integrated in HTML pages running in a web browser locally or remotely. The web application can be run in any recent web browser that support HTML5 features.

To make the application accessible from a remote PCs, create a web application that is available to other PCs using the Aurora Imaging Library HTTP API or other web server (such as IIS or Apache). If you are using other web servers, refer to the web server's documentation to make the contents of the application folder (App1) web accessible.

You can access the web page from a remote web browser using: `http:://[THIS_PC_NAME/THIS_PC_IP]/page.html`, where `[THIS_PC_NAME/THIS_PC_IP]` is your computer's name or IP address.

### Web browser particularities

The performance and functionality of a web application can be affected by the browser, its version, installed extensions, and add-ons.

Aurora Imaging Web Library has been tested with the following browsers:

- Internet Explorer (version version 11 and higher).
- Microsoft Edge. Supports up to 5 websockets at once. A web application can access up to 5 published objects at a time.
- Chrome.
- Firefox.

## C/C++ display

[`MdispSelect`](../../Reference/disp/MdispSelect.md) is not supported. Instead, you will need to get the image data and blit it to a user-defined window.

[`MdispSelectWindow`](../../Reference/disp/MdispSelectWindow.md) is also not supported.
