---
title: Firmware user experience (UX) best practices
description: Firmware user experience (UX) best practices
author: windows-driver-content
ms.author: windowsdriverdev
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows-hardware
ms.technology: windows-devices
---


# Firmware user experience (UX) best practices


A firmware update to a system can be important to the continued use of that machine. Microsoft wants to ensure that firmware updates are safely delivered, and installation is uninterrupted. To do this, we have recommended the following guidelines that are described in greater detail in the **Related resources** section.

1.  Progress bar/indicator that the system is installing an update.

2.  Windows Boot loader with **UpdateCapsule** function will provide a bitmap containing message for user.

3.  UEFI check for AC Power, or sufficient battery life (RSOC) or fail out with error codes recorded in ESRT.

The **Related resources** links contain the most recent information on firmware UX guidance.

## Related resources

[Windows UEFI firmware update platform](https://msdn.microsoft.com/en-us/windows/hardware/drivers/bringup/windows-uefi-firmware-update-platform)                                           
 
[Seamless crisis prevention and recovery ](https://msdn.microsoft.com/en-us/library/windows/hardware/dn917851)                                                               

[User experience for UEFI firmware updates](https://msdn.microsoft.com/en-us/windows/hardware/drivers/bringup/user-experience-for-uefi-firmware-updates)                                   

[Boot screen components](https://msdn.microsoft.com/en-us/windows/hardware/drivers/bringup/boot-screen-components)                                                                         

[ESRT table definition](https://msdn.microsoft.com/windows/hardware/drivers/bringup/esrt-table-definition)    

[Validating Windows UEFI Firmware Update Platform Functionality](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/validating-windows-uefi-firmware-update-platform-functionality) 




