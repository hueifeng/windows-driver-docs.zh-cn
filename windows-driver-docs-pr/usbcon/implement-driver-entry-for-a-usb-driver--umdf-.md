---
Description: 使用随 Microsoft Visual Studio 提供的 USB 用户模式驱动程序模板来写入 UMDF 客户端驱动程序。
title: 如何编写第一个 USB 客户端驱动程序 (UMDF)
ms.date: 06/03/2019
ms.localizationpriority: medium
ms.openlocfilehash: bfbe42db344877684761a07c1d1c92e2fcb96969
ms.sourcegitcommit: 4b7a6ac7c68e6ad6f27da5d1dc4deabd5d34b748
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 10/24/2019
ms.locfileid: "72838828"
---
# <a name="how-to-write-your-first-usb-client-driver-umdf"></a>如何编写第一个 USB 客户端驱动程序 (UMDF)


在本主题中，你将使用随 Microsoft Visual Studio 2019 一起提供的**USB 用户模式驱动程序**模板来编写基于用户模式驱动程序框架（UMDF）的客户端驱动程序。 生成和安装客户端驱动程序后，你将在**设备管理器**中查看客户端驱动程序，并在调试器中查看驱动程序输出。

UMDF （在本主题中称为框架）基于组件对象模型（COM）。 默认情况下，每个框架对象都必须实现[**IUnknown**](https://docs.microsoft.com/windows/desktop/api/unknwn/nn-unknwn-iunknown)及其方法： [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_))、 [**AddRef**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-addref)和[**Release**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-release)。 **AddRef**和**Release**方法管理对象的生存期，因此客户端驱动程序不需要维护引用计数。 通过**QueryInterface**方法，客户端驱动程序可以获取指向 Windows 驱动程序框架（WDF）对象模型中其他框架对象的接口指针。 框架对象执行复杂的驱动程序任务并与 Windows 交互。 某些框架对象公开的接口允许客户端驱动程序与框架交互。

基于 UMDF 的客户端驱动程序作为进程内 COM 服务器（DLL）实现， C++是用于编写 USB 设备的客户端驱动程序的首选语言。 通常情况下，客户端驱动程序实现框架公开的多个接口。 本主题引用客户端驱动程序定义的类，该类实现作为回调类的框架接口。 在对这些类进行实例化后，生成的回调对象将与特定的框架对象合作。 这种合作关系使客户端驱动程序可以对框架报告的设备或与系统相关的事件做出响应。 每当 Windows 通知框架有关某些事件时，框架会调用客户端驱动程序的回调（如果有）。 否则，框架将继续执行事件的默认处理。 模板代码定义驱动程序、设备和队列回调类。

有关模板生成的源代码的说明，请参阅[了解 USB 客户端驱动程序的 UMDF 模板代码](understanding-the-umdf-template-code-for-usb.md)。

### <a name="prerequisites"></a>必备条件

若要开发、调试和安装用户模式驱动程序，需要两台计算机：

-   运行 Windows 7 或更高版本的 Windows 操作系统的主计算机。 主计算机是您的开发环境，您可以在其中编写和调试驱动程序。
-   运行要测试其驱动程序的操作系统版本（例如，Windows 10，版本1903）的目标计算机。 目标计算机具有要调试的用户模式驱动程序和一个调试器。

在某些情况下，主机和目标计算机运行相同版本的 Windows 时，只能有一台运行 Windows 7 或更高版本的 Windows 的计算机。 本主题假定你使用两台计算机来开发、调试和安装用户模式驱动程序。

在开始之前，请确保满足以下要求：

**软件要求**

-   主计算机具有 Visual Studio 2019。
-   你的主计算机具有适用于 Windows 10 的最新 Windows 驱动程序工具包（WDK），版本1903。

    工具包包括开发、构建和调试 USB 客户端驱动程序所需的标头、库、工具、文档和调试工具。 可以从[如何获取 wdk](https://go.microsoft.com/fwlink/p/?linkid=617585)获取最新版本的 wdk。

-   您的主计算机具有适用于 Windows 的调试工具的最新版本。 可以从 WDK 获取最新版本，也可以[下载和安装适用于 Windows 的调试工具](https://go.microsoft.com/fwlink/p/?linkid=617701)。
-   如果你使用两台计算机，则必须为用户模式调试配置主机和目标计算机。 有关详细信息，请参阅[在 Visual Studio 中设置用户模式调试](https://docs.microsoft.com/windows-hardware/drivers/debugger/setting-up-user-mode-debugging-in-visual-studio)。

**硬件要求**

获取将为其写入客户端驱动程序的 USB 设备。 在大多数情况下，会向你提供 USB 设备及其硬件规范。 该规范描述了设备功能和支持的供应商命令。 使用规范来确定 USB 驱动程序的功能以及相关的设计决策。

如果你不熟悉 USB 驱动程序开发，请使用 OSR USB FX2 学习工具包来研究 WDK 附带的 USB 示例。 可以从[OSR Online](https://go.microsoft.com/fwlink/p/?linkid=617553)获取学习工具包。 它包含 USB FX2 设备以及实现客户端驱动程序所需的所有硬件规范。

**建议阅读**

-   [适用于所有驱动程序开发人员的概念](https://docs.microsoft.com/windows-hardware/drivers/gettingstarted/concepts-and-knowledge-for-all-driver-developers)
-   [设备节点和设备堆栈](https://docs.microsoft.com/windows-hardware/drivers/gettingstarted/device-nodes-and-device-stacks)
-   [Windows 驱动程序入门](https://docs.microsoft.com/windows-hardware/drivers/gettingstarted/index)
-   [用户模式驱动程序框架](https://docs.microsoft.com/windows-hardware/drivers/debugger/user-mode-driver-framework-debugging)
-   *使用 Windows Driver Foundation 开发驱动程序*，由 "Orwick" 和 "专家 Smith" 编写。 有关详细信息，请参阅[通过 WDF 开发驱动程序](https://go.microsoft.com/fwlink/p/?linkid=617702)。

<a name="instructions"></a>说明
------------

### <a href="" id="generate-the-umdf-driver-code-by-using-the-visual-studio-2019-usb-driver-template"></a>步骤1：使用 Visual Studio 2019 USB 驱动程序模板生成 UMDF 驱动程序代码

<a href="" id="generate"></a>有关生成 UMDF 驱动程序代码的说明，请参阅[基于模板编写 UMDF 驱动程序](https://docs.microsoft.com/windows-hardware/drivers/gettingstarted/writing-a-umdf-driver-based-on-a-template)。

**对于 USB 特定的代码，请在 Visual Studio 2019 中选择以下选项**

1.  在 "**新建项目**" 对话框顶部的 "搜索" 框中，键入 " **USB"。**
2.  在中间窗格中，选择 "**用户模式驱动程序，USB （UMDF V2）** "。
3.  单击“下一步”。
4.  输入项目名称，选择 "保存位置"，然后单击 "**创建**"。

以下屏幕截图显示了**USB 用户模式驱动程序**模板的 "**新建项目**" 对话框。

![visual studio 新项目选项](images/umdf-template-visual-studio-2019.png)

![visual studio 新项目选项第二屏幕](images/umdf-template-visual-studio-2019-2.png)

本主题假定项目名称为 "MyUSBDriver\_UMDF\_"。 它包含以下文件：

| 文件                      | 描述                                                                                                                                                                                                                                                                                                                                                                                                                                  |
|----------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Driver .h;驱动程序。c         | 声明和定义一个实现[**IDriverEntry**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wudfddi/nn-wudfddi-idriverentry)接口的回调类。 类定义框架驱动程序对象调用的方法。 此类的主要目的是为客户端驱动程序创建设备对象。                                                                                                                                                                     |
| 设备 .h;设备 c         | 声明和定义一个实现[**IPnpCallbackHardware**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wudfddi/nn-wudfddi-ipnpcallbackhardware)接口的回调类。 类定义框架设备对象调用的方法。 此类的主要目的是处理因即插即用（PnP）状态更改而发生的事件。 类还会分配和初始化客户端驱动程序在系统中加载所需的资源。 |
| IoQueue;IoQueue       | 声明和定义一个实现[**IQueueCallbackDeviceIoControl**](https://docs.microsoft.com/windows-hardware/drivers/ddi/wudfddi/nn-wudfddi-iqueuecallbackdeviceiocontrol)接口的回调类。 类定义框架队列对象调用的方法。 此类的目的是检索在框架中排队的 i/o 请求。                                                                                                                               |
| Internal。h                 | 提供客户端驱动程序和与 USB 设备通信的用户应用程序共享的常见声明。 它还声明跟踪函数和宏。                                                                                                                                                                                                                                                                          |
| Dllsup .cpp                 | 包含驱动程序模块入口点的实现。                                                                                                                                                                                                                                                                                                                                                                              |
| *&lt;项目名称&gt;* .inf | 在目标计算机上安装客户端驱动程序所需的 INF 文件。                                                                                                                                                                                                                                                                                                                                                               |
| 导出 .def                | .DEF 文件，用于导出驱动程序模块的入口点函数名称。                                                                                                                                                                                                                                                                                                                                                                    |

 

### <a href="" id="modify-the-inf-file-to-add-information-about-your-device"></a>步骤2：修改 INF 文件以添加有关设备的信息

<a href="" id="modify"></a>构建驱动程序之前，必须用有关设备的信息（尤其是硬件 ID 字符串）修改模板 INF 文件。

**提供硬件 ID 字符串**

1.  将 USB 设备连接到主机计算机，并让 Windows 枚举设备。
2.  打开**设备管理器**并打开设备的 "属性"。
3.  在 "**详细信息**" 选项卡上，选择 "属性" 下的**Hardward id** **。**

    设备的硬件 ID 显示在列表框中。 右键单击并复制硬件 ID 字符串。

4.  在**解决方案资源管理器**中，展开 "**驱动程序文件**"，然后打开 INF。
5.  将以下硬件 ID 字符串替换为。

    `[Standard.NT$ARCH$]`

    `%DeviceName%=MyDevice_Install, USB\VID_vvvv&PID_pppp`

请注意驱动程序的信息（INF）文件中的**AddReg**条目。

`[CoInstallers_AddReg] ;`

`HKR,,CoInstallers32,0x00010008,"WudfCoinstaller.dll"`

`HKR,,CoInstallers32,0x00010008,"WudfUpdate_01011.dll"`

`HKR,,CoInstallers32,0x00010008,"WdfCoInstaller01011.dll,WdfCoInstaller"`

`HKR,,CoInstallers32,0x00010008,"WinUsbCoinstaller2.dll"`

- WudfCoinstaller （配置共同安装程序）
- WUDFUpdate\_ *&lt;版本&gt;* （可再发行的共同安装程序）
- Wdfcoinstaller<em>&lt;版本&gt;</em>（KMDF 的共同安装程序）
- Winusbcoinstaller2 （（Winusb 的共同安装程序）
- MyUSBDriver\_UMDF\_.dll （客户端驱动程序模块）

如果 INF AddReg 指令引用了 UMDF 可再发行组件（WUDFUpdate\_ *&lt;版本&gt;* ），则不得引用配置共同安装程序（WUDFCoInstaller）。 在 INF 中引用这两个共同安装程序将导致安装错误。

所有基于 UMDF 的 USB 客户端驱动程序都需要两个 Microsoft 提供的驱动程序：反射器和 WinUSB。

-   反射器—如果驱动程序已成功加载，则会将反射器作为内核模式堆栈中最顶层的驱动程序加载。 反射器必须是内核模式堆栈中的顶层驱动程序。 为了满足此要求，模板的 INF 文件将反射器指定为一个服务，将 WinUSB 指定为 INF 中的较低筛选器驱动程序：

    `[MyDevice_Install.NT.Services]`

    `AddService=WUDFRd,0x000001fa,WUDFRD_ServiceInstall  ; flag 0x2 sets this as the service for the device`

    `AddService=WinUsb,0x000001f8,WinUsb_ServiceInstall  ; this service is installed because its a filter.`

-   WinUSB-安装包必须包含 Winusb 的 coinstallers，因为对于客户端驱动程序，WinUSB 是内核模式 USB 驱动程序堆栈的网关。 加载的另一个组件是客户端驱动程序的主机进程（Wudfhost）中的用户模式 DLL，名为 WinUsb。 Winusb 公开[Winusb 函数](https://docs.microsoft.com/previous-versions/windows/hardware/drivers/ff540046(v=vs.85)#winusb)，可简化客户端驱动程序和 Winusb 之间的通信过程。

### <a href="" id="build-the-usb-client-driver-code"></a>步骤3：生成 USB 客户端驱动程序代码

<a href="" id="build"></a>
**生成驱动程序**

1.  在 Visual Studio 2019 中打开驱动程序项目或解决方案。
2.  右键单击 "**解决方案资源管理器**中的解决方案，然后选择" **Configuration Manager**"。
3.  从 " **Configuration Manager**中，选择**活动解决方案配置**（例如"**调试**"或"**发布**"）以及与你感兴趣的生成类型相对应的**活动解决方案平台**（例如， **Win32**）。
4.  在整个项目中验证你的设备接口 GUID 是否准确。 
    - 设备接口 GUID 是在 node.js 中定义的，并且从设备的 `MyUSBDriverUMDFCreateDevice` 中引用。 创建名为 "MyUSBDriver\_UMDF\_" 的项目时，Visual Studio 2019 将定义具有名称 `GUID_DEVINTERFACE_MyUSBDriver_UMDF_` 的设备接口 GUID，但会调用具有错误参数 "GUID_DEVINTERFACE_MyUSBDriverUMDF" `WdfDeviceCreateDeviceInterface`。 将不正确的参数替换为在 Trace .h 中定义的名称，以确保正确生成驱动程序。 
4.  在 "**生成**" 菜单中，单击 "**生成解决方案**"。

有关详细信息，请参阅[构建驱动程序](https://docs.microsoft.com/windows-hardware/drivers/develop/building-a-driver)。

### <a href="" id="configure-a-computer-for-testing-and-debugging"></a>步骤4：配置计算机以进行测试和调试

若要测试和调试驱动程序，请在主计算机上运行调试器，并在目标计算机上运行该驱动程序。 到目前为止，你已在主计算机上使用 Visual Studio 来构建驱动程序。 接下来，需要配置目标计算机。 若要配置目标计算机，请按照[设置计算机以进行驱动程序部署和测试](https://docs.microsoft.com/windows-hardware/drivers/gettingstarted/provision-a-target-computer-wdk-8-1)中的说明进行操作。

### <a href="" id="enable-tracing-for-kernel-debugging"></a>步骤5：为内核调试启用跟踪

模板代码包含几个跟踪消息（TraceEvents），可以帮助您跟踪函数调用。 源代码中的所有函数都包含标记例程的入口和出口的跟踪消息。 对于错误，跟踪消息包含错误代码和有意义的字符串。 由于已为驱动程序项目启用 WPP 跟踪，因此在生成过程中创建的 PDB 符号文件包含跟踪消息格式设置说明。 如果将主机和目标计算机配置为使用 WPP 跟踪，则驱动程序可以将跟踪消息发送到文件或调试器。

**为主机配置 WPP 跟踪**

1. 通过从 PDB 符号文件中提取跟踪消息格式指令来创建跟踪消息格式（TMF）文件。

   你可以使用 Tracepdb 来创建 TMF 文件。 该工具位于<em>&lt;安装文件夹&gt;</em>Windows 工具包\\10\\bin\\ *&lt;&gt;* 以下命令将为驱动程序项目创建 TMF 文件。

   **tracepdb-f \[PDBFiles\]-p \[TMFDirectory\]**

   **-F**选项指定 PDB 符号文件的位置和名称。 **-P**选项指定由 Tracepdb 创建的 TMF 文件的位置。 有关详细信息，请参阅[**Tracepdb 命令**](https://docs.microsoft.com/windows-hardware/drivers/devtest/tracepdb-commands)。

   在指定位置，将看到三个文件（项目中每个 .c 文件一个）。 它们具有 GUID 文件名。

2. 在调试器中，键入以下命令：
   1.  **。 load Wmitrace**

       加载 Wmitrace 扩展名。

   2.  **。链**

       验证是否已加载调试器扩展。

   3.  * *！ wmitrace. searchpath + * * *&lt;TMF 文件位置&gt;*

       将 TMF 文件的位置添加到调试器扩展的搜索路径。

       输出如下所示：

       `Trace Format search path is: 'C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE;c:\drivers\tmf'`

**将目标计算机配置为进行 WPP 跟踪**

1. 请确保目标计算机上已有 Tracelog 工具。 该工具位于<em>&lt;安装\_文件夹，&gt;</em>Windows 工具包\\10\\工具\\ *&lt;&gt;* ！ 有关详细信息，请参阅[**Tracelog 命令语法**](https://docs.microsoft.com/windows-hardware/drivers/devtest/tracelog-command-syntax)。
2. 打开**命令窗口**并以管理员身份运行。
3. 键入下列命令：

   **tracelog-start MyTrace \#c918ee71-68c7-4140-8f7d-c907abbcb05d-旗 0xFFFF-level 7-**

   命令启动一个名为 MyTrace 的跟踪会话。

   **Guid**参数指定跟踪提供程序的 guid，它是客户端驱动程序。 你可以从 Visual Studio 2019 项目中的 Trace 获取 GUID。 作为另一种选择，可以键入以下命令，并在 guid.empty 文件中指定 GUID。 文件包含连字符格式的 GUID：

   **tracelog-start MyTrace\\驱动程序\\提供程序。 guid-标志 0xFFFF-级别 7-rt-kd**

   您可以通过键入以下命令来停止跟踪会话：

   **tracelog-停止 MyTrace**

### <a href="" id="deploy-the-driver-on-the-target-computer"></a>步骤6：在目标计算机上部署驱动程序

1. 在 "**解决方案资源管理器**" 窗口中，右键单击<em>&lt;项目名称&gt;</em>"**包**"，然后选择 "**属性**"。
2. 在左窗格中，导航到 "**配置属性" &gt; 驱动程序安装 &gt; 部署**。
3. 选中 "启用部署"，并选中 "导入到驱动程序存储区"。
4. 对于 "**远程计算机名称**"，指定目标计算机的名称。
5. 选择“安装并验证”。
6. 单击“确定”。
7. 在“调试”菜单上，选择“开始调试”或按键盘上的 **F5**。

**请注意**  在**硬件 Id 驱动程序更新**下*不*指定设备的硬件 id。 只能在驱动程序的信息（INF）文件中指定硬件 ID。

 

### <a href="" id="view-the-driver-in-device-manager"></a>步骤7：在设备管理器中查看驱动程序

<a href="" id="devicemanager"></a>
1.  输入以下命令打开**设备管理器**。

    **devmgmt.msc**

2.  验证**设备管理器**显示以下节点。

    **USB 设备**

    **MyUSBDriver\_UMDF\_设备**

### <a href="" id="view-the-output-in-the-debugger"></a>步骤8：在调试器中查看输出

验证跟踪消息显示在主计算机上的**调试器 "即时" 窗口**中。

输出应如下所示。

``` syntax
[0]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::OnPrepareHardware Entry
[0]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::OnPrepareHardware Exit
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::CreateInstanceAndInitialize Entry
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::Initialize Entry
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::Initialize Exit
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::CreateInstanceAndInitialize Exit
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::Configure Entry
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyIoQueue::CreateInstanceAndInitialize Entry
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyIoQueue::Initialize Entry
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyIoQueue::Initialize Exit
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyIoQueue::CreateInstanceAndInitialize Exit
[1]0744.05F0::00/00/0000-00:00:00.000 [MyUSBDriver_UMDF_]CMyDevice::Configure Exit
```

<a name="remarks"></a>备注
-------

让我们看看框架和客户端驱动程序如何协同工作以与 Windows 交互，并处理发送到 USB 设备的请求。 此图显示了在系统中为基于 UMDF 的 USB 客户端驱动程序加载的模块。

![用户模式客户端驱动程序体系结构](images/umdfstack.png)

此处介绍了每个模块的用途：

-   应用程序-一种用户模式进程，该进程发出 i/o 请求以便与 USB 设备通信。
-   I/o 管理器-一种 Windows 组件，用于创建 i/o 请求数据包（Irp）来表示接收的应用程序请求，并将这些请求转发到目标设备的内核模式设备堆栈的顶部。
-   反射器—在内核模式设备堆栈（WUDFRd）顶部安装的 Microsoft 提供的内核模式驱动程序。 反射器将从 i/o 管理器接收的 Irp 重定向到客户端驱动程序主机进程。 收到请求后，框架和客户端驱动程序将处理该请求。
-   主机进程—用户模式驱动程序在其中运行的进程（Wudfhost）。 它还承载框架和 i/o 发送器。
-   客户端驱动程序— USB 设备的用户模式功能驱动程序。
-   UMDF —框架模块，它代表客户端驱动程序处理与 Windows 的大多数交互。 它公开了客户端驱动程序可用于执行常见驱动程序任务的用户模式设备驱动程序接口（DDIs）。
-   调度程序—在宿主进程中运行的机制;确定如何在用户模式驱动程序处理请求后将请求转发到内核模式，并已达到用户模式堆栈的底部。 在此图中，调度程序将请求转发到用户模式 DLL Winusb。
-   Winusb-一种 Microsoft 提供的用户模式 DLL，该 DLL 公开[Winusb 函数](https://docs.microsoft.com/previous-versions/windows/hardware/drivers/ff540046(v=vs.85)#winusb)，这些函数简化了客户端驱动程序和 Winusb （Winusb，在内核模式下加载）之间的通信处理。
-   Winusb-Microsoft 提供的驱动程序，适用于 USB 设备的所有 UMDF 客户端驱动程序都需要此驱动程序。 驱动程序必须安装在反射器下，并充当内核模式下 USB 驱动程序堆栈的网关。 有关详细信息，请参阅[WinUSB](winusb.md)。
-   USB 驱动程序堆栈-由 Microsoft 提供的一组驱动程序，用于处理与 USB 设备的协议级别的通信。 有关详细信息，请参阅[Windows 中的 USB 主机端驱动程序](usb-3-0-driver-stack-architecture.md)。

每当应用程序发出对 USB 驱动程序堆栈的请求时，Windows i/o 管理器都会向反射器发送请求，以在用户模式下将该请求定向到客户端驱动程序。 客户端驱动程序通过调用特定的 UMDF 方法来处理请求，该方法在内部调用[WinUSB 函数](https://docs.microsoft.com/previous-versions/windows/hardware/drivers/ff540046(v=vs.85)#winusb)以将请求发送到 WinUSB。 收到请求后，WinUSB 将处理该请求或将其转发到 USB 驱动程序堆栈。

## <a name="related-topics"></a>相关主题
[了解 USB 客户端驱动程序的 UMDF 模板代码](understanding-the-umdf-template-code-for-usb.md)  
[如何在 USB 设备的 UMDF 驱动程序中启用 USB 选择性挂起和系统唤醒](https://go.microsoft.com/fwlink/p/?linkid=617587)  
[USB 客户端驱动程序开发入门](getting-started-with-usb-client-driver-development.md)  



