---
title: 将 KMDF 外设驱动程序连接到串行端口
description: SerCx2 管理的串行端口上的外围设备的 KMDF 驱动程序需要某些硬件资源来运行设备。 这些资源包含驱动程序打开串行端口逻辑连接所需的信息。
ms.assetid: EDE62C5E-3563-42EE-884E-DF473CD724A5
ms.date: 04/20/2017
ms.localizationpriority: medium
ms.openlocfilehash: 4ed455ad337eb9bbfd3200e41c456c3624e766b6
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72845294"
---
# <a name="connecting-a-kmdf-peripheral-driver-to-a-serial-port"></a>将 KMDF 外设驱动程序连接到串行端口


SerCx2 管理的串行端口上的外围设备的 KMDF 驱动程序需要某些硬件资源来运行设备。 这些资源包含驱动程序打开串行端口逻辑连接所需的信息。 其他资源可能包括中断，以及一个或多个 GPIO 输入或输出插针。

此驱动程序实现一组即插即用和电源管理事件回调函数。 若要将这些函数注册到 KMDF，驱动程序的[*EvtDriverDeviceAdd*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfdriver/nc-wdfdriver-evt_wdf_driver_device_add)事件回调函数会调用[**WdfDeviceInitSetPnpPowerEventCallbacks**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfdevice/nf-wdfdevice-wdfdeviceinitsetpnppowereventcallbacks)方法。 该框架调用电源管理事件回调功能，以通知驱动程序电源设备电源状态发生的更改。 这些函数包括[*EvtDevicePrepareHardware*](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfdevice/nc-wdfdevice-evt_wdf_device_prepare_hardware)函数，该函数执行使设备可供驱动程序访问所需的任何操作。

在串行连接的外围设备进入未初始化的 D0 设备电源状态之后，驱动程序框架将调用*EvtDevicePrepareHardware*函数，告诉外围设备驱动程序准备设备以供使用。 在此调用期间，驱动程序收到两个硬件资源列表作为输入参数。 *ResourcesRaw*参数是[*原始资源*](https://docs.microsoft.com/windows-hardware/drivers/wdf/raw-and-translated-resources)列表的 WDFCMRESLIST 对象句柄， *ResourcesTranslated*参数是已[*转换资源*](https://docs.microsoft.com/windows-hardware/drivers/wdf/raw-and-translated-resources)列表的 WDFCMRESLIST 对象句柄。 已翻译的资源包括驱动程序建立与外围设备的逻辑连接所需的*连接 ID* 。

下面的代码示例演示*EvtDevicePrepareHardware*函数如何从*ResourcesTranslated*参数获取连接 ID。

```cpp
BOOLEAN fConnectionIdFound = FALSE;
LARGE_INTEGER connectionId = 0;
ULONG resourceCount;
NTSTATUS status = STATUS_SUCCESS;

resourceCount = WdfCmResourceListGetCount(ResourcesTranslated);

// Loop through the resources and save the relevant ones.

for (ULONG ix = 0; ix < resourceCount; ix++)
{
    PCM_PARTIAL_RESOURCE_DESCRIPTOR pDescriptor;

    pDescriptor = WdfCmResourceListGetDescriptor(ResourcesTranslated, ix);

    if (pDescriptor == NULL)
    {
        status = E_POINTER;
        break;
    }

    // Determine the resource type.
    switch (pDescriptor->Type)
    {
    case CmResourceTypeConnection:
        {
            // Check against the expected connection types.

            UCHAR Class = pDescriptor->u.Connection.Class;
            UCHAR Type = pDescriptor->u.Connection.Type;

            if (Class == CM_RESOURCE_CONNECTION_CLASS_SERIAL)
            {
                if (Type == CM_RESOURCE_CONNECTION_TYPE_SERIAL_UART)
                {
                    if (fConnectionIdFound == FALSE)
                    {
                        // Save the connection ID.

                        connectionId.LowPart = pDescriptor->u.Connection.IdLowPart;
                        connectionId.HighPart = pDescriptor->u.Connection.IdHighPart;
                        fConnectionIdFound = TRUE;
                    }
                }
            }

            if (Class == CM_RESOURCE_CONNECTION_CLASS_GPIO)
            {
                // Check for GPIO pin resource.
                ...
            }
        }
        break;

    case CmResourceTypeInterrupt:
        {
            // Check for interrupt resource.
            ...
        }
        break;

    default:
        // Don't care about other resource descriptors.
        break;
    }
}
```

前面的代码示例将串行连接外围设备的连接 ID 复制到名为 `connectionId`的变量中。

下面的代码示例演示如何将此连接 ID 合并到设备路径名称中，该名称可用于打开到外围设备的逻辑连接。 此设备路径名称将资源中心标识为要从中获取访问外围设备所需参数的系统组件。

```cpp
// Use the connection ID to create the full device path name.
 
DECLARE_UNICODE_STRING_SIZE(szDeviceName, RESOURCE_HUB_PATH_SIZE);

status = RESOURCE_HUB_CREATE_PATH_FROM_ID(&szDeviceName,
                                          connectionId.LowPart,
                                          connectionId.HighPart);

if (!NT_SUCCESS(status))
{
     // Error handling
     ...
}
```

在上面的代码示例中， **DECLARE\_unicode\_STRING\_SIZE**宏创建一个名为 `szDeviceName` 的已初始化**UNICODE\_字符串**变量的声明，该变量的缓冲区足以包含设备资源中心使用的格式的路径名称。 此宏在 Ntdef 头文件中定义。 **资源\_中心\_路径\_大小**常量指定设备路径名中的字节数。 **资源\_中心\_从\_ID 宏创建\_路径\_从**连接 ID 生成设备路径名。 **资源\_中心\_路径\_大小**和**资源\_中心\_** 在 Reshub 头文件中定义\_ID\_创建\_路径。

下面的代码示例使用设备路径名称打开连接到串行连接的外围设备的文件句柄（名为 `SerialIoTarget`）。

```cpp
// Open the peripheral device on the serial port as a remote I/O target.
 
WDF_IO_TARGET_OPEN_PARAMS openParams;
WDF_IO_TARGET_OPEN_PARAMS_INIT_OPEN_BY_NAME(&openParams,
                                            &szDeviceName,
                                            (GENERIC_READ | GENERIC_WRITE));

openParams.ShareAccess = 0;
openParams.CreateDisposition = FILE_OPEN;
openParams.FileAttributes = FILE_ATTRIBUTE_NORMAL;

status = WdfIoTargetOpen(SerialIoTarget, &openParams);

if (!NT_SUCCESS(status))
{
    // Error handling
    ...
}
```

在上面的代码示例中， [**WDF\_IO\_目标\_打开\_参数\_INIT\_通过\_NAME**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdf_io_target_open_params_init_open_by_name)函数初始化 WDF\_\_\_[**打开\_参数**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/ns-wdfiotarget-_wdf_io_target_open_params)结构，以便驱动程序可以通过指定设备名称来打开连接到串行连接的外围设备的逻辑连接。 `SerialIoTarget` 变量是框架 i/o 目标对象的 WDFIOTARGET 句柄。 此句柄是从以前对[**WdfIoTargetCreate**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdfiotargetcreate)方法的调用获得的，该方法未在示例中显示。 如果对[**WdfIoTargetOpen**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdfiotargetopen)方法的调用成功，驱动程序可以使用 `SerialIoTarget` 句柄向外围设备发送 i/o 请求。

在*EvtDriverDeviceAdd*事件回调函数中，外围设备驱动程序可以调用[**WdfRequestCreate**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestcreate)方法来分配框架请求对象以供驱动程序使用。 以后不再需要该对象时，驱动程序将调用[**WdfObjectDelete**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfobject/nf-wdfobject-wdfobjectdelete)方法来删除该对象。 驱动程序可以重复使用从**WdfRequestCreate**调用获取的框架请求对象，将 i/o 请求发送到外围设备。 为了同步发送读取、写入或 IOCTL 请求，驱动程序将调用[**WdfIoTargetSendReadSynchronously**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdfiotargetsendreadsynchronously)、 [**WdfIoTargetSendWriteSynchronously**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdfiotargetsendwritesynchronously)或[**WdfIoTargetSendIoctlSynchronously**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfiotarget/nf-wdfiotarget-wdfiotargetsendioctlsynchronously)方法。

在下面的代码示例中，驱动程序调用**WdfIoTargetSendWriteSynchronously** ，以同步方式向外围设备发送[**IRP\_MJ\_写入**](https://docs.microsoft.com/windows-hardware/drivers/kernel/irp-mj-write)请求。 在此示例开始时，`pBuffer` 变量指向包含要写入外围设备的数据的非分页缓冲区，而 `dataSize` 变量指定此数据的大小（以字节为单位）。

```cpp
ULONG_PTR bytesWritten;
NTSTATUS status;

// Describe the input buffer.

WDF_MEMORY_DESCRIPTOR memoryDescriptor;
WDF_MEMORY_DESCRIPTOR_INIT_BUFFER(&memoryDescriptor, pBuffer, dataSize);

// Configure the write request to time out after 2 seconds.

WDF_REQUEST_SEND_OPTIONS requestOptions;
WDF_REQUEST_SEND_OPTIONS_INIT(&requestOptions, WDF_REQUEST_SEND_OPTION_TIMEOUT);
requestOptions.Timeout = WDF_REL_TIMEOUT_IN_SEC(2);

// Send the write request synchronously.

status = WdfIoTargetSendWriteSynchronously(SerialIoTarget,
                                           SerialRequest,
                                           &memoryDescriptor,
                                           NULL,
                                           &requestOptions,
                                           &bytesWritten);
if (!NT_SUCCESS(status))
{
    // Error handling
    ...
}
```

前面的代码示例执行以下操作：

1.  [**Wdf\_内存\_描述符\_INIT\_BUFFER**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfmemory/nf-wdfmemory-wdf_memory_descriptor_init_buffer)函数调用初始化 `memoryDescriptor` 变量，这是描述输入缓冲区的[**WDF\_内存\_描述符**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfmemory/ns-wdfmemory-_wdf_memory_descriptor)结构。 以前，驱动程序调用了例程（如[**ExAllocatePoolWithTag**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdm/nf-wdm-exallocatepoolwithtag) ），以便从非分页池分配缓冲区，并将写入数据复制到此缓冲区。
2.  [**WDF\_请求\_发送\_选项\_INIT**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdf_request_send_options_init)函数调用初始化 `requestOptions` 变量，这是一个[**WDF\_请求\_SEND\_OPTIONS**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfrequest/ns-wdfrequest-_wdf_request_send_options)结构，其中包含可选设置用于写入请求。 在此示例中，结构将请求配置为在两秒钟后未完成时超时。
3.  对**WdfIoTargetSendWriteSynchronously**方法的调用会将写入请求发送到外围设备。 在写操作完成或超时后，方法会同步返回。如有必要，另一个驱动程序线程可以调用[**WdfRequestCancelSentRequest**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestcancelsentrequest)来取消请求。

在**WdfIoTargetSendWriteSynchronously**调用中，驱动程序提供名为 `SerialRequest`的变量，该变量是驱动程序之前创建的框架请求对象的句柄。 **WdfIoTargetSendWriteSynchronously**调用之后，驱动程序通常应调用[**WdfRequestReuse**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wdfrequest/nf-wdfrequest-wdfrequestreuse)方法来准备要再次使用的 framework 请求对象。

 

 




