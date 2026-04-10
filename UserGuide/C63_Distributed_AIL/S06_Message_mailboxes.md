---
doctype: UserGuide
part: "Miscellaneous"
chapter: Distributed_AIL
section: Message_mailboxes
module_tag: app
product: Aurora-Imaging-Library-user-guide
version: 1100
path: "UserGuide / Miscellaneous / distributed-ail / Message mailboxes"
---

# Message mailboxes

To pass messages between the publishing and the monitoring application in a monitoring configuration, you can use a message mailbox. Message mailboxes are, at their simplest, an Aurora Imaging Library object for storing generic messages. The publishing application can use them, for example, to store a generated custom error message, image statistics, or image meta-data (such as EXIF information). Messages can be associated with a tag to identify the type of message.

> **Note:** Although this is the expected use for message mailboxes, you can use them in a non-Distributed Aurora Imaging Library application.

## Message mailbox allocation and use

To allocate a message mailbox, use [`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with [`M_MESSAGE_MAILBOX`](../../Reference/obj/MobjAlloc.md). To write a message to a message mailbox, use the [`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md) function. To read a message from a message mailbox, use the [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) function. To make the message mailbox accessible to the monitoring application, use [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_DAIL_PUBLISH`](../../Reference/obj/MobjControl.md).

[`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) will only read the message if it doesn't time out, and you pass it a sufficient amount of memory in which to write the read message. To establish the amount of memory to allocate to read the next message, you can call [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) with its [`MessagePtr`](../../Reference/obj/MobjMessageRead.md) parameter to [`M_NULL`](../../Reference/obj/MobjMessageRead.md) and use the return value. Alternatively, you can use [`MobjInquire`](../../Reference/obj/MobjInquire.md) with [`M_MESSAGE_LENGTH`](../../Reference/obj/MobjInquire.md) to determine the size of the next message. [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) will also report the status of the operation, with a status of [`M_SUCCESS`](../../Reference/obj/MobjMessageRead.md) for a successful read operation, or a status of [`M_BUFFER_TOO_SMALL`](../../Reference/obj/MobjMessageRead.md) or [`M_READ_TIMEOUT`](../../Reference/obj/MobjMessageRead.md) if the read operation was not successful.

## Message mailbox operation modes

When you allocate a message mailbox, you must specify the mode in which the mailbox should operate: overwrite mode ([`M_OVERWRITE`](../../Reference/obj/MobjAlloc.md)) or queued mode ([`M_QUEUE`](../../Reference/obj/MobjAlloc.md)).

When a message mailbox operates in overwrite mode, it only allows one message to be stored in the mailbox at a time. When a new write ([`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md)) operation occurs, the previous message is overwritten with the new message. This means that the current message can be read any number of times until a subsequent write operation occurs.

When a message mailbox operates in a queued mode, multiple messages can be simultaneously stored in a queued mailbox. The queued mailbox operates in a first-in first-out (FIFO) fashion. That is, the first message stored in a queued mailbox is the first message to be read. Furthermore, unlike in overwrite mode, a message written to a queued mailbox is, by default, removed from the message mailbox upon a read operation.

It is possible to override the behavior of deleting a message upon a read operation in a queued mailbox. To do so, call [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) with [`M_KEEP_IN_QUEUE`](../../Reference/obj/MobjMessageRead.md); this will keep the message in the queued mailbox until the next read operation that doesn't make use of the [`M_KEEP_IN_QUEUE`](../../Reference/obj/MobjMessageRead.md) operation flag. This will cause the next message read from this queued mailbox to be the same message as was previously read. This could be useful if a publishing application needs to preview a message, written by the monitoring application, to determine if the message is relevant to that particular publishing application or if it's destined for another publishing application.

By default, a queued mailbox can hold up to 100 messages, although you can change this limit using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_QUEUE_SIZE`](../../Reference/obj/MobjControl.md). If the queue is full, the default behavior is to wait until the write times out, since there is the possibility of a read operation occurring before the timeout. You can also hook a function to the write timeout and have it quickly reset the message mailbox using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_RESET`](../../Reference/obj/MobjControl.md). Instead of waiting for a timeout, you can have [`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md) generate an error when the message mailbox is full, using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_QUEUE_FULL_MODE`](../../Reference/obj/MobjControl.md) set to [`M_ERROR`](../../Reference/obj/MobjControl.md).

## Steps to using a message mailbox

The following steps provide a basic methodology for using the Aurora Imaging Library message mailbox in a Distributed Aurora Imaging Library monitoring configuration. In the publishing application(s), perform the following steps:

1. Allocate a message mailbox using [`MobjAlloc`](../../Reference/obj/MobjAlloc.md) with either [`M_OVERWRITE`](../../Reference/obj/MobjAlloc.md) or [`M_QUEUE`](../../Reference/obj/MobjAlloc.md).
2. Publish the message mailbox using [`MobjControl`](../../Reference/obj/MobjControl.md) with [`M_DAIL_PUBLISH`](../../Reference/obj/MobjControl.md). This allows the monitoring application access to the message mailbox.
3. Write a message to the message mailbox using [`MobjMessageWrite`](../../Reference/obj/MobjMessageWrite.md). This function also allows you to add a user-defined message tag to the message. The tag could indicate, for example, that the message contains image statistics.
4. Free the allocated message mailbox using [`MobjFree`](../../Reference/obj/MobjFree.md) when it is no longer required, unless [`M_UNIQUE_ID`](../../Reference/obj/MobjAlloc.md) was specified during allocation.

In the monitoring application, perform the following steps:

1. Inquire the message mailbox's Aurora Imaging Library identifier from the publishing application using [`MappInquireConnection`](../../Reference/app/MappInquireConnection.md) with [`M_DAIL_PUBLISHED_NAME`](../../Reference/app/MappInquireConnection.md).
2. Hook a user-defined function to handle when a new message is written to the message mailbox, using [`MobjHookFunction`](../../Reference/obj/MobjHookFunction.md) with [`M_MESSAGE_RECEIVED`](../../Reference/obj/MobjHookFunction.md).
3. In the user-defined hook function, use [`MobjMessageRead`](../../Reference/obj/MobjMessageRead.md) to read the message that triggered the hook function.
4. Use the message's tag to determine what type of message the publishing application has classified the message as, and perform some action or processing.
