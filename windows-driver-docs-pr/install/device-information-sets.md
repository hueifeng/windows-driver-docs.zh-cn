---
title: 设备信息集
description: 设备信息集
ms.assetid: 20539c63-10b1-408a-8c60-da444f54b64e
keywords:
- 设备信息元素 WDK 设备安装
- 设备信息设置 WDK 设备安装
- 信息将 WDK 设备设置
- 枚举设备信息 WDK
- SetupDiEnumDeviceInfo
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: c3cb7c60cea5fa761d67f1ee3693b275e661f121
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63357802"
---
# <a name="device-information-sets"></a>设备信息集





在用户模式下，属于任何一个的设备[设备安装程序类](device-setup-classes.md)或[设备接口类](device-interface-classes.md)通过使用管理*设备信息元素*和*设备信息设置。* 设备信息集包含的设备信息元素属于某些设备安装程序类或设备接口类的所有设备。

设备信息的每个元素包含的设备的图柄*devnode*，和与该元素描述的设备关联到的所有设备接口的链接列表的指针。 如果设备的信息集描述的安装程序类的成员，该元素可能不指向任何设备的接口，因为安装程序类成员不一定与接口关联。

下图显示了设备的信息集的内部结构。

![说明设备信息集的关系图](images/devinfosets.png)

### <a name="creating-a-device-information-set"></a>创建设备的信息集

使用创建的设备信息设置后[ **SetupDiCreateDeviceInfoList**](https://msdn.microsoft.com/library/windows/hardware/ff550956)，可以创建设备信息元素，并将其添加到在时使用的列表[ **SetupDiCreateDeviceInfo**](https://msdn.microsoft.com/library/windows/hardware/ff550952)。 或者， [ **SetupDiGetClassDevs** ](https://msdn.microsoft.com/library/windows/hardware/ff551069)可以调用以创建设置组成与指定的设备安装程序类或设备接口类相关联的所有设备的设备信息。

### <a name="enumerating-device-information"></a>枚举设备信息

设备信息集创建后，可以枚举设备和属于集的设备接口，但不同的操作所需的每种类型的枚举。 [**SetupDiEnumDeviceInfo** ](https://msdn.microsoft.com/library/windows/hardware/ff551010)枚举所有属于信息集满足某些条件的设备。 每次调用**SetupDiEnumDeviceInfo**提取[ **SP_DEVINFO_DATA** ](https://msdn.microsoft.com/library/windows/hardware/ff552344)大致对应于设备信息元素的结构。 SP_DEVINFO_DATA 包含设备所属的类的 GUID 和一个*设备实例*指向设备 devnode 的句柄。 SP_DEVINFO_DATA 结构和完整的设备元素之间的主要区别是 SP_DEVINFO_DATA 作用*不*包含的接口与设备关联的链接的列表。 因此， **SetupDiEnumDeviceInfo**不能用于枚举中的设备信息集的接口。

若要枚举中设备的信息集的设备接口，请调用[ **SetupDiEnumDeviceInterfaces**](https://msdn.microsoft.com/library/windows/hardware/ff551015)。 此例程中的设备信息的所有设备信息元素介绍了设置，提取每个元素的接口列表中的接口并返回一个接口，每次调用。 如果**SetupDiEnumDeviceInterfaces**传递 SP_DEVINFO_DATA 结构作为其第二个参数的输入，它限制为仅这些接口与由 SP_DEVINFO_DATA 设备相关联的枚举。

**SetupDiEnumDeviceInterfaces**将返回[ **SP_DEVICE_INTERFACE_DATA** ](https://msdn.microsoft.com/library/windows/hardware/ff552342)结构。 SP_DEVICE_INTERFACE_DATA 包含接口类 GUID 和其他信息的接口，包括已编码可用于获取接口的名称的信息的保留的字段。 若要获取的接口名称，一个更多步骤是必需的：[**SetupDiGetDeviceInterfaceDetail** ](https://msdn.microsoft.com/library/windows/hardware/ff551120)必须调用。 **SetupDiGetDeviceInterfaceDetail**返回类型的结构[ **SP_DEVICE_INTERFACE_DETAIL_DATA** ](https://msdn.microsoft.com/library/windows/hardware/ff552343) ，其中包含定义的接口的系统对象树中的路径。

 

 




