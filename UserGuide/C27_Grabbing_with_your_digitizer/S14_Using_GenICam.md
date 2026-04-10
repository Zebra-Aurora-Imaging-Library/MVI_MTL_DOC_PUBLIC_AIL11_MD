---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Using_GenICam
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Using GenICam"
---

# Using Aurora Imaging Library with GenICam

Aurora Imaging Library supports the use of GenICam when dealing with GigE Vision, CoaXPress (CXP), USB3 Vision, and Camera Link cameras (the latter might require third-party software). GenICam is an industry standard designed to provide a common software interface for machine vision cameras, and is administered by the European Machine Vision Association (EMVA). For more information on GenICam and the GenICam standard feature naming convention (SFNC), see: [emva.org/standards-technology/genicam/](https://www.emva.org/standards-technology/genicam/). For more information regarding which boards can connect to the above-mentioned cameras, refer to the Aurora Imaging Library _Hardware-specific Notes._

> **Note:** Note that when dealing with Camera Link cameras, you must first install, identify, and enable the CLProtocol for your camera; to do so, refer to [Using GenICam with Camera Link cameras](S14_Using_GenICam.md).

## Basic concepts when dealing with GenICam

The following terms are common when dealing with GenICam.

- **Camera's device description file** (XML). An XML file on your camera that contains data used to identify the camera's features (such as, the feature name, type, and value). A copy of this XML file is also stored on your computer. The camera description file on your computer and the information in the camera are synchronized when the camera's features are inquired or written. For some cameras, some features (such as the camera's temperature), are also synchronized at a polling period with a regular, camera-defined interval. The polling period can be enabled or disabled using [`MdigControl`](../../Reference/dig/MdigControl.md)with[`M_GC_FEATURE_POLLING`](../../Reference/dig/MdigControl.md).
- **Feature**. A specific property of the camera (such as, the pixel depth) that can be controlled and/or inquired through the camera's device description file. Features are subdivided into several different types, such as features of type category (a group of features), command (an action), and features that store data (including features of type string, boolean, and double). Note that while the feature types that store data have a value that can be inquired, category type features and command type features do not.

## Steps to view your GenICam-compliant camera's features

Aurora Imaging Library provides several ways to access your GenICam-compliant camera's features. When dealing with an existing Aurora Imaging Library program, or an application written to work regardless of the camera and frame-grabber combination, you can create a generalized application by using the traditional Aurora Imaging Library control and inquire types (for example, those in [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md)) that are listed as supported for your Aurora Imaging Library system). When dealing with an application written only for acquisition with GenICam-compliant cameras, you can access specific features of your camera directly using [`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md).

> **Note:** Not all GenICam features can be accessed using [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md).

In [`MdigControl`](../../Reference/dig/MdigControl.md) and[`MdigInquire`](../../Reference/dig/MdigInquire.md), if a standard GenICam SFNC concept is similar to an existing Aurora Imaging Library concept, the traditional Aurora Imaging Library constant is used. All other constants that are used to access GenICam SFNC-specific features start with [`M_GC...`](../../Reference/dig/MdigControl.md). These constants are only supported with GenICam SFNC-compliant products.

Your application can parse the camera's device description file (XML) and return a list of all the features of the camera, starting from the highest level feature of the XML structure (the root) or from a specified feature name (provided the specified feature name is a category containing additional features). To accumulate a complete list of the GenICam features available on your camera, inquire the list of features, by performing the following:

1. Count the number of features in the camera's device description file at the highest-level feature of the XML structure (using [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md) with [`AIL_TEXT("Root")`](../../Reference/dig/MdigInquireFeature.md) and [`M_SUBFEATURE_COUNT`](../../Reference/dig/MdigInquireFeature.md)).
2. For each feature, inquire the feature's name (using [`M_SUBFEATURE_NAME + n`](../../Reference/dig/MdigInquireFeature.md)), and feature type (using [`M_SUBFEATURE_TYPE + n`](../../Reference/dig/MdigInquireFeature.md)). If the feature type is not a category or a command, inquire the feature's value (using [`M_FEATURE_VALUE`](../../Reference/dig/MdigInquireFeature.md)). Print the result to the screen.
3. Optionally, inquire additional attributes of each subfeature, using [`AIL_TEXT("FeatureName")`](../../Reference/dig/MdigInquireFeature.md).
   > **Note:** Note that features that are commands and categories do not have a feature value (that is, [`M_FEATURE_VALUE`](../../Reference/dig/MdigInquireFeature.md)); all other attributes of the feature can be inquired (for example, its description, size, tooltip, and type).
4. Repeat this process for each category found, replacing Root with the name of the category.

The _enumfeatures.cpp_ example lists the available features, starting from the root. To run/view this and other examples, use Aurora Imaging Example Launcher.

Other ways to view the GenICam-compliant camera's features include:

- Opening Feature Browser using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md).
- Opening Feature Browser from Aurora Imaging Capture Works. In the utility, select your camera from the tree of available cameras, and then click on the **Feature Browser** button*[Image: Feature_Browser_button.PNG]*
  
  .
- Opening Feature Browser from Aurora Imaging Intellicam. In the utility, select your camera from the drop-down list of available cameras, and then click on the **Feature Browser** button*[Image: Feature_Browser_button.PNG]*
  
  .

## Using GenICam with Camera Link cameras

Aurora Imaging Library supports using the GenICam CLProtocol to allow you to communicate with your Camera Link camera using the GenICam standard. Within your application, you must enable this functionality for each camera individually. If your camera is not GenCP-compliant, you must also first install the third-party, vendor-supplied, standard-complaint GenICam CLProtocol library for that camera.

> **Note:** Note that using the GenICam CLProtocol will occupy your Camera Link connection's COM port until GenICam CLProtocol is disabled.

To configure your digitizer to use the GenICam CLProtocol, you must select and enable the device ID used by the installed GenCP or third-party CLProtocol library to identify your Camera Link camera. To do so:

1. Determine the number of device IDs supported by installed GenICam CLProtocol libraries (including GenCP), using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CLPROTOCOL_DEVICE_ID_NUM`](../../Reference/dig/MdigInquire.md).
2. Determine the maximum string length required to store the device IDs using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CLPROTOCOL_DEVICE_ID_SIZE_MAX`](../../Reference/dig/MdigInquire.md).
3. Generate a listing of the device IDs using [`MdigInquire`](../../Reference/dig/MdigInquire.md) with [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigControl.md) for each device ID.
4. Using this listing, identify the device ID that matches your camera. If your camera is GenCP, select the device ID that matches the required GenCP version.
5. Specify the appropriate device ID using [`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_CLPROTOCOL_DEVICE_ID`](../../Reference/dig/MdigControl.md)with the device ID.
6. Enable the GenICam CLProtocol for your camera by using [`MdigControl`](../../Reference/dig/MdigControl.md)with [`M_GC_CLPROTOCOL`](../../Reference/dig/MdigControl.md).

The _CLProtocol.cpp_ example enumerates the device IDs supported by both the Aurora Imaging Library-supplied and third-party GenICam CLProtocol libraries installed on your computer, and provides some additional information relating to each device ID to help you select the right one. In addition, it initializes GenICam feature values and opens a Feature Browser, populated with those values. It also inquires the camera's name and model (using [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md)) and prints this information to screen. To run/view this and other examples, use Aurora Imaging Example Launcher.

Once the GenICam CLProtocol is enabled for use with your Camera Link camera, you can use any of the GenICam interfaces to control and inquire your camera's settings. You can do this using[`MdigControlFeature`](../../Reference/dig/MdigControlFeature.md) and [`MdigInquireFeature`](../../Reference/dig/MdigInquireFeature.md). You can also use many Aurora Imaging Library constants in [`MdigControl`](../../Reference/dig/MdigControl.md) and [`MdigInquire`](../../Reference/dig/MdigInquire.md) (provided that these constants are marked as supported for your board). These Aurora Imaging Library constants typically start with [`M_GC...`](../../Reference/dig/MdigControl.md). You can also use the Feature Browser using[`MdigControl`](../../Reference/dig/MdigControl.md) with [`M_GC_FEATURE_BROWSER`](../../Reference/dig/MdigControl.md), as described in the previous section.

Alternatively, you can select the appropriate GenICam CLProtocol library for your camera using the Aurora Imaging Configurator utility or Aurora Imaging Intellicam.

- In Aurora Imaging Configurator, select the **Boards Camera Link** item in the tree structure of the utility, and then select the **Enable GenICam for Camera Link (CLProtocol)** option from the presented page. A list of GenICam CLProtocol libraries and the device IDs they support are presented.
- In Aurora Imaging Intellicam, enable the **Enable GenICam for Camera Link (CLProtocol) feature browser**setting in **System Options** tab of the **Preferences**dialog (accessible using the**Options Preferences ** menu item. When you access the Feature Browser in Intellicam, a list of GenICam CLProtocol libraries and the device IDs they support are presented.
