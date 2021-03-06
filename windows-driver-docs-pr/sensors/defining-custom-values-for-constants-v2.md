---
title: 定义自定义值的传感器通用驱动程序常量
description: 定义自定义值的传感器通用驱动程序常量
ms.assetid: 53534391-4251-4AB6-9D3C-AE0BC624465A
ms.date: 07/20/2018
ms.localizationpriority: medium
ms.openlocfilehash: a251b9a542ddd7f0871ac92455c108b14c0a2a66
ms.sourcegitcommit: 0cc5051945559a242d941a6f2799d161d8eba2a7
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/23/2019
ms.locfileid: "63377851"
---
# <a name="defining-custom-values-for-sensor-constants"></a>定义自定义值的传感器常量


可以定义自定义值的类别、 传感器类型，数据字段、 属性和事件。

## <a name="guidelines-for-custom-values"></a>自定义值的指导原则

避免定义新常量，如果一组现有的平台定义的常量将起作用。 研究，并了解类别、 类型、 数据字段、 属性和事件中所述[常量](about-sensor-constants.md)部分，并决定传感器驱动程序是否适合平台框架。

如果您选择定义的自定义值，请遵循以下准则：

-   生成新 PROPERTYKEYs。 不要重复 Guid 使用从平台定义的常量并不使基于平台定义基础值的新的 Guid。

-   对于新的传感器数据类型相同的传感器类别中使用相同的 Guid。 若要使每种数据类型唯一，递增的 PID 一部分的属性键。

-   使用新定义的传感器的传感器属性完全相同的 Guid。 若要使每个属性唯一，递增的 PID 一部分的属性键。

-   对于您的传感器为定义的新传感器事件类型使用相同的 Guid。 若要使每个事件类型唯一，递增的 PID 一部分的属性键。

-   创建唯一的常量。 若要避免常量名之间的冲突，可帮助使名称唯一，内部传感器代码和其他实现之间的值每个自定义名称的开头。 例如，名为 Fabrikam 的公司可以开始使用每个常量定义`FABRIKAM_`。

-   文档和发布值。 如果希望开发人员能够从您的传感器通过 Windows 传感器 API 或位置 API 访问数据，必须文档自定义值，并发布了常量，例如通过提供的标头文件。 如果您的传感器是专有系统的一部分，则不需要发布自定义值。

## <a name="example"></a>示例

下面的代码示例演示如何定义自定义值的传感器常量。 该示例创建一个新的类别、 传感器类型和提供当前的本地时间的示例传感器的数据类型。 您可以假设传感器接收来自通过卫星或其他无线传输一个引用时钟时间信息。

```cpp
// Define a sensor ID.
// {0D77BEE3-7169-42bf-8379-28F9A9B59A57}
DEFINE_GUID(SAMPLE_SENSOR_TIME_ID,
0xd77bee3, 0x7169, 0x42bf, 0x83, 0x79, 0x28, 0xf9, 0xa9, 0xb5, 0x9a, 0x57);

// Define a custom category.
// {062A5C3B-44C1-4ad1-8EFC-0F65B2E4AD48}
DEFINE_GUID(SAMPLE_SENSOR_CATEGORY_DATE_TIME,
0x62a5c3b, 0x44c1, 0x4ad1, 0x8e, 0xfc, 0xf, 0x65, 0xb2, 0xe4, 0xad, 0x48);

// Define a custom type.
// {5F199A84-409F-4e35-B2DD-F9C79F5318A0}
DEFINE_GUID(SAMPLE_SENSOR_TYPE_TIME,
0x5f199a84, 0x409f, 0x4e35, 0xb2, 0xdd, 0xf9, 0xc7, 0x9f, 0x53, 0x18, 0xa0);

// Time/Date sensor fields.
// Because these are related, each field uses the same GUID, but changes the PID.
// {340946F2-9A77-42b0-8176-57D4DF00E5CA}
DEFINE_PROPERTYKEY(SAMPLE_SENSOR_DATA_TYPE_HOUR,
0x340946f2, 0x9a77, 0x42b0, 0x81, 0x76, 0x57, 0xd4, 0xdf, 0x0, 0xe5, 0xca, PID_FIRST_USABLE); // PID = 2

DEFINE_PROPERTYKEY(SAMPLE_SENSOR_DATA_TYPE_MINUTE,
0x340946f2, 0x9a77, 0x42b0, 0x81, 0x76, 0x57, 0xd4, 0xdf, 0x0, 0xe5, 0xca, PID_FIRST_USABLE + 1); // PID = 3

DEFINE_PROPERTYKEY(SAMPLE_SENSOR_DATA_TYPE_SECOND,
0x340946f2, 0x9a77, 0x42b0, 0x81, 0x76, 0x57, 0xd4, 0xdf, 0x0, 0xe5, 0xca, PID_FIRST_USABLE + 2); // PID = 4
```

### <a name="using-the-definepropertykey-macro"></a>使用定义\_PROPERTYKEY 宏

若要使用定义\_PROPERTYKEY 宏，使用以下两个选项之一：

-   在项目中包含 Initguid.h。 在这种情况下，宏会为您定义的属性键。 此方法适用于大多数情况下，但可以在大型、 复杂项目中导致命名冲突。

-   不包含 Initguid.h。 相反，编译您的定义为具有.lib 文件扩展名的静态库文件。 在这种情况下，该宏声明编译器你属性键的名称。 但是，你必须引用链接器设置中的.lib 文件。 这种方法最适合在大型项目中使用多个模块。

使用宏，而不包含 Initguid.h 和无需引用库文件将导致 LNK2001 错误。

## <a name="related-topics"></a>相关主题

[传感器驱动程序 ADXL345Acc 示例](https://go.microsoft.com/fwlink/p/?LinkId=617957)

<!--
https://go.microsoft.com/fwlink/p/?LinkId=617957: https://github.com/Microsoft/Windows-driver-samples/tree/master/sensors/ADXL345Acc
-->


