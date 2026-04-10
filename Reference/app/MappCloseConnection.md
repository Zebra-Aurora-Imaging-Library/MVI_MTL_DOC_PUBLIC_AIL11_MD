---
doctype: Reference
module: app
function: MappCloseConnection
product: Aurora-Imaging-Library-reference
preliminary: true
version: 1100
path: "Reference / app / MappCloseConnection"
---

# MappCloseConnection

> Closes a connection to a remote Aurora Imaging Library application.

## Syntax

```c
void MappCloseConnection(
    AIL_ID RemoteContextAppId  //in
)
```

## Description

Closes the connection opened by [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md). All objects allocated from this node must have been freed, using [`M...Free`](../../Reference/3dmap/M3dmapFree.md), before disconnecting.

## Parameters

### `RemoteContextAppId` *(in, AIL_ID)*

Specifies the identifier of the remote application context from which to disconnect. This application identifier must have been returned by a previous call to [`MappOpenConnection`](../../Reference/app/MappOpenConnection.md).
