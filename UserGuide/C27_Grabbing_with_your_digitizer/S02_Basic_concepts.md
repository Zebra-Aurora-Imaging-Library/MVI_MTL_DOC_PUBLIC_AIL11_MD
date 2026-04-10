---
doctype: UserGuide
part: "2D related information"
chapter: Grabbing_with_your_digitizer
section: Basic_concepts
module_tag: dig
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / 2D related information / grabbing / Basic concepts"
---

# Basic concepts for the Aurora Imaging Library digitizers module

The basic concepts and vocabulary conventions for the Aurora Imaging Library digitizers are:

- **Acquisition path**. A path that has the components to digitize or capture a video input signal.
- **Camera**. A generic term to refer to any physical device used for creating image/video data that can be transmitted it to a computer (for example, by connecting it to a frame grabber.
- **Configurable camera**. A generic term to refer to cameras that are configurable using software (for example, a GigE Vision camera or a Camera-Link camera).
- **Frame grabber**. A type of imaging board which can acquire video data from a camera.
- **Digitizer**. The acquisition path(s) with which to grab from one camera of the specified type. When several Aurora Imaging Library digitizers are allocated, their device number, along with their digitizer configuration format (DCF), identify if they represent the same path(s) (but perhaps for a different input format) or independent path(s) for simultaneous acquisition.
- **Data input channel (channel)**. Identifies which camera to use when several of its type can be connected to the same acquisition path(s) (for example, grab from channel 0 or channel 1 of digitizer 0).
- **Independent acquisition path**. An acquisition path that can, if required, acquire data from an input source independently from another such path on the same frame grabber. Each independent acquisition path has its own digitizing components to manage all video timing and synchronization for the path. When dealing with a grabberless interface camera, the connection between the camera and the interface board is considered an independent acquisition path.
- **Interface board**. A generic term to refer to the third-party board or chip on the motherboard used to communicate with one or more cameras, for example a Network interface card (NIC) is used to communicate with a GigE Vision-compliant camera.
- **Imaging board**. A generic term to refer to all boards that manipulate video data, including: frame grabbers, vision processors, compression boards, and other specialized boards.
- **Grabberless interface camera**. Any camera which uses a physical interface that does not require a frame grabber (for example, a USB or network camera).
- **Vision processor**. A type of imaging board which can process and analyze image/video data. A vision processor includes a CPU and additional memory.
- **GenICam**. An industry standard that specifies a common software interface for machine vision cameras, administered by the European Machine Vision Association (EMVA).
- **CoaXPress (CXP)**. A high performance digital interface for cameras, using packetized transmission over one or more coaxial cables connected to a compatible frame grabber. The CXP protocol can be used to stream any type of information, including 3D data and information about the camera's current configuration. CXP cameras can be configured and controlled over the same coaxial cable used to stream image data. Some CXP cameras also support power-over-CoaXPress (PoCXP).
  All CXP cameras comply with the GenICam interface standard.
- **Camera Link **. A high performance digital interface for cameras, using low voltage differential signaling (LVDS) over one or more Camera Link cables connected to a compatible frame grabber. The Camera Link streaming protocol is only designed to transmit image data and does not have provisions for streaming other types of data. Camera Link cameras can be configured and controlled over the same Camera Link cable used to stream image data. Some Camera Link cameras also support power-over-Camera-Link (PoCL).
  Some Camera Link cameras support the GenICam interface standard through the use of additional libraries that you must specify (such as a GenCP library installed with Aurora Imaging Library, or a third-party, vendor-supplied CLProtocol library).
- **USB3 Vision**. A software interface for cameras, utilizing standard USB 3.0 (or higher) connections. All USB3 Vision cameras comply with the GenICam interface standard. USB3 Vision cameras are a type of grabberless interface camera.
- **GigE Vision**. A software interface for network cameras, utilizing standard Gigabit Ethernet (or higher) connections. All GigE Vision cameras comply with the GenICam interface standard. GigE Vision cameras are a form of grabberless interface camera.
- **GenICam Standard Feature Naming Convention (SFNC)**. A set of standard camera feature (setting) names, defined as part of the GenICam standard. Camera manufacturers are encouraged (and sometimes mandated) to use these standard names for devices that implement GenICam.
- **GenICam Generic Data Container (GenDC)**. A data block encoding format for transmitting one or more types of data in a single grab, defined as an optional part of the GenICam standard. A GenDC container has one or more components, each of which can have its own data type and layout.
  Data transmitted in the GenDC format is suitable for grabbing into an Aurora Imaging Library container.
