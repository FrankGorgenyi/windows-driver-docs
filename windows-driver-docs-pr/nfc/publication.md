---
title: Publishing NFP messages
author: windows-driver-content
description: Publishing NFP messages
ms.assetid: 45BE3620-ADF4-413D-90B3-586FCEB5F606
---

# Publishing NFP messages


A publication is represented as a unique open handle within the driver. Active publications must have both a type and a data buffer. The type is set by opening a file name in the “Pubs” namespace. The data buffer is set by sending [**IOCTL\_NFP\_SET\_PAYLOAD**](https://msdn.microsoft.com/library/windows/hardware/jj853321).

A callback on attempted transmission is given through a completed [**IOCTL\_NFP\_GET\_NEXT\_TRANSMITTED\_MESSAGE**](https://msdn.microsoft.com/library/windows/hardware/jj853320).

A publication can be temporarily disabled via an [**IOCTL\_NFP\_DISABLE**](https://msdn.microsoft.com/library/windows/hardware/jj853315).

A publication can be re-enabled via an [**IOCTL\_NFP\_ENABLE**](https://msdn.microsoft.com/library/windows/hardware/jj853316).

##  Handles


A client that wants to publish a message will first open a new handle to the driver. Handles from prior publications, subscriptions, and so on, cannot be reused. If they are no longer needed, they will be closed by a well-behaved client.

The client will open a file handle in the “Pubs/&lt;Protocol&gt;.&lt;SubType&gt;” device-relative namespace. The following is a complete example.

``` syntax
\\?\root#ContosoProx#0000#{FB3842CD-9E2A-4F83-8FCC-4B0761139AE9}\Pubs\Windows.windows.com/SD
<---------------Device Interface Symbolic Link-----------------> <------File Name---------->
    <--------------------><------------------------------------> <--> <-----> <------------>
          DeviceID          NearFieldProximity Interface Class     *  Protocol   SubType
```

After opening the handle, a client should then set the payload of the message to be published with the [**IOCTL\_NFP\_SET\_PAYLOAD**](https://msdn.microsoft.com/library/windows/hardware/jj853321).

There is no facility to read back the specified file name (the publication type).

### File Name

In a driver’s *CreateFile* handler, the Device Interface *SymbolicLink* will be stripped away and only the device-relative File Name will remain. This File Name will be a null-terminated case-sensitive UTF-16LE string buffer that indicates the type of the publication (or subscription). The maximum size of this buffer is 502 characters including the NULL terminator. The driver must parse this path into three constituent components: “Pubs\\”, protocol, and subtype.

### Required Actions

-   The driver MUST parse the protocol component (before the first ‘.’). Any unrecognized protocol MUST complete with STATUS\_OBJECT\_PATH\_NOT\_FOUND
-   If the string is not NULL-terminated within the buffer length, the driver MUST complete the IOCTL with STATUS\_INVALID\_PARAMETER.
-   If the protocol requires a subtype and the subtype component of the string buffer is less than one (1) character or longer than 250 characters, the driver MUST complete the IOCTL with STATUS\_INVALID\_PARAMETER.
-   If the protocol component of the string buffer is longer than 250 characters, the driver MUST complete the IOCTL with STATUS\_INVALID\_PARAMETER.
-   The driver MUST interpret the first NULL as the end of the string.
-   The driver MAY recognize the “Pairing:Bluetooth” type for publications. The driver MAY recognize this type in order to preserve compatibility. (Note the use of a colon rather than the use of a period.)
-   The driver MUST recognize the “WindowsUri” type.
-   The driver MUST recognize the “DeviceArrived” type for subscriptions only.
-   The driver MUST recognize the “DeviceDeparted” type for subscriptions only.
-   The driver MUST recognize the “WindowsMime” type for subscriptions only.
-   The driver MUST recognize the “WindowsMime.” type.
-   If the protocol should only be recognized for subscriptions, and the IOCTL specifies “Pubs\\”, the driver MUST complete the IOCTL with STATUS\_OBJECT\_PATH\_NOT\_FOUND.
-   If the protocol should only be recognized for publications, and the IOCTL specifies “Subs\\”, the driver MUST complete the IOCTL with STATUS\_OBJECT\_PATH\_NOT\_FOUND.
-   Two open handles to the same type MUST represent two distinct entities.
-   Some protocols (namespaces) are reserved. Unless explicitly specified in this document, the driver MUST NOT recognize any protocols that begin with:
    -   "Windows”
    -   "Device”
    -   "Pairing”
    -   "NDEF”
    -   "NFC”
    -   "Iso14443Dep”
    -   "Iso14443TypeA”
    -   "Iso14443TypeB”
    -   "Iso15693Vicinity”
    -   "MifareClassic”
    -   "MifareUltralight”
    -   "FeliCa”

## Unpublish


When a client no longer wants a message published, it will close the publication handle. This indicates the message should no longer be transmitted. If a client process terminates, the system will close all open file handles on behalf of the client.

### Required Actions

-   When the handle is closed (IRP\_MJ\_CLOSE), the driver:
    -   MUST reclaim all memory used by the publication (both type and message data) except for buffers for ongoing transmissions of this publication.
    -   MUST NOT transmit the message if a device becomes proximate in the future.
    -   MUST NOT interrupt any in-progress transmission of the publication.
-   The driver MAY ignore IRP\_MJ\_CLEANUP.

The driver should assume that if the client publishes a message twice, it is because the client wants the message transmitted twice when a device is in proximity.

### Required Actions

The driver MUST accept and transmit duplicate published messages, even if published by the same client.

### Required Actions


The driver MUST remove all messages (and reclaim those resources) from the “Received” queue if the client has not sent a replacement [**IOCTL\_NFP\_GET\_NEXT\_SUBSCRIBED\_MESSAGE**](https://msdn.microsoft.com/library/windows/hardware/jj853319) within 10 - 20 seconds of the prior IOCTL completion.

## Unresponsive or Misbehaving Clients


If a client fails to send the required [**IOCTL\_NFP\_GET\_NEXT\_TRANSMITTED\_MESSAGE**](https://msdn.microsoft.com/library/windows/hardware/jj853320) request for a period of ten to twenty seconds \[10-20sec\], the driver should assume that the client is gone. Under normal circumstances, clients should refresh their requests well within one second \[1s\]. If this occurs, the driver must set the “CompleteEventImmediately” counter to zero and must not increment the counter until the client wakes up and sends the required IRP.

### Required Actions

The driver must set the “CompleteEventImmediately” counter to zero and must not increment the counter if the client has not sent a replacement [**IOCTL\_NFP\_GET\_NEXT\_TRANSMITTED\_MESSAGE**](https://msdn.microsoft.com/library/windows/hardware/jj853320) within 10 - 20 seconds of the prior IOCTL completion.

 

 

[Send comments about this topic to Microsoft](mailto:wsddocfb@microsoft.com?subject=Documentation%20feedback%20%5Bnfpdrivers\nfpdrivers%5D:%20Publishing%20NFP%20messages%20%20RELEASE:%20%284/5/2016%29&body=%0A%0APRIVACY%20STATEMENT%0A%0AWe%20use%20your%20feedback%20to%20improve%20the%20documentation.%20We%20don't%20use%20your%20email%20address%20for%20any%20other%20purpose,%20and%20we'll%20remove%20your%20email%20address%20from%20our%20system%20after%20the%20issue%20that%20you're%20reporting%20is%20fixed.%20While%20we're%20working%20to%20fix%20this%20issue,%20we%20might%20send%20you%20an%20email%20message%20to%20ask%20for%20more%20info.%20Later,%20we%20might%20also%20send%20you%20an%20email%20message%20to%20let%20you%20know%20that%20we've%20addressed%20your%20feedback.%0A%0AFor%20more%20info%20about%20Microsoft's%20privacy%20policy,%20see%20http://privacy.microsoft.com/default.aspx. "Send comments about this topic to Microsoft")



