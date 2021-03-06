---
title: OID_DOT11_SUPPORTED_OFDM_FREQUENCY_LIST
author: windows-driver-content
description: OID_DOT11_SUPPORTED_OFDM_FREQUENCY_LIST
ms.assetid: d6b22bc7-27c2-44cb-8f33-17e04596e0b1
ms.author: windowsdriverdev
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
keywords: 
 -OID_DOT11_SUPPORTED_OFDM_FREQUENCY_LIST Network Drivers Starting with Windows Vista
---

# OID\_DOT11\_SUPPORTED\_OFDM\_FREQUENCY\_LIST


**Important**  The [Native 802.11 Wireless LAN](https://msdn.microsoft.com/library/windows/hardware/ff560690) interface is deprecated in Windows 10 and later. Please use the WLAN Device Driver Interface (WDI) instead. For more information about WDI, see [WLAN Universal Windows driver model](https://msdn.microsoft.com/library/windows/hardware/dn897672).

 

When queried, the OID\_DOT11\_SUPPORTED\_OFDM\_FREQUENCY\_LIST OID requests that the miniport driver returns the list of channel center frequencies that the 802.11 station can operate with.

The data type for this OID is the [**DOT11\_SUPPORTED\_OFDM\_FREQUENCY\_LIST**](https://msdn.microsoft.com/library/windows/hardware/ff548800) structure.

You can use this OID to indicate to the operating system any frequency channels that should be scanned for conformance with performance specifications. The operating system does not use these OIDs during routine operation of the 802.11 station.

The OID MIB object is only valid for the following PHY types:

-   Orthogonal frequency division multiplexing (OFDM) PHY.
-   High-throughput (HT) 802.11n PHY or very high-throughput (VHT) 802.11ac PHY when operating in the 5-GHz band.

If the current PHY type is not set to **dot11\_phy\_type\_ofdm**, **dot11\_phy\_type\_ht**, or **dot11\_phy\_type\_vht**, the miniport driver must fail the query request by returning NDIS\_STATUS\_INVALID\_DATA from its [*MiniportOidRequest*](https://msdn.microsoft.com/library/windows/hardware/ff559416) function.

Support for this OID is optional, unless the station reports to the operating system that it supports the **dot11\_phy\_type\_ofdm** PHY type. In this case support for this OID is mandatory.

If the miniport driver is operating in Extensible Station (ExtSTA) mode, the current PHY type is determined through the ExtSTA **msDot11CurrentPhyID** management information base (MIB) object. This MIB object specifies the index of the current PHY type within the 802.11 station's list of supported PHY types. For more information about **msDot11CurrentPhyID**, see [OID\_DOT11\_CURRENT\_PHY\_ID](oid-dot11-current-phy-id.md).

Requirements
------------

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p>Version</p></td>
<td><p>Available in Windows 7 and later versions of the Windows operating systems.</p></td>
</tr>
<tr class="even">
<td><p>Header</p></td>
<td>Windot11.h (include Ndis.h)</td>
</tr>
</tbody>
</table>

## See also


[Native 802.11 MIB OIDs](https://msdn.microsoft.com/library/windows/hardware/ff560645)

[Native 802.11 Wireless LAN OIDs](https://msdn.microsoft.com/library/windows/hardware/ff560691)

 

 




