---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Basic_concepts
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Basic concepts"
---

# Basic concepts for Aurora Imaging Web Library

The basic concepts and vocabulary conventions for Aurora Imaging Web Library are:

- **Aurora Imaging Web Library server application**. Any Aurora Imaging Library application with server functionality enabled. For brevity, this is sometimes shortened to Aurora Imaging Web Library server.
- **Aurora Imaging Web Library client application**. An application that connects to an Aurora Imaging Web Library server application, using either the Aurora Imaging Web Library JavaScript library or the Aurora Imaging Web Library C/C++ library. For brevity, this is sometimes shortened to Aurora Imaging Web Library client.
  Since an Aurora Imaging Web Library client application does not require Aurora Imaging Library to be installed, it is not considered to be an Aurora Imaging Library application.
- **Instance of an Aurora Imaging Web Library client application**. Your Aurora Imaging Web Library client application, running on a particular computer. It is possible for your client application to be run on multiple computers simultaneously; each of these computers is running its own instance of the application. Some Aurora Imaging Web Library functions affect only the local instance of your application. Some functions affect both the server application and the local instance of the client application. Some functions affect both the server application and all connected instances of the client application.
- **Client-side Aurora Imaging Library identifier**. The Aurora Imaging Library identifier used in your Aurora Imaging Web Library client application to identify an Aurora Imaging Web Library server application or published Aurora Imaging Library object. Each instance of your client application uses a different client-side Aurora Imaging Library identifier to refer to the same server application or published Aurora Imaging Library object. Therefore, you cannot share or compare client-side Aurora Imaging Library identifiers between instances of your client application, or with the server application.
  For JavaScript, the client-side Aurora Imaging Library identifier of an Aurora Imaging Web Library server application (or a published object) is assigned before the connection is established with that particular server application (or published object). Therefore, the client-side Aurora Imaging Library identifier of an object can be inquired at any time, but is not usable until the connection is made and a connection event is generated.
- **Aurora Imaging Library HTTP server**. An Aurora Imaging Library object that you can use to host files on the local computer so that they are accessible in a web browser by remote computers. Typically, you can use an Aurora Imaging Library HTTP server to host your Aurora Imaging Web Library client application. In this case, users access your client application on the HTTP server (using a compatible web browser), and the client application connects to the server.
  Using an Aurora Imaging Library HTTP server is optional; you can distribute your Aurora Imaging Web Library client application to users by other means (for example, using third-party HTTP server software or manually copying the application files to the remote computer using a USB storage device).
