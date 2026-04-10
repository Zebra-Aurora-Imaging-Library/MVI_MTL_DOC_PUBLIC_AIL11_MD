---
doctype: UserGuide
part: "Other programming languages, Web API, and operating systems"
chapter: Using_AIWL_to_monitor_your_application
section: Creating_an_AIWL_client_with_JavaScript
module_tag: null
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Other programming languages, Web API, and operating systems / AIWL / Creating an AIWL client with JavaScript"
---

# Creating an Aurora Imaging Web Library client application with JavaScript

The Aurora Imaging Web Library JavaScript API is suitable for writing a client application that can run in a compatible web browser on a wide variety of platforms.

> **Note:** This section only discusses information specific to the Aurora Imaging Web Library JavaScript API. For general information about creating an Aurora Imaging Web Library client application, see [Fundamentals for creating your Aurora Imaging Web Library client application](S03_Using_AIWL_to_monitor_your_application.md). For details about the JavaScript versions of specific Aurora Imaging Web Library functions, see[JavaScript Aurora Imaging Web Library function reference](S05_JavaScript_AIWL_function_reference.md).

## JavaScript primer for C/C++ Aurora Imaging Library developers

Since JavaScript shares much of its syntax with C/C++, in general you should find it easy to write a simple JavaScript application that uses Aurora Imaging Web Library. It is useful to bear in mind the following differences between C/C++ and JavaScript:

- JavaScript is an interpreted language. You do not need to compile JavaScript code, nor do you need any kind of linking mechanism to access_ ail.js_ (except for putting it in the folder with your application). You can create and modify your JavaScript application using any text editor. You can run your application in a compatible web browser by calling your main JavaScript function from within an HTML file (as long as the HTML file, your application source code, and _ail.js_ are in the same folder). For more information, see [Running your JavaScript Aurora Imaging Web Library client application](S04_Creating_an_AIWL_client_with_JavaScript.md).
- JavaScript is a dynamically-typed language. You do not assign a data type to variables; the type of data in a variable is determined automatically each time you assign a value to it. In general, casting is done automatically when required (and possible). For example, in the following code, _multiplicationResult_is set to the number 30 (_someVariable_ is automatically cast as a _number_), and_printableOutput_ is set to the string "That other variable is the number 30" (_multiplicationResult_is automatically cast as a _string_):
  ```
  var someVariable = "5";
  var multiplicationResult = someVariable * 6;
  var printableOutput = "That other variable is the number " + multiplicationResult;
  ```
  This can lead to ambiguous situations, in which the JavaScript interpreter must decide how to cast a variable. For example in the following code, the JavaScript interpreter must decide whether to cast_firstVariable_ and_secondVariable_ as strings or numbers for the purposes of addition. In this case, _firstAddition_ is set to the number 8, while _secondAddition_is set to the string "26":
  ```
  var firstVariable = 2;
  var firstAddition = firstVariable + 6;
  var secondVariable = "2";
  var secondAddition = secondVariable + 6;
  ```
  You can determine what data type is currently stored in a variable using the built-in function `typeof`. For example,`typeof(2)`returns "number", while`typeof("2")`returns "string". Note that this does not work for arrays; `typeof(anArray)`returns "object".
  > **Note:** The Aurora Imaging Web Library JavaScript function reference lists the expected data type for each parameter. If you set a parameter to a variable that stores a different type of data, the JavaScript interpreter will attempt to cast the data to the correct data type. If a cast is not possible, either an error is generated or the value "undefined" is used. You should be aware of this when debugging your Aurora Imaging Web Library client application.
- JavaScript does not distinguish between integers and decimal values. All types of numbers have the data type _number_, unless they were defined by values within quotation marks; in this case, they have the data type _string_.
- An array is a type of JavaScript object. Arrays in JavaScript are similar to vectors in C++. You can increase or decrease the size of an array after it has been defined. You do not need to declare that a variable is an array object before assigning values to it, nor do you need to explicitly specify the size of an array.
- JavaScript objects are similar to C++ objects, but the terminology is different. Member variables in C++ are referred to as properties in JavaScript. Member functions in C++ are referred to as methods in JavaScript.
- Web browsers typically execute JavaScript in a single thread. However, some procedures (such as establishing a connection to an Aurora Imaging Web Library server) are completed asynchronously.

To learn more about using JavaScript, refer to third-party resources such as the Mozilla JavaScript documentation at [developer.mozilla.org/en-US/docs/Web/JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

## Aurora Imaging Library primer for JavaScript developers

The Aurora Imaging Web Library JavaScript API is closely modeled on the Aurora Imaging Library C API. Unlike JavaScript, C is not an object-oriented programming language. However, Aurora Imaging Library implements an object-oriented model within the framework of C. The following information will help you to understand the model for accessing Aurora Imaging Library objects.

- Each Aurora Imaging Library object has a client-side Aurora Imaging Library identifier. Instead of accessing the Aurora Imaging Library object directly, you store the client-side identifier of the Aurora Imaging Library object in a variable and pass that variable to a function.
  For example, you can zoom an Aurora Imaging Web Library display using the function [AIWL.MdispZoom](S05_JavaScript_AIWL_function_reference.md). This function is not a method of the display; it is a global function that you call with the client-side identifier of the display, as shown below. The first line inquires the identifier of the display named "MyDisplay". The second line zooms the display by calling [AIWL.MdispZoom](S05_JavaScript_AIWL_function_reference.md) with that identifier.
  ```
  var displayIdentifier = AIWL.MappInquireConnection(AIWLApp, AIWL.M_WEB_PUBLISHED_NAME, "MyDisplay", AIWL.M_DEFAULT);
  AIWL.MdispZoom(displayIdentifier, 2, 2);
  ```
  > **Note:** For clarity, this example assumes that a connection has already been established with the Aurora Imaging Web Library display using a previous call to [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md). Normally, you cannot access an Aurora Imaging Library object immediately after inquiring its client-side Aurora Imaging Library identifier. For more information, see [Accessing Aurora Imaging Web Library server applications and published Aurora Imaging Library objects](S04_Creating_an_AIWL_client_with_JavaScript.md).
- You do not access the properties of an Aurora Imaging Library object directly. Instead, you use a control or inquire function with the setting that specifies the property that you want to access. You can think of control and inquire functions as setter and getter functions for the Aurora Imaging Library object that you specify (using its client-side Aurora Imaging Library identifier).
- In Aurora Imaging Web Library, there are functions that take a callback function and set it up as the event handler for a specific type of event (hooks it to the event). When the event then happens, the callback function is executed. The Aurora Imaging Library functions capable of attaching (hooking) a callback function to an event are referred to as hook-type functions (for example,[AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md)), and the callback functions are referred to as hook-handler functions.
  Within a hook-handler function, you can retrieve information about the event that caused the hook-handler function to be called, using a hook-information function (for example,[AIWL.MappGetHookInfo](S05_JavaScript_AIWL_function_reference.md)).
  Hook-handler functions should have the prototype `function HookHandler(HookType, EventId, UserVar)`. The following is an example of a hook-handler function, previously hooked to `AIWL.M_CONNECT` events using[AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md), which stores the client-side Aurora Imaging Library identifier of the connected object.
  ```
  function NameOfYourFunction(hookType, eventId, userVar) {
  var ObjectId = AIWL.MobjGetHookInfo(eventId, AIWL.M_OBJECT_ID);
  }
  ```
  Optionally, when you hook a function, you can specify a JavaScript object that will be passed to the hook-handler function when it is called. This object is passed to the hook-handler function through its `UserVar` parameter. Omit this parameter if not used.

## Writing an Aurora Imaging Web Library client application in JavaScript

Your JavaScript Aurora Imaging Web Library client application must use the _ail.js_ library. The file is located in the installed ailweb folder (for example, _C:\Program Files\Aurora Imaging Library\Tools\ailweb_). This library provides functions to access Aurora Imaging Library objects, hook to updates, present a display in an HTML5 canvas, and send input data to your Aurora Imaging Web Library server application. The Aurora Imaging Web Library JavaScript API is closely modeled on the standard Aurora Imaging Library API.

### Differences between Aurora Imaging Library and Aurora Imaging Web Library

You should be aware of the following differences between Aurora Imaging Library and the Aurora Imaging Web Library JavaScript API:

- Client-side Aurora Imaging Library identifiers are not valid until Aurora Imaging Web Library has asynchronously completed the connection to the Aurora Imaging Web Library server application or Aurora Imaging Library object. For more information, see [Accessing Aurora Imaging Web Library server applications and published Aurora Imaging Library objects](S04_Creating_an_AIWL_client_with_JavaScript.md).
- Client-side Aurora Imaging Library identifiers have the data type _number_.
- JavaScript does not have a pass-by-pointer or pass-by-reference mechanism for number and string data types. Aurora Imaging Web Library functions that return data in a parameter instead write the data to the `data`property of a JavaScript object that you specify. Typically, this is optional, since most Aurora Imaging Web Library functions also return their result ([AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) is a notable exception). For more information, see[Returned values in Aurora Imaging Web Library](S04_Creating_an_AIWL_client_with_JavaScript.md).
- Hook-handler functions must be declared as: `function hookHandler(hookType, eventId, UserVar)`.
- [AIWL.MdispSelectWindow](S05_JavaScript_AIWL_function_reference.md) does not select an image buffer to a display; it presents a display in an existing HTML5 canvas.
- For a 3D display, only the final rendered image of the display is transmitted to your Aurora Imaging Web Library client application. 3D displays are therefore functionally equivalent to 2D displays from the perspective of your client. For this reason, with Aurora Imaging Web Library you use functions from the [AIWL.Mdisp](S05_JavaScript_AIWL_function_reference.md) module to work with both 2D and 3D displays (you still use functions from the [`M3ddisp...`](../../Reference/3ddisp/M3ddispControl.md) module to work with 3D displays in your Aurora Imaging Web Library server application).

### Returned values in Aurora Imaging Web Library

To maintain similarity with the Aurora Imaging Library API, many functions in Aurora Imaging Web Library can both return their result (such as an inquired value) and/or store it in an object that you specify. However, unlike C and C++, JavaScript does not have a pass-by-pointer or pass-by-reference mechanism for number and string data types (numbers and strings are always passed to functions by value; they cannot be used as "out" parameters). Conversely, JavaScript objects are always passed by sharing, which is functionally equivalent to pass-by-reference. Therefore, to mirror Aurora Imaging Library, functions in the Aurora Imaging Web Library API write their result to the `data` property of the specified JavaScript object.

> **Note:** You do not need to declare a`data`property for the object beforehand; Aurora Imaging Web Library does this automatically. Any existing properties of the object are removed.

For example, the following code creates the JavaScript object variable `UserVar`, and then uses it to store the result of an Aurora Imaging Web Library function:

```
var UserVar = {};
AIWL.MappOpenConnection("ws://localhost:7861", AIWL.M_DEFAULT, AIWL.M_DEFAULT, UserVar);
var AppId = UserVar.data;
```

After this code is executed, _AppId_stores the Aurora Imaging Library application identifier of the Aurora Imaging Web Library server application. Note that storing the data in another variable (_AppId_) is not necessary; however, doing so will typically improve the readability of your program.

### Accessing Aurora Imaging Web Library server applications and published Aurora Imaging Library objects

Before you pass the identifier of an Aurora Imaging Web Library server application or published Aurora Imaging Library object to an Aurora Imaging Web Library function, you must first establish a connection between your Aurora Imaging Web Library client application and the server application or Aurora Imaging Library object. You do this using [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) (for the server application) and using[AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md)with `AIWL.M_WEB_PUBLISHED_NAME` (for published Aurora Imaging Library objects). You cannot use [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) until a connection to an Aurora Imaging Web Library server application is established.

Each published Aurora Imaging Library object occupies one WebSocket in the user's web browser. If the web browser limits the number of open WebSockets per connection, this also limits the number of published Aurora Imaging Library objects that the Aurora Imaging Web Library client application can access.

Both[AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) and [AIWL.MappInquireConnection](S05_JavaScript_AIWL_function_reference.md) are executed asynchronously. Your program will continue to execute after calling one of these functions, before Aurora Imaging Web Library has established the connection. To ensure that code is executed after a connection event has finished, you must put that code in a hook-handler function. You can hook to connection events using[AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md) or [AIWL.MobjHookFunction](S05_JavaScript_AIWL_function_reference.md)with `AIWL.M_CONNECT`.

> **Note:** This does not apply to the Aurora Imaging Web Library C/C++ API. For C/C++, these functions are executed synchronously. Therefore, you do not need to hook to connection events, or be aware of connections to individual objects.

To hook a function to an application or object connection event, you must pass the client-side Aurora Imaging Library identifier of the Aurora Imaging Web Library server application or object to the appropriate function ([AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md) or [AIWL.MobjHookFunction](S05_JavaScript_AIWL_function_reference.md)). This identifier is returned when you initiate the connection (for example, using [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md)). You should hook to the connection event immediately after calling [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) to ensure that the hook-handler function is executed when the connection is finally established. For example:

```
var UserVar = {}; 
var URL = "ws://192.168.1.58:7861";
AIWL.MappOpenConnection(URL, AIWL.M_DEFAULT, AIWL.M_DEFAULT, UserVar);
var AppId = UserVar.data;
AIWL.MappHookFunction(AppId, AIWL.M_CONNECT, ConnectHandler);
```

Since [AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md) returns before the connection is established (asynchronously), the `ConnectHandler`callback function will be executed on the connection event (even though [AIWL.MappHookFunction](S05_JavaScript_AIWL_function_reference.md) is called after[AIWL.MappOpenConnection](S05_JavaScript_AIWL_function_reference.md)).

> **Note:** [AIWL.MappCloseConnection](S05_JavaScript_AIWL_function_reference.md)is also executed asynchronously. You can hook to the disconnection event using`AIWL.M_DISCONNECT`.

### Running your JavaScript Aurora Imaging Web Library client application

To test a JavaScript Aurora Imaging Web Library client application, you must create a simple HTML page and access that page from an HTML5-compatible web browser.

1. Create the application folder (for example, _C:\working\YourAuroraImagingWebLibraryClientApplication_).
2. Copy the Aurora Imaging Web Library JavaScript library (_ail.js_) to the application folder. The file is located in the installed ailweb folder (for example, _C:\Program Files\Aurora Imaging Library\Tools\ailweb_).
3. Save the source code for your JavaScript application as a file with the js file extension (for example, _MyAuroraImagingWebLibraryClientApplication.js_) and copy this file to the application folder.
4. Create an HTML page that loads the _ail.js_ library and your JavaScript application. For example:
   ```
   <script type="text/javascript" src="ail.js"></script>
   <script type="text/javascript" src="MyAuroraImagingWebLibraryClientApplication.js"></script>
   ```
   If you want to present a display in your Aurora Imaging Web Library client application, also create an HTML5 canvas in your HTML page. For example:
   ```
   <canvas id="displaycanvas" width="640" height="480"> </canvas>
   ```
   > **Note:** You can present an Aurora Imaging Library display in this HTML5 canvas by passing the `id` attribute of the canvas to[AIWL.MdispSelectWindow](S05_JavaScript_AIWL_function_reference.md).
   Save this page with the HTML file extension, in the application folder.
5. Open the HTML file with a[compatible web browser](S04_Creating_an_AIWL_client_with_JavaScript.md).

### Deploying your Aurora Imaging Web Library client application

Once you have finished developing your Aurora Imaging Web Library client application and embedding it in a web page, typically you will want to make it available for access by one or more remote computers. In most cases, you should use one of the following methods to make the web page available:

- Host the web page files on an Aurora Imaging Library HTTP server. For more information, see [Hosting files on an Aurora Imaging Library HTTP server](../C64_HTTP_servers/S01_Hosting_files_on_a_HTTP_server.md).
- Host the web page files on an HTTP server using third-party software.
- Manually copy the web page files to the remote computer (for example, using a network share or USB drive).
  > **Note:** If your Aurora Imaging Web Library client application does not work correctly when you access the HTML files directly on the remote computer, use an HTTP server instead.

This animation shows the relationship between a web-enabled device, an Aurora Imaging Library HTTP server object hosting the Aurora Imaging Web Library client application, and the Aurora Imaging Web Library server application.

*[Image: AIWL_HTTP_And_Client]*

## Browser compatibility

The Aurora Imaging Web Library JavaScript library requires a web browser that supports both JavaScript and the HTML5 WebSocket protocol. Aurora Imaging Web Library client applications have been tested successfully with the desktop versions of the following third-party web browsers:

- Google Chrome (version 16 and higher).
- Microsoft Edge (all versions).
- Microsoft Edge Legacy (all versions).
  > **Note:** This browser is limited to accessing 5 published Aurora Imaging Library objects (such as displays) at a time.
- Mozilla Firefox (version 11 and higher).

> **Note:** Note that, by default, some web browsers have a file URI strict-origin policy. This means that they treat each HTML file on your computer as though it was downloaded from a different source. This is a security feature to mitigate cross-site scripting attacks; however, if you are accessing your Aurora Imaging Web Library client application files locally (for example, using a path that starts with `file://` instead of `http://`) it can also prevent scripts from loading correctly (especially if your web page uses more than one HTML source file). If your Aurora Imaging Web Library client application does not work correctly when you access the HTML files directly, use an HTTP server instead (for example, an [Aurora Imaging Library HTTP server](../C64_HTTP_servers/S01_Hosting_files_on_a_HTTP_server.md)).

The Aurora Imaging Web Library JavaScript library adheres to industry-standard JavaScript development practices, including standards-compliant use of the HTML5 WebSocket protocol. However, third-party web browsers are frequently updated. Zebra can only guarantee compatibility with the versions of these third-party browsers that were current when the most recent Aurora Imaging Web Library driver update was released.
