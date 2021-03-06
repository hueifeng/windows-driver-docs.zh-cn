---
title: Bug 检查 0x11F INVALID_DRIVER_HANDLE
description: INVALID_DRIVER_HANDLE bug 检查具有 0x0000011F 值。 这表示有人关闭了之间插入驱动程序对象和引用了句柄的驱动程序的初始句柄。
ms.assetid: A669256B-737D-4215-8E0E-5500D7704F4E
keywords:
- Bug 检查 0x11F INVALID_DRIVER_HANDLE
- INVALID_DRIVER_HANDLE
ms.date: 01/30/2019
topic_type:
- apiref
api_name:
- INVALID_DRIVER_HANDLE
api_type:
- NA
ms.localizationpriority: medium
ms.openlocfilehash: 876be74283188178d45152b7c86588ece60c1111
ms.sourcegitcommit: d03b44343cd32b3653d0471afcdd3d35cb800c0d
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 07/02/2019
ms.locfileid: "67520860"
---
# <a name="bug-check-0x11f-invaliddriverhandle"></a>Bug 检查 0x11F：无效\_驱动程序\_处理


无效\_驱动程序\_句柄 bug 检查的值为 0x0000011F。 这表示有人关闭了之间插入驱动程序对象和引用了句柄的驱动程序的初始句柄。

> [!IMPORTANT]
> 本主题面向程序员。 如果你已使用计算机时收到一个蓝色的屏幕，错误代码的客户，请参阅[疑难解答蓝屏错误](https://www.windows.com/stopcode)。


## <a name="invaliddriverhandle-parameters"></a>无效\_驱动程序\_句柄参数


| 参数 | 描述                                         |
|-----------|-----------------------------------------------------|
| 1         | 驱动程序对象的句柄值。             |
| 2         | 尝试引用该对象返回的状态。 |
| 3         | PDRIVER 地址\_对象。                 |
| 4         | 保留                                            |

 

 

 




