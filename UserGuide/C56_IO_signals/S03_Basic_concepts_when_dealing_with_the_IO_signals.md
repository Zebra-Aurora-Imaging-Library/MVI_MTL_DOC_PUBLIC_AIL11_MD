---
doctype: UserGuide
part: "Communication"
chapter: IO_signals
section: Basic_concepts_when_dealing_with_the_IO_signals
module_tag: sysIo
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Communication / IOsignals / Basic concepts when dealing with the IO signals"
---

# Basic concepts when dealing with I/O signals

The basic concepts and vocabulary conventions when dealing with I/O signals are:

- **Auxiliary signal**. A non-video signal that can be controlled and supports one or more functionalities.
- **Bit-encoded value**. A number whose bits each represent a setting.
- **Camera control signal** (CC signal). A signal that can only be routed to the camera because it is transmitted on the same cable as is used to receive video.
- **Input/output signal** (I/O signal). A non-video signal that can be used to send or receive non-video information from another device. The term encompasses input signals, output signals, and bidirectional signals that can be configured to be used as either an input signal or an output signal.
- **Interrupt**. A notification that an event occurred. If an event handler (hooked function) has been set up to handle this event, it will start executing upon receiving notification that the event occurred.
- **Transport layer signal** (TL signal). The transport layer signal, typically a trigger signal, is an embedded bidirectional signal transmitted on the physical transport layer connection with your CoaXPress standard-compliant camera. Typically, the TL signal is bundled with other control and data signals along the same cable, and is therefore not pin-specific nor is it listed in the pin-out tables for your board.
- **User-bit**. A bit in a static-user-output register. A user-bit can be routed to an output signal (or a bidirectional signal set to output) to control its state (high or low).
- **Static-user-output register**. A register whose bits can be used to control the state of output signals (or bidirectional signals set to output). These bits, often referred to as user-bits, remain the same (static) until changed by the application.
- **Signal format**. The format of the signal specifies the electrical characteristics of the signal that is transmitted/received.
